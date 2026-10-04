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

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

AX는 에이전트 작업과 필요한 환경을 선언하면 Agent Substrate의 격리 실행으로 연결하는 작업 운영 도구다.
대화형 AI 모델이나 로컬 코딩 도구 자체가 아니라, 클러스터에서 작업의 준비·상태·중단과 재개를 관리하는 오케스트레이터다.
오케스트레이터는 여러 구성 요소가 원하는 상태에 도달하도록 조정하는 프로그램을 뜻한다.[1]

## 실행 명령만으로 표현하기 어려운 작업

에이전트 작업은 파일과 중간 상태를 쌓고 외부 모델 API와 도구 서버를 호출할 수 있다.
프로세스를 한 번 시작하는 것으로 끝나는 대신 실행 자원과 접근 환경, 현재 상태를 함께 관리해야 한다.
AX는 이 문제를 Task, Workspace, Model이라는 세 가지 선언으로 나눈다.
선언형이라는 말은 세부 실행 순서를 모두 지시하기보다 원하는 구성을 문서로 제출한다는 뜻이다.[1]

README는 매우 큰 규모의 작업 처리를 목표로 내세우지만, 이 글은 그 수치를 검증한 성능 보고서가 아니다.
실제 기반인 Agent Substrate가 먼저 클러스터에 있어야 하며 AX가 그 위에서 격리 실행을 요청한다.
AX 하나를 내려받아 독립적인 로컬 에이전트로 쓰는 구조와 다르다.[1]

## Task, Workspace, Model의 분담

Task는 실행 이미지와 환경 변수, CPU·메모리 요청과 한도, 사용할 Workspace 등을 담는다.
Workspace는 저장소와 MCP 서버, Skill 패키지처럼 작업 전에 준비할 맥락과 도구를 표현한다.
MCP는 에이전트가 외부 도구와 연결하는 규약이고 Skill은 특정 작업 지침 묶음이다.
Model은 플랫폼이 사용할 모델 제공자와 모델 이름, 자격 증명을 찾을 위치를 지정한다.[1][2]

공식 `examples/task.yaml`에는 세 객체가 하나의 파일에 들어 있다.
Task가 이름으로 Workspace를 참조하고 Workspace가 Git 저장소와 도구·Skill 검색 조건을 선언한다.
Model의 `secretKey`는 키의 실제 문자열 대신 Kubernetes Secret의 이름과 항목을 가리킨다.
작업 설명과 재사용 환경, 인증 정보를 같은 본문 문자열에 섞지 않고 관계로 연결하는 방식이다.[2]

제어 계층의 구조도 Kubernetes 객체를 그대로 무한히 늘리는 방식은 아니다.
설계 문서는 상태를 Redis에 보관하고 Redis Streams를 API 서버와 컨트롤러 사이의 작업 큐로 사용한다고 설명한다.
API 서버는 선언을 검증·저장하고 이벤트를 발행한다.
컨트롤러는 이벤트를 받아 Substrate의 실행 단위를 준비하며, task runner는 컨테이너 안에서 환경을 구성하고 에이전트 명령을 실행한다.[3]

## 예시로 따라가는 흐름

공식 예제의 `task123`을 문서상에서 따라가면, 먼저 Task는 지정된 이미지와 자원 요청·한도를 사용하고 `default-workspace`를 `/workspace`에 연결하도록 선언한다.
같은 파일의 Workspace에는 공개 Git 저장소의 `main` 브랜치와 도구 서버, Skill 레지스트리 질의가 들어 있다.
이는 “작업을 실행하라”는 한 줄 외에 어떤 자료와 도구가 준비되어야 하는지를 별도로 명시한 입력이다.[2]

README의 제출 단계는 이 파일을 `ax apply`로 보내는 흐름이다.
설계상 API 서버가 정보를 저장하고 컨트롤러가 Substrate 쪽 상태를 맞추면 runner가 환경을 준비한다.
사용자는 `ax watch`로 단계와 조건 변화에 접근하고, 디버그가 켜진 예제에서는 `ax ssh`로 내부를 조사할 수 있다.
예제의 `debug: true` 주석은 이 접속 기능이 기본으로 켜지는 것이 아님을 분명히 한다.[1][2][3]

