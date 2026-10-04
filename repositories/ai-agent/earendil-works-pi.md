---
title: "earendil-works/pi"
repository: "earendil-works/pi"
url: "https://github.com/earendil-works/pi"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# earendil-works/pi

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Pi는 터미널 코딩 에이전트와 이를 구성하는 모델 API·도구 실행·상태 관리 패키지를 함께 제공하는 Agent Harness 프로젝트다.
Harness는 모델이 요청을 받고 도구를 사용하며 대화를 이어갈 수 있도록 둘러싸는 실행 구조를 뜻한다.
모델 자체를 배포하는 저장소가 아니며, 터미널에서 직접 쓰거나 TypeScript 프로그램에 포함할 수 있다.[1][2]

## 모델 호출과 코딩 작업 사이의 층

모델 API에 문장을 보내 답을 받는 것과 실제 프로젝트 파일을 읽고 수정하는 작업은 다르다.
코딩 에이전트에는 도구 호출을 실행하고 결과를 다시 모델에 전달하는 흐름, 대화 상태, 사용자에게 진행을 보여주는 화면이 필요하다.
Pi는 이를 하나의 거대한 기능으로만 묶지 않고 패키지로 구분한다.[1]

`pi-ai`는 여러 모델 공급자를 다루는 공통 API, `pi-agent-core`는 도구 호출과 상태를 관리하는 런타임, `pi-coding-agent`는 사용자가 만나는 CLI다.
TUI 패키지는 터미널 화면 구성을 맡는다.
이런 경계를 알면 모델 공급자를 바꾸는 문제와 화면·도구 동작을 확장하는 문제를 같은 설정으로 혼동하지 않을 수 있다.[1]

## 사용 인터페이스와 확장 방식

코딩 에이전트 README는 터미널 대화 외에 print·JSON·RPC 모드와 TypeScript SDK를 소개한다.
SDK는 프로그램에서 세션을 생성하고 요청·이벤트를 다루는 인터페이스다.
RPC는 다른 프로세스에서 기능을 호출하는 방식이다.
대화형 화면을 쓰는 경우와 자동화 프로그램이 출력을 소비하는 경우에 필요한 경로가 다르다.[2]

프롬프트 템플릿, Skill, extension, theme을 만들거나 Pi package로 가져오는 확장 방식도 안내한다.
Skill이 작업 지침을 제공하는 것과 extension이 실행 기능을 등록하는 것은 구분해야 한다.
공식 도구 예제는 사용자 정의 도구를 extension의 `pi.registerTool()`로 등록하는 경로를 가리킨다.[2][3]

최소 SDK 예제는 현재 작업 디렉터리와 `~/.pi/agent`에서 기본 설정·Skill·extension·도구·맥락 파일을 발견한다고 설명한다.
짧은 코드가 아무 설정도 쓰지 않는 깨끗한 실행을 뜻하지는 않는다.
기존 사용자 설정을 활용하는 편리함과 어떤 지침이 로드되었는지 추적할 필요가 함께 생긴다.[4]

## 도구 선택과 세션 수명

`createAgentSession`으로 세션을 만들면 이벤트 구독을 통해 응답 조각을 받을 수 있다.
최소 예제는 `message_update` 가운데 `text_delta`일 때만 텍스트를 표준 출력에 쓰고, 요청이 끝난 뒤 메시지 목록도 확인한다.
화면에 보인 답변 조각과 세션에 남은 메시지 기록이 서로 다른 관찰 자료임을 보여준다.[4]

별도 도구 예제는 읽기·검색·파일 목록 도구만 선택한 세션과, 셸을 포함한 세션, 작업 디렉터리를 지정한 세션을 비교한다.
메모리 안에만 유지하는 `SessionManager.inMemory()`도 사용한다.
작업 범위, 제공할 도구, 상태 보관 방식을 호출하는 프로그램이 명시할 수 있다는 예시다.[3]

각 예제는 사용한 세션에 `dispose()`를 호출한다.
특히 최소 예제는 `finally` 블록에서 정리하므로 요청이 정상 완료되든 오류가 나든 자원 정리 경로를 둔다.
모델 호출 한 번을 보여주는 짧은 코드에도 시작과 종료를 분명히 두는 점이 SDK 사용 흐름의 일부다.[4]

## 예시로 따라가는 흐름

공식 `01-minimal.ts`는 “현재 디렉터리에 어떤 파일이 있는가?”라는 질문을 보낸다.
먼저 기본값으로 세션을 생성하고 이벤트 수신 함수를 연결한다.
그다음 `session.prompt()`를 기다리며, 들어오는 텍스트 조각을 화면에 표시한다.
완료 후에는 `session.state.messages`를 순회해 기록을 출력하고 마지막에 세션을 정리한다.[4]

