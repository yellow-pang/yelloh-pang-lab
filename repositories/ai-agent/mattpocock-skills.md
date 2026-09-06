---
title: "mattpocock/skills"
repository: "mattpocock/skills"
url: "https://github.com/mattpocock/skills"
category: "ai-agent"
created: "2026-09-05"
status: "starred"
star_reason: "Agent Skill을 어떻게 설계하고 필요한 것만 조합해 사용할 수 있는지 공부하기 위해"
tags:
  - ai-agent
  - agent-skills
  - developer-workflow
  - context-management
---

# mattpocock/skills

> https://github.com/mattpocock/skills

## 내가 이 Repository를 Star한 이유

개발자로 전향해 공부와 팀 프로젝트를 진행하면서 AI Agent를 자주 사용하게 되었고, 자연스럽게 Skill이라는 개념도 접하게 됐다.

특히 인턴 과정을 거치면서 반복 작업을 위한 Skill뿐 아니라 필요할 때 한 번 쓰고 버리는 일회성 Skill이라는 활용 방식도 있다는 점을 알게 됐을 때 꽤 놀랐다.

Agent에게 매번 긴 Prompt를 주는 것을 넘어 내가 원하는 작업 방식 자체를 만들어줄 수 있다는 점이 흥미로웠다.

하지만 여러 Skill과 Workflow를 사용하다 보니 무조건 많이 적용한다고 해서 좋은 것은 아니었다.

테스트와 검증이 반복되면서 작업이 오히려 무거워지거나, Context가 길어져 여러 세션과 문서를 오가야 하는 경우도 생겼다.

그러면서 **필요한 Skill만 골라 쓰거나, 기존 Skill을 조합해서 내 개발 방식에 맞게 만들 수는 없을까?** 하는 생각이 들었다.

`mattpocock/skills`는 여러 개발용 Skill이 실제로 어떻게 만들어지고 나뉘어 있는지 살펴볼 수 있는 Repository다.

앞으로 나에게 필요한 Skill을 찾고 직접 만드는 방법을 공부할 때 다시 참고하고 싶어서 Star했다.

## 한 줄 요약

`mattpocock/skills`는 AI 개발 Agent가 조사, 설계, 구현, 테스트 같은 작업을 더 일관되게 수행하도록 만든 재사용 가능한 Skill 모음이다.[1]

## 이 Repository는 무엇인가?

이 Repository는 AI 모델이나 Agent 자체를 개발하는 프로젝트가 아니다.

Claude Code나 Codex 같은 개발 Agent가 특정 작업을 수행할 때 읽고 따를 지침을 `SKILL.md` 형태로 모아둔 곳이다.

작성자는 Agent를 이용한 개발에서 자주 생기는 문제로 요구사항을 잘못 이해하는 일, 프로젝트 용어를 몰라 설명이 장황해지는 일, 충분한 피드백 없이 제대로 동작하지 않는 코드를 만드는 일, 빠르게 작성한 코드가 복잡해지는 일을 꼽는다.

이를 인터뷰, 조사, Test-Driven Development(TDD), 디버깅, 코드 리뷰, 구조 개선 같은 Skill로 나누어 다룬다.[1]

Prompt를 모아둔 것과 다른 점은 각 Skill에 사용 시점과 작업 절차가 들어 있다는 것이다.

어떤 Skill은 다른 Skill을 불러 함께 사용하고, 작업 결과를 Spec이나 Issue 같은 문서로 남기기도 한다.

필요한 작업 하나만 선택할 수도 있고, 여러 Skill을 이어 하나의 개발 Workflow로 만들 수도 있다.[4][5]

## Skill은 여기서 어떤 의미인가?

Agent는 사용자의 요청을 받아 파일을 읽거나 코드를 수정하고 명령을 실행하는 AI 도구다.

Skill은 Agent가 특정 종류의 작업을 어떻게 처리할지 정리해둔 재사용 가능한 지침이다.

예를 들어 "자료를 조사해줘"라는 요청만 받으면 Agent마다 조사 범위와 결과 형식이 달라질 수 있다.

`research` Skill은 공식 문서와 소스 코드 같은 1차 자료를 우선 확인하고, 출처가 포함된 Markdown으로 결과를 남기도록 작업 방식을 정한다.[7]

Context는 Agent가 현재 판단에 사용할 수 있는 대화와 문서, 코드 등의 정보다.

대화가 길어지거나 세션이 바뀌면 Context를 그대로 이어가기 어렵다.

