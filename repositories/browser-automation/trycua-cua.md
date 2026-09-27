---
title: "trycua/cua"
repository: "trycua/cua"
url: "https://github.com/trycua/cua"
category: "browser-automation"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "browser-automation"
  - "starred-draft"
---

# trycua/cua

> https://github.com/trycua/cua

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Cua는 AI Agent가 컴퓨터 앱을 관찰·조작하도록 데스크톱 자동화 도구, 격리된 실행 환경, 평가 도구를 제공하는 프로젝트 모음이다.[1]

## 이 Repository는 무엇인가?

Computer use는 Agent가 코드·API·그래픽 화면을 오가며 작업하는 방식을 뜻하며, Cua는 이를 위한 컴퓨터와 조작 도구를 제공한다.[1]

일반 Agent와 모델을 사용자가 연결하는 구조이고, 전문 판단 모델인 CUA-S1도 범용 Agent의 계획·추론을 대체하는 제품은 아니라고 설명한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Cua Driver는 macOS·Windows·Linux의 앱과 브라우저를 CLI, 외부 도구 연결 규약인 MCP, 형식이 지정된 SDK로 관찰·조작한다.[1]
- Fleets는 클라우드 데스크톱 풀을 제공하고, Lume은 Apple Silicon에서 로컬 macOS·Linux 가상 머신을 관리한다.[1]
- Cua Bench는 컴퓨터 사용 과제 생성·Agent 평가·행동 기록 내보내기를 제공하며, CUA-S1은 제한된 판단을 위한 별도 연구 구성요소이다.[1]

## 어떻게 동작하는가?

```text
사용할 Agent·모델과 로컬 또는 클라우드 실행 환경을 선택한다.
↓
Driver나 Sandbox SDK를 통해 앱 상태를 관찰하고 허용된 조작을 수행한다.
↓
앱 상태나 스크린샷으로 결과를 확인하고 클라우드 자원을 정리한다.
```

이는 공통 개념 흐름이며 로컬 환경과 Fleets의 인증 정보·이미지·작업·실행 요건은 서로 다르다.[1]

### 확인한 파일 구성

- `libs/cua-driver/README.md`는 Driver 연결 방식, 권한 모드, 선택적 시각 인식 확장과 내부 구성을 설명한다.[2]
- Driver의 `rust/`는 네이티브 실행 기반과 SDK·테스트를, `python/`과 `typescript/`는 각 언어용 SDK를 담당한다.[2]
- 루트 `pyproject.toml`은 Python 작업 공간과 `libs/python/agent`, `core`, `computer` 등 구성 패키지를 선언한다.[3]

## 어떤 기술을 사용하는가?

- `Rust·UniFFI`: Driver의 네이티브 실행 기능과 Python·TypeScript 연결을 구성하며, SDK 직접 사용은 데몬을 필수로 요구하지 않는다.[2]
- `Virtualization.Framework`: Lume이 Apple Silicon에서 가상 머신을 만드는 데 사용하는 운영체제 기반이다.[1]
- `Python·uv`: 루트 Python 작업 공간을 관리하며 해당 manifest의 Python 범위는 3.12 이상 3.14 미만이다.[3]

## 실제로 어디에 사용할 수 있는가?

- 개인 테스트 환경에서 Agent가 계산기 같은 앱을 조작한 뒤 결과를 다시 관찰하는 자동화 흐름을 학습할 수 있다.[1]
- Cua Bench의 모의 과제로 Agent의 행동과 평가 기준을 비교하거나, 격리된 가상 머신에서 조작 도구를 탐색할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 컴퓨터 조작 Driver, 실행 환경, 평가 도구, 전문 모델을 구분해 제공하여 하나의 Agent 제품으로 혼동하지 않는 것이 중요하다.[1]

## 장점

- Agent용 MCP·CLI와 애플리케이션용 Python·TypeScript SDK의 연결 경계를 구분해 설명한다.[2]
- 로컬 가상 머신과 클라우드 데스크톱, VM 없는 모의 평가라는 서로 다른 학습 경로를 제공한다.[1]

## 단점 / 주의점

- 백그라운드 조작은 앱·플랫폼 지원에 따라 달라지고, Fleets 풀은 사용 종료 뒤에도 유료 용량을 유지할 수 있어 자원 정리가 필요하다.[1]
- 기본 프로젝트와 Driver는 MIT이지만 선택적 `cua-som`과 `cua-perception`에는 AGPL 조건이 있으므로 모델·확장·의존성을 별도로 확인해야 한다.[1][2]
- Driver는 권한 모드를 구분하고 기존 로그인 Chromium 프로필 연결을 명시적 허가 대상으로 다루므로, 자동화 대상과 계정 접근 범위를 확인해야 한다.[2]

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

- [browser-use/browser-use](../ai-agent/browser-use-browser-use.md)

## 정리

Cua는 Agent에 컴퓨터 조작 도구와 실행·평가 환경을 제공하는 구성요소 모음이다.[1] 운영체제별 지원, 클라우드 자원 비용, 선택 확장의 권한과 라이선스를 구분해야 한다.[1][2]

## Sources

[1] trycua/cua 공식 README

<https://github.com/trycua/cua/blob/main/README.md>

[2] trycua/cua libs/cua-driver/README.md

<https://github.com/trycua/cua/blob/main/libs/cua-driver/README.md>

[3] trycua/cua pyproject.toml

<https://github.com/trycua/cua/blob/main/pyproject.toml>
