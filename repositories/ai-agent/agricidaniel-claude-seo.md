---
title: "AgriciDaniel/claude-seo"
repository: "AgriciDaniel/claude-seo"
url: "https://github.com/AgriciDaniel/claude-seo"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# AgriciDaniel/claude-seo

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude Code가 웹사이트의 검색 노출 문제를 조사하고 근거·우선순위가 있는 보고서를 만들도록 하는 SEO Skill·도구 모음이다.[1]

## 순위를 올려 주는 모델보다 감사 절차

Claude SEO는 Claude Code 안에서 웹사이트의 기술적 SEO, 콘텐츠, 구조화 데이터와 AI 검색 대응을 조사하는 플러그인이다.
SEO는 검색 엔진이 페이지를 발견하고 내용을 이해하도록 돕는 작업을 말한다.
프로젝트는 여러 전문 Skill과 하위 에이전트, Python 도구를 조합해 감사 결과와 실행 계획을 작성하도록 한다.[1]

단순히 검색 키워드를 추천하는 데 그치지 않고 크롤링 가능성, 사이트맵, 속도와 콘텐츠의 근거를 함께 다룬다.
구조화 데이터는 페이지의 대상·종류를 기계가 해석할 수 있게 표시하는 정보다.
README의 순위 상승·소요 시간·자체 사이트 성과는 프로젝트 측 설명이며 이번 조사에서 재현하거나 인과관계를 검증하지 않았다.[1]

## 전체 사이트 감사의 구체적인 산출물

대표 `skills/seo-audit/SKILL.md`는 홈페이지를 렌더링해 원본 HTML·렌더링된 HTML·추출 텍스트 등을 확보하도록 한다.
그다음 사이트 유형을 파악하고 내부 링크를 따라 조사하며, 기술·콘텐츠·구조화 데이터·속도 등으로 일을 나눈다.
하위 에이전트를 사용할 수 없으면 순차 실행하도록 되어 있어 병렬 실행만이 유일한 경로는 아니다.[2]

크롤링 설정에는 최대 500페이지, robots.txt 존중, 리다이렉트와 시간 제한이 명시되어 있다.
이는 도구가 따라야 할 조사 범위이지 이 초안에서 실제로 검사한 페이지 수가 아니다.
Google API나 특정 외부 데이터 자격 증명이 있을 때만 추가 분석을 여는 조건도 존재한다.[2]

산출물은 `{domain}-audit/` 아래의 종합 보고서, 우선순위 계획, `audit-data.json`과 분야별 findings다.
구조화된 JSON은 보고서 생성에 쓰는 공통 봉투 역할을 하며, PDF 출력은 별도 단계다.
단일 점수 외에 무엇을 발견했고 어떤 수정을 먼저 해야 하는지 남기도록 구성되어 있다.[2]

## 가상 사례: 개인 기술 블로그 점검

입력은 자신의 공개 기술 블로그 주소와 전체 사이트 감사 요청이라고 가정한다.
홈페이지와 내부 링크를 조사하고, 사이트맵·본문·이미지·구조화 데이터의 관찰 결과를 분야별로 모은다.
예상 산출물은 종합 보고서와 우선순위가 매겨진 개선 계획, 후속 보고서용 JSON이다.[1][2]

예를 들어 페이지의 제목과 내용이 맞지 않는다는 관찰이 있으면, 어떤 URL과 내용을 보았는지 근거를 남기는 식이다.
이 문장의 문제 상황은 가상이며 실제 사이트에서 발견한 오류가 아니다.
보고서 생성과 검색 엔진 순위 변화는 별개의 결과이므로 “점수 개선”을 방문자 증가로 바로 바꾸어 읽으면 안 된다.

## 실행 환경과 근거의 빈칸

코드 라이선스는 MIT다.[3]
상위 `skills/seo/SKILL.md`는 개별 명령에 맞는 하위 Skill을 고르고 Python 도구는 전용 런처를 통해 호출하도록 한다.
환경 준비는 사용자가 명시적으로 setup이나 의존성 복구를 요청한 경우에만 하며, 진단용 doctor와 설치를 구분한다.[4]

감사 권고에는 관찰 근거, 다른 수정과의 선후 관계, 실패했음을 알 방법과 추적 지표를 포함하도록 한다.
이는 경고를 길게 나열하는 대신 검증 가능한 개선안을 만들려는 지침이다.
그 지침이 존재해도 실제 보고서의 근거가 충분한지는 사람이 확인해야 한다.[4]

Claude Code 이용과 선택 API의 비용·인증 조건은 플러그인 코드의 무료 이용과 다르다.
README에는 FLOW 프레임워크에 CC BY 4.0 표기가 있어, 외부 프레임워크까지 모두 MIT라고 뭉뚱그리면 안 된다.[1]
실시간 검색·분석 데이터가 없는 경우와 수집 실패를 정상 점수로 채우지 않는지도 실제 도입 때 살펴볼 부분이다.

## 직접 읽어볼 자료

1. [README의 How an audit works·Pricing](https://github.com/AgriciDaniel/claude-seo/blob/main/README.md)
   보고서 형식과 Claude Code·외부 데이터 비용을 구분한다.
2. [전체 사이트 감사 Skill](https://github.com/AgriciDaniel/claude-seo/blob/main/skills/seo-audit/SKILL.md)
   조사 제한과 분야별 위임 조건, 실제 출력 파일 계약을 읽는다.
3. [상위 SEO Skill](https://github.com/AgriciDaniel/claude-seo/blob/main/skills/seo/SKILL.md)
   자동 설치를 피하는 환경 준비 규칙과 권고의 근거 요구를 확인한다.

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

[1] AgriciDaniel/claude-seo — README.md

<https://github.com/AgriciDaniel/claude-seo/blob/main/README.md>

[2] AgriciDaniel/claude-seo — skills/seo-audit/SKILL.md

<https://github.com/AgriciDaniel/claude-seo/blob/main/skills/seo-audit/SKILL.md>

[3] AgriciDaniel/claude-seo — LICENSE

<https://github.com/AgriciDaniel/claude-seo/blob/main/LICENSE>

[4] AgriciDaniel/claude-seo — skills/seo/SKILL.md

<https://github.com/AgriciDaniel/claude-seo/blob/main/skills/seo/SKILL.md>

[5] AgriciDaniel/claude-seo — 공식 루트 트리

<https://github.com/AgriciDaniel/claude-seo/tree/main>
