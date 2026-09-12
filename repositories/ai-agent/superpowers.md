---
title: "Superpowers 학습용 분석"
repository: "obra/superpowers"
url: "https://github.com/obra/superpowers"
category: "ai-agent"
created: "2026-09-08"
status: "starred"
star_reason: "이미 사용 중인 Agent 개발 Workflow의 원리와 각 Skill의 역할을 이해하기 위해"
tags:
  - ai-agent
  - agent-skills
  - software-development-methodology
  - tdd
  - developer-workflow
---

# Superpowers 학습용 분석

## 조사 기준과 먼저 내릴 결론

이 자료는 `obra/superpowers`의 조사 시점 최신 `main` 커밋 `b36e0829c6d0140e93cfef2ca599b1b07d4a7797`, `agentskills/agentskills`의 `69ef37e9424c0a7ea9dd2293b559e43ec8176379`, `affaan-m/ECC`의 `5064474d4d762dc9640234a41617cccb79185cec`를 기준으로 README와 실제 파일을 확인해 정리했다.

먼저 결론부터 말하면, **Superpowers는 개발 지식을 모아둔 Skill 목록이 아니라 Coding Agent의 의사결정 순서를 바꾸는 개발 방법론 패키지**다. 핵심은 Agent가 코드를 얼마나 빨리 작성하는지가 아니라, 잘못된 문제를 빠르게 구현하거나 검증되지 않은 성공을 보고하지 못하도록 중간에 여러 제동 장치를 두는 데 있다.[1]

다만 이 제동 장치는 품질을 자동 보장하는 마법이 아니다. 작업이 작거나 요구사항이 자주 바뀌면 절차 비용이 이익보다 커질 수 있고, TDD와 리뷰를 기계적으로 반복하면 코드뿐 아니라 테스트와 Context도 함께 비대해질 수 있다.

이하에서 **Repository가 명시한 사실**과 그 사실을 사용자의 경험에 적용한 **분석 또는 판단**을 구분해 설명한다.

## 1. Superpowers는 무엇인가

### 왜 만들어졌는가

작성자 Jesse Vincent의 초기 소개에 따르면, 출발점은 자신이 Coding Agent와 일하던 과정을 추출하고 체계화해 Agent를 더 잘 조종하려는 것이었다. 매번 명령어나 긴 Prompt를 붙여 넣는 대신, 세션 시작 시 bootstrap을 넣어 Agent가 관련 Skill을 찾아 사용하게 만들었다.[2]

여기서 **bootstrap**은 프로그램이나 Agent가 처음 시작할 때 기본 행동 방식을 불러오는 초기화 절차를 뜻한다.

Superpowers가 겨냥하는 문제는 다음과 같이 정리할 수 있다.

- Agent가 요구사항을 충분히 이해하기 전에 바로 코드를 수정한다.
- 대화에서 합의한 설계가 실제 구현 단계에서 빠지거나 달라진다.
- 테스트를 나중에 추가해 이미 만든 구현을 정당화한다.
- 오류 원인을 찾지 않고 떠오르는 수정부터 반복한다.
- Agent가 자신의 작업 완료 보고를 스스로 사실로 간주한다.
- 구현은 끝났지만 branch 통합, PR, worktree 정리가 임의로 처리된다.

README가 자신을 “complete software development methodology”라고 부르는 이유도 여기에 있다. **Methodology(방법론)**는 특정 작업 하나의 요령이 아니라, 문제 정의부터 설계·구현·검증·통합까지 일을 진행하는 원칙과 순서를 뜻한다. Superpowers는 이 전체 lifecycle을 여러 Skill로 나누고 다시 연결한다.[1]

### 일반 Prompt와 무엇이 다른가

일회성 Prompt로도 “계획을 세우고 TDD로 구현한 뒤 리뷰해줘”라고 요청할 수 있다. 차이는 재사용성과 활성화 구조에 있다.

Superpowers의 `using-superpowers`는 관련 Skill이 적용될 가능성이 조금이라도 있으면 응답이나 파일 탐색보다 먼저 Skill을 불러오도록 요구한다. 사용자 지침이 Skill보다 우선한다는 우선순위도 명시한다.[3]

따라서 다음과 같이 구분하는 것이 정확하다.

- **일반 Prompt**: 이번 대화에서 무엇을 원하는지 전달한다.
- **Superpowers Skill**: 특정 상황에서 어떤 순서와 기준으로 일할지를 재사용 가능한 절차로 제공한다.
- **bootstrap/plugin**: Agent가 그 Skill을 잊지 않고 발견·활성화하도록 연결한다.

중요한 한계도 있다. `MUST`, `HARD-GATE`, `Iron Law` 같은 문구는 강한 규칙처럼 쓰였지만, Markdown 자체가 운영체제 수준에서 코드 수정을 금지하는 것은 아니다. 대부분은 모델이 따라야 하는 **행동 지침**, 즉 soft constraint(모델의 준수에 의존하는 제약)다. 테스트 명령의 종료 코드나 Git 상태처럼 도구로 확인하는 부분은 결정적 증거가 될 수 있지만, Skill을 올바르게 선택하고 규칙을 지키는지는 Agent와 Harness 구현에 달려 있다.

## 2. Superpowers가 Agent의 행동을 어떻게 바꾸는가

### 일반적인 Coding Agent

```text
사용자 요청
→ 관련 파일 탐색
→ 바로 코드 수정
→ 테스트 실행
→ 완료 보고
```

