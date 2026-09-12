---
title: "ECC 분석: Superpowers를 대체할 수 있을까"
repository: "affaan-m/ECC"
url: "https://github.com/affaan-m/ECC"
category: "ai-agent"
created: "2026-09-08"
status: "starred"
star_reason: "Superpowers를 대체하거나 보완할 더 넓은 개발 Agent 환경인지 확인하기 위해"
tags:
  - ai-agent
  - agent-skills
  - agent-harness
  - tdd
  - developer-workflow
---

# ECC 분석: Superpowers를 대체할 수 있을까

> https://github.com/affaan-m/ECC

## 0. 이 Repository를 확인하는 이유

나는 평소 GitHub Trending과 인기 Repository를 살펴보면서,
개발에 도움이 될 만한 프로젝트를 발견하면 Star하고 있다.

현재 실제 개발에서는 Superpowers를 사용하고 있다.
Agent에게 Analysis, Plan, TDD, 구현, 검증, Code Review로 이어지는 흐름을 맡기고,
작업을 작은 단계로 나누어 확인하는 방식에 익숙해졌다.

하지만 계속 사용하다 보니 장점만큼 고민도 생겼다.

- 작은 작업도 과정이 생각보다 길어질 때가 있다.
- 회귀를 막기 위한 테스트가 계속 누적된다.
- 기존 코드와 테스트를 보존하는 방향으로 작업하면서 코드베이스가 함께 커진다.
- Agent가 현재 작업의 위험도보다 정해진 절차를 우선하는 듯한 순간이 있다.
- Superpowers 외에는 어떤 개발 Agent 환경과 방법론이 있는지 궁금해졌다.

이런 상황에서 인기 Repository를 찾아보다 ECC를 발견했다.
처음 보았을 때부터 Skills, Agents, Hooks, Rules, Memory 등 개발 Agent를 위한 요소가 매우 많았다.
작은 Skill 모음보다는 Superpowers를 대체하거나 보완할 수 있는 더 큰 개발 환경처럼 보였다.

그래서 이 Repository를 Star했다.
이번 분석에서 확인하려는 질문은 다음과 같다.

> ECC는 Superpowers와 무엇이 다르며,
> 실제로 Superpowers를 벗어나거나 대체하기 위한 선택지가 될 수 있는가?

이 문서는 ECC를 홍보하거나 곧바로 전체 도입하기 위한 글이 아니다.
내가 왜 관심을 가졌고, 현재 겪는 문제에 어떤 기능이 실제로 도움이 되는지,
반대로 어떤 복잡성을 새로 가져오는지를 나중에 다시 판단하기 위한 학습 기록이다.

## 조사 기준과 먼저 내릴 결론

이 문서는 ECC의 조사 시점 `main` 커밋 `5064474d4d762dc9640234a41617cccb79185cec`와
Superpowers의 `b36e0829c6d0140e93cfef2ca599b1b07d4a7797`을 기준으로 작성했다.
README의 소개 문구만 보지 않고 실제 Agent, Skill, Hook, Rule, Memory, 설치 문서도 함께 확인했다.

먼저 결론부터 말하면, **ECC는 Superpowers의 상위호환이라기보다 범위가 훨씬 넓은 Agent 개발 환경 확장 도구**에 가깝다.
Superpowers가 설계부터 검증까지 하나의 개발 방법을 강하게 연결한다면,
ECC는 여러 Coding Agent 위에 Agent·Skill·Rule·Hook·Memory·MCP와 설치 도구를 선택적으로 얹는다.[1][17]

따라서 현재 내 상황에는 ECC 전체로 즉시 교체하는 것보다,
Superpowers의 핵심 Workflow는 유지하면서 `refactor-clean`, 전문 reviewer, Context 점검처럼
겹치지 않는 기능을 작은 범위에서 시험하는 방식이 더 적합하다.

이하에서는 **Repository에 명시된 사실**과 그 사실을 내 상황에 적용한 **분석·판단**을 구분해 설명한다.

## 1. ECC란 무엇인가

### ECC의 공식 정의

ECC는 **Everything Claude Code**의 약자다.
Repository의 시작은 Claude Code를 오래 사용하며 정리한 Skills, Hooks, Subagents, MCP와 실제 사용 방식이었다.
작성자는 약 10개월 동안 매일 Claude Code를 사용한 뒤 자신의 전체 설정과 유효했던 방식을 공개한다고 설명한다.[2]
공식 사이트는 작성자를 Affaan Mustafa로 명시한다.[16]

현재 README는 ECC를 다음과 같이 설명한다.

> Coding Agent는 코드를 작성할 수 있지만,
> ECC는 계획·테스트·구현·리뷰·검증·기억·개선을 연결하는 coordinated engineering system and toolbox를 제공한다.[1]

여기서 **coordinated engineering system**은 여러 개발 절차와 역할이 따로 움직이지 않고
하나의 흐름으로 연결된 개발 체계를 뜻한다.

```text
plan
→ test
→ implement
→ review
→ verify
→ remember
→ improve
```

Repository 내부의 `AGENTS.md`는 ECC를 여러 Agent, Skill, Command와 자동 Hook Workflow를 제공하는
“production-ready AI coding plugin”이라고 부른다.[3]
README의 대표 이미지와 공식 사이트에서는 “agent harness operating system”이라는 표현도 사용한다.[1][16]

표현이 여러 개이므로 역할을 나누어 보는 편이 정확하다.

