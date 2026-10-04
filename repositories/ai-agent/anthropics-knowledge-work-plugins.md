---
title: "anthropics/knowledge-work-plugins"
repository: "anthropics/knowledge-work-plugins"
url: "https://github.com/anthropics/knowledge-work-plugins"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/knowledge-work-plugins

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Knowledge Work Plugins는 문서 작성, 자료 검색, 데이터 분석 등 역할별 작업 방법을 Claude에 제공하는 플러그인 모음이다.
Cowork를 중심으로 만들었고 Claude Code에서도 사용하도록 안내한다.
별도의 모델이나 스스로 실행되는 분석 서버가 아니라 전문 지침·명령·도구 연결을 호스트에 추가하는 파일 기반 구성이다.[1]

## 도구가 있어도 작업 기준은 필요하다

CSV를 읽을 수 있는 에이전트에게 “분석해 줘”라고만 말하면 어떤 질문에 답할지, 결측값을 어떻게 볼지, 결과를 어떻게 검증할지가 남는다.
이 프로젝트는 분야 지식을 담은 Skill과 사용자가 명시적으로 부르는 command, 외부 데이터를 가져오는 connector를 역할별 플러그인으로 묶는다.
사용 가능한 기능과 그 기능을 사용하는 절차를 함께 제공하려는 구조다.[1]

README는 기본 구성을 범용 출발점이라고 설명한다.
즉 설치 직후 모든 용어와 지표 정의가 사용자의 목적에 맞는 것으로 취급하지 않는다.
공개 데이터 학습에서도 모집단, 기간, 단위가 무엇인지 정해야 한다.
연결 도구를 추가하는 일과 어떤 분석을 옳다고 판단할지 정하는 일은 다른 결정이다.[1][2]

## 분석 생성과 분석 검증을 나누기

대표로 읽은 data 플러그인은 SQL 질의, 탐색, 시각화, 대시보드와 공유 전 검증을 다룬다.
데이터 창고에 연결하면 스키마를 읽고 질의를 실행할 수 있고, 연결이 없으면 CSV·Excel이나 사용자가 붙여 넣은 SQL 결과를 입력으로 받는 경로도 설명한다.
따라서 외부 데이터베이스 연결이 없다고 모든 기능이 사라지는 것은 아니지만 직접 질의와 제공된 결과 해석은 구별된다.[3]

`validate-data` Skill은 이미 만든 분석이 어떤 질문에 답하는지부터 검토한다.
적절한 자료와 기간을 사용했는지, 비교 대상이 공정한지, 지표의 분모가 일정한지 확인한 다음 계산·차트·결론을 살핀다.
SQL 문법이 맞는 것과 그 SQL이 질문에 맞는 모집단을 집계하는 것은 별개라는 점을 작업 순서에 반영한다.[2]

결과는 단일 점수보다 공유 가능, 한계를 밝히면 공유 가능, 수정 필요라는 단계별 평가와 구체적 문제 목록이다.
독립적인 일부 재계산, 부분합·전체합 비교, 기간 비교의 기준 확인 등이 제시된다.
사람은 검증 보고서에서 실제로 확인한 계산과 접근 가능한 자료가 없어 남겨진 불확실성을 분리해 읽어야 한다.[2]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
공개 행사 참가자 목록과 참석 기록을 결합해 참가 비율을 분석했다고 가정하자.
입력은 요약 그래프만이 아니라 사용한 SQL, 집계 결과, 모집단 정의, 기간 설명이다.
validate-data는 질문과 표 선택을 먼저 살피고 결합 후 같은 사람이 여러 번 세어지지 않았는지 확인하는 절차를 적용할 수 있다.[2]

Skill의 Join Explosion 항목은 다대다 결합이 행 수를 조용히 늘려 합계를 부풀리는 문제를 설명한다.
처리 과정에서는 결합 전후 행 수를 비교하고, 사람 수를 세는 것인지 참석 건수를 세는 것인지 구분해야 한다. `COUNT(DISTINCT ...)` 같은 방법도 목적에 맞을 때만 의미가 있으며, 무조건 중복 제거하면 정당한 여러 참석 기록을 지울 수 있다.
핵심은 질의 모양보다 집계 단위를 명확히 하는 것이다.[2]

