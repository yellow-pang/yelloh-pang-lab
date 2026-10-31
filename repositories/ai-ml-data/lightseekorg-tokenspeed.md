---
title: "lightseekorg/tokenspeed"
repository: "lightseekorg/tokenspeed"
url: "https://github.com/lightseekorg/tokenspeed"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-ml-data", "starred-draft"]
---

# lightseekorg/tokenspeed

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

TokenSpeed는 이미 학습된 언어 모델을 실행해 응답 토큰을 생성하는 추론 엔진이다.
에이전트처럼 모델을 반복 호출하는 작업을 목표로 하지만, 스스로 도구를 선택하는 에이전트나 모델 학습 데이터 모음은 아니다.[1]

## 요청 관리와 GPU 계산을 분리한다

여러 요청을 빠르게 처리하려면 계산 속도뿐 아니라 언제 어떤 요청에 메모리를 배정할지도 정해야 한다.
README는 C++ 제어 계층과 Python 실행 계층을 분리하고, 요청 수명과 KV 캐시의 소유권을 상태 기계로 표현한다고 설명한다.
상태 기계는 요청이 대기·실행 같은 어떤 단계에 있으며 다음에 무엇을 허용할지 명시하는 방식이다.[1]

커널은 GPU가 수행하는 낮은 수준의 계산 코드다.
프로젝트는 커널을 별도 계층과 등록 체계로 구성하며, 요청 진입점에는 Rust와 msgpack 연결을 설명한다.
따라서 Python 저장소라는 언어 표시만으로 요청 처리 전체가 Python에서 수행된다고 보면 안 된다.[1]

## 긴 입력을 한 번에 받아들이지 않는 스케줄러

`docs/design/scheduler.md`는 prefill을 `max_scheduled_tokens`로 제한한 덩어리로 나눠 처리한다고 설명한다.
Prefill은 입력 문맥을 먼저 계산하는 단계이며, KV 캐시는 이후 토큰 생성에서 재사용할 중간 계산 결과다.
로컬 prefill의 용량 승인은 전체 프롬프트가 아니라 이번에 계산할 덩어리를 기준으로 한다.[2]

캐시 그룹마다 필요한 페이지를 계산하고 coordinator가 공간을 줄 수 없으면 요청을 기다리게 한다.
문서는 request-pool 슬롯 부족과 KV 용량 부족도 구분한다. 두 자원의 부족이 같은 대응을 유발하는 것은 아니다.
분리 배치의 decode 역할은 원격 입력을 받는 다른 규칙이 있어, 덩어리 단위 설명을 모든 역할에 일반화하면 안 된다.[2]

## 가상 추론 시나리오

긴 공개 문서를 요약하는 요청과 짧은 코딩 질문을 같은 GPU 서버에서 처리하는 상황을 가정한다.
입력은 모델 체크포인트와 요청이며, 엔진은 입력 계산과 생성 단계에서 캐시 공간 및 계산 순서를 관리한다.[1][2]
사용자가 관찰할 결과는 응답 토큰, 첫 응답까지의 시간, 처리 중 메모리 사용량이다.
어떤 조합이 더 빠른지는 GPU·모델·입력 길이·동시 요청 수를 맞춰 측정해야 하며 README의 속도 표현만으로 결정할 수 없다.

## 환경과 실행 권한 주의점

Getting started는 NVIDIA GPU 또는 ROCm GPU 환경, 모델 체크포인트 접근과 충분한 공유 메모리를 전제로 한다.
CUDA와 ROCm nightly 색인을 동시에 설정하지 말라는 경고도 있어 커널 패키지와 장치의 조합을 확인해야 한다.[3]
컨테이너 예시는 privileged 및 host network·PID·IPC 옵션을 포함한다. 일반적인 격리 컨테이너와 같은 권한 경계로 취급하면 안 된다.
서버 예시의 전체 인터페이스 바인딩도 개인 실험에 필요한 공개 범위인지 따로 검토해야 한다.[3]
코드는 MIT 라이선스이며, 실행할 모델의 접근·배포 조건은 별도로 확인한다.[4]

## 직접 읽어볼 순서

1. README의 Scheduler, Kernels, Entrypoint를 읽어 제어와 계산의 역할을 나눈다.[1]
2. Scheduler 문서의 Admission is per chunk에서 요청이 기다리는 조건을 확인한다.[2]
3. Getting started의 장치·패키지 조합과 컨테이너 권한을 먼저 읽고, 성능 비교는 그다음 별도 실험으로 설계한다.[3]

## 자료 확인 범위

2026-10-28 공개 README, 루트 파일 트리와 아래 대표 자료를 확인했다.
설치·실행·성능 측정은 하지 않았으며, 활용 예시는 동작을 설명하기 위한 가상 상황이다.

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

## Sources

[1] lightseekorg/tokenspeed — README.md

<https://github.com/lightseekorg/tokenspeed/blob/main/README.md>

[2] lightseekorg/tokenspeed — docs/design/scheduler.md

<https://github.com/lightseekorg/tokenspeed/blob/main/docs/design/scheduler.md>

[3] lightseekorg/tokenspeed — docs/guides/getting-started.md

<https://github.com/lightseekorg/tokenspeed/blob/main/docs/guides/getting-started.md>

[4] lightseekorg/tokenspeed — LICENSE

<https://github.com/lightseekorg/tokenspeed/blob/main/LICENSE>