| 질문 | 판단 | 이유 |
| --- | --- | --- |
| 단순 Skill 모음인가 | 아니다 | Skill 외에 Agent, Rule, Hook, Memory, MCP 설정, 설치·업데이트·보안 검사 도구까지 포함한다. |
| Agent framework인가 | 일부 성격은 있다 | 전문 Agent 역할과 orchestration 지침을 제공하지만, 모델 실행 loop 자체를 처음부터 구현한 독립 Agent runtime은 아니다. |
| Agent Harness인가 | Harness 확장·운영 계층에 가깝다 | Claude Code나 Codex가 Tool 실행과 권한을 담당하고 ECC가 그 위에 규칙과 Workflow를 설치한다. |
| 개발 방법론인가 | 방법론도 포함한다 | Plan, TDD, Review, Verification 원칙이 있지만 이 하나만이 ECC의 전체 범위는 아니다. |
| 여러 Coding Agent 위에 얹는 환경인가 | 그렇다 | Claude Code를 기준으로 Codex, Cursor, OpenCode 등 여러 Harness용 배포 경로를 제공한다.[1] |

**Agent Harness**는 AI 모델이나 Agent가 실제 개발 업무를 하도록 둘러싼 실행 환경이다.
System Prompt, Tool, 권한, Sandbox, Session, Memory, Plugin과 Agent loop 등이 여기에 포함된다.

ECC는 이 Harness 자체를 완전히 대체하지 않는다.
오히려 기존 Harness에 개발 절차와 전문 역할, 자동 검사를 배포하고 여러 Harness 사이의 차이를 연결한다.
따라서 “Harness 운영체제”는 프로젝트의 방향을 설명하는 표현이고,
기술적으로는 **여러 Harness에 설치되는 확장·구성·Workflow 체계**라고 이해하는 것이 안전하다.

### ECC가 해결하려는 문제

README와 실제 구조에서 확인되는 문제의식은 다음과 같다.[1][3]

- 매 대화마다 계획, TDD, 리뷰 지침을 다시 Prompt로 작성해야 한다.
- 하나의 Agent가 모든 분야를 처리하면서 전문성과 Context 집중도가 떨어진다.
- 형식 검사, typecheck, 보안 점검 같은 반복 작업을 사람이 매번 요청해야 한다.
- Session이 끝나면 결정과 문제 해결 과정이 사라진다.
- Claude Code, Codex, Cursor 등 Harness마다 설정 형식과 지원 기능이 다르다.
- Skill·Rule·MCP가 많아지면 오히려 Context와 설정이 비대해진다.

마지막 문제까지 ECC가 스스로 다룬다는 점이 흥미롭다.
ECC는 많은 기능을 제공하면서 동시에 selective install과 `context-budget`을 제공한다.[1][13]
즉 “전부 켜라”보다 필요한 기능을 골라 관리하는 쪽이 실제 설계와 더 잘 맞는다.

## 2. 전체 구조

ECC의 핵심은 기능 수가 아니라 서로 다른 제어 방식이 함께 있다는 점이다.
각 요소가 개발 과정에서 개입하는 시점도 다르다.

| 구성 요소 | 쉬운 뜻 | 실제 개발에서 쓰이는 시점 |
| --- | --- | --- |
| **Skill** | 특정 작업 방법을 Agent에게 알려주는 재사용 가능한 지침과 자료 | TDD, 보안 검토, 검증처럼 특정 작업을 시작할 때 불러온다. |
| **Agent** | 계획자, reviewer처럼 좁은 역할을 맡는 별도 실행 주체 | 복잡한 계획, 구현 후 리뷰, build 오류 해결 등 관점을 분리할 때 사용한다. |
| **Hook** | 특정 이벤트가 일어날 때 자동 실행되는 동작 | Tool 실행 전 명령을 막거나, 파일 수정 후 format·typecheck를 실행하고, Session 종료 시 상태를 저장한다. |
| **Rule** | Agent가 계속 따라야 하는 고정 기준 | 보안, 코드 스타일, 테스트 원칙처럼 작업 전체에 넓게 적용한다. |
| **Command** | 사용자가 Workflow를 시작하는 명령 진입점 | `/plan`, `/code-review`처럼 명시적으로 작업을 시작한다. 현재 방향은 Skill 우선이며 Command는 호환용 성격이 강하다.[3] |
| **Memory** | Session 밖에 남기는 기억과 인수인계 자료 | 다음 Session이나 다른 Harness가 결정과 작업 상태를 이어받을 때 쓴다. |
| **MCP** | Agent를 외부 서비스와 연결하는 표준 인터페이스 | GitHub, browser, 문서 검색, 배포 서비스 등 외부 시스템 작업이 필요할 때 Tool을 제공한다. |
| **Context** | Agent가 현재 판단에 사용할 수 있는 정보 범위 | 개발·리뷰·조사 모드의 지침을 나누고, 압축 전 상태를 저장하며, 불필요한 Tool과 Rule을 줄일 때 관리한다. |

### Skills

`skills/`는 ECC의 현재 canonical workflow surface, 즉 **가장 기준이 되는 작업 지침 위치**다.
TDD, verification, security뿐 아니라 언어·Framework·문서·운영 분야까지 넓게 포함한다.
Skill은 특정 작업을 어떻게 수행할지 자세한 절차를 제공한다.[1][3]

개발 과정에서는 다음처럼 사용된다.

- 새 기능·버그·리팩토링: `tdd-workflow`
- 작업 완료 전 품질 gate: `verification-loop`
- 인증·입력·API 변경: `security-review`
- Context가 빠르게 차는 문제: `context-budget`
- Harness 사이 인수인계: `unified-memory`

### Agents

`agents/`의 Agent는 planner, code-reviewer, tdd-guide, refactor-cleaner처럼 역할이 분리되어 있다.
`AGENTS.md`는 복잡한 기능에는 planner, 코드 변경 뒤에는 code-reviewer,
버그나 새 기능에는 tdd-guide를 능동적으로 사용하도록 지시한다.[3]

이 구조의 목적은 한 Agent가 긴 Context 안에서 분석·구현·승인을 모두 처리하는 문제를 줄이는 것이다.
다만 각 Harness가 같은 방식으로 Subagent를 실행하는 것은 아니다.
Agent 정의 파일이 존재하는 것과 현재 사용하는 Harness에서 독립 Agent로 실행되는 것은 구분해야 한다.

### Hooks

