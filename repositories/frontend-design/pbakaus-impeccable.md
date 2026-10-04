---
title: "pbakaus/impeccable"
repository: "pbakaus/impeccable"
url: "https://github.com/pbakaus/impeccable"
category: "frontend-design"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "frontend-design"
  - "starred-draft"
---

# pbakaus/impeccable

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Impeccable은 AI 코딩 도우미가 화면을 설계·검토·수정할 때 읽는 지침과, 코드·브라우저 화면의 문제를 검사하는 실행 도구를 함께 제공한다.
완성된 UI 부품 모음이나 독립적인 AI 모델이 아니라 기존 코딩 도우미의 프런트엔드 작업 방식을 확장하는 프로젝트다.[1][2]

## 예쁜 화면보다 먼저 정리하는 것

같은 요청도 “처음 방문한 사람에게 소개하는 화면”과 “이미 쓰는 도구의 설정 화면”에서는 다른 답이 필요하다.
Impeccable은 제품이 누구를 위한 것인지와 화면을 어떤 모습으로 만들 것인지를 분리한다.
`PRODUCT.md`에는 사용자·목적·제약·확인된 근거를, `DESIGN.md`에는 기존 또는 새 시각 체계의 기록을 둔다.[1][3]

Skill은 AI가 읽고 따르는 작업 절차 문서다.
실제 Skill은 세션 초기에 `impeccable context`로 제품·디자인 문맥과 대상 화면의 안내를 불러온 뒤, 요청에 맞는 세부 문서를 읽도록 한다.
명시된 제품 요구가 유행하는 디자인 경고와 충돌하면 요구를 우선하며, 작은 다듬기와 전면 재설계도 구분한다.
지침이 있다는 사실 자체가 AI의 준수나 결과 품질을 보증하는 것은 아니다.[2]

방문자의 목적도 화면별로 나눈다.
설득과 행동을 위한 Persuade, 과업 수행을 위한 Operate, 이해를 위한 Read, 작품 경험을 위한 Experience가 있다.
제품이 개발 도구라도 소개 페이지와 사용 설명서는 서로 다른 모드일 수 있다는 관점이다.
이는 모델 종류를 고르는 옵션이 아니라 화면 판단에 사용할 관점이다.[2]

## 문서, 검사 엔진, 브라우저의 역할

명령 목록에는 `init`, `shape`, `audit`, `critique`, `polish`, `live` 등이 있지만 모든 명령이 같은 일을 하는 것은 아니다.
`audit`는 구현에서 확인 가능한 접근성·성능·반응형 동작 등을 보고서로 남기고, `critique`는 사용자 경험과 시각적 판단에 초점을 둔다.
`init`는 제품 사실을 수집하며 색상이나 글꼴을 결정하는 단계가 아니다.[1][4][3]

실행 부분은 Rust로 만든 엔진과 이를 찾거나 내려받는 launcher로 구성된다.
엔진 문서는 문맥 처리, hook, live mode, 정적 HTML, 브라우저 검사, 공통 규칙을 별도 구성요소로 나눈다.
브라우저용 규칙은 WebAssembly, 즉 브라우저에서 실행할 수 있는 형태로도 빌드된다.
따라서 이 저장소를 프롬프트 파일만 모아 둔 자료로 설명하면 실제 구조를 놓친다.[5]

README에 따르면 정해진 규칙을 적용하는 detector는 LLM이나 API 키 없이 동작한다.
반면 제품 요구를 해석하고 시각적 판단을 내리며 코드를 바꾸는 전체 작업은 사용하는 AI 도우미와 그 도구에 의존한다.
검사기의 API 키 불필요 조건을 전체 디자인 작업의 무비용 보장으로 넓혀 읽으면 안 된다.[1][2]

## 예시로 따라가는 흐름

README의 `/impeccable audit blog`를 이미 있는 블로그 화면에 적용한다고 가정하자.
실행 사례가 아니라 공식 명령과 audit 지침을 따라 설명한 상황이다.
제품 문서가 필요한 초기 설정에서는 기존 코드와 문서를 먼저 살피고, 빠진 사용자·목적·제약만 질문한다.
새 `PRODUCT.md`를 작성하기 전에 실제 답변이나 승인을 받도록 하며, 추측한 고객·가격·성과를 채워 넣지 못하게 한다.[1][3]

audit 요청에서는 대상 구현을 읽고 접근성, 성능, 테마, 반응형 화면, 구현 일관성을 나누어 검사한다.
예를 들어 모바일 화면에서 가로 넘침이 생기거나 입력칸 이름표가 빠졌다면, 파일·컴포넌트 위치와 사용자에게 미치는 영향, 심각도, 권장 수정 방향을 함께 적는 방식이다.
이 예의 결함이 실제 블로그에 있다는 뜻은 아니며 지침이 요구하는 보고 형태를 풀어 쓴 것이다.[4]

