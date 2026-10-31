---
title: "danyuchn/asd-ste100-skill"
repository: "danyuchn/asd-ste100-skill"
url: "https://github.com/danyuchn/asd-ste100-skill"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# danyuchn/asd-ste100-skill

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

영문 도구 설명·오류 메시지·에이전트 간 지시에서 모호한 표현을 줄이는 Claude Code용 Skill이다.
새 언어 모델이나 항공 문서 인증 도구가 아니라, Simplified Technical English의 문장 원칙을 빌려 온 작성 지침과 구조 검사기다.[1]

## 짧은 문장보다 중요한 것은 의미 보존

한 문장에 실행 조건과 예외, 후속 행동이 함께 있으면 다음 시스템이 어느 부분을 명령으로 읽어야 하는지 불분명해진다.
이 프로젝트는 한 단어를 같은 뜻으로 쓰고, 주체를 드러내며, 여러 지시를 나누는 방식으로 이 문제를 다룬다.[1]
다만 “실패했을 수 있다”를 “실패했다”로 바꾸면 글은 짧아져도 내용은 달라진다.
`SKILL.md`는 가능성 표현, 수치, 안전 조건과 적용 범위를 보존하고, 필요한 정밀성을 잃는다면 긴 표현을 남기도록 한다.[2]

## 지침과 검사기가 맡는 일이 다르다

`SKILL.md`는 절차·오류·도구 설명에 쓰는 Strict와 설명문에 쓰는 STE-flavored 모드를 구분한다.
먼저 원문 의미를 읽고 문장별 위반을 찾은 뒤 고치며, 기본 출력은 수정문만이다.
일부 표현을 유지했다면 `Kept as-is:`로 이유를 덧붙이고, 사용자가 요청할 때만 규칙별 전후 비교를 낸다.[2]

`scripts/ste-lint.py`는 Python 표준 라이브러리와 정규식으로 세미콜론, 긴 문장, 일부 표현 패턴 등을 찾는다.
검사 결과를 hard와 advisory로 구분하며 `--baseline`은 허용할 hard 위반 수를 지정한다.
수동태나 복합 시제처럼 문맥에 따라 필요한 항목은 advisory이며, 가능성 표현 자체는 제거 대상으로 삼지 않는다.[3]
검사기가 원문과 수정문을 비교해 의미가 같은지 증명하는 것은 아니다.
위반이 없다는 결과와 “원래 요구사항을 보존했다”는 판단은 구분해야 한다.[1][3]

## 가상 예시: 도구 설명을 다음 에이전트에 넘길 때

충돌 처리 도구의 설명에 자동 해결 조건과 수동 검토 조건이 섞여 있다고 가정하자.
사용자는 그 영문 설명을 주고 모호성을 줄여 달라고 요청한다.
지침은 상태 동기화, 충돌 발견, 설정 확인, 자동 해결 가능 여부, 수동 검토를 별도 문장으로 풀도록 돕는다.[1][2]
결과는 실행 코드가 아니라 다시 사용할 영문 설명이다.
검토자는 자동 처리가 “가능하다”는 조건을 “항상 한다”로 바꾸지 않았는지 원문과 대조한다.
이는 이해를 위한 시나리오이며 실제 재작성이나 검사 실행 결과는 아니다.

## 사용 전 확인할 경계

공식 ASD 승인 어휘 사전은 재배포하지 않는다.
따라서 고정 어휘 준수나 항공·방위 산업 수준의 STE 인증을 보장하지 않으며, 창작·마케팅 문체도 목표가 아니다.[1][2]
검사기는 모든 구동사를 알지 못하고 품사 분석이 필요한 명사 묶음 길이도 검사하지 않는다.
형식이 개선되어도 원문 주장의 사실성까지 확보되지는 않는다.[1]
코드는 MIT 라이선스이지만 그것이 ASD 표준 원문을 재배포할 권리를 주는 것은 아니다.[1][4]

## 직접 읽어볼 자료

- [1] README의 What This Skill Does에서 가능한 검사와 불가능한 검사를 먼저 구분한다.
- [2] `SKILL.md`의 Workflow와 Output Format에서 의미 보존 조건과 결과 형식을 읽는다.
- [3] `scripts/ste-lint.py`의 규칙 목록과 `report`에서 경고 종류·종료 판단을 확인한다.
- [4] LICENSE는 코드 이용 조건이며, 공식 표준의 이용 조건과 별도라는 점을 확인한다.

## 자료 확인 범위

2026-10-28 공개 README·루트 구조·위에 인용한 코드 또는 문서를 확인했다.
설치·프로그램 실행·성능 측정은 하지 않았다.

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

[1] danyuchn/asd-ste100-skill — README.md

<https://github.com/danyuchn/asd-ste100-skill/blob/master/README.md>

[2] danyuchn/asd-ste100-skill — SKILL.md

<https://github.com/danyuchn/asd-ste100-skill/blob/master/SKILL.md>

[3] danyuchn/asd-ste100-skill — scripts/ste-lint.py

<https://github.com/danyuchn/asd-ste100-skill/blob/master/scripts/ste-lint.py>

[4] danyuchn/asd-ste100-skill — LICENSE

<https://github.com/danyuchn/asd-ste100-skill/blob/master/LICENSE>