빠르지만 첫 해석이 틀리면 잘못된 구현을 끝까지 밀고 갈 수 있다. 테스트가 통과해도 요구사항 자체를 잘못 이해했을 가능성은 남는다.

### Superpowers의 전체 흐름

```text
사용자 요청
→ 관련 Skill 확인
→ 작업 규모 분류
→ 요구사항·대안·설계 확인
→ 사용자 승인
→ 필요하면 Spec과 구현 Plan 작성
→ 별도 branch/worktree와 깨끗한 테스트 기준선 준비
→ Task별 RED → GREEN → REFACTOR
→ Task별 독립 리뷰
→ 전체 branch 리뷰와 최신 검증
→ merge / PR / 유지 중 사용자가 통합 방법 선택
```

README의 기본 흐름은 `brainstorming → using-git-worktrees → writing-plans → subagent-driven-development 또는 executing-plans → TDD → code review → finishing branch`다.[1]

다만 실제 최신 `brainstorming` Skill은 모든 요청에 똑같이 긴 문서를 요구하지 않는다. 작업을 다음 세 경로로 분류한다.[4]

- **Spike**: 가능성을 빠르게 확인하는 버릴 실험
- **Bounded**: 기존 흐름 안에서 범위가 분명한 작은 수정
- **Architectural**: 새 subsystem이나 외부 interface 등 구조에 영향을 주는 작업

작은 `Bounded` 작업은 짧은 채팅 설계와 승인만 받고 별도 Spec·Plan 문서를 만들지 않는다. 반면 `Architectural` 작업은 질문, 대안 비교, 설계 승인, Spec, Plan을 거친다. 즉 현재 버전은 과정의 크기를 조절하려 하지만, **구현 전 사용자 승인**이라는 gate는 모든 경로에 남긴다.[4]

### 각 Skill이 해결하는 개발상의 문제

| Skill | 해결하려는 문제 | 행동을 바꾸는 핵심 장치 |
| --- | --- | --- |
| `brainstorming` | Agent가 불명확한 요청을 자기 방식으로 완성해 버리는 문제 | 작업 규모를 분류하고 요구사항·대안·설계를 보여준 뒤 사용자 승인 전에는 구현하지 않는다.[4] |
| `writing-plans` | 승인된 설계가 구현 중 사라지고, Task 간 interface나 파일 범위가 어긋나는 문제 | 파일 경로, interface, 테스트, 명령, 기대 결과를 작은 단계로 적고 Spec coverage와 placeholder를 자체 점검한다.[5] |
| `test-driven-development` | 구현 후 테스트가 기존 코드를 그대로 추인하고, 회귀 방지 장치 없이 변경하는 문제 | 실패하는 테스트를 먼저 보고, 최소 구현으로 통과시킨 뒤 중복과 이름을 정리하는 RED–GREEN–REFACTOR를 강제한다.[6] |
| `systematic-debugging` | 원인을 모른 채 여러 수정을 덧붙이는 guess-and-check 문제 | 재현·최근 변경·데이터 흐름을 조사하고, 하나의 가설을 최소 변경으로 검증한 뒤 root cause를 고친다.[7] |
| `using-git-worktrees` | 현재 작업 공간 오염, 병렬 작업 충돌, 원래부터 실패하던 테스트와 새 실패의 혼동 | 기존 격리 여부를 먼저 확인하고 native worktree 기능 또는 Git worktree를 사용하며, 시작 시 baseline test를 실행한다.[8] |
| `subagent-driven-development` | 긴 Context에서 Task가 섞이고 구현자가 자신의 작업을 스스로 승인하는 문제 | Task마다 필요한 Context만 받은 새 구현 Subagent를 쓰고, 별도 reviewer가 Spec 준수와 코드 품질을 검사한다.[9] |
| `requesting-code-review` | 작은 문제를 다음 Task까지 끌고 가 비용이 커지는 문제 | Task·주요 기능·merge 전 시점에 diff와 요구사항을 별도 reviewer에게 전달한다.[10] |
| `receiving-code-review` | Reviewer의 제안을 근거 없이 수용하거나, 반대로 방어적으로 무시하는 문제 | 피드백을 이해하고 현재 codebase에서 검증한 뒤 적용하거나 기술적 근거로 반박한다.[11] |
| `verification-before-completion` | “아마 될 것”이나 Subagent의 성공 보고를 실제 완료로 착각하는 문제 | 완료를 증명할 전체 명령을 새로 실행하고 exit code와 실패 수를 읽기 전에는 성공을 말하지 못하게 한다.[12] |
| `finishing-a-development-branch` | 테스트 통과 이후 merge·push·삭제를 Agent가 임의로 결정하는 문제 | 전체 suite를 다시 확인하고 merge, PR, branch 유지 중 선택권을 사용자에게 돌린 뒤 소유한 worktree만 정리한다.[13] |

`requesting-code-review`와 `receiving-code-review`가 따로 있는 이유도 중요하다. 리뷰를 요청하는 것만으로는 부족하다. Agent가 피드백을 무비판적으로 적용하면 새로운 회귀가 생길 수 있으므로, 피드백 자체도 코드와 테스트에 비추어 검증해야 한다.

## 3. Skill은 어떻게 동작하는가

### 단순 Markdown Prompt인가

Agent Skills 표준에서 최소 Skill은 `SKILL.md` 하나가 든 directory다. 이 파일에는 최소한 `name`, `description` metadata와 Markdown 지침이 들어간다. 필요하면 실행 script, 참고 문서, template, asset도 함께 묶을 수 있다.[14][15]