**Hook**은 Tool 호출이나 Session lifecycle 같은 이벤트에 반응하는 자동화다.
Skill이 “이 절차를 따라라”라는 지침이라면 Hook은 실제 command를 실행하거나 Tool 호출을 막을 수도 있다.

ECC Hook에는 다음과 같은 예가 있다.[4]

- command 실행 전에 dev server나 commit 명령을 검사한다.
- 파일 수정 뒤 Prettier, TypeScript 검사, `console.log` 경고를 수행한다.
- Session 시작 시 이전 Context를 불러온다.
- Context 압축 전에 상태를 저장한다.
- Session이 멈출 때 요약·학습·비용 정보를 기록한다.

PreToolUse Hook은 종료 코드에 따라 Tool 실행을 막을 수 있다.
이 점에서 Markdown Skill보다 강한 제어 장치다.
반면 잘못 설정하면 정상 명령을 막거나 파일 수정마다 지연을 추가할 수 있다.

### Rules

**Rule**은 Agent가 작업 전반에서 따라야 하는 고정 기준이다.
ECC는 공통 Rule과 TypeScript·Python 등 언어별 Rule을 분리한다.
공통 원칙과 언어 관습이 충돌하면 더 구체적인 언어 Rule이 우선한다.[5]

Repository는 Rule과 Skill을 다음처럼 구분한다.

- Rule: 무엇을 지켜야 하는가
- Skill: 특정 작업에서 그것을 어떻게 수행하는가

항상 읽히는 Rule이 많아지면 기존 프로젝트 규칙과 충돌하거나 Context를 차지할 수 있다.
Python Worker와 Next.js가 함께 있는 프로젝트라면 `common`, `python`, `typescript`를 모두 복사하기 전에
현재 `AGENTS.md`나 lint 설정과 중복되는 내용을 먼저 비교해야 한다.

### Commands

`commands/`에는 `/plan`, `/code-review`, `/refactor-clean` 같은 진입점이 있다.
하지만 현재 ECC는 새 Workflow를 `skills/`에 먼저 작성하고,
Command는 이전 slash command와 Harness 호환성을 위한 shim, 즉 **연결용 얇은 명령**으로 유지한다.[1][3]

따라서 Command 수가 많다는 사실을 독립 기능 수로 그대로 해석하면 중복 계산할 수 있다.

### Memory와 Continuous Learning

ECC의 Memory는 하나가 아니다.

1. Hook 기반 Session Memory는 시작·종료·Context 압축 시 작업 상태를 저장하고 복원한다.[4]
2. `continuous-learning-v2`는 Session의 수정·실패·반복 Workflow를 관찰해 작은 행동 규칙인 instinct로 만든다.[11]
3. `unified-memory`는 Claude, Codex, Hermes 등 여러 Harness가 공유할 수 있는 Markdown 기반 Memory Vault를 제공한다.[12]

**Instinct**는 반복해서 관찰한 작은 행동 패턴과 그 신뢰도를 함께 저장한 기록이다.
프로젝트별 기억과 전역 기억을 분리해 다른 코드베이스의 관습이 섞이는 문제를 줄이려 한다.[11]

Memory Vault의 내용은 신뢰된 정책이 아니라 `unreviewed` Context로 저장된다.
중요한 결정은 다시 코드, 테스트, Issue나 공식 문서에서 확인해야 한다.[12]
이 제한은 Memory가 오래되거나 잘못 학습될 위험을 인정한 설계다.

### MCP와 외부 Tool

**MCP(Model Context Protocol)**는 Agent가 외부 서비스나 프로그램의 기능을 Tool 형태로 사용하는 연결 규격이다.
ECC는 GitHub, browser, memory, 문서 검색 등 다양한 MCP 설정 예시를 제공하지만,
모두 기본으로 켜는 구조는 아니다.[1]

MCP가 많아지면 편리함과 함께 다음 비용이 생긴다.

- Tool 설명이 Context를 차지한다.
- 외부 process와 package의 공급망 위험이 생긴다.
- credential 권한 범위를 관리해야 한다.
- Agent가 비슷한 Tool 중 무엇을 써야 할지 판단해야 한다.

`context-budget`도 MCP Tool schema를 큰 Context 비용 요인으로 본다.[13]

### Security, Testing, Planning, Review, Verification

ECC는 이 기능들을 별도 계층으로 나누어 제공한다.

- **Security**: 보안 Rule, `security-review`, 설정을 검사하는 AgentShield가 담당한다.[1]
- **Testing**: `tdd-workflow`, `tdd-guide`, 언어별 testing Skill이 RED–GREEN–REFACTOR와 coverage 기준을 제공한다.[6]
- **Planning**: planner Agent가 요구사항, 영향 파일, 의존성, 위험과 구현 순서를 정리한다.[7]
- **Review**: code-reviewer가 diff뿐 아니라 주변 코드와 호출부를 읽고 근거가 높은 문제만 보고하도록 지시한다.[8]
- **Verification**: `verification-loop`가 build, typecheck, lint, test, security, diff를 순서대로 확인한다.[10]

이들은 하나의 기능을 여러 이름으로 복제한 것이라기보다,
계획·구현·검토·증명이라는 서로 다른 책임을 나눈 것이다.
다만 전체 설치에서는 비슷한 지침이 Rule, Agent, Skill에 중복될 가능성도 있다.

## 3. 실제 개발 흐름

### 일반적인 Coding Agent

```text
사용자 요청
→ 관련 코드 확인
→ 구현
→ 테스트
→ 완료 보고
```

이 방식은 빠르지만 요구사항을 잘못 해석하거나,
같은 Context 안에서 구현한 Agent가 자기 결과를 낙관적으로 평가할 수 있다.

### ECC가 제시하는 기본 흐름

README와 `AGENTS.md`의 기본 원칙을 연결하면 다음과 같다.[1][3]

