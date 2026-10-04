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

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Agent Substrate는 이미 만들어진 에이전트나 프로그램을 격리해 실행하고 중단·재개하는 런타임이다.
에이전트의 사고 방식이나 프롬프트를 만드는 SDK가 아니다.
오래 유지할 논리적 프로그램인 Actor와 실제 실행 자리를 제공하는 Worker를 나누어 많은 Actor가 상대적으로 작은 Worker 집합을 공유하도록 설계한다.
그 아래 자원 준비와 Worker 수명은 Kubernetes를 이용한다.[1]

## 기다리는 프로그램에도 실행 자리가 필요한가

에이전트는 외부 응답이나 다음 사용자 요청을 기다리는 시간이 있을 수 있다.
그런 프로그램마다 계속 실행 자원을 점유하면 논리적으로 존재하는 작업 수와 실제로 계산하는 작업 수가 크게 달라진다.
Substrate는 유휴 Actor를 상태와 함께 중단하고 요청이 오면 준비된 Worker에 재개하는 방식으로 이 둘을 나눈다.
이 설계의 효과는 작업의 대기 비율과 상태 크기, 저장·네트워크 환경에 따라 달라질 수 있다.[1]

README의 높은 밀도와 짧은 재개 지연 수치는 프로젝트가 소개하는 결과다.
이 조사에서는 동일한 장비·부하로 재현하지 않았으므로 개인 환경에서 얻을 처리량으로 제시하지 않는다.
핵심은 특정 배수보다 논리적 수명과 실행 자원의 수명을 분리한다는 구조다.

## 상태 보존은 무엇을 저장하는가에 달려 있다

프로그램 상태에는 메모리 안의 변수와 디스크 파일이 있다.
일반적인 프로그램 재시작에서는 파일이 남아도 메모리 변수가 초기화될 수 있다.
Substrate의 공식 counter 예제는 메모리 카운터와 durable volume의 파일 카운터를 함께 두어 차이를 보여준다.
durable volume은 실행 자리가 바뀌어도 보존할 데이터를 담는 저장 공간이다.[2]

예제의 `Full` 범위 스냅샷은 프로세스 메모리와 durable volume을 함께 담는다.
반면 `Data` 범위 정책에서는 파일 쪽 카운터만 이어지고 메모리 카운터는 새 시작처럼 초기화된다고 설명한다.
스냅샷은 특정 시점의 상태를 다시 시작할 수 있게 저장한 묶음이다.
‘상태를 보존한다’는 소개를 모든 정책에서 모든 종류의 상태가 유지된다는 뜻으로 읽으면 안 된다.[2]

격리 방식에는 gVisor와 microVM 경로가 있다.
gVisor는 프로그램과 호스트 운영체제 사이를 분리하는 실행 계층이고 microVM은 작은 가상 머신이다.
README는 여러 격리 기술에 일관된 수명 관리 동작을 제공한다고 설명하지만 각각의 보안 경계와 실제 운영 조건을 이 원고에서 시험한 것은 아니다.[1]

## 요청은 실행 중인 자리 대신 Actor를 향한다

Kubernetes의 WorkerPool과 Substrate의 ActorTemplate은 서로 다른 자원이다.
counter 문서는 WorkerPool은 Kubernetes CRD, ActorTemplate은 `atespace` 안에 있는 Substrate API 자원이라고 구별한다.
CRD는 Kubernetes에 추가한 사용자 정의 자원 종류다.
atespace를 Kubernetes namespace와 같은 이름만 다른 객체라고 생각하면 제어 경로를 혼동할 수 있다.[2]

ActorTemplate으로 Actor를 만들면 요청 라우터가 대상 Actor를 선택하고 필요할 때 가용 Worker에서 활성화한다.
라우터는 요청을 실제 실행 위치에 전달하는 통신 중계다.
프로그램이 어느 Worker에 있는지를 호출자가 매번 추적하는 대신 Actor 식별자를 사용하도록 하는 구조다.
메모리·파일 상태의 복원과 통신 연결을 함께 다루어야 재개된 프로그램에 요청이 도달한다.[1][2]

## 예시로 따라가는 흐름

공식 Counter Demo는 요청마다 메모리와 파일에 저장한 두 카운터를 증가시키는 작은 Go HTTP 서버다.
입력은 counter 템플릿에서 만든 Actor에 보내는 HTTP POST다.
요청의 `ate-target-actor` 헤더가 atespace와 Actor 이름을 지정한다.
라우터는 해당 Actor가 중단된 상태라면 Worker에서 재개하고 요청을 넘긴다.
응답을 확인한 다음 Actor의 `RUNNING` 상태와 Worker 배정을 확인하도록 문서가 안내한다.[2]

