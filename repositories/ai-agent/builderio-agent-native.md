---
title: "BuilderIO/agent-native"
repository: "BuilderIO/agent-native"
url: "https://github.com/BuilderIO/agent-native"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# BuilderIO/agent-native

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Agent-Native는 사람의 화면 조작과 AI Agent의 도구 호출이 같은 기능을 사용하도록 앱을 만드는 TypeScript 프레임워크다.
개발자가 기능을 action으로 한 번 정의하면 에이전트는 도구로 호출하고 UI는 코드에서 호출한다.
에이전트가 사람 대신 버튼을 찾아 클릭하는 방식과 달리, 둘이 공통 실행 경로를 사용하게 하는 설계다.[1]

## 채팅과 화면이 따로 움직이지 않게 한다

개인 자료 정리 앱에서 사람이 화면으로 이름을 바꾼 뒤 에이전트가 이전 이름으로 대답하거나, 에이전트가 만든 결과를 화면에서 다시 입력해야 한다면 두 인터페이스가 같은 앱처럼 작동하지 않는다.
Agent-Native는 이런 연결을 공유 action, 공유 데이터, 공유 화면 상태로 나눈다.
action은 입력 규칙·기능 설명·실행 코드를 묶은 단위이고, 화면 상태는 현재 페이지나 선택한 항목처럼 요청의 맥락을 제공하는 정보다.[1]

README는 두 호출 경로가 검증·권한·구현을 공유하며, 에이전트의 결과를 UI에서 보고 고치거나 승인할 수 있는 환경을 설명한다.
이 설계는 자연어 요청을 모든 권한의 대체물로 삼는다는 뜻이 아니다.
어떤 기능을 노출하고 누가 호출할 수 있게 할지는 action과 인증·권한 설계에서 정해야 한다.[1]

## 기능과 화면 이동을 같은 방식으로 드러낸다

대표 파일 `templates/chat/actions/hello.ts`는 작은 action을 보여 준다. `defineAction`에 기능 설명을 적고, Zod로 이름 입력이 문자열이며 생략 시 기본값을 갖는다고 정의한다.
Zod는 받은 데이터가 정한 형식에 맞는지 확인하기 위한 도구다.
실행 함수는 이름을 포함한 인사 메시지 객체를 반환한다.
도구 노출과 HTTP 호출 설정도 같은 정의 안에 들어 있다.[2]

README는 이 action을 에이전트가 `hello` 도구로 받고 React 화면은 `useActionQuery`로 호출한다고 설명한다.
HTTP, MCP, A2A, CLI 같은 다른 접점도 소개한다.
MCP는 에이전트에 도구를 연결하는 규약, A2A는 에이전트 사이의 통신 규약이다.
접점의 수보다 중요한 것은 각 화면마다 인사 기능을 다시 구현하지 않고 정의한 action을 기준으로 연결한다는 점이다.[1]

다른 대표 파일 `navigate.ts`는 화면 이동을 다룬다.
화면 이름이나 경로를 입력으로 받고 둘 다 없으면 오류를 내며, 현재 탭의 application state에 이동 명령을 기록한다.
설명에는 UI가 그 명령을 읽고 자동으로 지운다고 적혀 있다.
이 action은 `http: false`를 명시하므로, 모든 action이 모든 접점에 무조건 공개된다고 해석해서는 안 된다.[3]

데이터와 실행 기반도 분리되어 있다.
README는 운영용 PostgreSQL과 로컬 개발용 PGlite를 안내하고, 개발 가이드는 `DATABASE_URL`이 없을 때 로컬 저장 위치를 설명한다.
여러 패키지와 템플릿을 한 저장소에서 관리하지만 각 템플릿은 자기 action, UI, 데이터 스키마를 가진 독립 앱이라고 안내한다.[1][4]

## 예시로 따라가는 흐름

공식 인사 예제를 따라가 보자.
입력은 React 화면에서 `useActionQuery("hello", { name: "Alex" })`를 호출하는 경우다.
같은 기능을 자연어 요청으로 쓰는 에이전트 쪽에서는 `hello` 도구를 호출한다.
action 정의는 이름의 형식과 기본값을 정하고, 실행 함수에서 메시지 객체를 돌려준다.
소스상 반환 템플릿은 `Hello, ${name}!`이다.
이는 공식 코드의 입력·출력 정의를 설명한 것이며 직접 실행한 결과가 아니다.[1][2]

이 작은 사례에서는 데이터베이스를 수정하거나 외부 메시지를 보내지 않는다.
따라서 독자는 공통 정의가 어떤 형태인지에 집중할 수 있다.
사람이 확인할 부분은 UI와 도구 호출이 같은 action을 가리키는지, 입력 규칙과 반환 형식을 화면이 올바르게 다루는지다.
실제 데이터 편집 action으로 확장한다면 호출자 권한과 실패 처리가 추가로 필요하지만, 인사 예제만으로 그런 조건까지 모두 검증한 것은 아니다.[1][2]

화면 이동 예제까지 이어 보면 결과를 UI에 전달하는 다른 경로가 보인다.
에이전트가 이동할 view나 path를 전달하면 action은 이를 현재 탭의 상태에 기록하고 이동 안내를 반환한다.
이 코드만 읽었을 때 확실히 말할 수 있는 것은 이동 명령을 기록하는 단계다.
실제 화면이 명령을 읽고 원하는 경로로 이동했는지는 UI 쪽 소비 코드나 실행으로 확인해야 한다.
상태 기록 성공과 화면 변화 완료를 같은 사실로 처리하지 않는 것이 이 예제를 읽는 확인 지점이다.[3]

