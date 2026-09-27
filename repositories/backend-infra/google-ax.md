---
title: "google/ax"
repository: "google/ax"
url: "https://github.com/google/ax"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# google/ax

> https://github.com/google/ax

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

AX는 작업 내용과 준비할 환경을 설정 파일로 선언하면 Agent Substrate 위에서 격리된 에이전트 작업을 운영하는 도구이다.[1]

## 이 Repository는 무엇인가?

여러 작업을 배치하고 상태를 맞추는 오케스트레이터이며, 사용자가 원하는 상태를 YAML 설정 파일로 표현하는 선언형 방식을 사용한다.[1]

에이전트가 상태를 쌓고 외부 모델·도구를 호출하는 특성을 다루되, 실제 격리 실행은 Agent Substrate에 맡긴다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- `Task`는 작업과 자원 제한, `Workspace`는 Git 저장소·도구 서버·Skill 준비, `Model`은 플랫폼이 사용할 언어 모델 구성을 표현한다.[1]
- 명령행에서 작업의 상태 변화를 관찰하고, 실행 상태를 저장해 일시 중단하거나 이어서 실행하는 기능을 제공한다.[1]
- 디버그 설정이 켜진 작업에는 `ax ssh`로 접속할 수 있고, 활성 Kubernetes 접속 설정을 따라 대상 클러스터를 선택한다.[1]

## 어떻게 동작하는가?

```text
Agent Substrate가 있는 클러스터에 작업·환경·모델 선언을 제출한다.
↓
API 서버가 선언을 검증·저장하고 컨트롤러가 이벤트를 받아 Substrate의 실행 단위를 준비한다.
↓
작업 컨테이너가 환경을 구성하고 에이전트 명령을 실행하며 상태 변경을 제공한다.
```

이 흐름은 분산 실행을 운영하는 경로이며, Task는 생성 뒤 변경할 수 없고 Workspace와 Model에는 갱신 API가 있다.[2]

### 확인한 파일 구성

- `go.mod`는 AX 모듈과 Substrate·Redis·gRPC 의존성을 선언한다.[3]
- `runner`는 작업 컨테이너의 환경 준비와 실행을 담당하는 패키지로, 기본 실행 진입점과 사용자 정의 이미지에서 활용할 수 있다.[2]
- `pkg/apis/v1alpha1`에는 API 요청과 응답의 생성된 Go 타입이 위치한다고 설계 문서가 설명한다.[2]

## 어떤 기술을 사용하는가?

- `Go`: 도구와 제어 계층의 모듈 및 의존성을 관리하는 언어이다.[3]
- `Redis·Redis Streams`: 작업 상태 저장과 API 서버·컨트롤러 사이의 작업 이벤트 전달에 사용한다.[2]
- `gRPC`: 명령행과 제어 서버, 제어 계층과 Substrate 사이의 통신에 사용한다.[1][2]

## 실제로 어디에 사용할 수 있는가?

- 개인 클러스터 학습에서 작업 정의와 실제 실행 상태를 구분하는 선언형 관리 방식을 살펴볼 수 있다.[1][2]
- 작업을 중단·재개하면서 실행 환경과 저장된 상태를 어떻게 연결하는지 관찰하는 예제를 구성할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 Kubernetes 위에 배포하면서도 작업 상태와 이벤트를 Redis에 따로 두는 설계가 관찰점이다.[2]

## 장점

- 작업, 재사용 환경, 모델 구성을 구별해 서로 다른 관심사를 별도 선언으로 표현한다.[1]
- 설정 제출뿐 아니라 상태 관찰과 중단·재개를 같은 명령행 체계에서 제공한다.[1]

## 단점 / 주의점

- 핵심 개념과 명세를 정리하는 단계이며 안정 버전 전에 큰 호환성 변경이 생길 수 있다고 경고한다.[1]
- Kubernetes와 Agent Substrate가 선행 조건이고, 안내된 배포에는 Go·kubectl·ko와 이미지 저장소가 필요하다.[1]
- Apache-2.0 라이선스이며, README의 대규모 처리 목표를 이 조사에서 측정한 성능으로 해석해서는 안 된다.[1]

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

- [agent-substrate/substrate](agent-substrate-substrate.md)

## 정리

AX는 실행 환경 선언을 받아 Substrate의 격리 실행과 연결하는 작업 운영 계층이다.[1][2] 독립적인 로컬 챗봇이 아니라 선행 클러스터 구성이 필요하고 명세가 변하는 프로젝트이다.[1]

## Sources

[1] google/ax 공식 README

<https://github.com/google/ax/blob/main/README.md>

[2] google/ax DESIGN.md

<https://github.com/google/ax/blob/main/DESIGN.md>

[3] google/ax go.mod

<https://github.com/google/ax/blob/main/go.mod>