따라서 답은 **“가장 단순한 형태는 Markdown 지침이 맞지만, Skill이라는 개념 전체가 Markdown 한 장으로 제한되지는 않는다”**다.

```text
skill-name/
├── SKILL.md       필수: metadata + 작업 지침
├── scripts/       선택: 실행 가능한 코드
├── references/    선택: 자세한 참고 자료
└── assets/        선택: template이나 정적 자료
```

Agent Skills는 progressive disclosure, 즉 **단계적 공개** 방식을 사용한다.

1. 세션 시작 시 모든 Skill의 이름과 설명만 보여준다.
2. 작업과 관련된 Skill이 선택되면 `SKILL.md` 전체를 Context에 넣는다.
3. 필요한 경우에만 script나 reference를 추가로 읽는다.[14][16]

이 구조는 Skill을 많이 설치해도 모든 전문 지침을 처음부터 Context에 넣지 않기 위한 것이다.

### 규칙에 더 가까운가

Superpowers의 내용은 설명문보다 **절차 규칙**에 가깝다. 승인 전 구현 금지, 실패를 확인하기 전 production code 금지, fresh verification 전 완료 주장 금지처럼 순서와 gate를 정한다.[4][6][12]

하지만 표준 자체는 “이 지침을 운영체제 수준에서 반드시 집행하라”고 규정하지 않는다. Skill의 발견, Context 주입, 권한, 도구 실행은 client 또는 Harness가 담당한다. 공식 client 구현 가이드도 많은 구현이 모델의 판단으로 관련 Skill을 선택한다고 설명한다.[16]

그러므로 정확한 표현은 다음과 같다.

> Superpowers Skill은 문서 형식으로 저장된 행동 규칙이며, Harness가 이를 Context에 넣고 Agent가 따름으로써 작동한다. 일부 검증은 실제 Tool 결과로 단단하게 만들지만, Skill 문구 자체는 완전한 강제 실행 엔진이 아니다.

### Prompt, Skill, Tool, Agent, Agent Harness의 차이

| 개념 | 쉬운 뜻 | 예시 | 스스로 실행할 수 있는가 |
| --- | --- | --- | --- |
| **Prompt** | 지금 원하는 일을 자연어로 전달하는 입력 | “로그인 오류를 고쳐줘” | 아니오 |
| **Skill** | 특정 종류의 일을 처리하는 재사용 가능한 작업 지침과 자료 | `systematic-debugging/SKILL.md` | 혼자서는 아니오. Agent가 읽고 Tool을 사용한다. |
| **Tool** | Agent가 외부 세계에 실제 행동을 하는 기능 | 파일 읽기, patch, terminal, web search | 호출되면 정해진 기능을 실행한다. |
| **Agent** | 목표와 Context를 보고 다음 행동과 Tool 사용을 결정하는 실행 주체 | Coding Agent, reviewer Subagent | Harness 안에서 실행된다. |
| **Agent Harness** | 모델이 실제 일을 하도록 둘러싼 실행·제어 환경 | Prompt 조립, Tool registry, 권한, session, memory, sandbox, subagent 관리 | Agent loop를 운영한다. |

관계는 다음과 같다.

```text
사용자 Prompt
↓
Agent Harness가 모델, Context, Skill catalog, Tool을 연결
↓
Agent가 관련 Skill을 읽고 다음 행동을 판단
↓
Tool이 파일 수정·테스트·Git 작업을 실제 수행
```

## 4. 사용자가 경험한 개발 흐름과의 연결

사용자가 자주 본 `Analysis → Plan → 구현 → 검증 → Steps`라는 명칭 자체가 Superpowers의 공식 고정 단계는 아니다. 그러나 내용은 상당 부분 직접 연결된다.

| 사용자가 경험한 단계 | Superpowers와의 연결 | 왜 필요한가 |
| --- | --- | --- |
| Analysis | 기능이면 `brainstorming`, 오류면 `systematic-debugging` | 구현 전에 무엇을 만들지 또는 왜 고장 났는지 확정한다. |
| Plan | `writing-plans` | 설계를 파일·Task·테스트 단위의 실행 가능한 지침으로 바꾼다. |
| 구현 | `subagent-driven-development` 또는 `executing-plans` + TDD | 긴 작업을 작은 feedback loop로 나누고 각 변경의 목적을 좁힌다. |
| 검증 | Task review + 전체 review + `verification-before-completion` | 테스트 통과, 요구사항 충족, 코드 품질은 서로 다른 주장이라 각각 확인한다. |
| Steps | 구현 계획의 세부 Step이거나 완료 후 다음 행동 | Superpowers에서는 보통 구현 전에 Plan 안에 들어가며, 완료 후에는 branch 처리 선택이 이어진다. |

브랜치 생성과 Git Worktree 사용도 부가적인 장식이 아니다. Worktree는 같은 Repository의 다른 branch를 별도 directory에서 동시에 열 수 있는 Git 기능이다. 현재 작업 중인 변경을 건드리지 않고 Agent 작업을 격리하며, 여러 Agent가 같은 checkout을 수정해 충돌하는 위험을 줄인다. 시작할 때 test baseline을 확인하면 “이 실패가 내 변경 때문에 생겼는가?”도 구분할 수 있다.[8]

따라서 지금까지 Agent의 추천을 따르며 거친 과정은 무작위 의식이 아니었다. 각 단계는 서로 다른 실패를 겨냥한다.

