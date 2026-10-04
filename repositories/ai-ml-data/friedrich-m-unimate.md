---
title: "Friedrich-M/UniMate"
repository: "Friedrich-M/UniMate"
url: "https://github.com/Friedrich-M/UniMate"
category: "ai-ml-data"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# Friedrich-M/UniMate

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

UniMate는 글로 설명한 동작과 3D 캐릭터의 골격 정보를 받아, 서로 다른 골격의 움직임을 하나의 모델로 생성하려는 연구용 코드다.
완성된 캐릭터 제작 앱이나 텍스트만으로 형태까지 만드는 모델이 아니라, 이미 골격이 있는 대상의 애니메이션 생성·학습·데이터 준비를 다룬다.[1]

## 같은 걷기도 골격마다 다르다

사람, 네 발 동물, 날개가 있는 생물은 관절 수와 연결 방식이 다르다.
여기서 골격은 3D 모델을 움직이는 관절과 뼈의 구조이며, topology는 그 연결 관계를 뜻한다.
UniMate는 동작 설명뿐 아니라 기준 자세와 관절 구조를 조건으로 사용한다.
README는 골격마다 모델을 다시 학습하지 않는 생성을 목표로 소개하지만 많은 동작과 골격에서 여전히 실패하는 초기 단계라고도 밝힌다.[1]

관련 이름은 구분할 필요가 있다.
UniMate는 모델과 학습·추론 코드이고, UniML3D는 다양한 골격의 동작에 설명문과 구조 정보를 붙인 데이터 모음이다.
`data_process/`는 원본 자산을 이 학습용 형식으로 바꾸는 파이프라인이다.
체크포인트는 학습된 모델 상태를 저장한 파일로, 소스 코드만 받는 것과 이미 학습된 모델을 받는 것은 다르다.[1][2]

## 자료를 준비하고 움직임을 만드는 구성

데이터 준비는 원본 FBX·GLB에서 골격과 동작을 추출하고, 여러 방향의 화면을 렌더링한 뒤 설명문과 관절 주석을 만들며, 좌표·표현 방식을 맞춘 특징 데이터로 바꾸는 흐름이다.
Blender는 3D 자산을 읽고 내보내는 데 쓰이고, 텍스트·시각 모델은 설명문과 관절 이름 정리에 쓰인다.
마지막 애니메이션 단계는 생성한 움직임을 실제 외형에 적용해 GLB·FBX로 내보내는 역할이다.[2]

대표 설정 `uniml3d_60frames_graph_adaln.json`은 Truebones·Mixamo·Objaverse를 함께 사용하며, 한 구간을 60프레임으로 하고 관절 수 범위를 5~60으로 정한다.
이것은 모델 전체의 영구 한계가 아니라 해당 실험 설정의 데이터 수용 조건이다.
README는 다른 설정에서 관절 상한 등이 달라진다고 설명한다.[3][1]

이 설정의 `graph`는 프레임 안의 관절 관계와 관절별 시간 변화를 나누어 처리하는 방식이다.
`adaln`은 문장 정보를 신경망 내부 조절에 반영하는 선택이며, 다른 제공 설정은 문장 토큰에 직접 주의를 기울이는 `cross_attn`을 사용한다.
기본 텍스트 인코더는 문장을 숫자 표현으로 바꾸는 `google/flan-t5-base`다.
학습 방식인 flow matching은 잡음에서 동작 데이터로 이동하는 변화를 배우는 것으로 설명되어 있다.[1][3]

## 예시로 따라가는 흐름

공식 예제는 `test_cases.json`에 `Dog-walk`와 `a dog walks forward at a steady pace`를 짝지어 넣는다.
앞의 `Dog`는 데이터에 있는 대상 종류, 뒤의 `walk`는 출력 이름에 사용할 식별자다.
아무 동물 이름이나 적으면 골격을 만들어 주는 입력이 아니며, 해당 종류의 조건 데이터가 준비되어 있어야 한다.[1][4]

추론 프로그램은 실험 폴더의 `config.json`을 읽어 모델 구성을 복원하고 체크포인트를 찾는다.
별도 파일을 지정하지 않으면 단계 번호가 가장 큰 체크포인트를 선택하며, 설정과 저장 상태가 허용할 때 EMA 가중치를 사용한다.
EMA는 여러 학습 시점의 모델 상태를 이동 평균한 복사본이다.
이어서 데이터와 정규화 통계를 읽고, 사례의 대상 종류가 존재하는지 확인한다.
실제 입력 검사에서는 없는 종류나 빈 설명문을 경고하고 건너뛰며, 쓸 수 있는 사례가 하나도 없으면 오류를 낸다.[4]

문장 인코딩을 먼저 마친 뒤 생성 모델을 장치로 옮기는 순서도 확인된다.
코드 주석은 텍스트 인코더와 생성 모델이 동시에 GPU 메모리를 점유하지 않도록 하기 위한 순서라고 설명한다.
샘플 생성 결과는 `.npy` 동작 특징, 골격 미리보기 영상, 기준 자세 이미지와 사용한 문장 기록으로 남는다.
외형이 있는 캐릭터 파일이 필요하면 이 결과를 데이터 처리의 애니메이션 단계로 넘기는 별도 과정이 필요하다.[1][4]