```text
사용자 요청
→ 현재 Rule·프로젝트 Context 적용
→ planner 또는 전문 Agent로 요구사항·위험 분석
→ TDD로 실패하는 테스트 작성
→ 최소 구현
→ Hook의 자동 format·typecheck·안전 검사
→ 별도 code-reviewer가 변경 검토
→ verification-loop로 build·lint·test·security·diff 확인
→ Session 결과와 재사용할 교훈을 Memory에 저장
→ 반복된 패턴을 Skill이나 Workflow로 개선
```

이 흐름에서 모든 단계가 항상 자동 실행되는 것은 아니다.

- Rule은 설치 범위와 Harness의 로딩 방식에 따라 적용된다.
- Skill과 Agent는 자동 추천되거나 사용자가 직접 호출할 수 있다.
- Hook만 실제 이벤트에 연결되었을 때 자동 실행된다.
- Memory와 continuous learning은 별도 runtime·Hook 설정이 필요할 수 있다.
- Codex, Hermes 등에서는 Claude Code와 같은 Hook·Agent parity가 보장되지 않는다.[1][14][15]

따라서 ECC의 Workflow는 하나의 강제된 pipeline이라기보다,
**규칙·전문 Agent·자동화·기억을 조합해 만드는 개발 운영 체계**다.
어떤 조합을 설치했는지에 따라 실제 동작이 크게 달라진다.

## 4. Superpowers와 비교

이 비교에서 가장 중요한 질문은 기능 수가 아니다.
두 Repository가 어느 범위까지 책임지려 하는지를 봐야 한다.

### 핵심 철학의 차이

Superpowers는 자신을 composable Skill 위에 만든 완전한 software development methodology라고 정의한다.[17]
핵심은 brainstorming, plan, worktree, TDD, review, verification, branch 마무리를
하나의 예측 가능한 lifecycle로 연결하는 것이다.

ECC는 개발 방법론도 포함하지만 더 넓다.
여러 언어와 분야의 Skill, 전문 Agent, 항상 적용되는 Rule, 이벤트 Hook,
Memory, MCP, 보안 검사, 여러 Harness용 설치·운영을 한 생태계로 제공한다.[1]

```text
Superpowers
= 개발을 어떤 순서와 기준으로 진행할 것인가

ECC
= 여러 Agent 환경에 어떤 역할·지식·규칙·자동화·기억을 배치할 것인가
  + 그 안에 Plan·TDD·Review·Verification 방법론도 포함
```

### 항목별 비교

| 기준 | Superpowers | ECC | 해석 |
| --- | --- | --- | --- |
| 철학 | 성급한 구현을 막는 순서와 gate | 전문화·자동화·기억·다중 Harness 운영 | 깊고 좁은 방법론과 넓은 운영 체계의 차이다. |
| 목적 | 기능 개발 lifecycle을 일관되게 수행 | 반복 Prompt 없이 개발 환경 전체를 구성 | ECC가 책임지는 표면이 더 많다. |
| Skill 구조 | 소수의 Skill이 서로 강하게 연결 | 많은 독립·도메인 Skill과 설치 profile | ECC는 선택 폭이 크지만 탐색·선택 비용도 크다. |
| Analysis·Planning | brainstorming 후 writing-plans | planner, architect, plan 관련 Skill·Command | 둘 다 계획을 중시하지만 Superpowers는 사용자 승인 gate가 더 선명하다. |
| TDD | 실패 확인 전 production code 금지에 가까운 강한 절차 | `tdd-workflow`와 Rule이 test-first와 80% coverage를 요구 | ECC도 강하다. 오히려 일률적 coverage 목표가 테스트 누적을 늘릴 수 있다.[6] |
| Debugging | `systematic-debugging`이 root cause 조사 순서를 명시 | build resolver, 언어별 진단, Agent 자체 실패용 introspection 등 여러 경로 | 일반 debugging 방법론의 일관성은 Superpowers가 더 명확하고, ECC는 분야별 도구가 넓다. |
| Code Review | 구현과 분리된 fresh reviewer, feedback 검증 Skill | code-reviewer와 언어·보안별 reviewer | ECC는 전문 reviewer가 많고, Superpowers는 lifecycle 안의 리뷰 순서가 선명하다.[8][19] |
| Verification | 완료 주장 직전 fresh evidence를 요구 | build→type→lint→test→security→diff verification loop | 둘 다 증거를 요구한다. ECC는 검사 종류가 더 구체적이고 넓다.[10][20] |
| Git Worktree | 전용 Skill로 격리 생성·baseline·정리를 lifecycle에 포함 | 가이드와 병렬 운영에서 사용을 권장하며 일부 operator 기능도 존재 | ECC에도 worktree 개념은 있지만 Superpowers처럼 모든 기능 개발의 전용 gate로 연결되지는 않는다.[2][18] |
| Subagent | Task별 구현자와 별도 reviewer를 정해진 순서로 사용 | planner, reviewer, resolver 등 많은 전문 Agent를 선택·조합 | Superpowers는 orchestration recipe, ECC는 역할 catalog에 더 가깝다.[3][19] |
| Memory | 핵심 lifecycle 기능이 아님 | Session memory, instinct, Memory Vault | 장기 작업과 Harness 간 인수인계는 ECC가 훨씬 넓다.[11][12] |
| Hooks | 핵심이 아님 | Tool·Session 이벤트에서 자동 실행·차단 | ECC가 지침을 실제 runtime 동작으로 옮길 수 있는 중요한 차이다.[4] |
| Rules | 각 Skill 안의 강한 절차 규칙 | 공통·언어별 항상 적용 Rule pack | ECC는 조직적 표준화에 유리하지만 기존 규칙과 충돌할 수 있다.[5] |
| Context 관리 | worktree, Subagent, Plan 문서로 작업 Context를 분리 | Context mode, compaction Hook, Memory, `context-budget` | ECC가 관리 기능은 많지만 설치 요소 자체가 Context를 늘릴 수 있다.[13] |
| Agent 지원 범위 | 지원되는 Agent plugin에서 일관된 방법론 제공 | Claude Code 중심, Codex 지원, 그 밖의 adapter는 기능 차이가 큼 | ECC의 “지원”은 동일 기능을 뜻하지 않는다.[1][14] |
| 설치·설정 | 상대적으로 작은 Skill·plugin 설치 | profile, Rule, Hook, MCP, runtime, target 선택 | ECC가 더 어렵고 잘못 겹쳐 설치할 위험도 크다. |
| 사용자 개입 | 설계 승인과 branch 통합 결정을 명시적으로 요청 | 자동 Hook과 Agent routing을 늘릴 수 있음 | ECC는 자동화 수준을 높일 수 있지만 사용자가 흐름을 놓칠 수도 있다. |
| 작은 작업 부담 | 모든 gate를 적용하면 길어질 수 있음 | full 구성은 Tool마다 Hook과 여러 Rule이 개입할 수 있음 | 둘 다 과할 수 있으나 ECC는 선택 설치로 줄일 수 있다. |
| 큰 프로젝트 장점 | 일관된 lifecycle과 격리·리뷰 | 다중 언어, 전문 reviewer, Memory, 팀 규칙, Harness 연결 | 서비스와 역할이 다양할수록 ECC의 폭이 유리하다. |
| 테스트 누적 | TDD를 기계적으로 따르면 누적 가능 | TDD와 80% coverage 기준이 같은 문제를 더 만들 수도 있음 | ECC 도입만으로 현재 문제는 해결되지 않는다. |
| 코드 삭제·리팩토링 | 일반 TDD·review 안에서 처리하며 전용 dead-code 절차는 약함 | `refactor-clean`이 탐지→위험 분류→작은 삭제→재검증을 제시 | ECC가 직접적인 도구는 있지만 매우 보수적이라 명확한 삭제 기준이 필요하다.[9] |