## 포함된 구성과 별도로 준비할 것

프레임워크는 채팅, 인증·권한, 재사용 지침과 기억, 예약·이벤트 자동화, 전문 에이전트 위임을 README에서 소개한다.
그러나 모델, SQL 데이터베이스, 필요한 도구와 인프라는 사용자가 연결하는 구조다.
템플릿을 선택했다는 사실만으로 외부 모델 호출이나 운영 데이터 저장 환경까지 준비되는 것은 아니다.[1]

개발 가이드는 Node.js와 pnpm 요구 조건, 환경 변수와 패키지 구성을 설명한다.
확인한 core 패키지와 루트 패키지의 Node.js 요구 값은 `>=22.22.0`이며 개발 가이드는 pnpm 10 이상을 안내한다.
저장소 설치 과정에는 의존하는 작업 공간 패키지를 자동으로 빌드하는 `postinstall`도 있어, 문서 읽기와 실제 설치는 분리할 필요가 있다.[5][6][4]

라이선스 표기는 원본 초안의 주의점을 유지해야 할 부분이다.
README와 core 패키지는 MIT를 표시하지만 루트 `package.json`은 ISC를 표시한다.
따라서 저장소 전체의 표기를 하나의 값으로 단정하지 않고 재사용할 파일·패키지의 라이선스를 확인해야 한다.
이 불일치를 이번 조사에서 해결했다고 보지는 않는다.[1][5][6]

검증·권한을 공통 action에 모으는 구조도 보안 보증과 같지는 않다.
공개 개발 가이드에는 권한 범위, 비밀 정보, 데이터 접근 관련 검사 스크립트가 설명되어 있지만, 이 글은 해당 검사 전체나 모든 action을 실행해 본 감사가 아니다.
모델의 자연어 이해와 앱의 접근 통제는 각각 검토해야 한다.[4]

## 직접 읽어볼 자료

- [README의 공유 action 예제](https://github.com/BuilderIO/agent-native/blob/main/README.md)
  공유 기능·데이터·상태의 차이를 읽고 React 호출과 에이전트 도구의 연결을 확인한다.
  프레임워크가 제공하는 실행 구조와 사용자가 별도로 연결해야 할 모델·인프라를 구분하는 시작점이다.[1]
- [chat 템플릿의 hello action](https://github.com/BuilderIO/agent-native/blob/main/templates/chat/actions/hello.ts)
  설명, 입력 스키마, 호출 접점, 반환 함수를 순서대로 살핀다.
  짧은 예제이므로 입력이 어떤 결과로 바뀌는지를 전체 파일에서 놓치지 않고 확인할 수 있다.[2]
- [chat 템플릿의 navigate action](https://github.com/BuilderIO/agent-native/blob/main/templates/chat/actions/navigate.ts)
  view·path 검사와 현재 탭 상태 기록을 읽는다.
  action이 이동을 요청한 시점과 UI가 실제 이동한 시점을 구분하고, HTTP 노출을 끄는 설정도 확인한다.[3]
- [DEVELOPMENT.md](https://github.com/BuilderIO/agent-native/blob/main/DEVELOPMENT.md)
  패키지와 독립 템플릿의 경계, 환경 변수, PGlite와 PostgreSQL 선택을 살핀다.
  예제의 개념을 이해한 뒤 실제 개발 환경에 어떤 준비가 필요한지 확인하는 순서다.[4]

## 정리

Agent-Native는 UI와 에이전트의 기능 호출을 action으로 모으고 데이터와 화면 상태를 공유하게 하는 앱 구축 기반이다.
대표 예제는 공통 정의와 상태 전달을 보여 주지만, 실제 권한 설계·모델 연결·운영 검증과 라이선스 확인은 별도로 남는다.

## 자료 확인 범위

2026-09-27 기준 README와 루트 구성, 개발 가이드, 두 패키지 메타데이터, `hello.ts`와 `navigate.ts`를 확인했다.
템플릿 설치나 실행, 모델 호출, 권한 검사 시험은 하지 않았다.

## 나에게 어떤 가치가 있는가?

### 공부 가치

<!-- 사용자 작성 -->

### 직접 사용 가치

<!-- 사용자 작성 -->

### 참고 가치

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] BuilderIO/agent-native — README.md

<https://github.com/BuilderIO/agent-native/blob/main/README.md>

[2] BuilderIO/agent-native — templates/chat/actions/hello.ts

<https://github.com/BuilderIO/agent-native/blob/main/templates/chat/actions/hello.ts>

[3] BuilderIO/agent-native — templates/chat/actions/navigate.ts

<https://github.com/BuilderIO/agent-native/blob/main/templates/chat/actions/navigate.ts>

[4] BuilderIO/agent-native — DEVELOPMENT.md

<https://github.com/BuilderIO/agent-native/blob/main/DEVELOPMENT.md>

[5] BuilderIO/agent-native — package.json

<https://github.com/BuilderIO/agent-native/blob/main/package.json>

[6] BuilderIO/agent-native — packages/core/package.json

<https://github.com/BuilderIO/agent-native/blob/main/packages/core/package.json>
