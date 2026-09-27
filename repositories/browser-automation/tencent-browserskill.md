---
title: "Tencent/BrowserSkill"
repository: "Tencent/BrowserSkill"
url: "https://github.com/Tencent/BrowserSkill"
category: "browser-automation"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "browser-automation"
  - "starred-draft"
---

# Tencent/BrowserSkill

> https://github.com/Tencent/BrowserSkill

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

BrowserSkill은 AI 도우미가 사용자의 로그인 상태를 활용해 Chrome 또는 Edge의 페이지를 읽고 조작하도록 연결하는 도구이다.[1]

## 이 Repository는 무엇인가?

AI 모델 자체가 아니라 명령 도구 `bsk`, 백그라운드 연결 프로그램인 daemon, 브라우저 확장과 사용 지침인 Skill을 결합한다.[1]

기본적으로 별도 Agent Window에서 작업하며, 기존 사용자 탭은 명시적으로 빌려 사용한 뒤 원래 창으로 돌려준다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 페이지의 글과 조작 요소를 읽고 클릭·입력·탭 관리·화면 캡처를 수행하며, 로컬 모드에서 파일 송수신도 제공한다.[1]
- 허가된 웹사이트의 오류를 조사할 때 요청·응답·콘솔·페이지 변화를 작업별 증거로 수집하고 JSON으로 내보낼 수 있다.[1]
- 브라우저 인스턴스와 프로필을 선택할 수 있으며, 서버의 도우미와 사용자 컴퓨터의 브라우저를 인증된 원격 연결로 연결한다.[1]

## 어떻게 동작하는가?

```text
도우미·CLI·확장을 연결하고 대상 브라우저를 선택해 작업 세션을 만든다.
↓
페이지를 관찰해 얻은 최신 요소 참조로 조작하고 중요한 화면 변화 뒤에는 다시 관찰한다.
↓
목표 달성을 확인한 뒤 세션을 종료하고 빌린 탭을 돌려준다.
```

Skill은 성공과 실패 모두에서 세션 정리를 요구하며, 로그인·검증 등 사람의 참여가 필요한 단계는 확장 설정을 존중하도록 지시한다.[2]

### 확인한 파일 구성

- `Cargo.toml`은 `crates/bsk-cli`와 `crates/bsk-protocol`을 Rust 작업 공간으로 묶는다.[3]
- `crates/bsk-cli/skill/SKILL.md`는 관찰·조작·종료 규칙과 상황별 참고 문서 연결을 정의한다.[2]
- README는 `apps/extension`을 브라우저 자동화·디버깅 화면, `crates/bsk-protocol`을 통신 자료형과 JSON 스키마 영역으로 설명한다.[1]

## 어떤 기술을 사용하는가?

- `Rust`는 CLI·daemon 쪽 구현 기반이고 저장소는 pnpm 작업 공간을 함께 사용한다.[1][3]
- `WSS`는 서버와 브라우저 사이에서 사용하는 암호화된 WebSocket 연결이며, 브라우저가 연결을 시작한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인적으로 접근 권한이 있는 페이지의 내용을 읽어 정리하거나 반복적인 웹 양식 작업을 보조할 수 있다.[1]
- 자신의 웹 앱에서 오류를 재현하며 요청·콘솔·화면 증거를 모으는 학습 도구로 사용할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 브라우저 연결 계층과 AI 모델 선택을 분리하고, 행동 전 관찰과 작업별 세션 종료를 명시한 구성이 관찰점이다.[1][2]

## 장점

- 작업을 별도 보이는 창에 두고 필요한 순간 사람이 개입할 수 있는 경로를 제공한다.[1]
- Skill은 페이지 내용을 지시가 아닌 데이터로 취급하고 비밀정보 추출이나 승인 우회를 금지한다.[2]

## 단점 / 주의점

- Agent Window는 별도 보안 격리 환경이 아니며, 로그인 계정 권한으로 동작하므로 허가된 범위에서만 사용해야 한다.[1]
- 디버깅 증거에는 민감한 정보가 남을 수 있고 요청 재전송은 서버 데이터를 바꿀 수 있으며, 작업 종료만으로 저장 기록이 지워지지 않는다.[1]
- 확장은 Chromium 125 이상 Chrome·Edge를 대상으로 하며, 원격 모드에서는 파일 송수신을 지원하지 않고 스토어 배포가 저장소보다 늦을 수 있다.[1]

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

- [vercel-labs/agent-browser](../ai-agent/vercel-labs-agent-browser.md)

## 정리

BrowserSkill은 기존 로그인 브라우저의 관찰·조작 기능을 선택한 AI 도우미에 제공한다.[1] 계정 권한과 기록의 민감성을 함께 다뤄야 하며 실제 배포 버전의 지원 범위를 확인해야 한다.[1]

## Sources

[1] Tencent/BrowserSkill 공식 README

<https://github.com/Tencent/BrowserSkill/blob/main/README.md>

[2] Tencent/BrowserSkill crates/bsk-cli/skill/SKILL.md

<https://github.com/Tencent/BrowserSkill/blob/main/crates/bsk-cli/skill/SKILL.md>

[3] Tencent/BrowserSkill Cargo.toml

<https://github.com/Tencent/BrowserSkill/blob/main/Cargo.toml>