### ECC는 Superpowers의 상위호환인가

**아니다.** 기능 수만 보면 ECC가 훨씬 크지만,
더 많은 기능이 하나의 방법론을 더 잘 수행한다는 뜻은 아니다.

Superpowers의 강점은 “다음에 무엇을 해야 하는가”가 명확하다는 것이다.
ECC의 강점은 “현재 문제에 맞는 역할과 자동화를 무엇으로 구성할 것인가”의 선택지가 많다는 것이다.

ECC로 Superpowers와 비슷한 `Plan → TDD → Review → Verification` 흐름을 만들 수는 있다.
하지만 brainstorming의 승인 gate, task마다 fresh Subagent를 배치하는 순서,
branch 마무리까지 같은 의미로 자동 재현되는 것은 아니다.
사용자가 ECC의 여러 기능을 선택하고 연결해야 한다.

반대로 Superpowers만으로는 ECC의 cross-Harness Memory, event Hook,
언어별 reviewer, Agent configuration 보안 검사, Context 비용 audit를 그대로 얻기 어렵다.

## 5. Superpowers를 대체할 수 있는가

### 1. Superpowers를 완전히 대체할 수 있는 경우

다음 조건이라면 가능하다.

- Superpowers의 고정 lifecycle보다 필요한 Agent와 Skill을 직접 조합하고 싶다.
- 사용자 승인 gate와 branch 마무리 절차를 프로젝트 Rule로 직접 정의할 수 있다.
- ECC의 planner, TDD, reviewer, verification을 연결한 자체 Workflow를 운영할 의지가 있다.
- Claude Code처럼 ECC 기능 지원이 가장 완전한 Harness를 주로 사용한다.
- 설치 profile, Hook 권한, 업데이트와 Context 비용을 지속적으로 관리할 수 있다.

이 경우에도 “ECC 설치 = Superpowers 대체 완료”는 아니다.
Superpowers가 제공하던 순서와 책임을 ECC 구성으로 다시 설계해야 한다.

### 2. Superpowers와 함께 사용하는 것이 적합한 경우

다음 조건이라면 조합이 더 자연스럽다.

- Superpowers의 Analysis·Plan·TDD·Review·Verification 흐름은 계속 유용하다.
- dead code 정리, 언어별 review, Memory, Context audit처럼 부족한 기능만 필요하다.
- ECC 전체 Rule과 Hook이 기존 방식과 충돌하는 것을 피하고 싶다.
- 같은 프로젝트에서 Python과 Next.js처럼 다른 기술 영역을 전문적으로 점검하고 싶다.

이때는 겹치는 TDD·planning Rule을 중복 설치하지 않고,
ECC의 독립 기능만 선택해야 한다.
두 프로젝트의 강한 지침을 동시에 모두 켜면 더 좋은 개발이 아니라
중복 Plan, 중복 Review, 더 많은 테스트와 Context를 만들 수 있다.

### 3. 목적이 달라 직접 비교하기 어려운 경우

ECC를 다음 목적으로 본다면 Superpowers와 직접 비교하기 어렵다.

- 여러 Harness 사이의 공통 Memory와 handoff
- Hook 기반 format·typecheck·보안 자동화
- MCP와 외부 Tool 관리
- 다양한 언어·도메인 전문 Agent catalog
- Agent 설정 자체의 보안 검사

이 기능들은 Superpowers의 개발 방법론을 대체하기보다 그 바깥의 운영 문제를 해결한다.

### 내 상황에 대한 판단

현재 상황은 다음과 같다.

- 혼자 개발하던 프로젝트를 이제 2명이 함께 개발한다.
- Python Worker와 Next.js 서비스가 함께 있다.
- 회귀 테스트가 많이 쌓였다.
- 리팩토링과 불필요한 코드·테스트 제거가 필요하다.

이 조건에서는 **2번, Superpowers와 선택적으로 함께 사용하는 방식**에 가장 가깝다.

ECC의 장점은 분명하다.

