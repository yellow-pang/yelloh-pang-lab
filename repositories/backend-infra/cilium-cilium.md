---
title: "cilium/cilium"
repository: "cilium/cilium"
url: "https://github.com/cilium/cilium"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# cilium/cilium

> https://github.com/cilium/cilium

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Cilium은 Kubernetes에서 실행되는 애플리케이션의 통신 연결, 통신 규칙 적용, 네트워크 관찰을 함께 제공하는 eBPF 기반 프로젝트이다.[1]

## 이 Repository는 무엇인가?

eBPF는 Linux 커널의 여러 지점에 프로그램을 연결하는 기술이며, Cilium은 이를 실제 네트워크 트래픽을 처리하는 기반으로 사용한다.[1]

IP 주소만으로 접근을 구분하지 않고 같은 보안 정책을 공유하는 컨테이너 집단에 식별자를 부여하여, 네트워크 주소와 보안 규칙을 분리한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 호스트 사이에 가상 네트워크를 만드는 overlay 방식과 Linux의 일반 경로표를 이용하는 native routing 방식을 제공한다.[1]
- 여러 애플리케이션으로 요청을 나누는 부하 분산을 제공하며, Kubernetes의 kube-proxy를 대체하는 구성도 지원한다.[1]
- Hubble을 통해 서비스 연결 관계와 트래픽 흐름을 관찰하고, 정책 위반이나 DNS 문제 등 통신이 차단된 이유를 확인할 수 있다.[1]

## 어떻게 동작하는가?

```text
Kubernetes 애플리케이션과 네트워크·접근 정책을 준비한다.
↓
Cilium이 식별자와 eBPF 기반 처리로 트래픽을 전달하고 정책을 적용한다.
↓
애플리케이션 연결과 함께 흐름 정보, 지표, 차단 사유를 제공한다.
```

위 흐름은 README가 설명하는 연결·정책·관찰 기능의 개념적 요약이며, native routing에서는 기반 네트워크가 컨테이너 IP를 전달할 수 있어야 한다.[1]

### 확인한 파일 구성

- `README.rst`는 CNI라는 컨테이너 네트워크 연결 방식부터 부하 분산, Cluster Mesh, 정책, 관찰 기능까지 프로젝트의 범위를 설명한다.[1]
- `go.mod`는 Go 모듈과 의존성을 선언하며, eBPF 라이브러리, Kubernetes API, Gateway API 관련 패키지가 포함되어 있다.[2]

## 어떤 기술을 사용하는가?

- `eBPF`: 커널의 네트워크 입출력과 소켓 등에 연결되어 통신·보안·관찰 처리를 수행하는 기반이다.[1]
- `Go`: 저장소의 모듈과 라이브러리 의존성이 `go.mod`로 관리되는 개발 언어이다.[2]
- `Prometheus`와 `Grafana`: README에서 지표와 경보를 위한 연동 대상으로 안내한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 실습 클러스터에서 IP·포트 규칙과 HTTP 경로 수준의 접근 규칙이 어떻게 다른지 학습할 수 있다.[1]
- Hubble의 흐름 정보로 서비스 사이의 연결을 읽고, 통신 실패와 정책 차단을 구분하는 학습에 활용할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 IP가 바뀔 수 있는 컨테이너 환경에서 주소 대신 보안 식별자를 정책의 기준으로 삼는 설계를 살펴볼 수 있다.[1]

## 장점

- 연결 기능과 정책 적용, 문제 관찰을 같은 프로젝트 범위에서 다루므로 이들 사이의 관계를 함께 살펴볼 수 있다.[1]
- Cluster Mesh가 여러 Kubernetes 클러스터 사이의 서비스 발견과 공통 식별자 기반 정책을 제공한다.[1]

## 단점 / 주의점

- Linux 커널의 eBPF를 기반으로 하며, README는 개발용 snapshot·RC·CI 이미지를 실제 운영용으로 사용하지 말라고 명시한다.[1]
- 사용자 공간 구성 요소는 Apache-2.0, BPF 코드 템플릿은 GPL-2.0-only 또는 BSD-2-Clause 중 선택하는 이중 라이선스이므로 적용 부분을 구분해야 한다.[1]

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

Cilium은 eBPF 기반 네트워크에 식별자 중심 정책과 관찰 기능을 결합한다.[1] 실제 구성을 판단할 때는 연결 방식의 네트워크 조건과 안정 배포판·개발 이미지의 구분을 확인해야 한다.[1]

## Sources

[1] cilium/cilium 공식 README

<https://github.com/cilium/cilium/blob/main/README.rst>

[2] cilium/cilium go.mod

<https://github.com/cilium/cilium/blob/main/go.mod>
