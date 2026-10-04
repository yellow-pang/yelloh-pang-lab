---
title: "ChromeDevTools/chrome-devtools-mcp"
repository: "ChromeDevTools/chrome-devtools-mcp"
url: "https://github.com/ChromeDevTools/chrome-devtools-mcp"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# ChromeDevTools/chrome-devtools-mcp

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Chrome DevTools MCP는 AI 코딩 도우미가 실행 중인 Chrome을 조작하고 개발자 도구의 진단 자료를 읽도록 연결하는 서버다.
MCP는 모델을 사용하는 앱과 외부 도구 사이에서 기능과 입력·결과를 주고받는 규약이다.
이 프로젝트는 자체 코딩 모델이나 웹사이트가 아니라 그 연결 역할을 맡으며, MCP 없이 사용할 수 있는 CLI도 공식 README에서 안내한다.[1]

## 코드를 고친 뒤 브라우저에서 확인해야 할 것

페이지가 느리거나 버튼이 반응하지 않을 때 소스만 읽어서는 실제 오류를 설명하기 어렵다.
화면 캡처, 콘솔 오류, 네트워크 요청, 시간별 실행 기록이 함께 필요할 수 있다.
이 서버는 에이전트가 그런 자료를 도구 호출로 가져오게 하고, Puppeteer를 이용한 Chrome 조작과 결과 대기를 제공한다.[1]

그러나 “브라우저에 연결했다”와 “성능 문제를 찾았다”는 다른 단계다.
공식 첫 프롬프트는 특정 사이트의 성능을 확인하라는 요청이고, 클라이언트가 브라우저를 열어 성능 trace를 기록하는 흐름을 예상한다.
서버 연결 자체만으로 브라우저가 자동 시작되는 것은 아니며, 브라우저가 필요한 도구를 처음 사용할 때 시작한다고 설명한다.[1]

## 요소를 읽는 스냅샷과 진단 기록

도구 참조의 `take_snapshot`은 접근성 트리를 바탕으로 페이지를 텍스트로 표현하고 요소마다 uid를 제공한다.
접근성 트리는 버튼·입력란 등의 역할과 이름을 보조 기술이 이해할 수 있게 정리한 구조다.
에이전트는 최신 스냅샷에서 얻은 식별자로 클릭하거나 값을 채울 수 있다.
화면 이미지는 배치 확인에 유용하지만 텍스트 스냅샷과 같은 자료는 아니다.[2]

참조에는 입력, 탐색, 네트워크, 콘솔, 성능, 메모리 등 서로 다른 도구 묶음이 있다.
예를 들어 입력 폼을 채우는 도구와 오류 메시지의 세부 정보를 읽는 도구는 목적이 다르다.
하나의 큰 “웹 자동화” 기능으로 생각하기보다 행동을 수행하는 호출과 상태를 관측하는 호출을 나누어 읽어야 한다.[2]

성능 trace는 페이지가 동작하는 동안의 이벤트를 기록한 자료다. `performance_start_trace`와 `performance_stop_trace`가 기록의 범위를 정하고, `performance_analyze_insight`는 기록 결과에 나온 특정 분석 항목을 더 자세히 살핀다.
이때 insight set ID는 실제 결과가 제공한 목록에서 골라야 한다.
모델이 그럴듯한 분석 이름이나 ID를 지어내는 경로가 아니다.[2]

## 재로딩을 포함하는 성능 도구

대표 구현 `src/tools/performance.ts`에서 trace 시작은 읽기 전용으로 표시되지 않는다. `reload`와 `autoStop`의 기본값이 true이고, 재로딩을 선택하면 기존 URL을 기억한 뒤 `about:blank`로 이동하고 기록을 시작해 원래 페이지로 돌아간다.
이미 trace가 실행 중이면 먼저 멈추라고 응답하며 동시에 하나만 기록할 수 있도록 검사한다.[3]

이 세부사항은 측정 계획에 직접 영향을 준다.
사용자가 폼을 작성 중인 페이지에서 아무 생각 없이 trace를 시작하면 페이지 상태를 바꿀 수 있다.
또 페이지 로딩을 보는 기록과 사용자가 특정 버튼을 누르는 상호작용을 보는 기록은 범위가 다르다.
도구가 제공된다는 사실보다 어떤 시작·종료 조건으로 자료를 모았는지가 해석의 근거다.[2][3]

## 예시로 따라가는 흐름

공식 README의 “Check the performance of https://developers.chrome.com” 요청을 따라 읽어보자.
먼저 클라이언트는 해당 페이지를 열고 그 페이지 ID를 대상으로 기록을 시작해야 한다.
도구 참조도 자동 재로딩이나 자동 종료를 사용할 때는 trace 시작 전에 올바른 URL로 이동하라고 안내한다.
기본 재로딩 경로에서는 페이지 로딩을 기록하고 결과에 분석 항목들이 제공된다.
필요한 경우 원시 trace를 지정 경로에 저장할 수도 있다.[1][2][3]