- Python과 TypeScript reviewer를 분리해 언어별 문제를 볼 수 있다.
- `refactor-clean`으로 dead code를 위험도별로 나눠 작은 batch로 제거할 수 있다.[9]
- `verification-loop`에서 Worker와 Next.js의 검사 명령을 분리해 최종 gate를 만들 수 있다.[10]
- Memory Vault는 두 사람 또는 서로 다른 Harness 사이의 결정과 검증 결과를 전달할 수 있다.[12]
- `context-budget`은 Skill·Rule·MCP가 새 복잡성이 되는지 측정하는 기준을 준다.[13]

하지만 현재 고민을 자동으로 해결하지는 않는다.

특히 ECC의 TDD Workflow는 80% 이상 coverage와 unit·integration·E2E를 폭넓게 요구하고,
RED·GREEN·refactor 증거 보고까지 남긴다.[6]
이를 모든 작은 변경에 적용하면 이미 겪은 테스트·절차 누적이 더 커질 수 있다.

`refactor-clean`도 기본적으로 “확실하지 않으면 보존”하는 보수적 접근이다.[9]
안전하지만, 삭제가 필요한 코드베이스에서 테스트 통과만을 보존 기준으로 삼으면
오래된 요구사항과 중복 테스트가 계속 살아남을 수 있다.

따라서 삭제 작업에는 다음 기준을 별도로 줘야 한다.

```text
1. 현재 사용자 행동과 외부 계약을 먼저 목록화한다.
2. 각 테스트가 어떤 현재 요구사항을 보호하는지 연결한다.
3. 같은 행동을 중복 검증하는 테스트는 통합 후보로 분류한다.
4. 구현 세부사항만 고정하는 테스트는 삭제·수정 후보로 분류한다.
5. 사라진 요구사항의 코드와 테스트는 함께 제거할 수 있다.
6. coverage 수치가 내려가더라도 중요한 행동 보호가 유지되는지 따로 판단한다.
7. 삭제 전후 build·typecheck·핵심 회귀·E2E 결과를 비교한다.
```

**테스트가 존재한다는 사실은 그 코드가 계속 필요하다는 증거가 아니다.**
테스트는 현재 필요한 행동을 보호할 때 가치가 있다.
사라진 요구사항이나 중복 구현을 보존하는 테스트라면 production code와 함께 삭제할 수 있다.

## 6. 실제로 먼저 써볼 만한 기능

전체 설치보다 현재 문제와 직접 연결되는 기능부터 좁게 시험한다.

| 우선순위 | ECC 기능 | 해결하려는 문제 | Superpowers의 가까운 개념 | 먼저 써볼 가치 | 도입 난이도 | 주의점 |
| ---: | --- | --- | --- | --- | --- | --- |
| 1 | `refactor-clean` / `refactor-cleaner` | dead code, 중복, 불필요 dependency를 위험도별로 제거 | TDD + code review + verification | 매우 높음 | 중간 | test 통과만 삭제 기준으로 쓰지 말고 현재 요구사항과 public API를 먼저 확정한다.[9] |
| 2 | `code-reviewer` + Python/TypeScript reviewer | 구현자가 놓친 언어별·Framework별 문제 확인 | requesting/receiving code review | 높음 | 낮음 | reviewer 수를 늘리기보다 역할을 분리하고, 중복·추측 finding은 걸러낸다.[8] |
| 3 | `context-budget` | Rule·Skill·MCP·Agent가 Context를 얼마나 차지하는지 확인 | 직접 대응 없음 | 높음 | 낮음 | token 값은 추정치이므로 절대값보다 도입 전후 차이를 본다.[13] |
| 4 | `verification-loop` | Worker와 Next.js의 build·type·lint·test·diff를 한 번에 확인 | verification-before-completion | 높음 | 중간 | README 예시 명령을 그대로 쓰지 말고 실제 프로젝트 명령으로 바꾼다.[10] |
| 5 | `unified-memory` | 2명 또는 여러 Harness 사이 결정·검증 결과 handoff | Plan 문서와 Subagent Context 전달 | 보통 | 중간 | task tracker나 공식 설계 문서 대용으로 쓰지 않고, 비밀정보를 저장하지 않는다.[12] |
| 6 | 선택적 Hook | 자주 빠뜨리는 format·typecheck·commit 전 검사를 자동화 | 대응 없음 | 조건부 | 높음 | 처음에는 no-hooks로 시작하고 반복되는 실수가 확인된 Hook만 켠다.[4] |
| 7 | `security-review` 또는 AgentShield | Next.js API, 입력, secret과 Agent 설정의 위험 점검 | 일반 code review 일부 | 조건부로 높음 | 중간 | generic checklist를 현재 architecture의 실제 위협 모델과 구분한다.[1] |

### 추천 도입 순서

```text
1주차: ECC를 설치하지 않고 refactor-clean과 reviewer 지침을 읽기 전용으로 적용
↓
2주차: context-budget과 verification-loop를 현재 명령에 맞게 시험
↓
3주차: 두 사람의 handoff 문제가 실제로 있으면 unified-memory 실험
↓
반복 실수가 확인된 뒤에만 필요한 Hook 하나 추가
```

Superpowers와 ECC의 TDD를 동시에 활성화하는 것은 첫 실험에서 제외하는 편이 낫다.
현재 필요한 것은 TDD 규칙을 하나 더 추가하는 일이 아니라,
어떤 테스트와 코드가 현재 행동을 지키고 어떤 것은 유지 비용만 만드는지 분류하는 일이기 때문이다.

## 7. 단점과 위험

### 기능이 지나치게 많다

README 기준 ECC에는 많은 Agent, Skill과 Command가 포함된다.[1]
선택지가 많으면 전문 작업을 찾기 쉽지만,
비슷한 기능 중 어느 것을 써야 하는지 결정하는 비용과 문서 동기화 부담도 커진다.

### Context 사용량이 늘 수 있다

Skill 본문이 필요할 때만 로드되어도 Agent·Skill metadata, 항상 적용되는 Rule,
MCP Tool schema, SessionStart Memory가 함께 누적될 수 있다.
ECC에 `context-budget`이 존재한다는 사실 자체가 이 문제가 현실적인 운영 대상임을 보여준다.[13]

