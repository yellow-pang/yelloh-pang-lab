---
title: "tile-ai/tilelang"
repository: "tile-ai/tilelang"
url: "https://github.com/tile-ai/tilelang"
category: "ai-ml-data"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# tile-ai/tilelang

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

TileLang은 행렬 곱이나 어텐션 같은 수치 연산을 GPU·CPU·NPU에서 실행할 작은 계산 프로그램으로 작성하고 컴파일하는 Python 문법의 전용 언어다.
모델을 대화로 사용하는 앱이나 학습된 모델이 아니라, 모델 내부의 계산을 구현하는 도구다.[1]

## 큰 계산을 작은 타일로 나눈다

커널은 GPU 같은 장치에서 실행할 계산 프로그램을 뜻한다.
TileLang이 대상으로 하는 GEMM은 행렬 곱 계열 연산이며, FlashAttention은 어텐션 계산을 다루는 대표 사례다.
행렬 전체를 한 번에 처리한다고만 적는 대신 일정 크기의 사각형 조각, 즉 타일로 나누어 어디에 옮기고 어떤 순서로 계산할지 표현한다.[1][2]

Python처럼 보인다고 일반 Python 반복문을 GPU에서 그대로 돌리는 것은 아니다.
`T.Kernel`, `T.copy`, `T.gemm` 등의 전용 표현을 컴파일러가 해석해 장치용 코드로 낮춘다.
낮춘다는 것은 사람이 쓰기 쉬운 표현을 기계가 실행할 더 구체적인 명령으로 바꾸는 과정이다.
README는 기반 컴파일러 구조로 TVM을 사용한다고 설명한다.[1][2][3]

타일 단위 표현은 메모리 이동과 계산을 조정할 여지를 남긴다.
그러나 짧은 코드가 곧 모든 장치에서 가장 빠른 코드라는 뜻은 아니다.
대표 예제부터 타일 크기, 스레드 수, 자료형을 명시하고 있어 작성자는 데이터 크기와 장치 특성을 이해해야 한다.[2]

## 작성, 변환, 실행은 서로 다른 층이다

작성 단계의 `tilelang.language`는 배열의 모양, 자료형, 메모리 공간, 병렬 연산을 표현한다.
JIT는 실행에 필요한 시점에 특정 입력 형태에 맞는 코드를 만드는 방식으로, README는 첫 사용 때 입력 모양과 컴파일 시점 인자에 맞춰 커널을 구체화한다고 설명한다.[1]

Backend 문서는 장치용 변환 경로와 실행 경로를 구분한다.
장치 backend는 언어 확장, 대상 선택, 변환 단계, 코드 생성을 담당한다.
실행 backend는 생성물을 빌드하고 불러와 실제로 실행한다.
예를 들어 CUDA가 목표 장치라는 사실과 `nvrtc`를 사용해 실행 코드를 준비한다는 선택은 같은 항목이 아니다.[3]

공통 `BackendContext`는 선택된 장치와 호환 실행 경로를 담고 이후 컴파일 단계로 전달된다.
구성상 `tilelang/backend/`는 공통 선택 구조, `tilelang/<backend>/`는 장치별 처리, `tilelang/jit/`는 공통 JIT와 실행 연결을 맡는다.
이는 새 장치를 추가할 때 어떤 구현을 공유하고 무엇을 따로 작성해야 하는지 보여주는 구조다.[3]

## 예시로 따라가는 흐름

공식 `examples/quickstart.py`는 FP16 행렬 두 개를 곱한 뒤 음수 결과를 0으로 만드는 ReLU를 같은 커널 안에 넣는다.
FP16과 FP32는 수를 각각 다른 비트 수로 표현하는 방식이며, 이 예제는 입력·출력은 FP16, 중간 누적은 FP32로 둔다.
행렬 크기는 각각의 축을 1024로, 출력 타일은 128×128, 곱셈에서 순회하는 축의 타일은 32로 설정한다.[2]

먼저 `T.Kernel`이 출력 타일들을 맡을 실행 묶음을 정의한다.
각 묶음은 A와 B의 조각을 장치 안의 공유 메모리로 복사하고 `T.gemm`으로 부분 곱을 누적한다.
공유 메모리는 같은 실행 묶음이 함께 쓰는 임시 저장 공간이다.
`T.Pipelined`는 이 이동과 계산을 여러 단계로 조직하고, 누적이 끝나면 `T.Parallel` 구간에서 각 값에 ReLU를 적용해 최종 결과를 쓴다.[2]

예제의 실행부는 `matmul.compile`로 지정한 크기의 커널을 만들고 PyTorch가 GPU에 준비한 난수 행렬을 전달한다.
출력은 같은 입력에 대한 `torch.relu(a @ b)`와 비교하며 `rtol=1e-2`, `atol=1e-2` 허용 오차를 사용한다.
즉 “실행이 끝났다”와 “기준 계산에 허용 오차 내로 맞았다”를 다른 단계로 확인한다.[2]

그다음 생성한 커널 소스를 출력하고 profiler로 지연 시간을 측정하는 코드가 이어진다.
이 파일의 출력 문구와 측정 구문은 공식 예제의 내용이지 이번 조사에서 얻은 실행 결과가 아니다.
사람이 직접 확인할 때에는 자신의 입력 크기·자료형에서도 정확도가 맞는지, 처음 컴파일하는 비용과 반복 실행 시간을 구분했는지, 실제 장치와 비교 조건이 같은지 살펴야 한다.[2]