검사기는 발견 사항을 제공하지만 문맥상 오탐인지 다시 확인해야 한다.
audit의 결과물은 우선순위가 붙은 보고서이며, 이 단계에서 곧바로 문제를 고치지 말라고 명시한다.
사람이 범위와 권장 조치를 검토한 뒤 별도의 수정 요청으로 넘어가는 흐름이다.
또 터치 제스처는 작은 화면의 스크린샷만으로 검증했다고 할 수 없어, 실제 어떤 입력과 브라우저로 확인했는지, 무엇을 미검증으로 남겼는지도 보고하도록 한다.[4]

## 자동 검사는 인증이 아니다

detector의 성공 종료는 요청한 검사를 수행했으며 주요 발견이 없었다는 뜻이지, 접근성 준수나 디자인 완성의 증명은 아니다.
README는 렌더링된 화면을 여러 크기로 직접 살펴보는 검증을 대체하지 않는다고 명시한다.
URL 검사에서는 설치된 Chrome 계열 브라우저를 사용하며, 다른 출처의 CSS를 읽는 데에도 브라우저 보안 제한이 남는다.[1]

hook은 파일 편집 같은 사건에 반응해 자동 실행하는 연결 장치다.
지원 도구에 따라 편집 후 결과를 알리거나 편집 전 쓰기를 막는 방식이 다르다.
특히 README는 Claude Code의 설치된 command hook이 모델 도구 승인과 별개로 실행되어 엔진을 내려받을 수 있다고 경고한다.
따라서 Skill 문서뿐 아니라 설치되는 hook과 로컬 쓰기 범위를 함께 확인해야 한다.[1]

Live mode는 로컬 소스와 개발 서버를 대상으로 하며 배포된 운영 사이트에 helper를 주입하는 경로는 지원하지 않는다.
복사 문구 수정 적용 시 `package.json`의 선택적 검증 스크립트를 사용자 권한으로 실행할 수 있다고 안내한다.
낯선 프로젝트를 실행하기 전에 이 스크립트를 읽어야 하며, 동작시키려고 운영 사이트의 보안 정책을 약화시키면 안 된다.[1]

## 라이선스·비용·문서 차이

프로젝트의 라이선스는 Apache-2.0이며 재배포 시 라이선스 사본, 변경 고지, 관련 저작권·기여 고지 등의 조건이 있다.[6]
`NOTICE.md`는 iOS·Android 참고 문서가 MIT 조건의 제삼자 자료에서 파생되었다고 밝힌다.
따라서 전체를 출처 표시 없이 재사용해도 된다는 뜻은 아니다.[7]

읽은 자료만으로 AI 서비스의 요금이나 이미지 생성 비용을 확정하지 않는다.
엔진·규칙 공개와 코딩 도우미 이용료는 별개이며, 이미지부터 만드는 경로와 코드부터 만드는 경로도 설정에서 구분한다.[1][3]
같은 커밋 안에서도 README는 `craft`를 전체 제작 흐름으로 소개하지만 실제 Skill은 일반 새 작업의 폐기 예정 별칭으로 표시한다.
이 문서에서는 명확히 구현 지침이 남은 `init`와 `audit`를 대표 흐름으로 삼았다.[1][2]

## 직접 읽어볼 자료

1. [README](https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/README.md)

   설치 항목보다 먼저 제품 문서와 명령의 역할을 읽는다.
   이어 Design hook, Live mode, CLI 항목에서 자동 실행·운영 사이트 제한·검사 성공의 의미를 확인한다.
2. [Skill 원본](https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/skill/SKILL.src.md)

   문맥 로딩과 명령별 참고 문서 선택을 따라가며 작은 수정과 재설계를 구분한다.
   README의 명령 소개와 실제 라우팅 지침이 일치하는지도 살펴본다.
3. [audit 지침](https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/skill/reference/audit.md)

   점수보다 먼저 발견 위치·사용자 영향·오탐 검증·미확인 입력을 어떻게 기록하는지 읽는다.
   보고와 수정이 별도 단계라는 경계가 구체적으로 드러난다.

## 자료 확인 범위

2026-10-04 기준 고정 커밋 `e103efe779e2dd01274dabae83531fef00bf2563`의 README, Skill 원본, init·audit 지침, 엔진 구성 설명, LICENSE와 NOTICE를 읽었다.
설치, 프로젝트 실행, hook 등록, 브라우저 검사, 디자인 변경과 성능 검증은 하지 않았다.

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

[1] pbakaus/impeccable — README.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/README.md>

[2] pbakaus/impeccable — skill/SKILL.src.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/skill/SKILL.src.md>

[3] pbakaus/impeccable — skill/reference/init.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/skill/reference/init.md>

[4] pbakaus/impeccable — skill/reference/audit.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/skill/reference/audit.md>

[5] pbakaus/impeccable — docs/ENGINE.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/docs/ENGINE.md>

[6] pbakaus/impeccable — LICENSE

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/LICENSE>

[7] pbakaus/impeccable — NOTICE.md

<https://github.com/pbakaus/impeccable/blob/e103efe779e2dd01274dabae83531fef00bf2563/NOTICE.md>
