---
title: "VoltAgent/awesome-design-md"
repository: "VoltAgent/awesome-design-md"
url: "https://github.com/VoltAgent/awesome-design-md"
category: "frontend-design"
created: "2026-09-06"
status: "starred"
star_reason: "AI Agent에게 디자인 기준을 전달하는 방법과 DESIGN.md 활용 방식을 공부하기 위해"
tags:
  - ai-agent
  - design-system
  - design-md
  - frontend
  - ui-ux
---

# VoltAgent/awesome-design-md

> https://github.com/VoltAgent/awesome-design-md

## 내가 이 Repository를 Star한 이유

개인 프로젝트나 팀 프로젝트에서 UI를 맡을 때마다 디자인이 고민이었다.
UI를 구현하는 데 필요한 기본적인 지식은 있지만, 디자인 감각이나 사용자가 좋아할 만한 화면을 만드는 데에는 부족한 부분이 많다고 느끼고 있다.

AI Agent를 사용하면서 개발 속도는 빨라졌지만, 아무런 기준 없이 UI를 만들어달라고 하면 결과가 서로 비슷하게 나오는 경우가 많았다.
다른 사람이 AI로 만든 UI에서도 비슷한 Layout, 색상, Component 구성이 반복되면 "AI가 만든 디자인 같다"는 생각이 들 때가 있었다.
디자인 Reference를 Agent에게 주는 방법도 사용해봤지만, Reference와 비슷한 형태를 만들 뿐 프로젝트 전체의 디자인 기준까지 잡히지는 않았다.

그러던 중 다른 개발자가 디자인 관련 Skill과 최신 디자인 Reference를 Agent에게 제공해 UI를 만드는 것을 보면서, Agent에게 디자인을 전달하는 방법 자체에 관심이 생겼다.
지난 글에서 Agent Skill 자체를 살펴봤다면, 이번에는 그 개념이 디자인 작업에서 어떻게 활용될 수 있는지가 궁금했다.

그 과정에서 GitHub의 `VoltAgent/awesome-design-md`를 발견했다.
많은 관심을 받은 Repository였지만, 단순히 인기가 높아서 관심을 가진 것은 아니었다.
이 Repository는 디자인 Skill 모음이 아니라 여러 유명 서비스의 디자인 특징을 Agent가 읽을 수 있는 `DESIGN.md`로 정리한 Collection이다.

디자인 감각을 Agent에게 전부 맡기려는 것은 아니다.
**좋은 디자인은 어떤 규칙으로 만들어지고, 그 규칙을 Agent에게 어떻게 전달하면 좋은가?** 이 질문을 공부해보고 싶어서 이 Repository를 Star했다.

## 한 줄 요약

`VoltAgent/awesome-design-md`는 여러 공개 웹사이트의 디자인 특징을 AI Agent가 읽을 수 있는 `DESIGN.md`로 정리한 디자인 분석 문서 모음이다.[1]

## 이 Repository는 무엇인가?

이 Repository는 UI Framework나 Component Library가 아니다.

Claude, Linear, Airbnb, Stripe처럼 잘 알려진 웹사이트의 색상, Typography, 간격, Component 형태, 반응형 규칙을 분석해 Markdown 문서로 제공한다.[1]

각 디자인 폴더는 이해하기 쉬운 구조로 되어 있다.[2]

```text
design-md/
├── claude/
│   ├── DESIGN.md
│   └── README.md
├── linear.app/
│   ├── DESIGN.md
│   └── README.md
└── ...
```

실제로 실행되는 Button이나 Card 코드, Figma Library, 이미지 Asset을 제공하지는 않는다.

대신 AI Agent가 현재 프로젝트의 기술에 맞춰 UI를 만들 때 참고할 **시각적 규칙**을 제공한다.

| 제공하는 것 | 제공하지 않는 것 |
| --- | --- |
| 색상과 Typography Token | 완성된 React·Vue Component |
| Layout과 간격 원칙 | Figma Component Library |
| Button, Card, Input의 모양 | Logo와 이미지 Asset |
| 반응형 화면 지침 | 디자인 결과의 자동 검증 |
| 지켜야 할 규칙과 피해야 할 표현 | 각 브랜드의 공식 디자인 시스템 |

## DESIGN.md는 무엇인가?

