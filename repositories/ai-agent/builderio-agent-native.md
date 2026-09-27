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

> https://github.com/BuilderIO/agent-native

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Agent-Native는 AI Agent와 화면 UI가 같은 기능·데이터·상태를 공유하는 앱을 만드는 TypeScript 프레임워크이다.[1]

## 이 Repository는 무엇인가?

Agent가 화면을 대신 클릭하도록 만드는 방식이 아니라, 사람의 UI 조작과 Agent의 도구 호출이 같은 action을 사용하도록 설계한다.[1]

action은 입력 규칙과 실행 코드를 묶은 기능 단위이며, 두 호출 경로가 검증·권한·구현을 공유한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 하나의 action을 Agent 도구와 React UI뿐 아니라 HTTP, 도구 연결 규약인 MCP, Agent 간 통신인 A2A, CLI에서도 호출할 수 있다.[1]
- 현재 페이지·선택한 항목·활성 화면 같은 UI 상태를 Agent에 전달하고, 서로의 데이터 변경을 공유한다.[1]
- 채팅, 인증·권한, 재사용 지침인 Skills와 기억, 예약·이벤트 자동화, 전문 Agent 간 위임을 포함한다.[1]

## 어떻게 동작하는가?

```text
개발자가 action의 설명·입력 규칙·실행 코드를 정의한다.
↓
사용자 UI 또는 Agent가 같은 action과 권한 검증을 통해 작업한다.
↓
공유 데이터에 반영된 결과를 화면과 Agent가 함께 사용한다.
```

README의 인사 예제는 `defineAction`과 Zod 입력 규칙, React의 `useActionQuery`가 같은 기능을 연결하는 최소 사례이다.[1]

### 확인한 파일 구성

- `packages/core`는 CLI, 서버 플러그인, Agent 도구, Vite 플러그인을 포함하는 핵심 프레임워크 패키지이다.[2][3]
- `templates/`는 각자의 package.json·데이터 스키마·action·UI를 가진 예제 앱을 모으며, `packages/code-agents-ui`는 재사용 React 화면을 제공한다.[2]
- 루트 `package.json`은 작업 공간 빌드·검사와 설치 후 선행 패키지 빌드 작업을 선언한다.[4][2]

## 어떤 기술을 사용하는가?

- `TypeScript·React·Zod`: 기능 구현과 화면 연결, action 입력 형식 검증에 사용한다.[1]
- `PostgreSQL·PGlite`: 운영용 SQL 데이터 저장과 로컬 개발용 저장을 맡으며, 개발 문서는 PostgreSQL 호환 SQL을 요구한다.[1][2]
- `pnpm`: 여러 패키지와 예제 앱을 한 저장소에서 관리하는 monorepo 작업 공간을 구성한다.[2]

## 실제로 어디에 사용할 수 있는가?

- 개인 학습 앱에서 같은 데이터 편집을 버튼과 자연어 Agent 요청으로 제공하는 구조를 실습할 수 있다.[1]
- 메일·일정·분석·슬라이드 예제를 읽으며 사용자가 Agent 결과를 확인하고 수정하는 UI 설계를 학습할 수 있다.[1][2]

## 왜 주목할 만한가?

학습 관점에서는 Agent용 기능과 UI용 기능을 따로 복제하지 않고 action이라는 공통 실행 경계에 모으는 점이 핵심 관찰 대상이다.[1]

## 장점

- 같은 기능의 검증과 권한 처리를 Agent·UI가 공유하는 명시적 구조를 제공한다.[1]
- 핵심 라이브러리와 독립 예제 앱을 나누어 공통 기능과 앱별 구성을 비교할 수 있다.[2]

## 단점 / 주의점

- LLM과 인프라를 별도로 연결해야 하며, core의 Node.js 요구사항은 `>=22.22.0`이고 개발 가이드는 pnpm 10 이상을 안내한다.[1][2][3]
- README와 core 패키지는 MIT를 표시하지만 루트 package.json은 ISC를 표시하므로, 재사용 범위별 라이선스를 확인해야 한다.[1][4][3]

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

## 정리

Agent-Native는 UI와 Agent가 공통 action·데이터·상태를 사용하는 앱 구축 기반이다.[1] 모델·데이터베이스·호스팅 설정이 필요한 프레임워크이며, 저장소 전체의 라이선스 표기는 단일 값으로 단정할 수 없다.[1][4][3]

## Sources

[1] BuilderIO/agent-native 공식 README

<https://github.com/BuilderIO/agent-native/blob/main/README.md>

[2] BuilderIO/agent-native DEVELOPMENT.md

<https://github.com/BuilderIO/agent-native/blob/main/DEVELOPMENT.md>

[3] BuilderIO/agent-native packages/core/package.json

<https://github.com/BuilderIO/agent-native/blob/main/packages/core/package.json>

[4] BuilderIO/agent-native package.json

<https://github.com/BuilderIO/agent-native/blob/main/package.json>
