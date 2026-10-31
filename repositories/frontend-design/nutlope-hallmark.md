---
title: "Nutlope/hallmark"
repository: "Nutlope/hallmark"
url: "https://github.com/Nutlope/hallmark"
category: "frontend-design"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["frontend-design", "starred-draft"]
---

# Nutlope/hallmark

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

코딩 에이전트가 웹 페이지의 디자인 방향을 고르고 브라우저 검사를 거치도록 하는 디자인 Skill이다.[1]

## 예쁜 화면보다 반복되는 기본값을 다룬다

Hallmark는 Claude Code·Codex·Cursor·OpenCode 등에서 쓰는 디자인 지침과 보조 스크립트 묶음이다.
새 디자인 모델이나 완성된 웹사이트 빌더가 아니라, 코딩 에이전트가 화면을 만들고 검토하는 방법을 규정하는 Skill이다.
Skill은 특정 요청에 맞춰 불러오는 작업 지침이라는 뜻이다.[1]

README는 생성형 화면이 비슷한 제목·장식·카드 배치로 수렴하는 문제를 다룬다.
페이지의 목적에서 레이아웃을 정하고, 테마는 색·글꼴·표면 같은 시각적 재료로 사용한다.
테마가 페이지의 내용이나 섹션 순서를 고정하지 않는다는 구분이 핵심이다.[1]

## 지침이 강제하는 것과 선택하는 것

대표 `SKILL.md`는 색·글꼴 토큰, 키보드 초점, 모션 감소 설정, 좁은 화면 대응 등을 Floor 규칙에 둔다.
토큰은 색상이나 간격처럼 여러 곳에서 공유할 값을 이름으로 정의한 것이다.
꾸며낸 지표·추천사·로고를 쓰지 말라는 사실성 규칙도 디자인 조건과 함께 들어 있다.[2]

작업 전에는 기존 `design.md`·`DESIGN.md`, 프로젝트 글꼴과 색상, 실제 제품 자료를 먼저 읽는다.
대상·사용 목적·분위기·사용 가능한 사진 등을 확인한 뒤, 첫 화면을 먼저 만들고 전체 페이지로 확장한다.
기존 디자인 변경에는 명시적인 Hallmark 지명이 필요하며, `audit`는 보고, `redesign`은 콘텐츠와 연결을 보존하는 재설계로 구분한다.[2]

README의 마지막 검토는 브라우저 기반 `sloplint --render`와 새로운 맥락의 검토자를 포함한다.
Floor는 넘을 수 없는 기준, Reflex는 이유를 남겨 예외를 둘 수 있는 기본 경향, Finish는 마무리 품질 점검으로 나눈다.
다만 규칙이 존재한다는 사실과 실제 생성물이 그 규칙을 모두 지켰다는 것은 별도로 확인해야 한다.[1][2]

## 가상 사례: 공개 사진 전시 페이지

입력은 직접 촬영한 사진, 전시 소개 문구, 방문자가 해야 할 행동과 원하는 분위기라고 가정한다.
Hallmark 지침을 따르는 에이전트는 기존 디자인 체계를 읽고, 사용할 시각 방향을 정한 뒤 첫 화면을 만든다.
그다음 나머지 내용을 구성하고 여러 화면 폭에서 검사하는 흐름이다.[1][2]

예상 산출물은 HTML·CSS 페이지와 선택한 방향, 검토 결과가 담긴 인계 내용이다.
이는 생성·브라우저 실행을 하지 않은 가상 예시다.
예약이나 신청 폼이 있다면 실제 전송이 되는지 확인해야 하며, 성공 안내 문구만 뜨는 화면을 완성된 기능으로 보면 안 된다.[1]

## 도구 의존성과 사진의 권리

README는 Node.js가 필요하고, 브라우저 검사는 Chrome과 `puppeteer-core`도 요구한다고 설명한다.
브라우저 준비가 없으면 정적 검사는 가능하지만 렌더링 검증과 동등하지 않다.
호스트별 호출 방식과 기존 프로젝트 디자인 규칙도 확인해야 한다.[1][2]

코드 라이선스는 MIT다.[3]
하지만 사진 목록이 가리키는 Wikimedia Commons 사진은 각자의 CC0·퍼블릭 도메인·CC BY·CC BY-SA 조건을 따른다.
저장소가 MIT라고 이미지까지 같은 조건으로 바뀌지 않으며, 실제 페이지의 출처 표기와 재사용 조건을 따로 확인해야 한다.[1]

“AI가 만든 것처럼 보이지 않는다”는 표현은 프로젝트의 목표이지 객관적 품질 인증은 아니다.
정해진 미감 규칙을 적용하는 것과 특정 독자가 정보를 잘 찾는지는 다른 검토 문제다.

## 정리

Hallmark는 테마 선택만이 아니라 사전 조사·첫 화면 검토·브라우저 검사를 묶은 제작 절차다.
자료의 진실성, 기존 연결 보존, 코드와 사진의 다른 라이선스를 함께 읽으면 역할이 더 분명해진다.

## 직접 읽어볼 자료

1. [README의 How a build runs·Licence](https://github.com/Nutlope/hallmark/blob/main/README.md)
   제작 순서와 사진별 별도 권리를 함께 확인한다.
2. [Hallmark SKILL.md](https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md)
   Floor 규칙과 기존 디자인에 개입할 수 있는 조건부터 읽는다.

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

[1] Nutlope/hallmark — README.md

<https://github.com/Nutlope/hallmark/blob/main/README.md>

[2] Nutlope/hallmark — skills/hallmark/SKILL.md

<https://github.com/Nutlope/hallmark/blob/main/skills/hallmark/SKILL.md>

[3] Nutlope/hallmark — LICENSE

<https://github.com/Nutlope/hallmark/blob/main/LICENSE>

[4] Nutlope/hallmark — 공식 루트 트리

<https://github.com/Nutlope/hallmark/tree/main>