이후 Actor를 수동으로 중단하고 같은 대상을 다시 호출하면 저장된 스냅샷에서 재개하며 다른 Worker를 사용할 수도 있다.
예제의 관찰 대상은 이전 값을 이어받는 메모리 카운터와 파일 카운터다.
사람이 확인해야 할 것은 응답이 단순히 성공했는가뿐 아니라 두 상태가 모두 연속적인지, 템플릿이 실제로 `Full` 정책을 쓰는지와 Actor의 배정이 어떻게 바뀌는지다.
이 설명은 공식 실습의 흐름이며 직접 실행해 카운터 값을 측정한 결과가 아니다.
삭제는 중단과 달리 Actor를 영구 제거하는 동작으로 소개되므로 자원 정리 명령도 같은 수준의 일시 정지로 취급해서는 안 된다.[2]

## 통신과 자원 준비의 실제 제약

counter 예제는 기본 포트 외에 HTTP CONNECT로 다른 수신 포트에 접근하는 흐름도 보여준다.
그러나 문서는 현재 터널을 통한 HTTP(S)만 지원하고 비HTTP 원시 TCP 프로토콜은 이 방식으로 접근할 수 없다고 제한한다.
포트를 지정할 수 있다는 기능을 모든 네트워크 프로토콜의 투명한 전달로 확대하면 안 된다.[2]

개발용 로컬 클러스터와 클라우드 구성 모두 단순한 Python 패키지 설치보다 큰 운영 준비가 필요하다.
README는 노드의 `ate.dev/substrate-version` 라벨과 Worker 배정의 관계를 설명한다.
설치 때 없던 새 노드는 버전 라벨을 붙이기 전까지 Worker를 받지 못한다.
자동 확장으로 노드 수가 늘었다는 것과 Substrate 실행 용량이 바로 늘었다는 것을 구분해야 한다.[1]

counter 문서의 클라우드 실습은 스냅샷 저장용 버킷과 이미지 빌드 도구 등을 요구한다.
저장소 접근권과 보존 정책이 재개의 일부이므로 CPU·메모리만 준비한다고 충분하지 않다.
상태 스냅샷에는 프로그램이 취급하던 자료가 들어갈 수 있어 접근 제어와 폐기 범위도 함께 고려해야 한다.[2]

프로젝트는 pre-1.0이며 하위 호환성을 보장하지 않는다고 명시한다.
README의 Apache-2.0 안내는 코드 이용 조건이고, API 안정성이나 실행 격리의 독립적인 보안 검증을 뜻하지 않는다.
에이전트 프레임워크에 크게 종속되지 않는 설계와 어떤 프로그램이든 수정 없이 동일하게 복원된다는 보장도 서로 다른 주장이다.[1]

## 직접 읽어볼 자료

- [README의 Actor·Worker와 런타임 설명](https://github.com/agent-substrate/substrate/blob/main/README.md)
  에이전트를 만드는 SDK와 만들어진 프로그램을 실행하는 시스템의 차이를 먼저 확인한다.
  Actor가 존재하는 기간과 Worker를 점유하는 기간을 나누어 읽으면 자원 공유의 전제가 드러난다.
- [공식 Counter Demo](https://github.com/agent-substrate/substrate/blob/main/demos/counter/README.md)
  두 카운터가 어디에 저장되는지 표시하고 Full·Data 정책에서 무엇이 남는지 비교한다.
  요청 라우팅, 중단, 재개와 영구 삭제를 구분해 각각의 관찰 항목을 확인한다.
- [README의 Status·Quickstart](https://github.com/agent-substrate/substrate/blob/main/README.md)
  pre-1.0 경고와 새 노드의 버전 라벨 요구를 읽는다.
  클러스터 자원이 존재하는 상태와 실제 Actor를 받을 준비가 끝난 상태가 왜 다른지 살핀다.

## 정리

Agent Substrate는 프로그램의 논리적 수명과 실제 실행 자리를 분리한다.
중단·재개의 의미는 스냅샷 정책과 저장·라우팅 준비에 달려 있으며 대규모 성능과 호환성은 별도 검증 대상이다.

## 자료 확인 범위

2026-09-27 README와 Counter Demo의 공식 실습 문서를 확인했다.
클러스터 생성, 컨테이너 빌드, Actor 생성·중단·재개·삭제와 처리량 측정은 실행하지 않았다.

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

## Sources

[1] agent-substrate/substrate — README.md

<https://github.com/agent-substrate/substrate/blob/main/README.md>

[2] agent-substrate/substrate — demos/counter/README.md

<https://github.com/agent-substrate/substrate/blob/main/demos/counter/README.md>