- Analysis는 **틀린 문제를 푸는 실패**를 막는다.
- Plan은 **합의가 구현에서 유실되는 실패**를 막는다.
- TDD는 **검증할 수 없는 구현과 회귀**를 줄인다.
- Code Review는 **구현자의 시야에 갇힌 결함**을 찾는다.
- 완료 전 검증은 **근거 없는 성공 보고**를 막는다.
- branch 마무리는 **기술적 완료와 Repository 통합을 분리**한다.

## 5. Superpowers의 구체적인 장점

### Agent의 성급한 구현을 막는다

가장 직접적인 장치는 `brainstorming`의 승인 gate다. 최신 Skill은 작은 작업도 설계 길이만 줄일 뿐 승인 자체는 생략하지 않는다.[4] “어떻게 만들까?” 전에 “무엇을 왜 만들까?”를 사용자와 합의하게 한다.

### 요구사항 누락을 줄인다

대화만으로 합의하면 긴 세션이나 Context 압축 뒤에 조건이 사라질 수 있다. `writing-plans`는 Spec의 각 요구사항이 어느 Task에 들어가는지 자체 검토하고, 정확한 interface와 파일 경로를 기록한다.[5] Subagent reviewer는 다시 Spec 준수와 코드 품질을 따로 판정한다.[9]

### 회귀 버그를 줄인다

**회귀 버그(regression bug)**는 새 변경 때문에 과거에 되던 기능이 다시 고장 나는 문제다. TDD로 새 동작을 재현하는 테스트를 만들고, GREEN 단계에서 기존 suite도 함께 실행하면 같은 오류가 돌아오는 것을 감지할 수 있다.[6]

단, 테스트가 잘못된 동작이나 내부 구현만 고정하면 안전망이 아니라 변경 방해물이 된다. 테스트의 존재보다 품질과 수준이 중요하다.

### 테스트가 중요해지는 이유

Superpowers에서 테스트는 마지막 성적표가 아니다. 다음 코드를 작성할 근거이자, 변경 전후의 차이를 보여주는 executable specification, 즉 **실행 가능한 명세** 역할을 한다. Kent Beck도 테스트 하나를 동작 변경에 대한 요청으로 설명하며, TDD의 산출물을 현재 동작과 향후 변경 가능성으로 본다.[19]

### Agent 자신에게 Code Review를 다시 시키는 이유

엄밀히 말하면 같은 작업 Agent가 자기 코드를 그대로 다시 읽는 방식이 아니다. Controller Agent가 구현 Task를 별도 Subagent에게 맡기고, 새로운 Context를 받은 reviewer Subagent가 diff와 요구사항을 검토한다.[9][10]

목적은 다음과 같다.

- 구현 과정에서 생긴 확증편향을 줄인다.
- 전체 대화가 아니라 요구사항과 diff에 집중한다.
- 오류가 다음 Task에 전파되기 전에 찾는다.
- Coordinator의 Context를 구현 세부사항으로 가득 채우지 않는다.

그래도 독립성에는 한계가 있다. 같은 모델 계열이 같은 잘못된 가정을 공유할 수 있고, Agent review는 human review와 동일하지 않다. 보안, 제품 의도, 큰 architecture 결정은 사람이 확인할 필요가 있다.

### Subagent를 사용하는 이유

**Subagent**는 큰 작업을 위임받아 별도 Context에서 수행하는 보조 Agent다. Task 하나에 필요한 자료만 주면 오래된 대화나 다른 Task의 정보에 덜 끌려간다. Coordinator는 전체 Plan과 진행 상태를 유지하고, implementer와 reviewer는 좁은 문제에 집중한다.[9]

대신 호출 비용, 대기 시간, Context 전달 오류가 늘어난다. 서로 강하게 얽힌 Task를 억지로 분리하면 오히려 interface 불일치가 생길 수 있다.

### Git Worktree를 쓰는 이유

- main branch와 현재 사용자의 미완성 변경을 보호한다.
- 여러 기능 branch를 별도 directory에서 병렬로 진행한다.
- Agent가 만든 파일과 dependency 설치를 격리한다.
- 시작 전 baseline과 완료 후 변경 결과를 비교하기 쉽다.

그러나 작은 순차 수정 하나에는 일반 branch만으로 충분할 수 있고, dependency 설치가 무거운 프로젝트에서는 worktree마다 setup 비용이 생긴다.

## 6. 단점과 주의점

### 작은 수정에도 과정이 길어질 수 있다

현재 `brainstorming`은 Spike·Bounded·Architectural 경로로 ceremony, 즉 **절차적 형식의 양**을 조절한다. 이는 과한 절차 문제를 인식한 설계로 보인다. 그래도 모든 작업의 승인 gate, 폭넓은 TDD 기본값, Task별 review는 한 줄 수정에 비싸게 느껴질 수 있다.[4][6][9]

### 테스트가 과도하게 늘어날 수 있다

Superpowers TDD Skill은 새 함수와 동작에 테스트를 강하게 요구하지만, suite 전체에서 중복 테스트를 주기적으로 줄이는 별도 workflow는 핵심 흐름에 없다. TDD Skill의 REFACTOR는 중복 제거를 말하지만 보통 현재 작은 cycle에 집중한다.[6]

여러 계층에서 같은 동작을 반복 검증하면 테스트 수도 늘고 변경할 테스트도 많아진다. Practical Test Pyramid는 테스트 작성·실행·유지에도 비용이 있으며, 낮은 수준에서 이미 확인한 동작을 높은 수준에서 그대로 반복하지 말라고 경고한다.[20]

### Agent가 프로세스를 기계적으로 따를 수 있다