## 장치 지원은 같은 수준이 아니다

README는 CUDA를 주된 backend로, ROCm·Metal·Ascend 950을 지원 대상으로, LLVM CPU·CuTe DSL·WebGPU를 실험적 대상으로 구분한다.
별도 저장소에서 개발되는 생태계 adapter는 본 저장소의 배포 wheel에 포함되지 않으며 호환 일정도 독립적일 수 있다.
wheel은 미리 빌드한 Python 배포 패키지다.[1]

이 구분은 특정 제조사의 모든 장치나 모든 연산이 동일하게 작동한다는 보장이 아니다.
README는 일부 CUDA 기능에 특정 GPU 세대가 필요하고, ROCm의 gfx950 경로는 아직 CI에서 검증하지 않는다고 적는다.
Metal의 특정 행렬 기능도 지원되는 M5 시스템 조건을 붙인다.
Ascend 950은 CANN과 `torch_npu`, 소스 빌드 조건을 확인해야 한다.[1]

설치 안내의 Python 최소 버전은 3.10이다.
AMD 경로에서는 ROCm용 PyTorch와 호스트의 `hipcc`를 제공하는 ROCm 설치가 필요하므로 `pip install tilelang` 한 줄만으로 모든 준비가 끝나지 않는다.
소스 빌드는 수정된 TVM 등의 하위 의존성을 포함해야 한다.
대표 quickstart가 `device="cuda"` 텐서를 만드는 예제라는 점도 Apple Metal이나 CPU에서 그대로 실행된다고 가정하지 말아야 할 이유다.[4][2]

문서 간 표기도 주의할 부분이다.
quickstart의 주석은 대상 선택을 CUDA·HIP·CPU 중심으로 설명하지만 루트 README의 지원 표는 더 넓다.
장치 지원 여부는 짧은 코드 주석 하나보다 지원 수준 표와 해당 backend 요구사항을 함께 읽어 판단해야 한다.[1][2]

## 라이선스·비용·성능 해석

LICENSE에는 MIT 허가와 저작권·허가 고지 유지, 보증 부재 조항이 있다.
동시에 2024-12-01부터 2025-03-14까지의 기간에 추가 협업 조건이 있었다는 문구도 들어 있다.
그 조건 전문은 이 파일에 없으므로 이를 숨기거나 현재 모든 사용에 별도 제약이 있다고 확대 해석하지 않는다.
배포·상업적 재사용 판단에서는 이 고지와 의존성 조건을 별도로 확인할 필요가 있다.[5]

읽은 자료는 TileLang 자체의 호스팅 요금제를 안내하지 않는다.
코드 사용 허가와 GPU 구매·임대, 클라우드 실행, 드라이버·도구 체인 환경 준비 비용은 별개다.[1][4][5]
README에는 장치별 벤치마크 소개가 있지만 이번에는 측정 스크립트를 실행하거나 비교 환경을 재현하지 않았다.
따라서 특정 라이브러리보다 항상 빠르다거나 임의 모델의 속도가 같은 비율로 개선된다고 결론 내리지 않는다.[1]

## 직접 읽어볼 자료

1. [README](https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/README.md)

   Quick Start로 언어가 표현하는 계산을 먼저 보고 Platform and Backend Support에서 자신의 장치가 어느 수준에 속하는지 확인한다.
   성능 소개와 지원 범위는 서로 다른 주장으로 읽는다.
2. [quickstart 예제](https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/examples/quickstart.py)

   메모리 할당, 복사, 행렬 곱, ReLU, 결과 복사의 순서를 따라간다.
   실행부에서 기준값 비교와 시간 측정을 분리하는 방식까지 읽어야 예제의 검증 의도를 이해할 수 있다.
3. [backend 구조](https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/tilelang/backend/README.md)

   장치 선택과 실행 방식의 차이를 익힌 뒤 전체 변환 그림을 읽는다.
   처음부터 개별 최적화 코드를 보기보다 공통 부분과 장치별 책임을 구분하는 자료다.

## 자료 확인 범위

2026-10-04 기준 고정 커밋 `d82101ff08acea8d3e3ca7d0ab40985a3ca2a781`의 README, quickstart 전체, 설치 안내의 기본·ROCm·소스 빌드 조건, backend 구조의 역할 설명, LICENSE를 읽었다.
설치, 프로젝트 실행, 컴파일, GPU 커널 실행, 성능 검증은 하지 않았다.

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

[1] tile-ai/tilelang — README.md

<https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/README.md>

[2] tile-ai/tilelang — examples/quickstart.py

<https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/examples/quickstart.py>

[3] tile-ai/tilelang — tilelang/backend/README.md

<https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/tilelang/backend/README.md>

[4] tile-ai/tilelang — docs/get_started/Installation.md

<https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/docs/get_started/Installation.md>

[5] tile-ai/tilelang — LICENSE

<https://github.com/tile-ai/tilelang/blob/d82101ff08acea8d3e3ca7d0ab40985a3ca2a781/LICENSE>
