---
title: "HKUDS/DeepTutor"
repository: "HKUDS/DeepTutor"
url: "https://github.com/HKUDS/DeepTutor"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-ml-data", "starred-draft"]
---

# HKUDS/DeepTutor

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

학습 자료, 질문, 연습 문제와 기억을 연결하는 에이전트 기반 학습 작업공간이다.[1]

## 답변 한 번보다 긴 학습 흐름

DeepTutor는 채팅, 문제 풀이, 퀴즈, 조사, 시각화와 학습 경로를 하나의 작업공간에 연결하는 애플리케이션이다.
새로운 언어 모델 자체가 아니라 모델 제공자와 자료 저장소, 도구를 조합하는 시스템이다.
README는 여러 학습 모드가 공통 실행 환경과 세션 맥락을 공유하되, 목적에 맞는 반복 처리 구조는 구분한다고 설명한다.[1]

책이나 문서로 공부할 때 질문에 답하는 일과, 이해한 내용을 연습 문제로 확인하는 일은 이어져 있다.
이 프로젝트는 지식 기반, 노트, 문제 은행, 기억을 지원되는 학습 흐름에서 재사용하는 데 초점을 둔다.
RAG는 관련 자료를 먼저 찾아 모델의 답변에 참고시키는 방식이며, 여기서는 여러 검색 엔진을 선택할 수 있다.[1]

## 공통 루프와 학습별 처리의 분리

대표 구현 `deeptutor/agents/loop/pipeline.py`의 `AgenticLoopPipeline`은 한 번의 요청을 실행하는 공통 호스트다.
호스트는 모델 클라이언트, 대화 목록, 도구 호출과 실행 예산을 준비하는 역할이다.
반복 횟수와 토큰 예산을 관리한다는 점에서 단순히 한 문장을 모델에 보내는 래퍼와 구별된다.[2]

하위 클래스는 프롬프트 묶음과 조립기를 바꿔 다른 학습 프로토콜을 만들 수 있다.
도구 구성은 기본적으로 공통 경로를 사용하며, 더 좁은 도구 범위가 필요하면 해당 메서드를 재정의하도록 설명한다.
이 구조는 모드마다 모든 기반 기능을 복제하지 않고 학습 방식의 차이를 넣는 설계다.[2]

README의 Content Workspace는 에이전트가 읽는 콘텐츠 폴더와 비공개 실행 설정을 구분한다.
생성 파일은 `outputs/<capability>/<session>/<turn>/` 아래에 두고, 제시한 스냅샷은 원본 파일이 바뀌어도 유지된다고 설명한다.
학습 자료의 현재 상태와 이미 제시한 결과를 구별하려는 장치로 읽을 수 있다.[1]

## 가상 사례: 공개 교재로 개념 복습하기

공개 사용이 허용된 확률 교재와 “조건부확률을 예제로 설명해 달라”는 질문이 입력이라고 가정한다.
교재를 지식 기반으로 준비하고 해당 학습 공간에서 사용할 수 있게 한 뒤 질문하는 흐름이다.
모델은 허용된 검색·학습 도구를 이용할 수 있고, 이어 퀴즈 모드에서 이해도를 점검하는 문제를 요청할 수 있다.[1][2]

예상 산출물은 설명 대화와 연습 문제이며, 이것은 기능을 이해하기 위한 예시이지 생성 성공 기록이 아니다.
사용자는 답변이 실제 교재의 어떤 부분에 근거하는지와 문제의 정답·해설이 맞는지 따로 확인해야 한다.
자료를 연결했다는 사실만으로 모든 답변에 정확한 출처와 올바른 풀이가 보장되는 것은 아니다.

## 준비 조건과 권한의 범위

README의 로컬 패키지 설치 경로는 Python과 Node.js 런타임을 요구하며, 모델 제공자 설정이 별도로 필요하다.
검색·임베딩·음성·영상 등 선택 기능은 각 엔진이나 외부 서비스의 조건도 따른다.
따라서 웹 화면이 열리는 상태와 학습 기능 전체가 준비된 상태를 구분해야 한다.[1]

출력 폴더 밖 실행은 읽기 전용으로 설명되지만, 격리 기능을 사용할 수 없는 로컬 제한 프로세스 대안은 best effort라고 명시한다.
읽기 전용이라는 표현을 모든 환경에서 동일한 강도의 보안 격리로 해석하면 안 된다.[1]
코드 라이선스는 Apache-2.0이다. 외부 모델 이용료와 교재의 이용 권한은 이 코드 라이선스와 별개의 조건이다.[3]

## 정리

DeepTutor의 특징은 개별 학습 화면보다 공통 세션·자료·도구를 연결하는 실행 구조에 있다.
개인화의 품질은 저장된 기억의 존재만이 아니라 자료 검색, 정답 검토, 사용 가능한 도구와 권한에 달려 있다.

## 직접 읽어볼 자료

1. [README의 Key Features·Content Workspace](https://github.com/HKUDS/DeepTutor/blob/main/README.md)
   학습 모드와 자료 재사용 범위, 생성 파일의 경계를 함께 읽는다.
2. [공통 에이전트 루프](https://github.com/HKUDS/DeepTutor/blob/main/deeptutor/agents/loop/pipeline.py)
   프롬프트 프로필, 실행 예산, 도구 구성 책임이 어떻게 나뉘는지 확인한다.

## 자료 확인 범위

2026-10-28 공식 README·루트 트리와 위 대표 문서·코드를 읽었다.[4]
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

[1] HKUDS/DeepTutor — README.md

<https://github.com/HKUDS/DeepTutor/blob/main/README.md>

[2] HKUDS/DeepTutor — deeptutor/agents/loop/pipeline.py

<https://github.com/HKUDS/DeepTutor/blob/main/deeptutor/agents/loop/pipeline.py>

[3] HKUDS/DeepTutor — LICENSE

<https://github.com/HKUDS/DeepTutor/blob/main/LICENSE>

[4] HKUDS/DeepTutor — 공식 루트 트리

<https://github.com/HKUDS/DeepTutor/tree/main>