이 Repository는 Handoff, Spec, Issue, `CONTEXT.md`, ADR(Architecture Decision Record, 중요한 설계 결정을 남기는 문서) 같은 기록을 활용해 필요한 정보를 대화 밖에도 남긴다.[5][6]

Workflow는 여러 작업을 순서대로 연결한 흐름이다.

아이디어를 질문으로 다듬고, Spec을 만들고, 구현 단위로 나눈 뒤 테스트와 코드 리뷰를 거치는 과정이 한 예다.

이 Repository에는 독립적인 Skill과 이런 Skill을 연결하는 Workflow가 함께 들어 있다.[5]

## Repository에서 눈에 들어온 점

### Skill을 작은 단위로 나눈다

`handoff`, `research`, `prototype`, `to-spec`처럼 목적에 따라 Skill이 따로 나뉘어 있다.

하나의 거대한 지침에 모든 개발 절차를 넣는 대신 필요한 작업만 선택하거나 조합하기 쉬운 구조다.

공식 README도 Skill을 작고 수정하기 쉬우며 조합할 수 있게 설계했다고 설명한다.[1]

### 호출 주체를 구분한다

Skill은 User-invoked와 Model-invoked로 나뉜다.

User-invoked Skill은 사용자가 이름을 직접 입력해야 시작된다.

Model-invoked Skill은 사용자가 부를 수도 있고, Agent가 현재 작업에 필요하다고 판단해 선택할 수도 있다.[4]

모든 Workflow가 자동으로 시작되는 것을 원하지 않을 때 이 구분이 유용해 보였다.

작업 방향을 크게 바꾸는 절차는 사용자가 시작하고, 반복해서 필요한 세부 원칙은 Agent가 상황에 맞게 선택하도록 둘 수 있다.

### 대화 내용을 문서로 남긴다

`handoff`는 현재 대화를 다음 Agent나 세션이 이어받을 수 있는 문서로 줄여준다.

이미 Spec, Plan, ADR, Issue, Commit에 기록된 내용은 다시 복사하지 않고 경로나 URL로 연결한다.[6]

Context를 무조건 길게 유지하는 대신 중요한 내용을 역할에 맞는 문서로 옮기는 방식이다.

세션이 길어질 때 무엇을 남기고, 다음 세션에서 무엇을 다시 읽게 할지 고민하는 데 참고할 만하다.

### 독립적인 Skill을 Workflow로 연결할 수 있다

`ask-matt`에는 아이디어를 다듬는 `grill-with-docs`부터 `to-spec`, `to-tickets`, `implement`, `tdd`, `code-review`로 이어지는 흐름이 정리되어 있다.

규모가 큰 작업에는 `wayfinder`, 어려운 버그에는 `diagnosing-bugs` 같은 다른 진입점도 있다.[5]

개별 Skill부터 시작해 필요하면 개발 과정 전체로 확장할 수 있다는 뜻이다.

반대로 전체 흐름을 모두 적용하면 생각보다 무거워질 수도 있다.

## 관심 있게 본 Skill

### `handoff`

현재 대화를 압축해 새로운 Agent나 세션이 이어받을 수 있게 하는 Skill이다.

기존 문서를 다시 붙여 넣지 않고 참조하며, 다음 세션에서 사용하면 좋은 Skill도 함께 적는다.[6]

세션이 바뀔 때 어떤 정보까지 넘겨야 하는지, 이미 저장된 문서와 중복을 어떻게 피할지가 특히 궁금했다.

Context가 길어진 작업을 나누는 방식을 고민할 때 가장 먼저 읽어보고 싶은 Skill이다.

### `research`

공식 문서, 소스 코드, Spec, 공식 API 같은 1차 자료를 조사하고 출처가 있는 Markdown으로 남기는 Skill이다.

조사를 Background Agent에 맡긴 동안 원래 작업을 계속할 수 있도록 설계되어 있다.[7]

공개 GitHub Repository를 조사하고 글로 남기는 과정과 직접 연결되는 부분이 많다.

현재 사용 중인 조사 절차와 비교하면 어떤 지침이 꼭 필요하고 어떤 부분은 줄일 수 있는지 살펴볼 수 있을 것 같다.

### `prototype`

설계 질문 하나에 답하기 위한 일회성 Prototype 코드를 만드는 Skill이다.

상태나 Logic이 자연스러운지 확인하는 경우와 UI 모양을 비교하는 경우를 나누고, Production 코드 수준의 완성도보다 빠른 확인에 집중한다.[8]