Skill 곳곳의 `MUST`, “no exceptions”, “delete and start over”는 모델이 지름길을 합리화하지 못하게 하는 데 효과적이다. 반대로 상황 판단을 눌러 throwaway code, generated code, config 변경에도 과한 테스트와 문서를 만들 수 있다. 공식 Skill도 이들을 예외 후보로 두지만 사용자의 허락을 요구한다.[6]

### 큰 Codebase에서는 복잡성이 늘 수 있다

Codebase가 커지면 Spec, Plan, Task brief, progress ledger, test suite, review report 사이의 일관성을 유지하는 비용이 증가한다. Task가 독립적이지 않은데 Subagent별로 분리하면 각 Agent가 전체 영향을 놓칠 수 있다. Plan이 작성된 뒤 codebase가 바뀌면 정확했던 파일 경로나 interface도 낡는다.

### 개발 지식이 부족하면 Agent 판단을 그대로 받아들이기 쉽다

설계와 Plan을 보여주는 것만으로 사용자가 올바르게 승인할 수 있는 것은 아니다. 초보자는 “테스트 300개 통과”, “review clean” 같은 문구를 품질의 전부로 오해할 수 있다.

이를 줄이려면 Agent에게 결론만 요구하지 말고 다음을 함께 요구하는 편이 좋다.

- 대안 2~3개와 각각의 trade-off
- 이번 변경에서 지키려는 사용자 동작
- 삭제하거나 유지한 legacy code의 근거
- 새 테스트가 잡는 실제 실패
- 리뷰에서 확신하지 못한 부분
- 테스트 밖에서 사람이 확인할 acceptance scenario

### 언제 전체 Superpowers 흐름을 쓰지 않는 편이 나은가

- 보존하지 않을 feasibility spike
- 자동 생성 코드 자체를 수정하는 작업
- 동작 없는 문구·주석·단순 설정 변경
- 테스트 Harness 구축 비용이 변경 위험보다 훨씬 큰 일회성 script
- UI 감각, 생성형 결과, 탐색적 data analysis처럼 assertion을 먼저 고정하기 어려운 탐색
- 장애 확산을 막는 긴급한 임시 조치

마지막 경우에도 임시 조치 후 root cause 조사와 회귀 테스트는 필요하다. 핵심은 “Superpowers를 쓴다/안 쓴다”의 이분법이 아니라, 작업 위험과 불확실성에 맞게 필요한 Skill과 gate를 선택하는 것이다.

## 7. 테스트와 코드가 함께 커진 경험 분석

### 먼저 판단

이 현상을 **Superpowers 하나의 결함**으로 보기도, **TDD를 잘못했다는 개인의 실수**로만 보기도 어렵다. 다음 네 요인이 겹칠 수 있다.

| 원인 | 어떻게 누적을 만들 수 있는가 | 판단 |
| --- | --- | --- |
| Superpowers의 기본 성향 | test-first, 모든 기존 test 통과, 최소 변경, 관련 없는 refactor 금지가 보수적인 추가 작업으로 이어질 수 있다. | 일부 기여 가능 |
| TDD 적용 방식 | RED와 GREEN만 반복하고 REFACTOR에서 중복·불필요한 구조를 줄이지 않으면 생산 코드와 테스트가 계속 늘어난다. | 흔한 핵심 원인 |
| Agent에게 준 기준 | “기존 코드와 테스트를 모두 보존하라”만 있고 삭제·대체 조건이 없으면 Agent는 호환 branch와 test를 추가하는 쪽을 택한다. | 직접적인 원인 가능성이 큼 |
| 자연스러운 기술 부채 | 요구사항과 이해가 변하면 한때 합리적이던 abstraction과 test가 낡는다. | 어느 프로젝트에서나 발생 가능 |

**기술 부채(technical debt)**는 내부 품질의 부족 때문에 이후 변경에 추가 비용이 드는 상태를 빚에 비유한 말이다. Fowler는 Codebase가 변화하며 이런 cruft가 쌓이기 쉽고, 자주 수정하는 영역부터 점진적으로 갚는 접근을 설명한다.[23]

### Superpowers 자체의 문제인가

일부는 그렇다. Superpowers는 Agent의 성급한 삭제보다 보수적인 보존을 선호하고, TDD Skill은 “새 production code 전에 실패 테스트”를 매우 강하게 요구한다.[6] 이는 개별 변경 안전성에는 유리하지만 다음 질문을 자동으로 해결하지는 않는다.

- 기존 기능이 아직 실제로 쓰이는가?
- 오래된 테스트가 현재 요구사항인가?
- 같은 위험을 여러 test layer가 중복 검증하는가?
- 테스트만 호출하는 production API가 남아 있는가?
- 과거 compatibility가 이제 제거 가능한가?

즉 Superpowers는 **변경을 안전하게 추가하는 방법**은 자세히 제공하지만, 제품 전체의 기능·테스트 portfolio를 주기적으로 줄이는 정책까지 대신 정하지는 않는다.

### TDD를 잘못 적용한 문제인가

RED–GREEN만 하고 REFACTOR를 사실상 생략했다면 TDD의 일부만 사용한 것이다. Superpowers 자체도 GREEN 뒤에 duplication 제거, naming 개선, helper 추출을 요구한다.[6] Kent Beck의 설명에서도 마지막 단계는 duplication과 excess complexity를 제거하는 것이다.[19]

하지만 “매 cycle마다 무조건 큰 구조 변경을 해야 한다”는 뜻도 아니다. 다음 변경을 어렵게 하는 구조가 보일 때 작은 정리를 하고, 더 큰 정리는 별도 위험 평가와 Plan으로 다루는 편이 맞다.