### Rule이 많으면 현재 코드베이스와 충돌한다

`80% coverage`, immutability, file length 같은 일반 기준이 모든 프로젝트에 같은 답은 아니다.[3]
Python Worker의 성능 경로나 Next.js Framework 관습과 충돌할 때는
Repository의 실제 convention과 언어별 Rule이 우선하도록 조정해야 한다.

### 설정과 설치가 복잡하다

같은 Harness에 plugin, manual install, legacy sync를 겹치면 중복될 수 있다.
Claude plugin은 Rule을 직접 배포하지 못하고,
Hook runtime은 권한과 side effect를 이해한 뒤 명시적으로 켜야 한다.[1][4]

### Agent별 호환성이 다르다

Claude Code가 기준 구현이다.
Codex는 native plugin과 Skill을 지원하지만 Claude Agent 파일과 Hook profile이 그대로 재현되지는 않는다.[1][14]
Hermes adapter는 Rule·Skill·Command·Agent 지침을 설치하지만 Hermes 설정 파일 자체는 건드리지 않는다.[15]

따라서 “ECC가 지원한다”는 문구는 다음 세 질문으로 나눠 확인해야 한다.

1. 파일을 설치할 수 있는가?
2. Harness가 Skill과 Agent를 native하게 발견하는가?
3. Hook과 자동 차단까지 같은 방식으로 실행되는가?

### 빠른 업데이트가 설정을 흔들 수 있다

기능과 adapter가 빠르게 바뀌면 README, 실제 catalog, 특정 Harness용 manifest의 시점이 달라질 수 있다.
업데이트 전에는 변경 diff와 install dry-run을 보고,
업데이트 뒤에는 doctor와 프로젝트 자체 test를 실행하는 운영 절차가 필요하다.

### 작은 프로젝트에는 과할 수 있다

한두 파일을 수정하는 프로젝트에서 planner, TDD Agent, 여러 reviewer,
Hook, Memory, MCP까지 켜면 작업보다 환경 관리가 더 커질 수 있다.
이 경우 필요한 Skill 한두 개를 직접 불러오는 편이 낫다.

### 자동화가 개발 이해를 가릴 수 있다

Hook과 Agent routing이 많아지면 “왜 이 단계가 필요한가”보다
“ECC가 그렇게 하니까”라는 이유로 프로세스를 따를 위험이 있다.
이는 Superpowers를 기계적으로 따르며 느낀 문제를 더 큰 시스템에서 반복하는 것이다.

자동화의 성공 기준은 기능을 많이 활성화한 것이 아니다.

- 어떤 위험을 막기 위해 켰는지 설명할 수 있는가?
- 실패했을 때 어느 Rule·Skill·Hook이 개입했는지 추적할 수 있는가?
- 얻은 품질 향상이 시간·Context·테스트 유지 비용보다 큰가?

이 질문에 답할 수 없는 기능은 끄는 편이 낫다.

## 8. 내가 확인해야 할 실험

Superpowers와 ECC를 비교할 때는 다른 Repository나 다른 Task를 사용하면 결과를 해석하기 어렵다.
같은 commit에서 같은 요구사항을 두 branch 또는 worktree로 나누고,
한쪽은 현재 Superpowers Workflow, 다른 쪽은 선택한 ECC 기능으로 수행한다.[2][18]

공통 측정 항목은 다음과 같이 둔다.

- 시작부터 완료까지 걸린 시간
- Agent가 요청한 확인 질문 수
- 생성·수정·삭제한 production code와 test 수
- 실행한 검증 명령과 실패 횟수
- Code Review의 실제 유효 finding과 잘못된 경고 수
- 사람이 중간에 바로잡은 횟수
- 최종 diff 크기와 이해하기 어려운 변경 수
- 가능하면 token 또는 비용

### 실험 1. 간단한 버그 수정

동일한 재현 가능한 버그를 고친다.

- Superpowers: systematic debugging → TDD → review → verification
- ECC: 최소 Rule + tdd-guide 또는 관련 resolver → code-reviewer → verification-loop
- 확인할 점: root cause를 찾기 전 수정했는지, 불필요한 테스트가 몇 개 늘었는지, 완료 시간이 얼마나 다른지

### 실험 2. 작은 신규 기능 구현

Python Worker 또는 Next.js 한쪽에 범위가 작은 기능을 추가한다.

- acceptance criteria를 양쪽에 동일하게 제공한다.
- Superpowers는 brainstorming과 Plan을 사용한다.
- ECC는 planner와 tdd-workflow를 사용하되 Hook은 끈다.
- 확인할 점: Plan 길이, 사용자 질문의 질, 요구사항 누락, 생성된 테스트의 중복

### 실험 3. 기존 코드 리팩토링

외부 동작을 바꾸지 않고 큰 함수나 중복 module을 정리한다.

- Superpowers는 기존 테스트를 안전망으로 사용해 작은 단계로 바꾼다.
- ECC는 planner + refactor-cleaner + 언어별 reviewer를 사용한다.
- 확인할 점: 실제 복잡도가 줄었는지, wrapper와 helper만 늘지 않았는지, diff를 사람이 설명할 수 있는지

### 실험 4. 불필요한 테스트와 코드 삭제

현재 가장 중요한 실험이다.
삭제 후보와 반드시 지켜야 할 사용자 행동을 먼저 같은 문서로 제공한다.

- 같은 행동을 반복 검증하는 test 두세 개를 통합 후보로 지정한다.
- 사라진 요구사항에 연결된 code와 test를 함께 삭제할 수 있다고 명시한다.
- coverage 수치만 유지하도록 요구하지 않는다.
- 확인할 점: 삭제한 production/test LOC, 남긴 행동 보장, full regression 결과, Agent가 불필요한 보존을 선택한 이유

### 실험 5. 실패 테스트 디버깅

동일한 flaky test 또는 의도적으로 만든 환경 의존 실패를 제공한다.