그 뒤에는 비교 기간이 같은 길이인지, 차트 축이 오해를 만들지 않는지, 분석 문장의 결론이 자료를 넘지 않는지 본다.
출력에는 문제의 영향과 개선 제안, 확인한 계산, 독자에게 밝혀야 할 한계가 포함된다.
실제 오류나 참가 비율을 이 글에서 산출하지 않았으며, 자료를 읽는 지침이 어떻게 결과 검토로 이어지는지 보여주는 사례다.[2]

## 파일 기반 확장의 장점과 전제

루트 README는 구성 요소가 Markdown·JSON이고 별도의 빌드나 자체 인프라가 필요 없다고 설명한다.
이것은 플러그인 소스를 컴파일하지 않는다는 의미이지 Claude 실행 환경이나 연결 서비스, 데이터 접근 권한이 불필요하다는 뜻은 아니다.
MCP는 에이전트와 외부 도구를 연결하는 규약이며 실제 읽기·질의 권한은 연결별로 확인해야 한다.[1][3]

기존 초안이 지적한 이름 차이도 수집본에 남아 있다.
data README의 명령 표와 예제에는 `/validate`, 대표 Skill에는 `name: validate-data`와 `/validate-data`가 적혀 있다.
이 둘을 임의로 같은 호출이라고 확정하지 않고 실제 설치에서 노출되는 명령을 확인해야 한다.
이 글에서는 설치 시험을 하지 않았으므로 하나가 작동한다고 단정하지 않는다.[3][2]

검증 지침의 체크리스트 역시 모든 상황의 통계 법칙으로 받아들이면 안 된다.
문제 정의와 자료의 성격을 먼저 읽어야 하며, 인과관계·선택 편향·시간대·불완전 기간 비교 같은 함정이 무엇을 뜻하는지 살펴야 한다.
원자료에 접근할 수 없는 분석은 재계산 범위가 줄어든다.
도구가 작성한 신뢰 평가와 독립적인 사실 확인은 같지 않다.[2]

## 직접 읽어볼 자료

1. [루트 README의 How Plugins Work](https://github.com/anthropics/knowledge-work-plugins/blob/main/README.md)
   자동 참고하는 Skill, 직접 호출하는 command, 도구를 잇는 connector를 나누어 읽는다.
   기본 플러그인이 출발점이라는 설명을 보면 설정 파일을 가져오는 것과 분석 기준을 정하는 일의 차이가 드러난다.
2. [Data Analyst Plugin README](https://github.com/anthropics/knowledge-work-plugins/blob/main/data/README.md)
   연결이 있을 때와 없을 때의 입력 경로를 비교한다.
   예시 결과 숫자를 실제 측정 결과로 받아들이지 말고 SQL·파일·대시보드가 어떤 순서로 이어지는지를 따라가는 자료다.
3. [validate-data Skill](https://github.com/anthropics/knowledge-work-plugins/blob/main/data/skills/validate-data/SKILL.md)
   질문·모집단·지표 정의부터 읽고 Join Explosion과 기간 비교 함정으로 이동한다.
   마지막 보고서 형식에서 검증한 계산과 전달해야 할 한계가 어디에 놓이는지도 함께 확인한다.

## 정리

knowledge-work-plugins는 역할별 지침과 도구 접근을 함께 묶되 구체적인 맥락에 맞게 조정하는 확장 자료다.
데이터 사례의 핵심은 분석을 빨리 만드는 것보다 어떤 질문·집계·결론을 확인했는지 드러내는 데 있다.

## 자료 확인 범위

2026-09-27의 루트 README, data README, validate-data 지침을 확인했다.
명령명 차이를 보존했으며 플러그인 설치, 데이터 질의, 분석 계산은 실행하지 않았다.

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

- [anthropics/financial-services](anthropics-financial-services.md)

## Sources

[1] anthropics/knowledge-work-plugins — README.md

<https://github.com/anthropics/knowledge-work-plugins/blob/main/README.md>

[2] anthropics/knowledge-work-plugins — data/skills/validate-data/SKILL.md

<https://github.com/anthropics/knowledge-work-plugins/blob/main/data/skills/validate-data/SKILL.md>

[3] anthropics/knowledge-work-plugins — data/README.md

<https://github.com/anthropics/knowledge-work-plugins/blob/main/data/README.md>