모든 아이디어를 긴 계획과 테스트로 검증하지 않고, 작은 실행 결과를 먼저 보면서 판단하는 접근이 흥미롭다.

절차가 무거워졌다고 느낄 때 더 짧은 피드백 방법으로 참고할 만하다.

### `improve-codebase-architecture`

Codebase를 살펴보고 구조 개선 후보를 찾는 Skill이다.

최근 자주 바뀐 영역, 이해하기 위해 여러 파일을 오가야 하는 부분, 테스트하기 어려운 구조 등을 조사한다.

바로 수정하기보다 후보를 HTML 보고서로 보여주고 사용자가 선택하도록 한다.[9]

Agent가 자동으로 대규모 Refactoring을 시작하지 않고 먼저 관찰 결과를 보여준다는 점이 눈에 들어왔다.

코드 구조를 분석하는 기준과 사용자에게 선택권을 남기는 방식을 함께 볼 수 있다.

### `to-spec`

현재 대화와 Codebase에서 이미 파악한 내용을 Spec으로 정리하는 Skill이다.

새로운 인터뷰를 시작하기보다 지금까지 논의한 내용을 모으는 데 초점을 둔다.[10]

Plan이나 대화 속 결정을 다음 구현 단계가 읽을 수 있는 문서로 바꾸는 과정에 참고할 수 있다.

Context 전체를 그대로 넘기는 것과 핵심만 Spec으로 남기는 것의 차이도 비교해보고 싶다.

### `wayfinder`

한 Agent 세션 안에서 끝내기 어려운 큰 작업을 결정 Ticket의 지도로 만드는 Skill이다.

처음부터 모든 계획을 확정하지 않고, 현재 분명하게 말할 수 있는 질문부터 해결하면서 다음 경로를 찾는다.[11]

간단한 작업에는 과할 수 있지만, 여러 세션에 걸친 계획을 어떻게 나누는지 살펴보기에는 좋은 사례다.

특히 구현 Ticket이 아니라 아직 답이 필요한 결정부터 분리한다는 점을 참고하고 싶다.

## Superpowers와는 어떻게 다른가?

Superpowers는 자신을 개발 Agent를 위한 완전한 소프트웨어 개발 방법론으로 설명한다.

Brainstorming, Git Worktree, 구현 계획, TDD, 코드 리뷰, Branch 마무리까지 이어지는 기본 Workflow를 제공하고 관련 Skill이 자동으로 작동하는 방향을 강조한다.[13]

`mattpocock/skills`도 Spec, Ticket, TDD, 코드 리뷰까지 이어지는 Workflow가 있다.

다만 Skill을 작은 단위로 나누고, User-invoked와 Model-invoked를 구분하며, `skills.sh`를 사용하면 필요한 Skill을 선택해 수정할 수 있다는 점이 다르다.[1][4]

그래서 이 Repository를 Superpowers의 대체 도구나 더 가벼운 버전으로 보기는 어렵다.

개별 Skill만 골라 쓰면 가볍게 시작할 수 있지만, `ask-matt`에 정리된 전체 흐름과 Setup까지 적용하면 상당히 무거운 개발 절차가 된다.

어느 방식이 더 좋은지는 작업 규모와 개발자가 원하는 통제 수준에 따라 달라질 것 같다.

## 내가 이 Repository에서 보고 싶은 것

가장 궁금한 것은 작은 Skill 하나가 어느 정도까지 구체적이어야 하는지다.

너무 짧으면 Agent마다 결과가 달라질 수 있고, 너무 많은 규칙을 넣으면 간단한 작업도 무거워질 수 있다.

`handoff`와 `research`처럼 비교적 짧은 Skill이 어떤 기준을 남기고 나머지를 덜어냈는지 먼저 보고 싶다.

Plan, Step, Handoff 사이에서 Context를 어떻게 나누는지도 관심 있는 부분이다.

모든 대화를 계속 유지할지, Spec으로 옮길지, 다음 세션을 위한 Handoff를 만들지에 따라 작업 방식이 달라진다.

이 Repository가 그 경계를 어떻게 정하는지 살펴보고 싶다.

전체 Skill을 설치하지 않고 필요한 것만 가져오는 방식과, 가져온 Skill을 내 Workflow에 맞게 수정하는 방법도 확인해보고 싶다.

완성된 Workflow를 그대로 따르기보다 현재 반복하고 있는 작업에 실제로 필요한 절차를 찾는 것이 우선이다.