테스트도 refactoring 대상이다.

- 같은 동작을 반복하는 test는 합칠 수 있다.
- 내부 method 호출 순서를 고정한 test는 public behavior 중심으로 다시 쓸 수 있다.
- 요구사항이 삭제되면 해당 test도 삭제할 수 있다.
- parameter 조합만 다른 test는 table-driven test 등으로 정리할 수 있다.

Google의 Unit Testing 장은 유지 가능한 테스트를 “실제 버그가 있을 때 분명한 원인으로 실패하는 테스트”로 설명한다. 무관한 내부 변경에 깨지는 brittle test가 누적되면 suite가 커질수록 유지 비용이 증가한다.[21]

### Agent에게 삭제·리팩토링 기준을 주지 않은 문제인가

가능성이 크다. Agent에게 “기존 test를 모두 통과시켜라”라고만 하면 오래된 test도 현재의 절대 요구사항으로 해석하기 쉽다. “diff를 작게 유지하라”와 결합되면 기존 구조를 대체하기보다 새로운 adapter, fallback, compatibility path를 옆에 추가하게 된다.

앞으로는 다음처럼 기준을 명시할 수 있다.

```text
현재 사용자 요구사항과 공개 contract를 기준으로 판단한다.
기존 테스트를 무조건 보존하지 않는다.
요구사항이 사라졌거나 중복된 테스트는 근거를 제시하고 삭제한다.
테스트만 유일하게 호출하는 production code는 dead code 후보로 조사한다.
Refactor 단계에서 새로 생긴 중복과 더 이상 필요 없는 compatibility path를 점검한다.
삭제 전에는 실제 호출부, public API, migration 필요성, 상위 수준 acceptance test를 확인한다.
```

이렇게 해야 Agent의 목표가 “모든 과거 artifact 보존”에서 “현재 필요한 behavior 보존”으로 바뀐다.

### 자연스러운 기술 부채인가

일부는 자연스럽다. 제품과 개발자의 이해가 바뀌면 예전 설계가 지금의 최선이 아니게 된다. 기술 부채가 생겼다는 사실만으로 과거 결정이 무조건 잘못된 것은 아니다. 문제는 부채를 관찰하지 않고 계속 이자를 내는 상태다.[23]

Google의 대규모 dead code 삭제 사례는 테스트 실행 여부를 production code의 생존 신호로 사용할 수 없다고 설명한다. Test infrastructure는 실제 사용되지 않는 library의 test도 계속 실행하기 때문에, test가 돈다는 사실만으로 해당 library가 살아 있다고 볼 수 없다.[22]

### “테스트가 존재한다 = 그 코드는 반드시 필요하다”인가

**아니다.** 테스트는 “이 동작을 지켜야 한다”는 주장이지, 그 주장이 현재도 옳다는 보증서가 아니다.

다음처럼 나누어 판단해야 한다.

| 테스트 상태 | 대응 |
| --- | --- |
| 현재 사용자 요구사항이나 public contract를 유일하게 보호한다 | 유지 |
| 중요한 bug를 재현하며 같은 위험을 다른 test가 잡지 못한다 | 유지 |
| 내부 구현 구조나 mock 호출 순서만 고정한다 | behavior 중심으로 다시 작성하거나 삭제 검토 |
| 다른 낮은 수준 test와 완전히 중복된다 | 더 빠르고 원인이 분명한 test만 남길지 검토 |
| 제거된 기능이나 폐기된 compatibility를 요구한다 | production code와 함께 삭제 |
| 무엇을 보호하는지 아무도 설명하지 못한다 | history, 호출부, 장애 기록을 조사한 뒤 유지·재작성·삭제 결정 |

삭제 전에는 다음 질문을 순서대로 확인하는 것이 안전하다.

1. 이 테스트가 표현하는 behavior는 현재 요구사항인가?
2. 실제 production 호출부나 외부 사용자가 있는가?
3. 이 test가 없으면 어떤 현실적인 bug를 놓치는가?
4. 같은 위험을 더 낮고 빠른 test가 이미 검증하는가?
5. 내부 구조만 바꿔도 실패한다면 abstraction level이 너무 낮지 않은가?
6. code와 test를 함께 제거했을 때 상위 acceptance scenario는 여전히 충족되는가?

YAGNI는 미래에 쓸지도 모르는 기능을 미리 만들지 말라는 원칙이다. Fowler는 이것이 refactoring과 code health를 소홀히 하라는 뜻은 아니며, 오히려 변경하기 쉬운 Codebase가 전제라고 설명한다.[24]

### AI Agent가 TDD를 한다는 것의 추가 한계

사람이 테스트를 먼저 설계하는 TDD와, 같은 Agent가 요구사항·테스트·구현·검증을 모두 작성하는 방식은 같지 않다. Agent가 요구사항을 잘못 이해하면 test와 production code가 같은 오해를 공유하면서 모두 통과할 수 있다.

최근 Thoughtworks의 Birgitta Böckeler가 수행한 소규모 탐색에서는 완전한 Agent 내부 TDD와 비-TDD 결과 사이에 분명한 품질 차이가 나오지 않았고, 일부 결과는 비-TDD가 더 높게 평가됐다.[25] 이것은 제한된 greenfield task와 특정 모델에 대한 exploratory experiment이므로 “TDD가 무의미하다”는 일반 증거는 아니다. 다만 **Agent가 RED를 봤다는 사실만으로 사용자가 원한 behavior를 검증했다고 볼 수 없다**는 경고로는 유용하다.