`DESIGN.md`는 프로젝트의 UI가 **어떤 모습과 분위기를 가져야 하는지** 사람과 AI Agent가 함께 읽을 수 있도록 기록한 문서다.

Google Stitch는 이를 `AGENTS.md`에 대응하는 디자인 문서로 소개한다.[3]

하나의 `DESIGN.md`에는 두 종류의 정보가 함께 들어간다.[3][4]

```text
┌──────────────────────────────────────┐
│ YAML Front Matter                    │
│ 색상, 글꼴, 간격처럼 정확한 값       │
└──────────────────┬───────────────────┘
                   │
                   ▼
┌──────────────────────────────────────┐
│ Markdown 본문                        │
│ 값의 사용 목적과 전체 디자인 원칙    │
└──────────────────────────────────────┘
```

### 정확한 값을 담는 YAML Front Matter

문서 앞부분에는 Design Token이 들어간다.

Design Token은 여러 화면에서 반복해서 사용하는 색상, 글자 크기, 간격 같은 값을 이름으로 정리한 것이다.

```yaml
colors:
  primary: "#5e6ad2"
  canvas: "#010102"

typography:
  body-md:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: 400

rounded:
  md: 8px
```

예를 들어 Agent가 `primary`라는 이름을 계속 사용하면 화면마다 강조색이 달라지는 문제를 줄일 수 있다.

### 사용 이유를 설명하는 Markdown 본문

정확한 값만으로는 어떤 상황에서 무엇을 사용해야 하는지 알기 어렵다.

본문은 다음 내용을 자연어로 설명한다.

- 전체적인 분위기와 시각적 성격
- 색상을 사용하는 상황
- 제목과 본문의 크기 관계
- 화면의 최대 너비와 여백
- Button, Card, Input의 형태
- 그림자와 테두리 사용법
- Mobile 화면에서 Layout이 바뀌는 방식
- 지켜야 할 규칙과 피해야 할 표현

공식 Specification은 `Overview`, `Colors`, `Typography`, `Layout`, `Elevation & Depth`, `Shapes`, `Components`, `Do’s and Don’ts`를 기본 Section으로 제시한다.[4]

## 일반적인 디자인 시스템과 무엇이 다른가?

일반적인 디자인 시스템은 Designer와 개발자가 실제 제품을 함께 만들고 운영하기 위한 전체 체계다.

Figma Component, Design Token, 구현 코드, 접근성 지침, 문서와 변경 절차까지 포함할 수 있다.

`DESIGN.md`는 그보다 범위가 좁다.

디자인 시스템 자체라기보다 디자인 원칙을 Agent에게 전달하기 위한 **텍스트 형태의 디자인 계약서**에 가깝다.

| 구분 | 일반적인 디자인 시스템 | DESIGN.md |
| --- | --- | --- |
| 주요 독자 | Designer와 개발자 | 사람과 AI Agent |
| 주요 형태 | Figma, Component 코드, 문서, Token | Markdown과 YAML |
| Button 제공 방식 | 재사용 가능한 실제 Component | Button이 보여야 할 모습을 설명 |
| 적용 방식 | Package 설치 또는 Component 사용 | Agent가 읽고 현재 기술로 구현 |
| 일관성 유지 | 구현된 Component가 규칙을 강제 | Agent의 해석과 검증에 좌우됨 |

```text
일반적인 디자인 시스템
원칙 + Token + Figma + 구현 코드 + 운영 방식

DESIGN.md
Agent가 읽을 수 있는 Token + 디자인 설명
```

따라서 `DESIGN.md`를 프로젝트에 추가했다고 UI가 자동으로 완성되는 것은 아니다.

Agent가 이 문서를 읽고 React, Vue, CSS, Tailwind CSS 같은 현재 프로젝트 기술에 맞춰 구현해야 한다.

## AI Agent는 DESIGN.md를 어떻게 사용하는가?

전체 흐름을 먼저 보면 다음과 같다.

```text
UI 작업 요청
    ↓
DESIGN.md 읽기
    ↓
색상·Typography·간격 Token 확인
    ↓
Layout·Component·반응형 원칙 확인
    ↓
현재 프로젝트 기술로 UI 구현
    ↓
화면 확인 및 수정
    ↓
필요하면 DESIGN.md도 함께 개선
```

예를 들어 다음과 같이 요청할 수 있다.