## 직접 확인해볼 파일

- [`skills/productivity/handoff/SKILL.md`](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md)

  세션 사이에 Context를 넘길 때 무엇을 남기고 무엇을 참조하는지 볼 수 있다.

- [`skills/engineering/research/SKILL.md`](https://github.com/mattpocock/skills/blob/main/skills/engineering/research/SKILL.md)

  1차 자료 조사와 Markdown 기록 절차를 현재 Repository 분석 방식과 비교할 수 있다.

- [`.agents/invocation.md`](https://github.com/mattpocock/skills/blob/main/.agents/invocation.md)

  User-invoked와 Model-invoked를 구분하는 기준과 Agent별 표현 방식을 설명한다.

- [`skills/engineering/prototype/SKILL.md`](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md)

  긴 구현 전에 질문 하나를 빠르게 검증하는 Skill 구조를 확인할 수 있다.

## 주의해서 볼 점

모든 Skill을 그대로 도입할 필요는 없다.

`setup-matt-pocock-skills`를 포함한 전체 Engineering 흐름은 Issue tracker, Triage label, `CONTEXT.md`, ADR 같은 프로젝트 설정과 운영 방식을 전제로 한다.[12]

소프트웨어 구현 프로젝트에는 도움이 될 수 있지만, 문서 조사와 기록이 중심인 저장소에는 과한 부분이 있을 수 있다.

Claude Code Plugin이 주요 설치 방식 중 하나이고 Codex를 위한 설정 파일도 별도로 관리한다.

여러 Agent에서 사용할 수 있도록 작성되었지만, 각 Agent가 Skill 호출과 보조 도구를 처리하는 방식은 같지 않다.

가져오기 전에 현재 사용하는 Agent 환경과 맞지 않는 지침이 있는지 확인해야 한다.[1][4]

정식으로 소개하는 Skill과 `in-progress` 상태의 Skill도 함께 관리되고 있다.

Repository가 계속 바뀌고 있으므로 나중에 다시 볼 때는 Skill 이름과 구조가 달라졌는지 README와 실제 파일을 다시 확인하는 편이 좋다.[2][3]

## 나중에 해보고 싶은 것

- [ ] `handoff` Skill을 읽고 Context를 어떤 기준으로 줄이는지 확인하기
- [ ] `research` Skill을 현재 사용 중인 Repository 조사 과정과 비교하기
- [ ] Plan, Step, Handoff 과정에 가져올 수 있는 부분 찾기
- [ ] Context 관리 방식을 내 Skill 설계에 적용할 수 있는지 확인하기
- [ ] 필요한 Skill만 선택해 사용하는 구조 살펴보기

## 정리

AI 개발 Agent를 계속 사용하면서 좋은 Prompt뿐 아니라 작업 방식을 담은 Skill 자체에 관심이 생겼다.

모든 Workflow를 그대로 따르기보다 실제로 도움이 되는 작은 Skill을 골라 사용하고, 필요하면 내 방식에 맞게 바꾸고 싶다.

`mattpocock/skills`는 개별 Skill과 이들을 연결하는 Workflow를 함께 볼 수 있어 Skill 설계를 공부하기 좋은 사례다.

당장 전체를 설치하기보다 `handoff`와 `research`부터 직접 읽고 다시 참고하기 위해 Star했다.

## Sources

[1] mattpocock/skills README

<https://raw.githubusercontent.com/mattpocock/skills/main/README.md>

[2] GitHub Repository API: mattpocock/skills

<https://api.github.com/repos/mattpocock/skills>

[3] mattpocock/skills repository conventions

<https://raw.githubusercontent.com/mattpocock/skills/main/CLAUDE.md>

[4] Model-invoked vs user-invoked

<https://raw.githubusercontent.com/mattpocock/skills/main/.agents/invocation.md>

[5] ask-matt Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/ask-matt/SKILL.md>

[6] handoff Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/handoff/SKILL.md>

[7] research Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/research/SKILL.md>

[8] prototype Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/prototype/SKILL.md>

[9] improve-codebase-architecture Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/improve-codebase-architecture/SKILL.md>

[10] to-spec Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/to-spec/SKILL.md>

[11] wayfinder Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/wayfinder/SKILL.md>

[12] setup-matt-pocock-skills Skill

<https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/setup-matt-pocock-skills/SKILL.md>

[13] Superpowers README

<https://raw.githubusercontent.com/obra/superpowers/main/README.md>
