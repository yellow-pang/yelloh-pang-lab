---
title: "mastra-ai/mastra"
repository: "mastra-ai/mastra"
url: "https://github.com/mastra-ai/mastra"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# mastra-ai/mastra

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Mastra는 TypeScript로 모델 호출, 도구를 사용하는 Agent, 명시적인 Workflow를 묶어 AI 애플리케이션을 만드는 프레임워크다.[1]
새 언어 모델 자체도, 설치만 하면 모든 업무를 끝내는 완성형 비서도 아니다. 기존 React·Next.js·Node 애플리케이션에 통합하거나 별도 서버로 배포하는 개발 기반이다.[1]

## 자유롭게 판단하는 부분과 순서를 고정할 부분

Agent는 목표에 따라 모델이 도구를 고르고 여러 번 행동하는 구성이다.
Workflow는 개발자가 단계와 분기·병렬 처리 관계를 지정하는 구성이다.
Mastra는 이 둘을 제공하므로 모든 처리를 자연어 지시에만 맡기거나, 반대로 모든 분기를 수작업으로 고정하는 방식 사이에서 역할을 나눌 수 있다.[1]

README에는 모델 연결, 대화 이력·검색 기반 문맥, 중단과 재개, 평가와 관찰 기능이 함께 소개된다.
다만 기능 목록이 존재한다는 사실은 특정 서비스의 정확도나 운영 안정성을 이미 검증했다는 뜻이 아니다.[1]

## Workflow는 실행 전에 단계 관계를 만든다

루트에는 `packages`, `stores`, `server-adapters`, `examples`, `docs`, `ee`가 나뉘어 있다.
프레임워크 코어와 저장소·서버 연결, 예제, 별도 조건의 코드가 공존하는 모노레포 구조다.[2][1]

`packages/core/src/workflows/workflow.ts`의 `then`은 앞 단계 출력이 다음 단계 입력에 맞는지를 TypeScript 타입으로 연결한다.
이어 `stepFlow`와 `serializedStepFlow`에 단계를 추가하고 ID별 단계 표에도 등록한다.
즉 메서드를 연결하는 표현은 문장 장식이 아니라 실행 관계와 직렬화 가능한 표현을 쌓는 API다.[3]

같은 파일의 `parallel`은 병렬 항목을 만들고 각 단계를 등록하며, 출력 타입을 단계별 결과 묶음으로 표현한다.
병렬 처리란 서로 독립적인 일을 함께 진행하는 방식이며, 단계 사이 데이터 의존성이 사라진다는 뜻은 아니다. 이 대표 파일 확인은 그래프 구성 계약에 대한 조사이지 모든 실행 엔진의 동시성 검증은 아니다.[3]

## 가상 시나리오: 공개 자료 비교 노트 만들기

공개 문서 두 개에서 핵심 내용을 추출한 뒤 비교 초안을 만드는 개인 도구를 가정한다.
두 추출 단계는 병렬로 구성하고, 비교 단계는 앞선 결과를 받아 실행하도록 설계할 수 있다.
모호한 문장 해석에는 Agent를, 결과 취합과 승인 순서에는 Workflow를 쓰는 구성이다.[1][3]

중요한 외부 행동 전에 사용자의 입력을 기다리려면 중단·재개와 상태 저장 기능을 검토한다.
README는 저장소를 통해 실행 상태를 기억한다고 설명하므로, 중단 이후 살아남아야 할 데이터의 저장 방식도 설계 대상이다.
이 시나리오는 내장 비교 제품의 존재나 실제 실행 성공을 주장하는 예제가 아니다.[1]

## 라이선스와 도구 권한을 별도로 확인하기

기본 코드 대부분은 Apache-2.0이지만 모든 `ee/` 디렉터리는 Enterprise 조건이며 외부 구성 요소는 원래 라이선스를 따른다.[4]
Enterprise 라이선스는 소스 열람이 가능해도 오픈소스는 아니라고 명시한다.
개발·테스트와 운영 사용을 구분하고, 운영에는 허용하는 서면 계약과 유효한 License Key가 함께 필요하며 재배포에도 제한이 있다.[5]

따라서 “TypeScript 오픈소스 프레임워크”라는 소개만 보고 전체 저장소를 같은 조건으로 복제해서는 안 된다.
모델 제공자 연결과 외부 도구는 별도 계정·비용·권한 검토 대상이며, 프레임워크의 평가 기능이 이를 자동 승인하지는 않는다.[1][4][5]

## 직접 읽어볼 자료

- [1] Agent와 Workflow 설명을 비교해 모델 판단과 개발자 제어의 경계를 잡는다.
- [3] `then`, `parallel`, `branch`가 단계 정보를 어떻게 쌓는지 읽는다.
- [4][5] 루트 라이선스 매핑과 Enterprise의 개발·운영·재배포 조건을 연결해서 확인한다.

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

[1] mastra-ai/mastra — README

<https://github.com/mastra-ai/mastra/blob/main/README.md>

[2] mastra-ai/mastra — 루트 파일 목록

<https://api.github.com/repos/mastra-ai/mastra/contents>

[3] mastra-ai/mastra — packages/core/src/workflows/workflow.ts

<https://github.com/mastra-ai/mastra/blob/main/packages/core/src/workflows/workflow.ts>

[4] mastra-ai/mastra — LICENSE.md

<https://github.com/mastra-ai/mastra/blob/main/LICENSE.md>

[5] mastra-ai/mastra — ee/LICENSE

<https://github.com/mastra-ai/mastra/blob/main/ee/LICENSE>