이후 에이전트는 결과에 실제로 나온 insight set과 분석 이름을 골라 세부 내용을 요청한다.
사람은 제안된 원인을 코드에 바로 적용하기 전에 어떤 페이지, 어떤 기록 구간, 어떤 환경에서 관측했는지 확인해야 한다.
한 번의 로컬 측정 결과를 모든 사용자의 경험으로 일반화하면 안 된다.
이 글에서는 사이트를 열거나 trace를 기록하지 않았으므로 느린 원인, 지표값, 개선율을 제시하지 않는다.
예시가 보여주는 것은 자연어 요청이 페이지 선택, 기록, 세부 분석, 근거 검토로 나뉜다는 흐름이다.[1][2]

## 브라우저 접근은 데이터 접근이기도 하다

README는 브라우저와 DevTools의 내용이 MCP 클라이언트에 노출되며 데이터의 조사·수정이 가능하다고 경고한다.
개인 정보나 민감한 로그인 세션을 연결한다면 에이전트가 접근할 수 있는 범위가 넓어진다.
스크린샷만 받는 도구로 생각해서는 안 되며, 조작 권한과 관측 자료의 공유 범위를 먼저 정해야 한다.[1]

사용 통계 수집은 기본 활성화로 설명되어 있고, Chrome 자체의 통계 설정과 독립적이다.
성능 도구가 실사용자 자료를 조회하기 위해 trace URL을 CrUX API에 보낼 수 있다는 별도 안내도 있다. `--no-usage-statistics`와 `--no-performance-crux`는 각각 다른 경로를 끄므로 한쪽 설정이 모든 전송을 막는다고 해석하면 안 된다.[1]

공식 지원 대상은 Google Chrome과 Chrome for Testing이며, 다른 Chromium 기반 브라우저는 동작을 보장하지 않는다.
Node.js LTS, Chrome, npm 요구사항도 있다.
에이전트나 편집기에서 MCP 설정이 인식되는 것과 실제 브라우저·도구가 정상 연결되는 것은 각각 확인할 조건이다.[1]

## 직접 읽어볼 자료

1. [README의 Getting started와 Disclaimers](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md)
   연결 설정과 첫 요청을 읽고 바로 데이터 노출·통계·CrUX 조건을 확인한다.
   브라우저 시작 시점과 공식 지원 대상도 이 문서에서 구분할 수 있다.
2. [도구 참조](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/tool-reference.md)
   `take_snapshot`의 uid가 입력 도구에 어떻게 연결되는지, trace 분석의 ID는 어디서 얻는지 따라간다.
   작업용 호출과 관측용 호출의 입력을 비교하는 것이 읽기의 중심이다.
3. [성능 도구 구현](https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/src/tools/performance.ts)
   시작 함수의 기본값과 중복 기록 검사, 재로딩 순서를 살핀다.
   성능 측정이 페이지 상태를 바꾸는 작업일 수 있다는 사실을 실제 코드와 연결해서 확인한다.

## 정리

Chrome DevTools MCP는 코딩 도우미에게 실제 브라우저의 관측·조작 경로를 제공한다.
진단의 신뢰도는 도구 수가 아니라 올바른 페이지와 기록 조건, 실제 반환된 근거를 사용했는지에 달려 있다.

## 자료 확인 범위

2026-09-27 기준 공식 README, 도구 참조의 스냅샷·입력·성능 부분, performance 구현의 시작·종료 및 분석 인터페이스를 읽었다.
서버 설치, MCP 연결, 웹 탐색, 성능 측정은 실행하지 않았다.

## 사용자 생각

아래는 실제 판단을 넣기 전까지 비워두는 영역입니다.

- [ ] 지금 보려는 이유가 분명한가?
- [ ] 내 작업 환경에서 바로 적용할 수 있을까?
- [ ] 실험 10~20분으로 검증 가능한 값이 있는가?

## 나중에 할 일

- [ ] README 전체 읽기
- [ ] 설치/실행 예시가 있는지 확인
- [ ] 장단점, 주의점, 대체안 비교
- [ ] 블로그 글 제목/개인 결론 반영

## Sources

[1] ChromeDevTools/chrome-devtools-mcp — README.md

<https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/README.md>

[2] ChromeDevTools/chrome-devtools-mcp — docs/tool-reference.md

<https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/docs/tool-reference.md>

[3] ChromeDevTools/chrome-devtools-mcp — src/tools/performance.ts

<https://github.com/ChromeDevTools/chrome-devtools-mcp/blob/main/src/tools/performance.ts>
