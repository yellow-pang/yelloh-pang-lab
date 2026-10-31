---
title: "withmarbleapp/os-taxonomy"
repository: "withmarbleapp/os-taxonomy"
url: "https://github.com/withmarbleapp/os-taxonomy"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-ml-data", "starred-draft"]
---

# withmarbleapp/os-taxonomy

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Marble Skill Taxonomy는 초등 수준에서 배우는 개념을 작은 학습 주제로 나누고 선수 관계와 교육과정에 연결한 공개 데이터셋이다.
여기서 Skill은 에이전트 도구 능력보다 학습자가 익힐 개념·절차에 가깝다.
완성된 학습 관리 서비스나 학생의 실제 숙련도 데이터베이스는 아니다.[1]

## 과목 목록을 학습 관계로 바꾸기

주제마다 설명, 대략적인 연령 범위, 이해했는지 확인할 evidence, 연결된 교육과정 코드가 있다.
선수 관계는 어떤 주제를 배우려면 무엇을 먼저 알아야 하는지와 그 이유를 담는다.
`topicId`가 `prerequisiteId`에 의존하는 방향이므로, 다음에 배울 주제를 찾을 때는 간선 방향을 거꾸로 읽어야 한다.[1]

루트 `data/`는 JSON 자료, `schema/`는 구조 명세, `scripts/`는 검증 도구, `reports/`는 정비·검토 기록이다.
`topics.json`과 `dependencies.json`은 주제와 연결, `curriculum-standards.json`은 교육과정 식별자, `teaching-research.json`은 교수 안내를 제공한다.
자료는 주제 ID로 연결되며 교수 안내가 없는 주제도 명시적으로 남는다.[1]

## 구조 검증이 확인하는 것과 확인하지 않는 것

`scripts/validate.mjs`는 선언한 건수와 실제 배열 길이, 주제 ID 중복, 필수 속성을 검사한다.
선수 관계의 양 끝이 실제 주제를 가리키는지, 자기 자신을 가리키지 않는지, hard·soft 유형이 맞는지 확인한다.
순환 검사를 호출해 A를 배우려면 B가, B를 배우려면 A가 필요한 식의 고리를 탐지한다.[2]

교육과정 코드 참조와 파일 SHA-256도 검사한다. 해시는 같은 파일 내용인지 확인하는 지문이다.
본문을 배포하지 않는 codes-only 출처에 원문 데이터가 실리지 않았는지도 검사한다.
이런 검사는 데이터 연결과 배포 규칙을 지키는 데 유용하지만 교육 효과나 교수법의 정확성을 증명하지는 않는다.[2]

README는 교수 연구 기록이 에이전트 작성물이며 제안 활동이 실증 검증된 개입이 아니라고 명시한다.
기존 `verified`와 높은 `confidence` 표시도 이번 공개 시점에 출처를 다시 검증했다는 뜻이 아니다.
교육과정 코드와 연결되었다는 사실 역시 해당 기준 전체를 충족하거나 학습자가 숙달했다는 인증이 아니다.[1]

## 가상 시나리오: 문장 학습 카드의 선수 개념

공개 예시의 ‘Building sentences’ 주제로 작은 학습 카드를 만드는 상황을 가정한다.
주제 ID로 설명과 evidence를 찾고, 그 주제를 대상으로 하는 dependency를 따라 선수 개념을 표시한다.
교육과정 코드는 원문 출처 링크로 연결하고, 교수 연구가 없는 경우에는 안내를 자동으로 지어내지 않고 비워 둔다.[1]

`assessmentPrompt`의 이름 자리표시는 표시 전에 바꾸거나 제거한다.
산출물은 읽을 수 있는 개념 지도와 학습 질문이며, 한 번의 응답으로 아동의 숙련도를 확정하는 평가 도구가 아니다.
연령 범위도 개인별 진단값과 구분해야 한다.[1]

## 데이터와 텍스트의 라이선스가 다르다

데이터베이스의 구조·관계는 ODbL 1.0, Marble이 작성한 텍스트는 CC BY-SA 4.0으로 구분된다.
출처 표시와 동일조건 변경허락 의무를 검토해야 하며, 외부 교육과정 원문은 Marble이 재허가할 수 없다.[1][3]
`PROVENANCE.md`는 제한이 있는 출처의 원문 대신 코드만 제공하는 이유와 출처별 조건을 설명한다.
따라서 빈 설명을 자동으로 채워 재배포하기 전에 해당 교육과정의 이용 조건을 확인해야 한다.[3]
README에는 보조 `marble-builder` Skill도 있지만, 이 글은 그 전체 제작 워크플로가 아니라 데이터와 검증 구조를 조사했다.[1]

## 직접 읽어볼 자료

- [1] README의 A topic, A dependency, Teaching research를 읽어 필드 의미와 불확실성을 구분한다.
- [2] `validate.mjs`에서 관계 무결성·순환·해시 검사가 각각 어떤 오류를 막는지 확인한다.
- [3] PROVENANCE의 codes-only 원칙과 실제 사용할 교육과정의 개별 조건을 읽는다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 문서·코드의 관련 부분을 확인했다.
설치·모델 다운로드·서비스 실행은 하지 않았으며, 시나리오는 이해를 위한 가정이다.

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

[1] withmarbleapp/os-taxonomy — README.md

<https://github.com/withmarbleapp/os-taxonomy/blob/main/README.md>

[2] withmarbleapp/os-taxonomy — scripts/validate.mjs

<https://github.com/withmarbleapp/os-taxonomy/blob/main/scripts/validate.mjs>

[3] withmarbleapp/os-taxonomy — PROVENANCE.md

<https://github.com/withmarbleapp/os-taxonomy/blob/main/PROVENANCE.md>