입력은 파일 목록을 묻는 짧은 문장이지만 결과는 실행 위치와 설정된 모델·도구에 영향을 받는다.
예제 코드는 반환될 파일명을 하드코딩하지 않으며, 이 글도 실제 목록을 만들어 제시하지 않는다.
사람이 확인할 부분은 현재 디렉터리가 의도한 곳인지, 예상한 모델과 도구가 쓰였는지, 텍스트 답변이 실제 도구 결과와 일치하는지다.
제한된 읽기 작업만 원한다면 다음 도구 선택 예제와 비교해 편집·쓰기·셸을 제공할 필요가 있는지 검토할 수 있다.
다만 도구 목록을 줄이는 것과 OS 수준 격리는 별개의 조건이다.
이 예제는 읽기만 했으며 Pi 세션이나 모델 요청을 실제로 실행하지 않았다.[4][3]

## 기본값은 실행한 사용자의 권한이다

루트 README는 파일 시스템·프로세스·네트워크·자격 증명 접근을 제한하는 내장 권한 시스템이 없다고 명확히 밝힌다.
기본적으로 실행한 사용자와 프로세스의 권한을 갖는다.
따라서 “read-only”라는 예제 주석이나 모델의 계획을 보안 경계로 간주해서는 안 된다.
특히 셸 도구가 있으면 파일 편집 전용 도구를 빼는 것만으로 쓰기를 금지했다고 볼 수 없다.[1][3]

더 강한 경계가 필요하면 컨테이너나 샌드박스를 사용하도록 안내하며, 전체 프로세스를 격리하는 방식과 호스트의 Pi에서 도구만 별도 환경으로 보내는 방식을 구분한다.
호스트 인증을 어디에 두는지와 파일·네트워크가 어디서 실행되는지에 따라 보호 범위가 달라진다.[1]

또한 공급자 인증과 모델 비용은 별도 준비가 필요하다.
npm 설치 경로의 Node.js 요구사항과 공급자 로그인 절차는 코딩 에이전트 README에 명시되어 있다.
공개 세션 공유도 별도 도구로 안내하므로 세션에 포함된 코드·프롬프트·도구 결과를 검토하지 않고 공개하는 절차로 이해하면 안 된다.[1][2]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/earendil-works/pi/blob/main/README.md)
   모델 API, 런타임, 코딩 CLI의 패키지 역할을 구분한 뒤 Permissions & Containerization을 읽는다.
   확장성 설명보다 기본 권한 범위를 먼저 알아두는 자료다.
2. [코딩 에이전트 README](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/README.md)
   직접 대화와 자동화 모드, SDK의 차이를 확인한다.
   호스트에 필요한 실행 환경과 모델 인증이 어느 단계에서 준비되는지도 살핀다.
3. [최소 SDK 예제](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/examples/sdk/01-minimal.ts)
   생성·구독·요청·기록 확인·정리 순서를 따라간다.
   한 번의 응답 문자열이 아니라 지속 상태와 이벤트를 다루는 구조를 볼 수 있다.
4. [도구 선택 예제](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/examples/sdk/05-tools.ts)
   도구 이름과 작업 디렉터리, 메모리 세션 구성을 비교한다.
   기능을 노출하는 선택이 보안 격리와 같은지는 별도로 질문해야 한다.

## 정리

Pi는 코딩 에이전트의 실행 층을 재사용 가능한 패키지와 SDK로 제공한다.
필요한 인터페이스와 도구를 선택할 수 있지만 기본 권한을 제한하는 시스템은 없으므로, 실제 격리와 데이터 공개 범위는 별도로 정해야 한다.

## 자료 확인 범위

2026-09-27 공식 루트·코딩 에이전트 README와 SDK의 최소 사용·도구 선택 예제를 확인했다.
설치, 공급자 인증, 모델 호출, 파일 작업이나 격리 환경 실행은 수행하지 않았다.

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

[1] earendil-works/pi — README.md

<https://github.com/earendil-works/pi/blob/main/README.md>

[2] earendil-works/pi — packages/coding-agent/README.md

<https://github.com/earendil-works/pi/blob/main/packages/coding-agent/README.md>

[3] earendil-works/pi — packages/coding-agent/examples/sdk/05-tools.ts

<https://github.com/earendil-works/pi/blob/main/packages/coding-agent/examples/sdk/05-tools.ts>

[4] earendil-works/pi — packages/coding-agent/examples/sdk/01-minimal.ts

<https://github.com/earendil-works/pi/blob/main/packages/coding-agent/examples/sdk/01-minimal.ts>