사람은 걷는 방향이 설명과 맞는지, 관절 연결이 자연스러운지, 바닥과 발의 관계가 어색하지 않은지 미리보기에서 확인해야 한다.
파일이 생성되었다는 사실만으로 올바른 움직임이 완성되었다고 판단할 수 없다.
이 흐름은 소스와 공식 예제를 읽어 정리한 것이며 실제 모델 추론이나 영상 확인을 수행한 기록은 아니다.[1]

## 임의 골격 지원이라는 말의 현재 범위

README의 추론 절차는 모델이 학습한 데이터 특징 폴더가 있어야 하고 대상 골격도 그 데이터에서 가져온다고 명시한다.
새로운 분포의 rig, 즉 관절이 연결된 3D 자산을 위한 공식 전처리 공개는 같은 README에서 TODO로 남아 있다.[1]

한편 하위 데이터 문서에는 사용자 자산 내보내기와 단일 자산 전처리 경로가 이미 설명되어 있다.
이는 서로 다른 문서의 상태를 함께 읽어야 하는 부분이다.
전처리 도구가 있다는 사실만으로 임의 파일을 표준 추론에 바로 넣는 경로가 완성되었다고 단정하지 않는다.
특히 동작이 없는 자산은 기준 자세 조건만 만들고 `motions/`를 쓰지 않는다고 안내하는 반면, 확인한 표준 추론 로더는 참조할 학습·평가 클립이 있는 종류만 인정한다.[2][4]

학습 데이터 품질도 제약이다.
README는 일부 Objaverse 자산의 뒤집힌 기준 자세나 무관한 동작의 연결이 학습을 불안정하게 만들 수 있다고 경고한다.
공개 체크포인트 역시 preview로 소개되어 있다.
“실시간”이라는 표현은 프로젝트의 설명이며 이번 조사에서는 장치별 지연 시간, 최소 GPU 메모리, 생성 품질을 측정하지 않았다.[1]

## 라이선스와 비용은 데이터까지 나누어 본다

저장소 코드는 MIT 라이선스로 제공되며 재배포 시 고지 유지 조건과 보증 부재 조항이 있다.[5]
그러나 데이터까지 MIT가 되는 것은 아니다.
README는 Mixamo는 원 이용약관, Objaverse는 개별 원본 객체의 라이선스, Truebones ZOO는 상용 라이선스를 따른다고 명시한다.
특히 Truebones 동작 파일은 재배포할 수 없어 주석 자료만 제공하고 원본 팩은 별도 구매하도록 안내한다.[1][2]

데이터 준비에는 Blender·Python 환경뿐 아니라 GPU가 필요한 단계가 있다.
설명문 생성은 로컬 모델과 외부 API 경로를 구분하며, API 경로는 렌더링한 프레임을 요청 입력으로 보낸다고 설명한다.
따라서 API 키·사용료와 외부 전송 범위를 검토해야 하고, 로컬 경로 역시 GPU·저장 공간 비용이 사라지는 것은 아니다.
체크포인트 배포처의 별도 조건은 이번에 직접 읽지 않았으므로 코드 라이선스로 대신 판단하지 않는다.[2]

## 직접 읽어볼 자료

1. [README](https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/README.md)

   Training의 산출물 표와 Inference의 필수 입력을 연결해서 읽는다.
   이어 Note·License를 확인하면 연구 목표, 실제 데이터 의존성, 실패 가능성과 사용 권리를 구분할 수 있다.
2. [추론 진입점](https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/unimate/inference/sample.py)

   `main`, `_resolve_exp_paths`, `_load_test_cases_json` 순으로 모델 복원과 입력 검사를 살펴본다.
   예제의 대상 이름이 파일 이름 이상의 조건을 가진다는 점을 확인할 수 있다.
3. [데이터 처리 안내](https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/data_process/README.md)

   단계별 입력·출력 표 다음에 Requirements, Custom Assets를 읽는다.
   원본 자산에서 학습 데이터와 최종 애니메이션까지 필요한 변환, 외부 API와 상용 데이터의 경계를 확인한다.

## 자료 확인 범위

2026-10-04 기준 고정 커밋 `2c5b384715aa63d8639b1ed7eb74bfe614570c7a`의 README, 대표 학습 설정, 추론의 설정·모델 복원과 입력 검사, 데이터 준비 안내의 관련 절, LICENSE를 읽었다.
설치, 데이터·가중치 다운로드, 학습·추론 실행, 성능 검증은 하지 않았다.

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

[1] Friedrich-M/UniMate — README.md

<https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/README.md>

[2] Friedrich-M/UniMate — data_process/README.md

<https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/data_process/README.md>

[3] Friedrich-M/UniMate — configs/uniml3d_60frames_graph_adaln.json

<https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/configs/uniml3d_60frames_graph_adaln.json>

[4] Friedrich-M/UniMate — unimate/inference/sample.py

<https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/unimate/inference/sample.py>

[5] Friedrich-M/UniMate — LICENSE

<https://github.com/Friedrich-M/UniMate/blob/2c5b384715aa63d8639b1ed7eb74bfe614570c7a/LICENSE>
