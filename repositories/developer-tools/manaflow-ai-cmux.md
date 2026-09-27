---
title: "manaflow-ai/cmux"
repository: "manaflow-ai/cmux"
url: "https://github.com/manaflow-ai/cmux"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# manaflow-ai/cmux

> https://github.com/manaflow-ai/cmux

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

cmux는 AI 코딩 도우미를 여러 창에서 실행하면서 상태와 알림을 확인할 수 있는 macOS용 터미널 앱이다.[1]

## 이 Repository는 무엇인가?

명령을 입력하고 실행 결과를 보는 터미널에 작업 공간·분할 화면·내장 브라우저를 결합한다.[1]

Ghostty를 복제한 앱이 아니라 터미널 화면을 그리는 `libghostty` 라이브러리를 사용하는 별도 앱이며, 고정된 에이전트 작업 방식을 강요하지 않는다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 사이드바에 Git 브랜치·연결된 PR·작업 경로·열린 포트 등을 표시하고 화면을 가로 또는 세로로 분할한다.[1]
- 도우미가 입력을 기다리면 화면 테두리와 탭 표시, 알림 패널 등으로 알려 주며 최근 읽지 않은 알림으로 이동할 수 있다.[1]
- 내장 브라우저의 페이지 탐색·클릭·입력과 작업 공간 생성 등을 CLI 및 Unix 소켓, 즉 로컬 프로그램 간 통신 창구로 자동화한다.[1]

## 어떻게 동작하는가?

```text
작업 공간에서 터미널 기반 코딩 도우미를 실행한다.
↓
분할 화면에서 진행 상태를 보고 필요하면 브라우저를 옆에 열어 결과를 확인한다.
↓
도우미의 요청 알림을 확인해 해당 화면으로 이동하고 후속 입력을 전달한다.
```

알림은 표준 터미널 제어 신호 또는 CLI·도우미 훅으로 전달되며, 훅은 특정 이벤트 때 실행하는 연결 지침이다.[1]

### 확인한 파일 구성

- `Sources/AgentNotificationDelivery.swift`는 알림 설정을 읽고 허용된 이벤트를 공통 알림 전달 경로에 넣는다.[2]
- 루트 `package.json`에는 에이전트 세션 웹 화면의 빌드·테스트와 Bun 기반 보조 도구 명령이 정의되어 있다.[3]

## 어떤 기술을 사용하는가?

- `Swift`와 `AppKit`은 네이티브 macOS 앱의 구현 기반이며, `libghostty`는 GPU 기반 터미널 렌더링을 담당한다.[1]
- `SSH`는 원격 컴퓨터의 작업 공간에 연결하는 방식이며, 기존 Ghostty 설정의 테마·글꼴·색상을 읽을 수 있다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 프로젝트에서 여러 코딩 도우미 세션을 나란히 열고 어느 세션에 응답해야 하는지 확인할 수 있다.[1]
- 웹 개발 서버를 터미널 옆 브라우저에서 확인하거나 원격 터미널 작업 공간을 구성할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 터미널·브라우저·알림을 독립적으로 조합할 수 있는 제어 기능으로 제공하고, 도우미의 판단 자체를 대체하지 않는 점이 관찰점이다.[1]

## 장점

- 작업 경로와 상태 정보를 탭 제목 외에 사이드바에도 표시해 여러 세션의 맥락을 확인할 수 있다.[1]
- 사람이 사용하는 화면과 자동화 API가 같은 작업 공간·브라우저 기능을 다룬다.[1]

## 단점 / 주의점

- 데스크톱은 macOS용이며 소스 라이선스는 GPL-3.0-or-later이고, README의 iOS 앱은 별도의 베타 안내이다.[1]
- 세션 복원은 화면 배치와 기록 등의 재구성이며 임의의 실행 중 프로세스를 그대로 보존하지 않는다.[1]
- 지원 도우미의 자동 재개에는 저장된 세션 ID와 훅 설정이 필요하고, 사용자 지정 재개 명령의 자동 실행은 신뢰·승인 정책을 따른다.[1]

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

## 관련 Repository

- [superset-sh/superset](superset-sh-superset.md)

## 정리

cmux는 기존 CLI 도우미를 터미널·브라우저·알림이 연결된 작업 공간에서 사용하는 도구이다.[1] 세션 복원과 실행 프로세스의 생존은 별개이므로 재개 방식과 플랫폼 범위를 구분해야 한다.[1]

## Sources

[1] manaflow-ai/cmux 공식 README

<https://github.com/manaflow-ai/cmux/blob/main/README.md>

[2] manaflow-ai/cmux Sources/AgentNotificationDelivery.swift

<https://github.com/manaflow-ai/cmux/blob/main/Sources/AgentNotificationDelivery.swift>

[3] manaflow-ai/cmux package.json

<https://github.com/manaflow-ai/cmux/blob/main/package.json>