따라서 사람의 가장 가치 있는 개입 지점은 모든 코드를 직접 쓰는 것이 아니라 다음일 수 있다.

- 구현 전에 acceptance criteria를 검토한다.
- 첫 failing test가 정말 원하는 실패를 표현하는지 확인한다.
- Reviewer에게 현재 요구사항과 삭제 가능 범위를 전달한다.
- 주기적으로 test suite와 dead code를 기능 portfolio 관점에서 정리한다.

## 8. Hermes Agent와의 관계

Superpowers README에는 Hermes Agent 지원이 명시되어 있고, `hermes plugins install obra/superpowers --enable` 설치 명령도 제공한다. 다만 매우 긴 세션에서 첫 turn의 bootstrap이 Context compaction으로 사라지면 Skill triggering이 약해질 수 있으므로 새 session을 시작하라는 현재 제한도 함께 적혀 있다.[1]

Hermes 공식 문서는 Hermes를 자율 Agent로 설명하며, Tool, memory, subagent, Skill, plugin 같은 실행 기능을 제공한다고 밝힌다. Agent Skills open format과의 호환도 명시한다.[17]

따라서 사용자의 이해는 방향은 맞지만 다음처럼 수정하면 더 정확하다.

```text
LLM
↓ 추론
Hermes Agent
├─ Agent loop
├─ Context·session·memory
├─ Tool 실행과 권한
├─ Subagent
├─ Skill loader
└─ Plugin system
    ↓
Superpowers plugin
├─ using-superpowers bootstrap
└─ 개발 방법론을 구성하는 여러 Skill
```

- **“Hermes = Agent”**: 제품 수준 설명으로 맞다.
- **더 정확한 설명**: Hermes는 Agent를 실행하는 framework이자 Harness 역할까지 포함한다.
- **“Superpowers = Skill”**: 하나의 Skill이라고 하면 부족하다.
- **더 정확한 설명**: Superpowers는 여러 Skill, bootstrap, platform adapter를 묶어 Hermes 위에 개발 방법론을 추가하는 plugin이다.

즉 Hermes가 손과 작업장, 기억, Tool을 제공한다면 Superpowers는 소프트웨어 개발 일을 어떤 순서와 검증 기준으로 진행할지 제공한다.

## 9. ECC와의 짧은 비교

Superpowers와 ECC가 비슷해 보이는 이유는 둘 다 기존 Coding Agent에 설치되고, Skill을 중심으로 Plan·Test·Implement·Review·Verify 흐름을 반복 가능하게 만들기 때문이다.[18]

범위는 다르다.

| Superpowers | ECC |
| --- | --- |
| 비교적 적은 수의 Skill을 강하게 연결한 개발 방법론 | 매우 넓은 Skill catalog와 전문 Agent, command, rule, hook, memory, MCP 설정, 보안·설치·동기화 도구를 함께 제공 |
| 핵심 질문은 “개발을 어떤 순서로 진행할까?” | 핵심 범위에 “여러 Harness를 어떻게 설정·확장·운영할까?”까지 포함 |
| 일관된 기본 Workflow가 중심 | 필요한 module을 선택해 여러 Harness에 투영하는 성격이 강함 |

따라서 **“ECC가 Superpowers보다 Agent Harness에 가깝다”는 상대 비교는 타당**하다. 하지만 ECC 자체를 완전한 Harness라고 부르면 과장이다. 실제 모델 loop와 Tool 실행은 Claude Code, Codex, Hermes 같은 기존 Harness가 담당한다.

더 정확한 표현은 다음과 같다.

> ECC는 Agent Harness 자체라기보다 여러 Harness 위에 Skill, Agent, Rule, Hook, Memory, MCP와 운영 도구를 설치하고 조정하는 harness engineering layer 또는 종합 toolkit에 가깝다.[18]

ECC의 상세 구성과 각 platform의 지원 차이는 별도 글에서 다루는 편이 좋다.

## 10. 전체 평가

Superpowers의 가장 큰 가치는 Agent를 더 영리하게 만드는 데 있지 않다. **Agent가 흔히 실패하는 지점마다 멈춤, 기록, 격리, 독립 검토, 실행 증거를 배치한다는 것**에 있다.

```text
성급한 구현       → brainstorming 승인 gate
요구사항 유실     → Spec과 writing-plans
병렬 작업 충돌    → worktree
구현 편향         → failing test와 별도 reviewer
추측성 debugging  → root cause 조사
허위 완료         → fresh verification
임의 통합·삭제    → 사용자 선택과 안전한 branch 마무리
```

그러나 이 방법론의 성공 기준은 “모든 Skill을 빠짐없이 실행했는가?”가 아니다. 최종적으로 필요한 behavior를 더 정확히 구현했고, 불필요한 code와 test를 덜 남겼으며, 사용자가 중요한 판단을 이해하고 통제할 수 있었는지가 기준이어야 한다.

특히 사용자의 경험에서 다음 교훈이 중요하다.

> TDD는 테스트를 계속 추가하는 방법이 아니라, 작은 behavior를 안전하게 만들고 그 안전망 아래에서 code와 test의 불필요한 부분을 계속 정리하는 feedback loop다.

Superpowers는 그 loop를 시작하고 지키는 데 강하지만, 현재 요구사항에서 빠진 기능과 obsolete test를 언제 버릴지는 사용자의 제품 판단과 별도 유지보수 기준이 필요하다.

## [오늘 글에서 꼭 다룰 내용]