```text
프로젝트 Root의 DESIGN.md를 읽고
기존 기술과 Component를 사용해 회원가입 화면을 만들어줘.

색상, Typography, 간격, Button 규칙을 따르되
원본 브랜드의 이름과 Logo는 사용하지 마.
```

Agent는 `DESIGN.md`의 값을 CSS Variable이나 Tailwind CSS Theme으로 옮기고, 본문의 규칙에 따라 화면과 Component를 만들 수 있다.

Google의 공식 CLI는 문서 구조와 Token 참조를 검사하고, Tailwind CSS 또는 W3C Design Token 형식으로 내보내는 기능도 제공한다.[5]

다만 모든 Coding Agent가 `DESIGN.md`라는 파일명을 자동으로 읽는다고 보기는 어렵다.

Repository도 파일을 복사한 뒤 Agent에게 사용하라고 명시적으로 요청하는 방식을 안내한다.[1]

일반 Coding Agent를 사용할 때는 UI 작업 전에 `DESIGN.md`를 읽으라고 직접 알려주는 편이 안전하다.

## 어떤 디자인을 제공하는가?

AI 서비스와 개발 도구부터 Productivity, 금융, 자동차, Retro Web까지 여러 분야의 디자인이 정리되어 있다.[1]

| 분야 | 예시 |
| --- | --- |
| AI와 개발 도구 | Claude, Cursor, Vercel, Sentry |
| Productivity와 디자인 | Linear, Notion, Figma, Webflow |
| 금융과 E-commerce | Stripe, Coinbase, Airbnb, Shopify |
| 소비자 서비스와 자동차 | Apple, Spotify, Uber, Tesla |
| Retro Web | Dell 1996, Nintendo.com 2001 |

### 색상만 바꾼 문서는 아니다

실제 파일을 비교하면 사이트마다 Layout과 Component 규칙도 달라진다.

| 디자인 | 눈에 띄는 특징 |
| --- | --- |
| Claude | 따뜻한 Cream 배경, Terracotta 강조색, Serif 제목, Editorial Layout[8] |
| Linear | 거의 검은 배경, Lavender-blue 강조색, 높은 정보 밀도[9] |
| Airbnb | 흰 배경, Pink 강조색, 둥근 검색창과 사진 중심 Card |
| Notion | Navy Hero 영역, Purple CTA, Pastel Feature Card |
| Sentry | 어두운 Purple 배경, Lime 강조색, 개발 도구에 맞춘 구성 |
| Dell 1996 | 초기 Web 특유의 Banner, 굵은 구분선, Sticker 형태 이미지[10] |

Dell 1996과 Nintendo.com 2001처럼 특정 시대의 Web 디자인을 정리한 문서도 있어 최신 SaaS 스타일과 다른 방향도 비교할 수 있다.

## AGENTS.md / SKILL.md / DESIGN.md는 어떻게 다른가?

세 파일은 모두 Agent가 읽는 Markdown이지만 서로 다른 질문에 답한다.

| 파일 | 답하는 질문 | 주요 내용 | 적용 범위 |
| --- | --- | --- | --- |
| `AGENTS.md` | 이 프로젝트에서 어떻게 작업해야 하는가? | Build, Test, Coding Convention, 수정 제한 | 프로젝트 전반 |
| `SKILL.md` | 이 작업을 어떤 절차로 수행해야 하는가? | 작업 단계, 도구, 검증 방법, Template | 특정 종류의 작업 |
| `DESIGN.md` | 결과물이 어떻게 보여야 하는가? | 색상, 글꼴, 간격, Layout, Component | UI와 시각적 결과 |

`AGENTS.md`는 Coding Agent에게 프로젝트의 Build 방법, Test 명령어, Coding Convention 같은 기본 작업 규칙을 제공한다.[6]

`SKILL.md`는 PDF 처리, 조사, 코드 리뷰처럼 특정 작업을 수행하는 방법을 재사용 가능한 형태로 담는다.

Agent는 작업과 관련된 Skill을 선택했을 때 전체 지침과 필요한 Script, Reference, Asset을 읽는다.[7]

세 파일을 함께 사용하면 다음과 같이 역할을 나눌 수 있다.