- Superpowers의 systematic-debugging과 ECC의 resolver·review 조합을 비교한다.
- 최대 시도 횟수와 허용 command를 동일하게 둔다.
- 확인할 점: 가설을 세우기 전 retry했는지, test를 약화해 통과시키지 않았는지, 원인과 증거를 남겼는지

### 실험 결과 판정 기준

ECC가 더 많은 단계를 수행했다고 성공한 것이 아니다.
다음 세 조건을 함께 만족해야 현재 문제에 도움이 된다고 볼 수 있다.

1. 현재 사용자 행동과 public contract가 유지된다.
2. production code와 test의 유지 비용이 실제로 감소한다.
3. 두 개발자가 변경 이유와 검증 결과를 짧게 설명할 수 있다.

## 9. 최종 평가

### 한 줄 정의

> ECC는 여러 Coding Agent 위에 전문 Agent, Skill, Rule, Hook, Memory, MCP와 운영 도구를 선택적으로 배치하는 Agent Harness 확장·운영 체계다.

### 내가 Star한 이유와 실제 분석 결과가 일치했는가

**절반 이상 일치했다.**

처음에는 ECC가 Superpowers를 대체할 수 있는 더 큰 개발 방법론처럼 보였다.
실제로 Plan, TDD, Review, Verification을 모두 제공하고,
Superpowers에 없는 Memory, Hook, 전문 Agent, Context audit까지 포함한다.[1]

다만 “더 큰 방법론”이라는 첫인상은 정확하지 않았다.
ECC의 본질은 하나의 엄격한 lifecycle보다 여러 Harness에 개발 능력과 운영 자동화를 배포하는 데 있다.
따라서 Superpowers를 설치만으로 대체하는 제품이 아니라,
필요한 기능을 골라 기존 Workflow를 다시 구성하게 해주는 더 넓은 도구 상자에 가깝다.

### Superpowers 대비 가장 큰 차이

Superpowers의 중심은 **개발 순서의 일관성**이고,
ECC의 중심은 **개발 환경의 구성 가능성과 운영 범위**다.

Superpowers는 적은 Skill을 강하게 연결한다.
ECC는 많은 Skill·Agent·Rule·Hook을 선택해 연결한다.
전자는 과정이 길어질 수 있지만 이유와 순서가 비교적 분명하고,
후자는 필요한 기능만 잘 고르면 강력하지만 선택과 유지보수 책임이 사용자에게 더 많이 돌아온다.

### 내가 직접 써볼 가치

**4/5**

전체 설치가 아니라 `refactor-clean`, 전문 reviewer, `context-budget`, `verification-loop`를
현재 코드베이스의 작은 구간에 적용해볼 가치는 높다.
특히 Python과 Next.js가 함께 있고 코드·테스트 정리가 필요한 현재 상황과 직접 연결된다.

### 지금 바로 도입할 필요

**2/5**

전체 ECC 환경으로 즉시 전환할 필요는 낮다.
현재의 테스트 누적과 기계적인 프로세스 문제는 도구 부족보다
삭제·통합 기준과 작업 위험도에 맞춘 Workflow 조절이 먼저다.
ECC의 TDD와 Rule을 그대로 추가하면 같은 문제가 더 커질 수도 있다.

먼저 읽기 전용 분석과 A/B 실험으로 효과를 확인한 뒤,
실제 반복 문제를 해결한 기능만 선택 설치하는 편이 안전하다.

### 나중에 다시 볼 핵심 포인트

- ECC는 Superpowers의 상위호환이 아니라 더 넓은 Harness 운영·확장 체계다.
- Claude Code가 기준 구현이며, Codex·Hermes 등에서는 Skill·Agent·Hook 지원 범위가 다르다.
- 현재 문제에는 TDD를 하나 더 추가하기보다 `refactor-clean`과 요구사항 기반 test 정리가 더 중요하다.
- Rule, Hook, MCP, Memory는 편리함만큼 Context·권한·업데이트 관리 비용을 만든다.
- 전체 설치보다 선택 설치와 동일 Task A/B 비교가 먼저다.

### 다음 비교 후보

- Superpowers vs ECC의 동일 Task 실험 결과
- ECC의 `refactor-clean` vs 별도의 dead-code·test-pruning Workflow
- Codex native ECC plugin과 Claude Code ECC plugin의 실제 기능 차이
- ECC Memory Vault와 Repository 문서·Issue 기반 handoff 비교
- AgentShield와 일반 code security review의 역할 차이

## Sources

[1] ECC README

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/README.md>

[2] The Shorthand Guide to Everything Claude Code

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/the-shortform-guide.md>

[3] ECC Agent Instructions

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/AGENTS.md>

[4] ECC Hooks

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/hooks/README.md>

[5] ECC Rules

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/rules/README.md>

[6] ECC TDD Workflow Skill

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/skills/tdd-workflow/SKILL.md>

[7] ECC Planner Agent

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/agents/planner.md>

[8] ECC Code Reviewer Agent

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/agents/code-reviewer.md>

[9] ECC Refactor Clean Command

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/commands/refactor-clean.md>

[10] ECC Verification Loop Skill

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/skills/verification-loop/SKILL.md>

[11] ECC Continuous Learning v2 Skill

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/skills/continuous-learning-v2/SKILL.md>

[12] ECC Unified Memory Skill

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/skills/unified-memory/SKILL.md>

[13] ECC Context Budget Skill

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/skills/context-budget/SKILL.md>

[14] ECC Codex Plugin Notes

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/.codex-plugin/README.md>

[15] Hermes x ECC Setup

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/docs/HERMES-SETUP.md>

[16] ECC Tools

<https://ecc.tools>

[17] Superpowers README

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/README.md>

[18] Superpowers Using Git Worktrees Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/using-git-worktrees/SKILL.md>

[19] Superpowers Subagent Driven Development Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/subagent-driven-development/SKILL.md>

[20] Superpowers Verification Before Completion Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/verification-before-completion/SKILL.md>
