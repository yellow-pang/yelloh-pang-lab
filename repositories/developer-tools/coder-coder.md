---
title: "coder/coder"
repository: "coder/coder"
url: "https://github.com/coder/coder"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# coder/coder

> https://github.com/coder/coder

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Coder는 사용자가 관리하는 인프라에 개발용 작업 공간과 AI 코딩 에이전트를 제공하는 자체 호스팅 플랫폼이다.[1]

## 이 Repository는 무엇인가?

작업 공간은 편집기, 개발 의존성, 설정을 갖춘 환경이며, Coder는 이를 Terraform으로 정의하고 생성하도록 구성되어 있다.[1]

코드를 작성할 원격 환경의 준비와 접속을 관리하고, 사용하지 않는 자원을 자동으로 종료하는 문제를 다룬다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Terraform 템플릿으로 가상 머신, Kubernetes Pod, Docker 컨테이너 등 서로 다른 형태의 개발 환경을 정의할 수 있다.[1]
- WireGuard 터널로 작업 공간을 연결하고, VS Code나 JetBrains 등 기존 편집기에서 원격 환경에 접근하는 통합을 제공한다.[1]
- README의 Coder Agents는 작업 공간이 아닌 제어 계층에서 AI 처리 반복 과정을 실행하며, 작업 공간에 모델 API 키를 두지 않는 구조로 설명된다.[1]

## 어떻게 동작하는가?

```text
서버에 접속하여 개발 환경을 설명하는 Terraform 템플릿을 선택한다.
↓
템플릿에 따라 작업 공간을 생성하고 편집기나 코딩 에이전트로 접근한다.
↓
준비된 환경에서 개발하며, 유휴 자원은 자동 종료 대상이 된다.
```

위 흐름은 작업 공간 생성과 사용을 요약한 것으로, 직접 관리하는 서버와 선택한 템플릿의 인프라를 전제로 한다.[1]

### 확인한 파일 구성

- `compose.yaml`은 Coder와 PostgreSQL 서비스를 함께 정의하고, 데이터베이스 상태 확인과 영구 저장 볼륨을 설정한다.[2]
- `LICENSE`에는 AGPL v3가 수록되어 있으며, `LICENSE.enterprise`는 `enterprise` 디렉터리 소프트웨어에 별도 조건을 명시한다.[3][4]

## 어떤 기술을 사용하는가?

- `Terraform`: 서버나 컨테이너 같은 인프라를 코드로 설명하여 작업 공간 템플릿을 만드는 데 사용한다.[1]
- `WireGuard`: 사용자와 원격 작업 공간 사이의 터널 연결에 사용한다.[1]
- `PostgreSQL`: 운영 배포에서 사용하는 데이터베이스이며 공식 Compose 예제에도 별도 서비스로 포함된다.[1][2]

## 실제로 어디에 사용할 수 있는가?

- 개인 Docker 환경에서 템플릿과 작업 공간의 관계를 살펴보며 재현 가능한 개발 환경 구성을 학습할 수 있다.[1]
- 원격 작업 공간을 기존 편집기에 연결하거나, 코딩 에이전트가 별도 개발 환경에서 작업하는 구성을 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 환경을 설명하는 템플릿, 실제 작업 공간, AI 요청을 다루는 제어 계층을 구분하는 설계를 관찰할 수 있다.[1]

## 장점

- 여러 인프라 형태를 Terraform 템플릿이라는 공통 방식으로 정의할 수 있다.[1]
- 작업 공간 관리뿐 아니라 모델 사용의 인증·비용 추적·감사 기록을 모으는 기능을 함께 소개한다.[1]

## 단점 / 주의점

- 운영 배포에는 PostgreSQL 13 이상과 외부 접근 URL을 안내하며, 기본 내장 데이터베이스와 임시 접근 URL은 평가용 구성으로 구분한다.[1]
- Compose 예제는 Docker 소켓을 연결하고 작업 공간이 도달 가능한 접근 URL을 요구하므로, 예제를 그대로 일반 운영 설정으로 간주해서는 안 된다.[2]
- 루트 AGPL v3와 Enterprise 조건을 구분해야 하며, Enterprise 파일은 라이선스 키 기능 우회 금지와 운영 사용 조건을 명시한다.[3][4]

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

- [cloudflare/computer](../ai-agent/cloudflare-computer.md)

## 정리

Coder는 템플릿으로 개발 환경을 생성하고 원격 접속과 AI 코딩 작업을 연결하는 플랫폼이다.[1] 평가용 기본 설정과 운영 인프라 요구사항, Enterprise 코드의 별도 조건은 구분해야 한다.[1][4]

## Sources

[1] coder/coder 공식 README

<https://github.com/coder/coder/blob/main/README.md>

[2] coder/coder compose.yaml

<https://github.com/coder/coder/blob/main/compose.yaml>

[3] coder/coder LICENSE

<https://github.com/coder/coder/blob/main/LICENSE>

[4] coder/coder LICENSE.enterprise

<https://github.com/coder/coder/blob/main/LICENSE.enterprise>