```text
사용자: 결제 화면을 만들어줘.

AGENTS.md
├─ 기존 Component 구조를 유지한다.
├─ pnpm을 사용한다.
└─ 수정 후 Test를 실행한다.

SKILL.md
├─ 기존 화면을 조사한다.
├─ 작은 단위로 구현한다.
└─ Screenshot과 접근성을 검증한다.

DESIGN.md
├─ Primary 색상을 정한다.
├─ Button과 Input의 모양을 정한다.
└─ Mobile Layout 규칙을 정한다.
```

서로 대체하는 파일이 아니라 **프로젝트 규칙, 작업 절차, 시각적 규칙**을 나눠 맡는 관계다.

## 실제로 어디에 사용할 수 있는가?

### 새 프로젝트의 첫 화면 만들기

빈 화면에서 색상과 글꼴을 모두 정하기 어렵다면 원하는 방향과 가까운 `DESIGN.md`를 출발점으로 사용할 수 있다.

```text
Linear의 DESIGN.md를 참고하되 그대로 복제하지 말고,
현재 프로젝트에 맞는 이름과 색상으로 바꿔서
개발 도구 Landing Page를 만들어줘.
```

### 여러 화면의 분위기 통일하기

Agent에게 화면을 하나씩 요청하면 Button 크기, Card 모서리, 여백이 화면마다 달라질 수 있다.

프로젝트 전용 `DESIGN.md`를 만들고 계속 사용하면 공통 기준을 제공할 수 있다.

### Design Token 초안 만들기

가져온 문서에서 색상과 Typography를 골라 다음 구현물로 바꾸도록 요청할 수 있다.

- CSS Custom Properties
- Tailwind CSS Theme
- Theme 객체
- 공통 Button, Card, Input Component

### 여러 디자인 방향 비교하기

같은 화면을 Claude, Linear, Retro Web 방향으로 각각 만들어 비교할 수 있다.

전체 Application보다 Header, Hero, Card처럼 작은 범위로 시작하면 차이를 확인하기 쉽다.

```text
DESIGN.md 선택
    ↓
작은 화면 또는 Component 구현
    ↓
두세 가지 방향 비교
    ↓
필요한 규칙만 선택
    ↓
프로젝트 전용 DESIGN.md 작성
```

## 직접 써보기 쉬운가?

시작 자체는 쉽다.

Repository가 안내하는 기본 절차는 원하는 `DESIGN.md`를 프로젝트 Root에 복사하고, AI Agent에게 사용하라고 요청하는 것이다.[1]

별도 Runtime이나 Framework도 필요하지 않다.

하지만 좋은 결과를 얻으려면 다음 작업은 직접 확인해야 한다.

- 기존 CSS 및 Component 구조와 충돌하지 않는지
- 문서에 나온 Font를 실제로 사용할 수 있는지
- 필요한 Logo, 사진, Illustration이 준비되어 있는지
- Mobile과 Desktop Layout이 모두 자연스러운지
- 색상 대비와 Keyboard 조작 등 접근성에 문제가 없는지
- Agent가 문서의 규칙을 실제 코드에 지켰는지

따라서 **사용을 시작하기는 쉽지만, 결과 검증까지 자동으로 끝나는 도구는 아니다.**

## 디자인이 익숙하지 않은 개발자에게 어떤 가치가 있는가?

각 문서가 비슷한 구조로 작성되어 있어 서비스마다 색상, Typography, 간격과 Component 규칙을 어떻게 다르게 사용하는지 비교하기 쉽다.

나처럼 디자인을 전문적으로 공부하지 않은 개발자에게는 무엇을 기준으로 화면을 비교하고 Agent에게 요청해야 하는지 감을 잡는 데 도움이 될 것 같다.

### 직접 판단해야 하는 부분

- 사용자에게 필요한 정보 구조
- 사용하기 쉬운 Interaction
- 색상 대비와 Keyboard 조작 등 접근성
- 서비스 목적에 맞는 디자인 판단
- 프로젝트만의 독창적인 시각적 정체성
- 구현 결과가 실제로 좋은지 확인하는 과정

## 직접 확인해볼 파일

