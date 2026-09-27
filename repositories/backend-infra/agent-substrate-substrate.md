---
title: "agent-substrate/substrate"
repository: "agent-substrate/substrate"
url: "https://github.com/agent-substrate/substrate"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# agent-substrate/substrate

> https://github.com/agent-substrate/substrate

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Agent Substrate는 여러 에이전트의 실행 상태를 보존하면서 Kubernetes의 실행 자원을 나누어 사용하는 격리 실행 기반이다.[1]

## 이 Repository는 무엇인가?

에이전트를 만드는 개발 도구 모음이 아니라 이미 만들어진 프로그램을 실행하고 관리하는 런타임이다.[1]

프로그램 단위인 Actor를 실제 실행 자리를 제공하는 Worker에 배정하며, 유휴 시간이 많은 작업들이 작은 Worker 집합을 공유하도록 설계한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Actor 생성·제거와 일시 중단·재개를 관리하고 요청이 들어오면 해당 Actor로 통신을 전달한다.[1]
- 메모리와 파일 시스템 상태를 저장한 스냅샷을 이용해 중단한 실행을 다시 이어가는 방식을 제공한다.[1]
- 운영체제 수준에서 프로그램을 격리하는 gVisor와 작은 가상 머신인 microVM 등 여러 샌드박스 방식을 지원한다고 설명한다.[1]

## 어떻게 동작하는가?

```text
Kubernetes에 Worker 자원과 실행할 Actor 구성을 준비한다.
↓
요청에 맞춰 Actor를 Worker에 배정하거나 저장 상태에서 재개하고 통신을 연결한다.
↓
프로그램의 응답을 돌려주고 이후 중단·재개 과정에서 실행 상태를 유지한다.
```

개별 프로그램의 추론 방식이 아니라 실행 수명과 자원 공유를 다루는 흐름이며, 반드시 AI 에이전트만 실행해야 하는 시스템은 아니다.[1]

### 확인한 파일 구성

- `cmd/ateapi`는 Actor와 Worker를 관리하는 제어 API, `cmd/atelet`은 노드에서 Worker 감독과 스냅샷·상태 이동을 담당한다.[1]
- `cmd/atenet`은 네트워크 라우팅 계층, `cmd/ateom-gvisor`와 `cmd/ateom-microvm`은 각 격리 방식의 실행 보조 구성이다.[1]
- `go.mod`는 Go 모듈과 Kubernetes·gRPC·관측 도구 등의 의존성을 선언한다.[2]

## 어떤 기술을 사용하는가?

- `Kubernetes`: Pod와 자동 확장 기능을 통해 기반 자원과 Worker 수명을 관리한다.[1]
- `Go·gRPC`: 구현 모듈과 제어 API 통신에 사용한다.[1][2]
- `gVisor·cloud-hypervisor`: 각각 격리된 컨테이너와 microVM 실행 경로에 사용한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 상태가 있는 카운터 예제로 중단 전후 메모리·파일 상태와 요청 라우팅의 관계를 학습할 수 있다.[1]
- 동시에 존재하는 에이전트 수와 실제 실행 자원 수를 분리하는 자원 공유 구조를 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 오래 살아 있는 논리적 Actor와 실제 자리를 차지하는 Worker의 수명을 분리한 구조가 핵심 관찰점이다.[1]

## 장점

- 실행 프로그램의 프레임워크를 고정하지 않고 OCI 컨테이너와 격리 계층을 중심으로 관리한다.[1]
- 상태 보존뿐 아니라 실행 자원 배정과 통신 라우팅을 같은 기반 시스템에서 다룬다.[1]

## 단점 / 주의점

- 아직 pre-1.0 단계로 하위 호환성을 보장하지 않으며 API와 동작이 크게 바뀔 수 있다.[1]
- 새로 추가한 노드는 설치 버전에 맞는 라벨이 없으면 Worker를 받지 못하는 등 클러스터 운영 조건이 있다.[1]
- Apache-2.0 라이선스이며, README의 격리·고밀도·지연 시간 주장은 독립적으로 검증한 결과가 아니다.[1]

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

- [google/ax](google-ax.md)

## 정리

Agent Substrate는 에이전트의 상태를 보존하고 여러 작업에 실행 자원을 배정하는 기반 시스템이다.[1] Kubernetes 운영이 전제되고 안정화 전 단계이므로 실행 성능과 호환성은 별도 확인 대상이다.[1]

## Sources

[1] agent-substrate/substrate 공식 README

<https://github.com/agent-substrate/substrate/blob/main/README.md>

[2] agent-substrate/substrate go.mod

<https://github.com/agent-substrate/substrate/blob/main/go.mod>
