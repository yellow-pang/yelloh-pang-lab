---
title: "deepseek-ai/DeepGEMM"
repository: "deepseek-ai/DeepGEMM"
url: "https://github.com/deepseek-ai/DeepGEMM"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-ml-data", "starred-draft"]
---

# deepseek-ai/DeepGEMM

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

DeepGEMM은 대형 언어 모델에 쓰이는 행렬 계산과 관련 연산을 NVIDIA GPU에서 수행하는 CUDA 커널 라이브러리다.[1]
DeepSeek 모델의 가중치나 채팅 서버가 아니라, 모델 안쪽 계산을 담당하는 저수준 부품이다.
현재 README는 FP8·FP4·BF16 GEMM뿐 아니라 Mega MoE, 인덱서용 MQA, HyperConnection 연산도 소개한다.[1]

## 모델 전체가 아니라 계산 구간을 최적화하기

GEMM은 행렬 곱을 중심으로 하는 계산이고, 커널은 GPU에서 실행되는 작업 코드다.
FP8·FP4는 수를 비교적 적은 비트로 표현하는 형식으로, 값의 크기를 복원하기 위한 스케일 정보도 함께 다뤄야 한다.
DeepGEMM은 이런 데이터 형식과 GPU 메모리 배치를 맞춰 계산하는 인터페이스를 제공한다.[1]

MoE는 입력마다 여러 전문가 중 일부를 선택하는 모델 구조다.
README의 grouped GEMM은 전문가별 토큰 수가 다른 경우를 다루며, 연속 배치와 유효 부분을 마스크로 지정하는 배치를 구분한다.
Mega MoE는 전문가 간 전달과 선형 연산 등을 통합하고 통신과 계산을 겹치는 기능으로 설명된다.[1]

## Python 입구와 네이티브 커널의 경계

루트에는 `deep_gemm`, `csrc`, `tests`, `third-party`와 빌드 스크립트가 있다.
Python 호출 표면, 네이티브 구현, 테스트와 외부 구성 요소를 나누어 읽을 수 있다.[2]

`deep_gemm/__init__.py`는 `_C` 확장에서 GEMM·그룹 연산·MQA 등 여러 함수를 가져와 공개한다.
`fp8_gemm_nt` 같은 이름과 FP8/FP4 조합 함수, BF16 함수가 같은 입구에 있으므로 처음의 FP8 전용 소개만으로 현재 범위를 판단하면 좁다.[3]

같은 파일은 SM 개수와 tensor core 활용 설정 등도 가져온다.
이 파일은 어떤 연산을 Python에서 부를 수 있는지 보여주지만, 계산 정확도나 각 장치의 속도를 자체 증명하지는 않는다.
README는 DeepJIT을 통한 런타임 컴파일을 설명하므로 설치 완료와 최초 커널 실행 준비도 구별해야 한다.[3][1]

## 가상 시나리오: 작은 행렬 곱 구간 비교

지원되는 GPU에서 공개 실험용 모델의 특정 행렬 곱 구간을 비교하는 상황을 가정한다.
먼저 기준 계산의 입력 크기·자료형·전치 방향을 고정하고 DeepGEMM이 요구하는 형식으로 준비한다.
이어 출력 오차와 계산 시간을 따로 비교하되, 형식 변환과 첫 컴파일 시간까지 포함할지 명시한다.[1]

README는 입력 전치와 FP8 변환은 사용자가 별도로 처리해야 한다고 강조한다.
따라서 커널 한 번의 속도가 좋아도 입력 변환을 포함한 전체 처리 시간이 같은 비율로 줄지는 않는다.
이 문서는 실제 GPU 실행이나 README 성능 수치의 재현 결과가 아니다.[1]

## GPU 종류와 메모리 배치가 사용 조건이다

요구사항에는 NVIDIA SM90·SM100, CUDA Toolkit 12.9 이상, PyTorch 2.3 이상, C++20 지원 도구가 적혀 있다.
Mega MoE의 대칭 메모리 예시는 PyTorch 2.9 이상과 다중 프로세스 환경을 추가로 요구한다.[1]

SM90과 SM100은 스케일 자료형·지원 메모리 배치가 다르다.
단순히 CUDA가 설치되었다는 이유로 임의 GPU에서 같은 호출을 사용할 수는 없다.
Ascend 구현은 README가 별도 저장소로 안내하므로 이 CUDA 저장소와 같은 실행 경로로 취급하지 않는다.[1]
MIT 라이선스는 저작권·허가 고지 유지와 무보증 조건을 담으며 외부 의존성의 조건은 별도로 확인해야 한다.[4]

## 직접 읽어볼 자료

- [1] Requirements 뒤의 Interfaces Notices를 읽어 장치와 스케일 배치를 먼저 확인한다.
- [3] Python 입구의 `_C` imports에서 원하는 연산과 자료형 조합을 찾는다.
- [1][2] 해당 연산의 설명이 연결하는 테스트를 다음 읽기 대상으로 삼되, 본 초안은 테스트 실행을 하지 않았다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 목록, 아래 대표 구현·문서와 라이선스를 확인했다.
설치·빌드·프로그램 실행은 하지 않았으며, 예시는 이해를 위한 가상 시나리오다.

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

[1] deepseek-ai/DeepGEMM — README

<https://github.com/deepseek-ai/DeepGEMM/blob/main/README.md>

[2] deepseek-ai/DeepGEMM — 루트 파일 목록

<https://api.github.com/repos/deepseek-ai/DeepGEMM/contents>

[3] deepseek-ai/DeepGEMM — deep_gemm/__init__.py

<https://github.com/deepseek-ai/DeepGEMM/blob/main/deep_gemm/__init__.py>

[4] deepseek-ai/DeepGEMM — LICENSE

<https://github.com/deepseek-ai/DeepGEMM/blob/main/LICENSE>
