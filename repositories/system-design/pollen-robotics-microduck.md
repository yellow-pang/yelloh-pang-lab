---
title: "pollen-robotics/microduck"
repository: "pollen-robotics/microduck"
url: "https://github.com/pollen-robotics/microduck"
category: "system-design"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["system-design", "starred-draft"]
---

# pollen-robotics/microduck

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

작은 이족보행 로봇의 모터 제어·센서·통신·업데이트를 분리해 운영하는 Rust 소프트웨어다.[1]

## 학습시키는 저장소가 아니라 움직이는 쪽

Microduck은 강화학습 정책으로 움직이는 소형 이족보행 로봇의 실행 소프트웨어다.
강화학습 정책은 관측한 상태에 맞춰 어떤 행동을 낼지 결정하도록 학습된 함수다.
README는 정책 학습을 별도 `microduck_rl` 저장소로 구분하고, 이 저장소는 내보낸 ONNX 정책을 읽는다고 설명한다.[1]

이 프로젝트에는 모터를 제어하는 반복 처리뿐 아니라 카메라, 무선 통신과 업데이트 경로가 들어 있다.
README 기준 제어 루프는 50 Hz로 작동하고 15개 서보를 다루며, 이는 문서상의 설계·기능 설명이지 이번 조사에서 측정한 성능이 아니다.
단일 Rust 작업공간 안에서 여러 데몬, 즉 계속 실행되는 서비스가 책임을 나눈다.[1]

## 안전 계층이 갖는 쓰기 권한

대표 코드 `duck-control/src/safety.rs`는 로봇에 값을 쓰는 핸들을 안전 계층이 소유한다고 설명한다.
상위 정책이나 클라이언트가 모터에 직접 쓰지 못하도록 Rust의 소유권 규칙을 이용하는 구조다.
정책의 출력이 곧바로 물리적 동작이 되지 않고 별도 경계를 거치는 이유가 드러난다.[2]

이 계층은 NaN·무한대 같은 유효하지 않은 수치를 거부하고, 목표값을 구동기의 이동 범위 안으로 제한한다.
명령이 끊기면 속도 명령을 0으로 만드는 deadman 조건도 있다.
다만 멈춤은 모터 힘을 풀어버리는 상태와 다르며, 이족보행 로봇이 서 있도록 하는 쪽으로 설명한다.[2]

중요한 한계도 코드에 직접 적혀 있다.
현재 범위 제한은 구동기 자체 범위이지 실제 각 관절의 기계적 안전 범위가 아니므로 위험한 관절 자세 전체를 막지 못한다.
넘어짐 판정은 보고값이고 이 계층이 자동으로 모든 동작을 중단하는 규칙은 아니라는 설명도 있다.[2]

## 가상 사례: 이동 입력이 끊긴 상황

입력은 게임패드에서 보내는 이동 의도와 로봇의 센서 상태라고 가정한다.
제어 루프가 정책의 관절 목표를 계산하고, 안전 계층이 목표값을 검사한 뒤 모터 쓰기 경로로 보낸다.[1][2]

통신이 끊겨 명령이 오래되면 속도 명령을 0으로 바꾸는 조건이 적용되는 흐름이다.
예상 결과는 무조건 전원을 끄는 것이 아니라 이동 의도를 멈추는 제어 상태다.
이는 코드 구조를 설명하는 예시이며 실제 로봇의 정지 거리·균형·낙상 안전을 시험한 결과가 아니다.[2]

## 복구 경로를 제어 루프와 분리한다

코드 라이선스는 Apache-2.0이다. 소프트웨어 사용 권한과 실제 하드웨어의 안전 책임은 별개다.[3]
공식 아키텍처 문서는 `robotd`가 모터를 소유하고 `configd`·`updaterd`·`btd`는 제어 루프가 죽어도 복구 경로로 남도록 설명한다.
서비스는 Unix 소켓의 JSON-RPC로 통신하고, 클라이언트는 모터 수치보다 의도를 보낸다.[4]

업데이트는 새 릴리스 디렉터리의 서명을 확인한 뒤 현재 링크를 바꾸고, 상태 점검에 실패하면 이전 릴리스로 돌아가는 설계다.
그러나 이 문서에는 draft 상태와 첫 출하 버전을 향한 설명이라는 범위 표시가 있다.
따라서 설계 문서 전체를 현재 모든 하드웨어에서 검증된 기능으로 옮겨 적지 않아야 한다.[4]

README는 시뮬레이션 경로도 안내하지만 가상 몸체에서 동작한 결과와 실제 관절·배터리·바닥 환경의 안전성은 다르다.
이번 조사는 어떤 시뮬레이터나 로봇도 실행하지 않았다.[1]

## 직접 읽어볼 자료

1. [README의 Under the hood](https://github.com/pollen-robotics/microduck/blob/main/README.md)
   정책 학습 저장소와 로봇 실행 저장소를 먼저 구분한다.
2. [안전 계층 구현](https://github.com/pollen-robotics/microduck/blob/main/duck-control/src/safety.rs)
   유효하지 않은 값 거부와 관절별 안전 범위의 공백을 함께 읽는다.
3. [전체 아키텍처](https://github.com/pollen-robotics/microduck/blob/main/docs/design/architecture.md)
   제어 서비스가 실패해도 남는 복구 경로와 문서의 draft 표시를 확인한다.

## 자료 확인 범위

2026-10-28 공식 README·루트 트리와 위 대표 문서·코드를 읽었다.[5]
설치·실행·외부 계정 연결은 하지 않았으며, 예시는 직접 사용 경험이 아닌 가상 흐름이다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] pollen-robotics/microduck — README.md

<https://github.com/pollen-robotics/microduck/blob/main/README.md>

[2] pollen-robotics/microduck — duck-control/src/safety.rs

<https://github.com/pollen-robotics/microduck/blob/main/duck-control/src/safety.rs>

[3] pollen-robotics/microduck — LICENSE

<https://github.com/pollen-robotics/microduck/blob/main/LICENSE>

[4] pollen-robotics/microduck — docs/design/architecture.md

<https://github.com/pollen-robotics/microduck/blob/main/docs/design/architecture.md>

[5] pollen-robotics/microduck — 공식 루트 트리

<https://github.com/pollen-robotics/microduck/tree/main>