이후 `suspend`와 `resume`은 실행 상태를 저장해 멈추고 이어가는 기능으로 설명된다.
사람은 제출한 선언의 존재, 실제 실행 상태, 내부 Workspace 준비를 각각 확인해야 한다.
이 글은 예제를 제출하지 않았으므로 README에 보이는 상태 표를 실제 결과로 재현하지 않는다.
외부 도구 서버나 모델 Secret도 예제의 참조일 뿐 현재 사용할 수 있는 접속점으로 보장하지 않는다.[1][2]

## 변경과 재개가 같은 일은 아니다

설계 문서의 API에서 Task는 생성 후 변경할 수 없는 객체다.
반면 Workspace와 Model에는 생성·갱신 API가 있다.
따라서 작업을 중단했다 다시 시작하는 일과 Task 명세 자체를 수정하는 일을 같은 조작으로 이해하면 안 된다.
이 구분은 실행 상태의 생명주기와 재사용 설정의 관리가 다른 문제임을 보여 준다.[3]

상태 관찰도 중요하다. `WatchTask`는 상태와 조건의 전환을 스트리밍하는 인터페이스로 설명된다.
단순 목록 조회가 현재 한 장면이라면 watch는 변화 과정을 보게 한다.
다만 상태가 Running이라는 사실만으로 에이전트가 의도한 산출물을 올바르게 만들었다고 판단할 수는 없다.
작업 내용에 대한 별도 결과 검토가 남는다.[1][3]

## 지금 확인할 제약

README 첫머리는 핵심 개념·프로토콜·명세를 계속 다듬는 중이며 안정 릴리스 전에 큰 호환성 변경이 있을 수 있다고 경고한다.
YAML의 `v1alpha1` 표기도 안정 계약으로 받아들이지 않는 편이 맞다.
설치 예제와 API 설계는 읽은 시점의 자료로 취급해야 한다.[1][2]

또한 Kubernetes, Agent Substrate와 이미지 레지스트리 등 선행 구성이 필요하다.
CLI는 활성 Kubernetes context를 따르므로 어느 클러스터를 대상으로 하는지 확인해야 한다.
격리 실행이 제공된다는 설명을 외부 API 비용이나 잘못된 도구 권한까지 자동으로 해결한다는 의미로 확대할 수 없다.
자원 한도와 외부 자격 증명의 범위는 각각 검토할 사항이다.[1]

## 직접 읽어볼 자료

- [공식 README](https://github.com/google/ax/blob/main/README.md)
  Why의 세 가지 선언과 선행 조건을 먼저 읽는다.
  자신이 보는 기능이 모델 추론인지 작업 운영인지 구분한 뒤 상태 관찰·중단·재개 명령이 어느 객체에 적용되는지 확인할 수 있다.
- [Task·Workspace·Model 공식 예제](https://github.com/google/ax/blob/main/examples/task.yaml)
  객체 사이의 이름 참조, 자원 한도, Secret 참조와 디버그 플래그를 차례로 따라간다.
  예제에 적힌 주소를 실제 가용 서비스로 간주하지 않고 어떤 설정을 준비해야 하는지 읽는 자료다.
- [제어 계층 설계](https://github.com/google/ax/blob/main/DESIGN.md)
  API 서버에서 Redis와 컨트롤러를 거쳐 Substrate로 가는 그림을 읽는다.
  이어 Task의 불변성과 Workspace·Model의 갱신 API를 비교하면 상태 변경과 명세 변경의 차이를 확인할 수 있다.

## 정리

AX는 작업·환경·모델 선언을 격리 실행의 생명주기로 연결한다.
클러스터 구성과 변경 가능한 초기 명세를 전제로 하며, 작업 상태 관리와 에이전트 산출물의 품질 검토는 서로 다른 책임이다.[1][3]

## 자료 확인 범위

2026-09-27 기준 README, 루트 구성, Task 예제와 설계 문서를 읽었다.
클러스터 배포, 작업 제출·중단·재개 및 성능 측정은 실행하지 않았다.

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

## Sources

[1] google/ax — README.md

<https://github.com/google/ax/blob/main/README.md>

[2] google/ax — examples/task.yaml

<https://github.com/google/ax/blob/main/examples/task.yaml>

[3] google/ax — DESIGN.md

<https://github.com/google/ax/blob/main/DESIGN.md>