- [`README.md`](https://github.com/VoltAgent/awesome-design-md/blob/main/README.md)

  전체 Collection과 기본 사용법을 확인할 수 있다.

- [`design-md/linear.app/DESIGN.md`](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/linear.app/DESIGN.md)

  어두운 개발 도구 UI에서 색상과 정보 밀도를 설명하는 방법을 볼 수 있다.

- [`design-md/claude/DESIGN.md`](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/claude/DESIGN.md)

  따뜻한 색상과 Editorial Typography를 AI 제품 UI에 적용한 사례다.

- [`design-md/notion/DESIGN.md`](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/notion/DESIGN.md)

  문서, 표, Navigation처럼 Application Component가 다양하다.

- [`design-md/dell-1996/DESIGN.md`](https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/dell-1996/DESIGN.md)

  특정 시대의 디자인을 같은 형식으로 표현한 사례다.

## 주의해서 볼 점

이 문서들은 각 회사가 제공한 공식 디자인 시스템이 아니다.

공개 웹사이트에서 관찰한 CSS 값과 시각적 특징을 독립적으로 분석한 결과다.

Contribution 문서도 기존 사이트와 비교해 잘못된 색상, 빠진 Token, 부족한 설명을 수정하도록 안내한다.[11]

Repository의 MIT License가 특정 회사의 Logo, 상표, 사진, Illustration, 상용 Font를 사용할 권리까지 제공하는 것은 아니다.

공개 제품에서는 디자인 원칙을 참고하더라도 브랜드 고유 요소의 사용 조건을 별도로 확인해야 한다.[1]

각 `DESIGN.md`는 상당히 자세하기 때문에 작은 프로젝트에서는 필요 없는 규칙까지 Agent의 Context를 차지할 수 있다.

전체를 그대로 유지하기보다 실제로 필요한 색상, Typography, Layout, Component 규칙만 남겨 프로젝트 전용 문서로 다듬는 방법을 고려할 수 있다.

또한 공식 Specification과 Repository의 개별 문서는 현재 `version: alpha`를 사용한다.[4][8]

형식과 관련 도구가 앞으로 달라질 가능성이 있다.

## 나중에 생각해볼 질문

1. 완성된 브랜드 스타일을 가져오는 것이 목적인가, 아니면 `DESIGN.md`의 구조와 작성 방법을 배우는 것이 목적인가?
2. 디자인 초안을 빠르게 만드는 것과 프로젝트 고유의 시각적 정체성을 만드는 것 사이에서 이 자료를 어디까지 사용해야 할까?
3. `AGENTS.md`, `SKILL.md`, `DESIGN.md`를 함께 둔다면 어떤 규칙을 어느 파일에 기록해야 중복을 줄일 수 있을까?

## 정리

`VoltAgent/awesome-design-md`는 완성된 UI Component를 제공하는 Repository가 아니라 AI Agent에게 디자인 방향을 전달하는 문서 모음이다.

정확한 Design Token과 자연어 원칙을 함께 제공해 추상적인 Prompt보다 구체적인 기준을 줄 수 있다.

설치 없이 작은 UI부터 시험하기 쉽지만, Agent가 만든 결과의 접근성, 완성도, 브랜드 고유 요소는 사람이 따로 확인해야 한다.

먼저 관심 있는 `DESIGN.md` 몇 개를 직접 비교해보고, 나중에는 내 프로젝트에 맞는 디자인 기준을 어떻게 만들 수 있을지도 살펴보고 싶다.

## Sources

[1] VoltAgent/awesome-design-md README

<https://github.com/VoltAgent/awesome-design-md/blob/main/README.md>

[2] GitHub Repository tree

<https://api.github.com/repos/VoltAgent/awesome-design-md/git/trees/main?recursive=1>

[3] Google Stitch: What is DESIGN.md?

<https://stitch.withgoogle.com/docs/design-md/overview>

[4] Google Stitch: DESIGN.md specification

<https://stitch.withgoogle.com/docs/design-md/specification>

[5] Google Stitch: DESIGN.md CLI

<https://stitch.withgoogle.com/docs/design-md/cli>

[6] AGENTS.md official site

<https://agents.md>

[7] Agent Skills specification

<https://agentskills.io/specification>

[8] Claude DESIGN.md

<https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/claude/DESIGN.md>

[9] Linear DESIGN.md

<https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/linear.app/DESIGN.md>

[10] Dell 1996 DESIGN.md

<https://github.com/VoltAgent/awesome-design-md/blob/main/design-md/dell-1996/DESIGN.md>

[11] Contributing guide

<https://github.com/VoltAgent/awesome-design-md/blob/main/CONTRIBUTING.md>