1. 이미 사용하던 `Analysis → Plan → TDD → Review → Verification`이 Superpowers의 개발 방법론이었다는 점
2. Superpowers가 Skill 모음이 아니라 전체 개발 lifecycle의 순서와 gate를 연결한 package라는 점
3. `brainstorming`이 Agent의 즉시 구현을 막고 사용자 승인을 요구하는 이유
4. `writing-plans`, TDD, Code Review가 각각 요구사항 유실·회귀·구현 편향이라는 다른 문제를 해결한다는 점
5. Skill은 Markdown에서 시작하지만 Harness의 Skill loader와 Tool이 있어야 실제 행동으로 이어진다는 점
6. Worktree와 Subagent가 속도 기능이 아니라 격리와 Context 관리 장치라는 점
7. 테스트가 존재한다고 해당 code가 영구히 필요한 것은 아니라는 점
8. Superpowers는 품질 보증 장치이지, 좋은 설계와 사용자 판단을 대신하는 자동 정답기가 아니라는 점

## [너무 깊어서 다음 글로 넘길 내용]

- RED–GREEN–REFACTOR를 실제 project에서 어느 크기로 반복할지
- behavior test와 implementation-detail test를 구분하는 구체적 사례
- test pyramid, mutation testing, coverage를 이용한 test suite 다이어트
- Subagent별 model 선택, Context 전달, review loop의 비용 분석
- Worktree 기반 병렬 개발의 Git 명령과 정리 절차
- Hermes에서 Superpowers bootstrap과 Context compaction이 작동하는 방식
- ECC의 Agent, Rule, Hook, Memory, MCP, 보안 scanner 전체 구조

## [내가 직접 사용하면서 확인해볼 실험]

1. 같은 작은 기능을 `Bounded` 경로와 전체 Spec·Plan 경로로 각각 수행해 시간, token, 수정 파일 수, 결함 수를 비교한다.
2. 다음 TDD 작업에서 RED–GREEN 뒤에 “불필요한 production code와 중복 test를 찾는 REFACTOR checklist”를 넣고 최종 line/test 수 변화를 기록한다.
3. 오래된 기능 하나를 골라 production 호출부, public contract, 관련 test를 추적한 뒤 code와 test를 함께 제거해 full suite와 acceptance scenario를 실행한다.
4. 같은 Task를 구현 Agent의 self-review와 별도 reviewer Subagent review에 맡겨 발견 항목의 차이를 비교한다.
5. 동일 기능에서 엄격한 Agent 내부 TDD와 사람이 failing acceptance test를 먼저 승인하는 방식을 비교한다.

## [글 제목 후보]

1. 쓰고 있던 Superpowers를 다시 읽어보니, Agent가 바로 코딩하지 않은 이유
2. Analysis부터 Code Review까지: Superpowers가 Coding Agent의 순서를 바꾸는 방법
3. 테스트가 많으면 안전할까? Superpowers와 TDD를 사용하며 생긴 질문
4. Superpowers는 Skill 모음이 아니라 개발 방법론이었다
5. Agent의 추천을 따라가던 개발에서, 각 단계의 이유를 이해하는 개발로

## Sources

[1] obra/superpowers README

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/README.md>
[2] Superpowers: How I'm using coding agents in October 2025

<https://blog.fsck.com/2025/10/09/superpowers>
[3] Superpowers using-superpowers bootstrap Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/using-superpowers/SKILL.md>
[4] Superpowers brainstorming Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/brainstorming/SKILL.md>
[5] Superpowers writing-plans Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/writing-plans/SKILL.md>
[6] Superpowers test-driven-development Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/test-driven-development/SKILL.md>
[7] Superpowers systematic-debugging Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/systematic-debugging/SKILL.md>
[8] Superpowers using-git-worktrees Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/using-git-worktrees/SKILL.md>
[9] Superpowers subagent-driven-development Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/subagent-driven-development/SKILL.md>
[10] Superpowers requesting-code-review Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/requesting-code-review/SKILL.md>
[11] Superpowers receiving-code-review Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/receiving-code-review/SKILL.md>
[12] Superpowers verification-before-completion Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/verification-before-completion/SKILL.md>
[13] Superpowers finishing-a-development-branch Skill

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/skills/finishing-a-development-branch/SKILL.md>
[14] Agent Skills: What are skills?

<https://agentskills.io/what-are-skills>
[15] Agent Skills Specification

<https://agentskills.io/specification>
[16] How to add skills support to your agent

<https://agentskills.io/client-implementation/adding-skills-support>
[17] Hermes Agent Documentation

<https://hermes-agent.nousresearch.com/docs>
[18] affaan-m/ECC README

<https://github.com/affaan-m/ECC/blob/5064474d4d762dc9640234a41617cccb79185cec/README.md>
[19] TDD is Kanban for Code

<https://newsletter.kentbeck.com/p/tdd-is-kanban-for-code>
[20] The Practical Test Pyramid

<https://martinfowler.com/articles/practical-test-pyramid.html>
[21] Software Engineering at Google: Unit Testing

<https://abseil.io/resources/swe-book/html/ch12.html>
[22] Sensenmann: Code Deletion at Scale

<https://testing.googleblog.com/2023/04/sensenmann-code-deletion-at-scale.html>
[23] Technical Debt

<https://martinfowler.com/bliki/TechnicalDebt.html>
[24] Yagni

<https://martinfowler.com/bliki/Yagni.html>
[25] TDD inside the agent loop - theater or actual value?

<https://martinfowler.com/articles/exploring-gen-ai/tdd-in-the-agent-loop.html>
