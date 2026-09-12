---
title: "Hermes Agent 학습용 분석"
repository: "NousResearch/hermes-agent"
url: "https://github.com/NousResearch/hermes-agent"
category: "ai-agent"
created: "2026-09-12"
status: "starred"
star_reason: "Model과 Agent의 차이, 그리고 Session을 넘어 이어지는 Memory 구조를 실제 Agent 환경에서 이해하기 위해"
tags:
  - ai-agent
  - agent-harness
  - coding-agent
  - llm
  - memory
  - agent-skills
---

# Hermes Agent 학습용 분석

## 조사 기준

이 문서는 `NousResearch/hermes-agent`의 조사 시점 최신 `main` 커밋
`99d7f17a1c7449e170f2aa3eb6c451b2f5172dfa`와 공식 문서를 기준으로 작성했다.

README에 나온 기능 이름만 옮기지 않고,
Agent loop, Tool registry, Memory, Session 저장소, Context 압축,
Provider 선택 구조와 실제 관련 파일을 함께 확인했다.

Repository에 명시된 내용은 **확인된 사실**로,
그 내용을 내가 사용하려는 방식에 적용한 평가는 **내 해석**으로 구분한다.

## 0. 이 Repository를 확인하는 이유

처음에는 AI Model과 AI Agent의 차이를 명확하게 구분하지 못했다.
좋은 AI를 사용한다는 것은 곧 성능이 좋은 Model을 선택하는 일이라고 생각했다.

그러다 먼저 실무를 경험하고 있던 산업기능요원 개발자가
자신은 좋은 Model만 찾는 것이 아니라,
Agent를 개발하거나 좋은 Agent 환경을 찾아 활용한다는 이야기를 하는 것을 들었다.
그 말을 계기로 같은 Model을 사용하더라도
그 Model이 어떤 도구와 기억, 실행 환경 안에서 움직이는지에 따라
실제 작업 경험이 달라질 수 있다는 점에 관심이 생겼다.

이후 다른 사람에게 Hermes Agent가 괜찮다는 추천도 받았다.
Coding Agent와 범용 Agent를 더 찾아보던 중 Hermes Agent를 확인했고,
여러 Tool, Skills, Memory, 과거 대화 검색,
Session을 넘어 이어지는 기억과 다양한 Model Provider를 지원한다는 점이 눈에 들어왔다.

그중에서도 가장 궁금했던 것은 **Session을 새로 만들어도 이전 작업을 기억하고 이어갈 수 있는가**였다.
나는 작업 Session이 길어지고 새 대화를 자주 만드는 편이라,
한 대화의 Context가 끝날 때마다 배경과 선호를 처음부터 다시 설명해야 하는 문제가 컸다.

그래서 Hermes Agent를 Star하고,
실제로 GitHub Repository를 조사해 Markdown 학습 기록을 만드는 Agent로 사용하기 시작했다.
이번 분석에서 확인하고 싶은 질문은 다음과 같다.

> Model과 Agent는 정확히 무엇이 다르고,
> Hermes Agent는 Model 위에서 어떤 역할을 하며,
> 왜 Model 호출만으로는 부족하고 Agent 환경이 필요한가?

## 1. Hermes Agent란 무엇인가

### Hermes Model, Nous Research, Hermes Agent의 관계

세 이름은 관련되어 있지만 같은 대상을 뜻하지 않는다.
Nous Research의 공식 Release 목록도 Hermes 4 계열은 `MODEL`,
Hermes Agent는 `AGENT`로 분리해 표시한다.[1]

| 이름 | 무엇인가 | 관계 |
| --- | --- | --- |
| **Nous Research** | AI Model, 학습 방법, Agent와 관련 도구를 연구·개발하는 조직 | Hermes Model 계열과 Hermes Agent를 만든 주체 |
| **Hermes Model** | Nous Research가 공개한 LLM 계열 | 텍스트를 이해하고 생성하는 두뇌에 해당 |
| **Hermes Agent** | LLM에 Tool, Memory, Session, Skills, 실행 Loop를 연결하는 오픈소스 Agent 환경 | Hermes Model을 포함한 여러 Model을 사용할 수 있는 별도 Software |

따라서 다음 등식은 틀렸다.

```text
Hermes Agent = 특정 Hermes Model
```

더 정확한 관계는 다음과 같다.

```text
Nous Research
├── Hermes Model 계열: LLM
├── Nous Portal: Model inference와 Hosted tool을 묶어 제공하는 선택적 Gateway
└── Hermes Agent: 여러 LLM을 실행하고 도구를 사용하게 하는 Agent/Harness
```

### Hermes Agent의 공식적인 정체성

Hermes Agent README는 이를 사용자의 컴퓨터에서 실행되고,
기억을 유지하며, Tool을 사용하고, Skills를 학습하는 Agent로 소개한다.[2]

구조적으로는 **Agent이면서 Agent Harness**라고 보는 것이 가장 정확하다.

- **Agent**인 이유: 목표를 받아 다음 행동을 선택하고 Tool 결과를 보고 다시 판단한다.
- **Harness**인 이유: Model 호출, Tool schema, Session, Memory, 권한, Provider, Context 관리와 UI를 하나의 실행 환경으로 묶는다.
- **Framework**인 이유: Plugin, Skill, MCP, Memory provider와 Context engine을 확장할 수 있다.

이름이 같다고 Hermes Agent가 Hermes Model에 최적화된 전용 실행기인 것도 아니다.
Nous Portal 문서는 Hermes 4를 Chat과 추론에 강한 Model로 설명하면서도,
빠른 Tool calling loop에는 맞지 않아 Hermes Agent의 주 Model로 권장하지 않는다고 명시한다.[3]
이 점은 Hermes Model과 Hermes Agent가 결합된 한 제품이 아니라는 사실을 더 분명하게 보여준다.

### Hermes Agent가 실제로 여러 Model을 사용하는가

확인 결과 그렇다.
Hermes는 Nous Portal, OpenAI Codex, Anthropic, OpenRouter, Gemini,
DeepSeek, xAI, AWS Bedrock, Vertex AI, LM Studio, Custom Endpoint 등
여러 Provider 경로를 지원한다.
`hermes model`로 Provider를 설정하고,
대화 중에는 `/model`로 이미 설정된 Provider와 Model 사이를 전환할 수 있다.[2]

여기서 **Provider**는 Model을 실제 API나 로컬 Server로 제공하는 주체 또는 접속 경로다.
같은 Model 이름도 어느 Provider를 통하느냐에 따라 인증, 가격, Context 크기와 API 형식이 달라질 수 있다.

중요한 점은 Hermes Agent의 능력이 Model과 완전히 독립적이지는 않다는 것이다.
Harness가 같아도 Model의 추론 능력, Tool calling 정확도와 Context 길이에 따라 결과가 달라진다.
Hermes는 두뇌를 교체할 수 있게 하지만,
모든 두뇌가 같은 품질로 Agent 작업을 수행하게 만들지는 않는다.

## 2. Model과 Agent의 차이

### 먼저 한 문장으로 구분하기

- **Model**은 입력을 받아 다음 출력과 행동 후보를 생성한다.
- **Agent**는 Model의 판단을 실제 행동과 관찰의 반복으로 연결한다.
- **Agent Harness**는 이 반복이 안전하고 지속적으로 실행되도록 주변 환경을 제공한다.

### 초보자를 위한 비유

`Model = 두뇌`, `Agent = 일을 수행하는 사람`이라는 비유는 출발점으로 유용하다.
다만 Agent 안에도 Model이 포함되므로,
사람과 두뇌가 완전히 분리된 제품이라고 생각하면 오해가 생긴다.

| 개념 | 쉬운 비유 | 실제 의미 |
| --- | --- | --- |
| **LLM / Model** | 두뇌 | 입력을 해석하고 텍스트 또는 Tool 호출을 생성하는 신경망 |
| **Agent** | 목표를 받고 일하는 사람 | Model을 이용해 다음 행동을 고르고 결과를 관찰하며 목표까지 반복하는 Software |
| **Tool** | Terminal, Browser, 편집기 같은 도구 | Agent가 외부 세계를 읽거나 변경하기 위해 호출하는 함수 |
| **Skill** | 작업 매뉴얼 | 특정 상황에서 어떤 순서와 기준으로 일할지 적은 재사용 가능한 지침과 자료 |
| **Memory** | 오래 남길 메모와 경험 | 다음 Session에도 제공할 선별된 정보 또는 검색 가능한 과거 기록 |
| **Context** | 지금 책상 위에 펼쳐 둔 자료 | 현재 Model 호출에 실제로 포함되는 지시, 대화, Tool 결과와 기억 |
| **Agent Loop** | 생각 → 행동 → 결과 확인의 반복 | Model 호출, Tool 실행, 결과 추가, 재호출을 목표 완료까지 반복하는 제어 흐름 |
| **Agent Harness** | 사무실과 업무 시스템 전체 | Model, Tool, 권한, Session, Memory, Context, UI와 Provider를 묶는 실행 환경 |

### Model 호출과 Agent 실행의 차이

Model만 한 번 호출하면 흐름은 대체로 여기서 끝난다.

```text
질문
→ Model이 답변 생성
→ 종료
```

Agent는 외부 상태를 확인하고 행동을 반복한다.

```text
목표 전달
→ Model이 다음 행동 선택
→ Tool 실행
→ 실제 결과를 Context에 추가
→ Model이 결과를 해석
→ 다음 Tool 실행 또는 최종 답변
```

예를 들어 “이 Repository를 분석해 문서로 저장해줘”라는 요청에서
Model은 설명문 초안을 만들 수 있다.
반면 Agent는 Repository를 Clone하거나 Web에서 읽고,
실제 파일 구조를 검색하고,
Markdown 파일을 작성한 뒤,
Git diff와 문서 형식을 검증할 수 있다.

Hermes의 `AIAgent`는 System prompt와 Tool schema를 구성하고,
Provider에 요청을 보낸 뒤,
응답에 Tool call이 있으면 실행 결과를 대화에 추가해 다시 Model을 호출한다.[4][5]
이 반복 구조가 Model을 작업 수행 Agent로 바꾸는 핵심이다.

### 각 요소가 없을 때 생기는 차이

- Tool이 없으면 Model은 외부 정보를 직접 확인하거나 파일을 변경하지 못한다.
- Skill이 없으면 매번 작업 방법을 Prompt로 다시 설명하거나 Model의 즉흥적인 절차에 맡겨야 한다.
- Memory가 없으면 새 Session마다 사용자 선호와 환경 정보를 다시 전달해야 한다.
- Session history가 없으면 긴 작업의 정확한 과정을 복원하기 어렵다.
- Context 관리가 없으면 대화가 길어질수록 Context window를 초과하거나 중요한 정보가 밀려난다.
- Harness가 없으면 이 기능을 사용자가 직접 API 코드로 연결하고 저장·권한·복구까지 구현해야 한다.

## 3. Hermes Agent 전체 구조

### 전체 흐름

Repository의 Architecture 문서와 실제 구조를 요약하면 다음과 같다.[4]

```text
사용자
  ↓
CLI / TUI / Desktop / Messaging / ACP / API
  ↓
AIAgent
  ├── Prompt Builder
  ├── Model Provider Resolver
  ├── Agent Loop
  ├── Context Engine
  └── Tool Dispatcher
        ├── Terminal / File / Git
        ├── Web / Browser
        ├── Skills / Memory / Session Search
        ├── Subagent
        ├── MCP
        └── Cron
  ↓
Session DB + Memory files + Workspace files
```

### 요청한 구성 요소의 실제 존재 여부

| 구성 요소 | 확인 결과 | 구현 또는 동작 |
| --- | --- | --- |
| **Agent loop** | 있음 | `agent/conversation_loop.py`, `agent/turn_*.py`에서 Model 응답과 Tool 결과를 반복 처리 |
| **Tool calling** | 있음 | `model_tools.py`, `tools/registry.py`, `agent/tool_executor.py`가 Tool schema와 실행을 연결 |
| **Skills** | 있음 | `~/.hermes/skills/`의 `SKILL.md`를 필요할 때 `skill_view`로 불러옴 |
| **Memory** | 있음 | `MEMORY.md`, `USER.md`, Memory tool, 선택 가능한 외부 Memory provider |
| **Session** | 있음 | 대화마다 ID, 제목, Provider, Model, 작업 경로와 메시지를 저장 |
| **Conversation history** | 있음 | `state.db`에 사용자·Assistant·Tool 메시지와 결과를 저장하고 resume 가능 |
| **Persistent memory** | 있음 | Profile별 파일 Memory를 새 Session의 System prompt에 주입 |
| **Terminal** | 있음 | Local, Docker, SSH와 여러 Sandbox backend 지원 |
| **Web** | 있음 | `web_search`, `web_extract`로 검색과 문서 추출 |
| **Browser** | 있음 | Local 또는 Cloud Browser를 탐색·클릭·입력·Screenshot 분석에 사용 |
| **File 접근** | 있음 | 읽기, 검색, 새 파일 작성, Patch 지원 |
| **Subagent** | 있음 | 독립 Context를 가진 Child Agent를 병렬 또는 순차 작업에 사용 |
| **MCP** | 있음 | Local stdio 또는 Remote HTTP MCP Server의 Tool을 발견해 등록 |
| **Scheduling** | 있음 | `cronjob` Tool과 `hermes cron`으로 일회성·반복 작업 관리 |
| **Model provider** | 있음 | 다양한 Cloud, OAuth, OpenAI-compatible, Local endpoint 지원 |
| **Context 관리** | 있음 | Token 사용량 추적, Tool 결과 정리, 요약 압축, Session search 복구 포인터 사용 |

Hermes의 Tool은 Web, Terminal과 File, Browser, Media,
Agent orchestration, Memory, Scheduling과 Integration Toolset으로 묶여 있다.
Toolset은 실행 Surface별로 켜거나 끌 수 있다.[6]

### Tool calling은 어떻게 동작하는가

Agent loop에서 Model은 일반 문장 대신
“어떤 Tool을 어떤 인자로 호출할지”를 구조화해 반환할 수 있다.
Hermes는 이름을 Registry에서 찾고,
필요하면 위험 명령 승인을 거친 뒤 Tool을 실행한다.
결과는 `tool` 역할 메시지로 Conversation에 들어가고 Model이 다시 판단한다.[5]

여러 독립 Tool call은 병렬 실행할 수 있지만,
사용자 입력이 필요한 Tool은 순차 처리한다.
따라서 Tool calling은 Model이 직접 Terminal을 조작한다는 뜻이 아니라,
Model이 요청한 구조화된 호출을 Harness가 검증하고 실행한다는 뜻이다.

### Skills는 Prompt 모음과 무엇이 다른가

Hermes Skill은 `SKILL.md`를 중심으로 References, Scripts, Templates와 Assets를 묶을 수 있는
재사용 가능한 작업 지식 단위다.
목록에는 이름과 설명만 먼저 노출하고,
필요할 때 본문과 관련 자료를 단계적으로 읽는 Progressive Disclosure 방식을 사용한다.[7]

```text
Skill 목록과 짧은 설명
→ 관련 Skill 선택
→ SKILL.md 로드
→ 필요한 Reference 또는 Script만 추가 로드
→ Tool로 절차 실행
```

이 구조는 모든 매뉴얼을 System prompt에 넣는 것보다 Context 사용량을 줄인다.
또한 Agent는 반복해서 얻은 절차적 교훈을 새 Skill로 저장하거나 기존 Skill을 고칠 수 있다.
다만 Skill은 Markdown 지시이므로 운영체제 수준의 강제 규칙은 아니다.
Agent가 올바르게 선택하고 따라야 하며,
중요한 보안 제약은 Tool 권한과 Hook 같은 별도 장치로 막아야 한다.

### Terminal, File, Web과 Browser

Hermes는 Terminal에서 명령을 실행하고,
파일을 읽고 검색하고 Patch할 수 있다.
Terminal backend를 Local로 두면 사용자의 실제 기기에서 동작하므로 가장 편리하지만 권한 위험도 크다.
Docker, SSH와 Cloud sandbox를 사용하면 실행 환경을 분리할 수 있다.[6]

Web search/extract는 공개 정보를 빠르게 수집하는 데 적합하다.
로그인, 버튼 클릭, 동적 UI와 시각 검증이 필요하면 Browser automation을 사용한다.
Browser backend에 따라 별도 API key, Browser 설치 또는 Cloud 서비스 설정이 필요할 수 있다.

### Subagent

`delegate_task`는 별도 Context와 Terminal Session을 가진 Child Agent를 만든다.
Child는 부모의 전체 Conversation을 자동으로 알지 못하며,
전달받은 Goal과 Context, Workspace 규칙으로 작업한 뒤 최종 요약만 부모에게 반환한다.[8]

이는 큰 조사 결과나 Log가 Main Context를 채우는 것을 줄이고
독립적인 조사 항목을 병렬 처리하는 데 유용하다.
반대로 필요한 배경을 빠뜨리면 Child가 잘못된 전제로 일할 수 있고,
병렬 Child 수만큼 Model 호출 비용도 증가한다.

### MCP

MCP(Model Context Protocol)는 외부 Tool Server를 Agent에 연결하는 표준이다.
Hermes는 Local stdio와 Remote HTTP Server에 연결해 Tool을 발견하고,
내장 Tool과 함께 호출할 수 있다.[9]

MCP가 있으면 GitHub, Database, 외부 API 같은 기능을 Hermes 본체에 직접 구현하지 않아도 된다.
그러나 MCP Server는 실행 Code와 Credential에 접근할 수 있으므로
설치 Manifest, 노출 Tool과 권한을 검토해야 한다.

### Scheduling과 장기 실행

Hermes의 Cron은 일회성 또는 반복 작업을 예약하고,
필요한 Skill을 붙여 새 Agent Session에서 실행하며,
결과를 파일이나 설정된 Messaging platform으로 전달할 수 있다.[10]

이 기능은 “매일 Repository Release 확인”처럼 반복되는 조사에 유용하다.
다만 Cron이 Agent process 전체를 영원히 살려 두는 것은 아니다.
각 실행은 새로운 작업이며,
Provider 인증, Gateway와 Delivery 설정이 유지되어야 한다.

## 4. Memory가 어떻게 동작하는가

### 가장 먼저 구분할 여섯 가지

Hermes에서 “기억한다”는 말은 하나의 기능을 뜻하지 않는다.

| 종류 | 저장 위치와 범위 | 자동 여부 | 다음 Session에서 쓰는 방법 |
| --- | --- | --- | --- |
| **현재 대화 Context** | 현재 Model 요청에 포함된 메시지 | 대화 중 구성 | 같은 Session에서 즉시 사용 |
| **Session history** | Profile의 `state.db` | 모든 대화를 자동 저장 | 같은 Session을 resume |
| **Persistent memory** | `~/.hermes/memories/MEMORY.md`, `USER.md` | Agent가 Memory tool로 선별 저장하며 Background review가 제안·기록 가능 | 새 Session 시작 시 System prompt에 주입 |
| **과거 Conversation 검색** | `state.db`의 SQLite FTS5 index | Session 저장과 함께 검색 가능해짐 | `session_search`로 필요한 메시지만 조회 |
| **Workspace context** | Repository의 `.hermes.md`, `AGENTS.md`, `CLAUDE.md` 등 | 사용자가 파일로 관리 | 해당 작업 경로에서 Session 시작 시 로드 |
| **Workspace snapshot** | Session 시작 시점의 경로와 Git·Project 상태 | Code workspace에서 자동 생성 | 시작 Context에 들어가지만 최신 상태를 계속 추적하지는 않음 |

### 현재 대화 Context

Context는 Model이 지금 보고 있는 작업대다.
System prompt, 최근 Conversation, Tool 결과,
Memory snapshot과 Project 규칙 중 실제 요청에 포함된 것만 여기에 있다.

Session DB에 저장된 정보 전체가 항상 Context에 들어오는 것은 아니다.
History는 창고이고 Context는 작업대에 가깝다.
저장되어 있어도 Resume하거나 검색해 가져오지 않으면 Model은 알 수 없다.

### Session history

Hermes는 CLI와 Messaging conversation을 Session으로 만들고
메시지, Tool call과 결과, Model, System prompt snapshot, Token 수와 작업 경로를
SQLite `state.db`에 저장한다.[11]

같은 작업을 정확히 이어가고 싶다면 새 Session을 만드는 것보다
`--continue`, `--resume` 또는 `/resume`으로 기존 Session을 여는 방식이 가장 확실하다.
이 경우 이전 대화 기록을 다시 불러온다.

이것은 장기 Memory와 다르다.
Session history는 대화의 상세 기록이고,
Persistent memory는 여러 Session에서 항상 알아야 할 핵심만 선별한 짧은 메모다.

### Persistent memory

기본 Persistent memory는 두 파일로 구성된다.[12]

- `MEMORY.md`: 환경 사실, 전역 규칙과 장기간 유효한 학습 내용
- `USER.md`: 사용자 성향, 설명 방식과 선호

두 저장소에는 글자 수 제한이 있고,
새 Session 시작 시 System prompt에 Frozen snapshot으로 삽입된다.
Session 도중 Memory tool로 바꾼 내용은 Disk에는 즉시 저장되지만,
이미 구성된 System prompt를 바꾸지 않기 때문에 다음 Session부터 기본 Context에 반영된다.[12]

### 자동으로 모든 것을 기억하는가

아니다.
다음 두 동작을 구분해야 한다.

1. **Conversation 저장은 자동**이다.
   모든 대화가 Session history로 보존되므로 나중에 Resume하거나 검색할 수 있다.
2. **Persistent memory는 선별 저장**이다.
   Agent가 Memory tool을 호출하거나 Background review가 유용한 선호·환경·교훈을 판단해 저장한다.
   Conversation 전체를 `MEMORY.md`에 복사하지 않는다.

기본 설정에서는 Agent가 Memory를 스스로 기록할 수 있다.
`memory.write_approval`을 켜면 Foreground와 Background 저장을 승인 대기 상태로 두고
사용자가 검토한 뒤 반영할 수 있다.[12]

즉 “명시적으로 `기억해줘`라고 말해야만 저장된다”도 아니고,
“말한 모든 것이 영구 기억된다”도 아니다.
Agent의 선별 판단이 개입하므로 중요한 내용은 저장 여부를 직접 확인하는 편이 안전하다.

### 과거 Conversation 검색

`session_search`는 SQLite FTS5 Full-text search를 사용해
과거 Session의 실제 메시지를 찾는다.[12][11]
검색 결과를 LLM이 미리 요약해 별도 지식으로 저장하는 방식이 아니라,
질의 시점에 DB에서 관련 메시지를 찾아 Context로 가져오는 Retrieval 방식이다.

```text
새 Session에서 과거 작업 질문
→ session_search로 키워드 검색
→ 관련 Session과 Message 발견
→ 앞뒤 Message 또는 Session 전체 일부 확인
→ 현재 작업에 필요한 사실만 사용
```

이 기능 덕분에 모든 과거 대화를 Persistent memory에 넣지 않아도 된다.
다만 Agent가 무엇을 검색해야 하는지 알아야 하므로,
Repository 이름, 파일명, 오류 문자열 같은 Anchor가 구체적일수록 복구가 쉽다.

### 장기 기억의 실제 정체

Hermes의 장기 기억은 하나의 무한한 기억 장치가 아니라 다음의 조합이다.

```text
항상 필요한 작은 기억
= MEMORY.md + USER.md

정확한 과거 기록
= state.db의 Session history + session_search

반복 가능한 작업 방법
= Skills

Project별 고정 규칙
= .hermes.md / AGENTS.md / CLAUDE.md
```

이 조합은 “모든 것을 항상 Prompt에 넣기”보다 Token 효율이 좋다.
반면 필요한 내용을 어느 층에 저장할지 잘못 선택하면
전역 Memory가 일시적인 Project 정보로 오염되거나,
중요한 사실이 검색해야만 보이는 상태가 될 수 있다.

### Workspace 단위 기억은 있는가

기본 `MEMORY.md`와 `USER.md`는 **Workspace별이 아니라 Profile별**이다.
같은 Profile로 여러 Repository를 열면 이 Memory snapshot은 공통으로 사용된다.

Workspace에 가까운 지속 Context는 다음 기능이 담당한다.

- Repository에 저장된 `.hermes.md`, `AGENTS.md`, `CLAUDE.md` 등 Project context file
- Session DB에 기록된 작업 경로와 Workspace 기준 Session 필터
- 해당 Repository에서 이어서 여는 기존 Session
- Project 전용 Skill 또는 별도 Hermes Profile

Hermes가 Code workspace를 감지하면 Root path, Git branch와 변경 상태,
최근 Commit, Manifest와 검증 명령 같은 시작 시점 Snapshot도 Context에 넣는다.
이것은 별도의 장기 기억 저장소가 아니며 Session 중 자동 갱신되지 않는다.
따라서 실제 행동 전에는 Git과 File 상태를 다시 확인해야 한다.[4]

Project context file은 현재 작업 경로에서 발견되어 System prompt에 주입된다.[13]
따라서 Project 규칙과 Build 명령처럼 Repository와 함께 관리할 내용은
전역 Memory보다 `AGENTS.md` 같은 파일에 두는 편이 낫다.

완전히 격리된 Memory가 필요하면 Project별 Profile을 만들 수 있다.
Profile은 Config, Sessions, Skills와 Memory가 분리된다.
외부 Memory provider는 별도의 Workspace/Identity scope를 제공할 수 있지만,
그 동작은 선택한 Plugin의 구현과 설정을 따로 확인해야 한다.

### Context가 길어지면 Memory가 되는가

아니다.
긴 Context는 현재 Session의 정보일 뿐 자동으로 장기 Memory와 같아지지 않는다.
Hermes의 기본 Context engine은 Context 사용량이 커지면 오래된 Tool 결과를 줄이고
중간 Conversation을 요약하며 최근 메시지를 보존한다.[14]

현재 기본 문서는 In-place compression을 설명한다.
같은 Session ID 안에서 오래된 메시지를 비활성화하고 Summary와 최근 Tail로 active Context를 다시 구성하지만,
원래 메시지는 Session search로 찾을 수 있도록 DB에 남긴다.[14]

압축은 Context를 절약하는 장치이지 완벽한 기억 장치가 아니다.
Summary가 잘못되거나 식별자가 빠질 수 있으므로,
중요한 결정은 파일, Persistent memory 또는 검색 가능한 Session에 명시적으로 남겨야 한다.

### 개인정보와 잘못된 기억의 위험

Memory와 Session history가 편리할수록 보존 범위도 커진다.
주의할 점은 다음과 같다.

- 잘못 해석한 선호가 Persistent memory에 저장되면 다음 Session에서도 반복될 수 있다.
- 오래된 환경 정보가 갱신되지 않으면 잘못된 명령을 선택할 수 있다.
- 여러 Project 정보가 Profile Memory에 섞이면 관계없는 Session에 노출될 수 있다.
- Session DB에는 대화와 Tool 결과가 남으므로 민감한 내용을 입력하지 않는 것이 우선이다.
- 외부 Memory provider를 사용하면 데이터가 어느 서비스로 전송·보존되는지 별도 검토해야 한다.

Hermes는 Memory 입력의 Injection pattern 검사,
Tool 출력 Secret redaction과 Shell command 승인 모드를 제공한다.[15]
그러나 Scanner가 모든 민감정보와 오판을 막아주는 것은 아니다.
`write_approval`을 켜고,
`/memory pending`과 Memory 파일을 주기적으로 검토하며,
틀린 기억은 수정하거나 삭제하는 운영이 필요하다.

## 5. Coding Agent로 볼 수 있는가

### 판단

Hermes는 **범용 Agent Framework이면서 Coding Agent로 사용할 수 있는 Agent**다.
Coding만을 위해 설계된 전용 제품으로 범위를 제한하지 않지만,
Codebase를 읽고 수정하고 Terminal·Git·Test·Browser를 실행하는 구성은 갖추고 있다.

### Coding Agent 기준별 평가

| 기준 | Hermes의 지원 | 평가 |
| --- | --- | --- |
| 코드 읽기 | `read_file`, `search_files` | 충분함 |
| 코드 수정 | `write_file`, `patch` | 충분함 |
| Terminal 실행 | Local·Container·Remote backend | 강함. 권한 관리 필요 |
| 테스트 | Terminal에서 Project test command 실행 | 가능. 어떤 Test를 돌릴지는 Agent·Skill·Project 규칙에 좌우 |
| Git | Terminal과 Git 관련 Skills 사용 | 가능. 전용 Workflow의 강제력은 설정에 좌우 |
| Browser | Web app 확인과 UI 조작 | 강한 편. Backend 설정 비용 존재 |
| Planning | Todo, `/plan`, Planning Skill | 가능 |
| Memory | Persistent memory, Session search, Project context | 강한 차별점이지만 관리 필요 |
| Skill | Bundled·외부·Agent 생성 Skill | 강한 확장 지점 |
| Subagent | 독립 Context Child Agent | 병렬 조사·리뷰·분업 가능 |
| 장기 실행 | Background process, Gateway, Cron | 가능. 실행 종류별 수명 차이를 이해해야 함 |
| 반복 작업 | Cron, Skill, Script, Batch | 강함 |

### 전용 Coding Agent와의 차이

Hermes는 Coding Workflow 자체를 하나로 강제하지 않는다.
요구사항 분석, TDD, Review와 Branch 통합 절차는
Project 지침이나 Superpowers 같은 Skill package를 통해 정교하게 만들어야 한다.

이 자유도는 장점이자 부담이다.
자신의 Workflow를 설계하고 Model과 Tool을 바꾸려는 사람에게는 유연하지만,
설치 직후부터 검증된 개발 UX를 기대하는 사람에게는 Codex나 Claude Code가 더 간단할 수 있다.

## 6. 현재 내가 사용하는 방식과 연결

### Repository 조사 흐름

현재 사용 방식은 다음과 같다.

```text
GitHub Repository URL 전달
→ README와 공식 문서 확인
→ Repository Clone 또는 실제 Tree 조사
→ 핵심 구현 파일 확인
→ Star한 이유와 실제 가치 검토
→ 출처가 연결된 Markdown 학습 기록 작성
→ Markdown과 Git 변경 범위 검증
```

### Hermes가 이 작업에 적합한 이유

**확인된 기능**을 현재 Workflow에 연결하면 다음 장점이 있다.

1. **Web과 Browser**로 README, 공식 문서와 동적 Page를 확인할 수 있다.
2. **Terminal과 File Tool**로 Repository 구조와 구현을 직접 조사할 수 있다.
3. **Subagent**로 Memory, 구조, 비교 대상을 병렬 조사할 수 있다.
4. **Skills**에 조사 절차, 인용 규칙과 문체를 저장해 다음 분석에 재사용할 수 있다.
5. **Session search**로 이전 Repository 분석에서 확인한 비교 기준과 오류를 다시 찾을 수 있다.
6. **Persistent memory**로 전역적인 사용자 선호를 새 Session에 이어갈 수 있다.
7. **File write와 검증 Tool**로 답변을 넘어서 실제 Markdown Artifact를 남길 수 있다.

**내 해석:** 이 작업에서 가장 중요한 가치는 한 번의 답변 품질보다
조사 → 근거 확인 → 문서 작성 → 검증이라는 반복 Workflow를 누적할 수 있다는 점이다.
Model을 바꾸더라도 Skill과 Project 규칙, Session DB와 Memory는 Harness 쪽에 남는다.

### 한계

- Repository가 크면 전체 구현을 모두 읽을 수 없으므로 대표 경로를 선정해야 한다.
- Subagent의 요약은 원문 검증 없이 사실로 받아들이면 안 된다.
- Web 검색 결과는 최신 Branch와 문서 배포 시점이 다를 수 있다.
- Citation을 자동으로 붙여도 인용문이 실제 주장을 뒷받침하는지는 별도 검토가 필요하다.
- Agent가 사용자의 Star 이유와 최종 평가를 추측하지 않도록 개인 배경을 명시해야 한다.
- 긴 조사에서 Tool 결과와 비교 자료가 Context를 많이 차지하므로 중간 Artifact와 Session search가 필요하다.

### Coding Agent로 맡기기 적절한 범위

Hermes에는 다음과 같은 작업을 맡길 수 있다.

- Codebase 구조 파악과 관련 Symbol 추적
- 작은 Bug 수정과 Regression test 추가
- 명확한 Spec에 따른 기능 구현
- Test·Lint·Build 실행과 실패 원인 조사
- Git diff Review와 문서 갱신
- Browser를 이용한 Local web app의 기본 검증
- 반복 점검을 Skill이나 Cron으로 자동화
- 큰 작업을 Subagent에게 나눠 조사·구현·Review

반면 Database 삭제, 배포, Credential 변경,
대규모 자동 Refactor와 외부 Message 발송은
명시적인 승인과 좁은 권한, Rollback 방법을 준비한 뒤 맡겨야 한다.

## 7. Superpowers와의 관계

### 같은 종류의 도구인가

같은 범주로 직접 비교하면 핵심을 놓치기 쉽다.
Superpowers README는 자신을 Coding Agent 위에 설치하는
재사용 가능한 Skill과 초기 지시로 구성된 Software development methodology라고 정의한다.[16]

```text
Hermes Agent
= Model과 Tool을 연결해 실제 작업을 수행하는 Agent/Harness

Superpowers
= Coding Agent가 어떤 순서와 기준으로 개발할지 제공하는 Methodology/Skill package
```

따라서 내가 이해한 다음 관계는 대체로 맞다.

```text
Hermes
→ 실제로 판단하고 Tool을 사용해 작업을 수행하는 Agent

Superpowers
→ 그 Agent에게 설계, 계획, TDD, Review와 검증 방법을 제공하는 체계
```

조금 더 정확히 표현하면,
Hermes는 Agent 그 자체뿐 아니라 Agent가 동작하는 Harness까지 제공한다.
Superpowers는 이 Harness 위에서 Agent의 개발 행동을 바꾸는 확장 Layer다.

### Hermes에서 Superpowers를 사용할 수 있는가

사용할 수 있다.
조사 기준 Superpowers README에는 Hermes Agent용 설치 명령이 별도로 있고,
Repository에는 `.hermes-plugin/plugin.yaml`, Bootstrap Hook과 Hermes Tool mapping 문서가 포함되어 있다.[16]

```bash
hermes plugins install obra/superpowers --enable
```

Superpowers는 `read_file`, `patch`, `terminal`, `delegate_task`, `skill_view` 같은
Hermes Tool에 자신의 Workflow action을 연결한다.

다만 “설치 가능”과 “항상 완벽하게 같은 방식으로 작동”은 다르다.
조사 기준 Superpowers 문서는 Hermes의 매우 긴 Session이 압축된 뒤
첫 Turn의 Bootstrap을 잃을 수 있으므로 Skill triggering이 멈추면 새 Session을 시작하라고 안내한다.[16]
Harness별 Hook lifecycle과 Tool 이름 차이가 있으므로
Claude Code나 Codex에서의 동작과 완전히 같다고 가정하면 안 된다.

## 8. Codex / Claude Code와의 차이

### 비교 범위

세 제품은 모두 Model에 Tool과 작업 Loop를 연결하는 Agent 환경이다.
차이는 “Agent인가 아닌가”보다
어떤 Model 생태계와 Workflow를 중심으로 설계됐고,
어디까지 사용자가 교체·확장·운영할 수 있는가에 있다.

Codex는 OpenAI가 Software development를 위해 제공하는 Coding Agent다.[17]
Claude Code는 Anthropic의 Coding Agent로 Terminal, IDE와 Cloud 환경에서 Code 작업을 수행한다.[18]
Hermes는 Nous Research가 공개한 범용 Agent Framework로 Coding을 포함한 여러 작업을 한 Core에서 실행한다.[2]

### 구조적 비교

| 항목 | Codex | Claude Code | Hermes Agent |
| --- | --- | --- | --- |
| **무엇이 Agent인가** | Codex CLI/App/Cloud의 Coding Agent runtime | Claude Code runtime | `AIAgent` Core와 이를 감싼 CLI·Gateway·Desktop·ACP |
| **Model 선택 자유도** | OpenAI Model이 기본 중심이지만 Custom provider와 Ollama·LM Studio의 Local OSS 경로도 지원[19] | Anthropic API와 Bedrock·Google Cloud·Microsoft Foundry를 통한 Claude Model 경로 지원[20] | 다양한 Provider, OpenAI-compatible endpoint와 Local model 선택 가능 |
| **Memory** | Local Memories와 Session history 지원. Memories는 기본 비활성화 상태에서 사용자가 켤 수 있음[21] | `CLAUDE.md`와 Repository별 Auto memory를 Session 시작 시 로드[22] | Profile별 `MEMORY.md`·`USER.md`, SQLite Session search, 외부 Memory provider |
| **Skill** | Open Agent Skills 형식, Project·User·System scope와 Progressive Disclosure[23] | Agent Skills 형식, Personal·Project·Plugin scope와 자동/명시 호출[24] | Agent Skills 호환, Bundled·Hub·Agent-created Skill과 Skill 관리 Tool |
| **Tool** | Code·Shell·Web·MCP 중심의 Coding Tool | File·Shell·Web·MCP와 Plugin Tool | Coding Tool에 Browser, Media, Messaging, Cron 등 범용 Toolset 추가 |
| **개발 Workflow** | Code 작성·Review·Test·Cloud task에 최적화 | Plan, Code, Test, Review와 GitHub/Cloud 작업에 최적화 | 기본 Loop는 범용. Project 규칙과 Skill로 Workflow 구성 |
| **확장성** | `AGENTS.md`, Skills, MCP, Plugins, Subagents[23][25] | `CLAUDE.md`, Skills, Hooks, MCP, Plugins, Subagents[22][24][26] | Skills, Tool Plugin, MCP, Hook, Provider, Memory/Context engine까지 교체 가능 |
| **로컬 환경** | CLI와 IDE에서 Local 작업, 별도 Cloud task 지원 | CLI·IDE·Desktop Local 작업과 Cloud Session 지원 | Local CLI/TUI/Desktop, Container·SSH·Cloud terminal backend, Self-hosted Gateway |
| **범용 Agent 기능** | 중심은 Software development | 중심은 Software development | 조사, Messaging, Browser, Media, Home automation, Scheduling까지 넓음 |

### Memory 비교에서 주의할 점

Hermes만 Memory가 있고 다른 Coding Agent에는 없다고 말하는 것은 현재 기준으로 맞지 않다.
Codex도 Local Memories를 제공하고,
Claude Code도 Repository별 Auto memory와 `CLAUDE.md`를 제공한다.[21][22]

Hermes의 구별점은 Memory라는 이름의 존재 자체보다
다음 요소가 하나의 Profile 안에서 연결된다는 데 있다.

- 작고 항상 주입되는 `MEMORY.md`와 `USER.md`
- 전체 Conversation을 저장하는 SQLite Session DB
- 과거 메시지를 직접 찾는 `session_search`
- 절차를 장기 보존하는 Agent-managed Skills
- Memory provider Plugin 교체
- CLI 밖의 Messaging·Cron Session까지 같은 Store에 연결

### 어느 쪽이 더 좋은가

**Codex가 더 적절할 가능성이 큰 경우**

- 대부분의 작업이 Software development다.
- OpenAI Model과 Codex의 개발 UX를 선호한다.
- 별도 Agent infrastructure를 관리하기보다 설치 후 바로 Code 작업에 집중하고 싶다.
- Local과 Cloud Coding task, Review와 GitHub 흐름의 통합이 중요하다.

**Claude Code가 더 적절할 가능성이 큰 경우**

- Claude Model을 중심으로 깊은 Codebase 탐색과 개발을 한다.
- `CLAUDE.md`, Hooks, Plugin과 Subagent 생태계를 활용하고 싶다.
- 전용 Coding Agent의 정돈된 권한·Plan·Review 경험을 우선한다.

**Hermes가 더 적절할 가능성이 큰 경우**

- Model과 Provider를 바꾸면서 같은 Agent 환경을 유지하고 싶다.
- Coding, 조사, Browser, 파일 작업, Messaging과 Scheduling을 한 Agent에 연결하고 싶다.
- Memory와 Session search를 직접 통제하고 싶다.
- Self-hosting과 Profile, Plugin, Tool backend를 세밀하게 구성하고 싶다.

## 9. Hermes를 사용할 이유

### 특정 Model에 종속되고 싶지 않은 사람

Hermes의 가장 분명한 장점이다.
Harness와 Memory, Skills를 유지한 채 Provider와 Model을 바꿀 수 있다.
같은 Task를 여러 Model로 비교하거나
가격·속도·Context 길이에 맞춰 Model을 선택하기 좋다.

단, Model을 바꾸면 Tool calling 형식과 품질,
지원 Context와 Reasoning 옵션이 달라질 수 있다.
Model 독립성은 결과의 동일성을 뜻하지 않는다.

### Memory를 중요하게 생각하는 사람

여러 Session에 걸쳐 사용자 선호, 환경과 과거 작업을 이어가는 구조가 명확하다.
항상 필요한 작은 Memory와 필요할 때 검색하는 Session history를 분리한 점도 실용적이다.

반면 기억을 검토하지 않으면 오래되거나 잘못된 정보가 반복될 수 있다.
Memory를 중요하게 생각할수록 “자동 저장”보다
수정·삭제·승인과 Scope 관리 기능까지 함께 봐야 한다.

### 여러 종류의 작업을 한 Agent에서 하고 싶은 사람

Code 수정 외에도 Web 조사, Browser 조작, 문서 생성,
Media 처리, Messaging과 Cron을 같은 Agent loop에서 사용할 수 있다.
Repository 조사와 학습 기록처럼
여러 종류의 Tool이 이어지는 작업에 특히 잘 맞는다.

### Agent 환경을 직접 Customize하고 싶은 사람

Provider, Toolset, Terminal backend, Skills, Plugin, MCP,
Memory provider와 Context engine까지 선택할 수 있다.
Agent를 제품으로 사용하면서 동시에 구조를 학습하고 싶은 사람에게 유용하다.

반대로 설정 자체에 시간을 쓰고 싶지 않다면 이 자유도는 비용이다.

### Coding 외 조사·자동화·파일 작업을 하고 싶은 사람

전용 Coding Agent보다 범용 Workflow를 만들기 쉽다.
예를 들어 “Repository Release를 매주 확인해 Markdown에 반영하고 알림 전송”은
Web, File, Skill, Cron과 Messaging을 조합한 작업이다.
Hermes의 넓은 Tool surface가 이런 연결에 적합하다.

## 10. 단점과 주의점

### 설치와 설정 복잡성

기본 Chat만 시작하는 것은 어렵지 않을 수 있지만,
Hermes의 장점을 모두 쓰려면 Provider 인증, Web 검색,
Browser backend, Sandbox, MCP, Messaging Gateway와 Cron delivery를 각각 설정해야 한다.

기능이 동작하지 않을 때도 Model 문제인지,
Provider 인증인지, Tool backend인지, Skill trigger인지,
Gateway routing인지 원인을 나누어 진단해야 한다.

### Model Provider 관리

Provider마다 API key, OAuth, Subscription과 사용량 정책이 다르다.
Main Model 외에도 압축, Vision, Web 추출과 Background review에
별도 Auxiliary Model을 사용할 수 있어 예상보다 호출 경로가 많아질 수 있다.[2]

Model을 자유롭게 바꿀 수 있는 만큼
가격, Context window, Data policy와 Tool calling 품질을 사용자가 비교해야 한다.

### Memory 관리

Memory는 완전 자동 복구 장치가 아니다.
중요한 사실이 저장되지 않을 수 있고,
반대로 잘못된 추측이나 한시적인 환경 정보가 오래 남을 수 있다.

정기적으로 다음을 확인해야 한다.

- `MEMORY.md`와 `USER.md`에 무엇이 들어갔는가
- 전역 Memory에 Project 전용 정보가 섞이지 않았는가
- 오래된 항목을 교체하거나 지워야 하는가
- Background review와 자동 Skill 수정이 필요한 수준으로 제한되어 있는가

### Context와 비용

Memory, Skill description, Project context file과 MCP Tool schema도 Context를 사용한다.
Tool 수가 많고 Subagent를 병렬로 쓰면 Main 답변보다 전체 Token 사용량이 크게 늘 수 있다.

Compression은 길이를 줄이지만 Summary 손실 가능성이 있다.
Session search가 복구 통로를 제공해도 Agent가 적절한 검색어를 선택해야 한다.
긴 Session 하나를 무한히 유지하기보다
작업 단위 Session, 중간 Markdown Artifact와 명시적인 Decision 기록을 함께 쓰는 편이 안전하다.

### Tool 권한과 보안

Agent가 Local Terminal, File과 로그인된 Browser를 사용할 수 있다는 것은
사용자와 비슷한 권한으로 실제 상태를 바꿀 수 있다는 뜻이다.

- Shell command 승인 모드를 유지한다.
- 가능한 작업은 Docker나 Remote sandbox에 격리한다.
- Toolset과 MCP Tool을 필요한 범위만 활성화한다.
- 로그인 Cookie를 사용하는 Browser profile은 필요할 때만 켠다.
- Credential을 Prompt나 Repository 파일에 넣지 않는다.
- 외부 전송, 배포, 삭제와 Git history 변경은 사람의 확인을 거친다.

Hermes의 Secret redaction과 Approval은 방어층이지 완전한 Sandbox가 아니다.[15]
특히 File write 자체와 외부 Tool의 Side effect는
Shell 위험 명령 탐지만으로 모두 통제되지 않는다.

`state.db`에는 대화뿐 아니라 Tool call, System prompt snapshot과 Workspace metadata가 남을 수 있다.[11]
검사한 구현에서는 이 파일과 기본 Memory Markdown에 대한 Application-level 저장 암호화를 확인하지 못했다.
따라서 OS 계정 권한과 Disk 암호화를 포함해 Hermes Home 전체를 민감한 Local data로 다뤄야 한다.

또한 `MEMORY.md`에서 항목을 지워도 과거 Session의 System prompt snapshot까지 함께 지워진다고 가정하면 안 된다.
Context 압축도 Privacy 삭제가 아니므로,
삭제가 목적이라면 Memory, Session DB와 외부 Memory provider를 각각 확인해야 한다.[11][15]

### 잘못된 행동을 기억할 가능성

Agent가 틀린 해결법을 Skill로 만들거나
사용자의 일회성 요구를 영구 선호로 저장하면
다음 Session에서 같은 문제가 더 일관되게 반복될 수 있다.

따라서 Self-improvement는 무조건 좋은 방향으로만 누적되지 않는다.
Memory와 Skill 변경 알림을 켜고,
중요한 Workflow는 Git으로 관리되는 Project 문서와 Test로 검증해야 한다.

### 완성형 Coding Agent와 비교한 부족 가능성

- 특정 Model에 최적화된 Prompt, Patch와 Review UX가 전용 제품보다 덜 정교할 수 있다.
- 기본 개발 방법론이 고정되어 있지 않아 사용자가 Workflow를 구성해야 한다.
- Provider별 기능 편차와 호환 문제를 직접 다뤄야 한다.
- 넓은 기능 범위만큼 설정과 고장 지점이 많다.
- Community Plugin과 MCP의 품질·보안 수준이 일정하지 않다.

Repository가 제공하는 기능 수만으로 실제 안정성과 결과 품질이 보장되지는 않는다.
내 환경에서 같은 Task를 반복 실행해 성공률과 비용을 확인해야 한다.

## 11. 내가 직접 확인해볼 실험

### 실험 1. Model 단독 Chat과 Hermes 비교

**목적:** Model과 Agent의 차이를 행동으로 확인한다.

1. 같은 Model의 일반 Chat에 Repository URL과 분석 요청을 보낸다.
2. Hermes에서 동일한 요청을 실행하되 Web, Terminal과 File Tool을 허용한다.
3. 실제 Commit 고정, 구현 파일 확인, Markdown 작성과 검증 여부를 비교한다.

**관찰할 것:** 답변 문장 품질보다 외부 사실 확인과 실제 Artifact 생성 여부.

### 실험 2. 새 Session에서 기억 확인

**목적:** Context, Persistent memory와 Session search를 구분한다.

1. Session A에서 선호 한 가지를 말하고 Memory 저장 여부를 확인한다.
2. 새 Session B를 시작해 그 선호가 기본 Context에 반영되는지 묻는다.
3. Memory에 넣지 않은 Session A의 세부사항을 질문한다.
4. 답하지 못하면 `session_search`로 찾도록 요청한다.

**관찰할 것:** 자동으로 아는 정보, 검색해야 아는 정보, 찾지 못한 정보의 차이.

### 실험 3. Model 교체 후 같은 Workflow 실행

**목적:** Harness가 유지되고 Model 품질이 달라지는 지점을 본다.

1. 같은 Repository와 같은 Skill, 같은 출력 Template을 준비한다.
2. Model A와 Model B로 별도 Session에서 실행한다.
3. Tool call 성공률, 출처 정확성, 수정 횟수, Token과 소요 시간을 기록한다.

**관찰할 것:** Provider를 바꿔도 Skill·Memory·Tool은 남지만 판단 품질은 달라지는가.

### 실험 4. Skill 추가 전후 비교

**목적:** Skill이 단순 Prompt와 어떤 차이를 만드는지 확인한다.

1. Repository 분석 Skill 없이 짧은 요청만 전달한다.
2. 같은 Task를 조사 순서, Citation과 검증 기준이 있는 Skill로 실행한다.
3. README 요약에 그치는지, 구현 파일과 Sources까지 확인하는지 비교한다.

**관찰할 것:** 결과 내용뿐 아니라 작업 순서와 검증 행동이 안정되는가.

### 실험 5. 여러 Session에 걸친 Repository 분석

**목적:** 긴 작업의 실제 연속성을 시험한다.

1. Session A에서 구조를 조사하고 조사 기준 Commit과 미해결 질문을 문서에 남긴다.
2. Session B에서 Memory만으로 이어서 진행해 본다.
3. 부족하면 Session search와 이전 Artifact를 사용한다.
4. Session C에서 다른 Model로 최종 문서를 검토한다.

**관찰할 것:** Memory, Search와 File Artifact 중 무엇이 가장 신뢰할 수 있는 Handoff 수단인가.

**예상:** Persistent memory는 방향과 선호를 이어주는 데 유용하고,
정확한 작업 상태는 Session history와 Version-controlled Markdown이 더 신뢰할 만할 가능성이 크다.

## 12. 최종 평가

### 한 줄 정의

Hermes Agent는 여러 LLM을 교체해 사용하면서
Tool, Skills, Memory, Session, Subagent와 자동화를 연결할 수 있는
오픈소스 범용 Agent이자 Agent Harness다.

### 내가 Hermes를 찾아본 이유

좋은 Model을 선택하는 것과 좋은 Agent 환경을 사용하는 것이
왜 다른 문제인지 이해하고 싶었다.
특히 자주 새 Session을 만드는 작업 방식에서
이전 선호와 작업 기록을 다시 활용할 수 있는 구조를 직접 확인하고 싶었다.

### Model과 Agent의 차이를 이해하는 데 도움이 되었는가

도움이 되었다.
Repository를 살펴보니 Model은 Agent의 판단을 담당하는 핵심 부품이지만,
실제 일을 끝내려면 Agent loop, Tool 실행, 권한, Context 관리,
Session 저장과 Memory가 함께 필요하다는 점이 구체적으로 보였다.

같은 Model을 Chat UI에서 한 번 호출하는 것과
Hermes 안에서 파일을 읽고 Web을 조사하고 결과를 검증하게 하는 것은
Model의 이름이 같아도 다른 사용 경험이다.

### 가장 인상적인 기능

Persistent memory 하나보다
**작은 상시 Memory + 전체 Session DB + 과거 Conversation 검색 + Skills**를
서로 다른 기억 층으로 분리한 구조가 가장 인상적이다.

모든 정보를 늘 Context에 넣지 않고,
중요한 선호는 즉시 제공하고,
세부 작업 기록은 필요할 때 검색하며,
반복 절차는 Skill로 옮기는 방식이 Agent를 장기간 사용하는 문제에 현실적으로 접근한다.

### Memory 기능에 대한 실제 평가

기대했던 “새 Session에서도 이어지는 기억”은 실제로 존재한다.
하지만 사람이 가진 연속적인 기억처럼 자동으로 모든 사건을 이해하고 떠올리는 기능은 아니다.

- 핵심 Memory는 선별되고 크기가 제한된다.
- 상세 Conversation은 자동 저장되지만 검색하거나 Resume해야 한다.
- Project별 규칙은 별도 Context file이 더 적합하다.
- 잘못된 기억과 오래된 정보는 사용자가 검토해야 한다.

따라서 Memory는 강력한 장점이지만,
중요한 작업 상태를 맡기는 유일한 저장소로 사용해서는 안 된다.
Session search와 Markdown Artifact, Git 기록을 함께 써야 한다.

### Coding Agent로 사용할 가치

**4/5**

Code 읽기·수정, Terminal, Test, Git, Browser와 Subagent를 갖추고 있어
실제 Coding Agent 역할을 수행할 수 있다.
Model 선택과 Workflow 확장 자유도도 높다.

다만 전용 Coding Agent처럼 하나의 정돈된 개발 경험을 기대하기보다
Provider, 권한, Skill과 검증 절차를 직접 설계하고 관리할 준비가 필요하다.

### Repository 조사/학습 Agent로 사용할 가치

**5/5**

Web, Repository 파일, Terminal, Subagent, Memory, Citation Skill과 Markdown 작성이
하나의 Workflow 안에서 자연스럽게 이어진다.
내가 Repository를 Star한 이유와 실제 구현 근거를 함께 남기는 작업에 잘 맞는다.

점수는 모든 조사 결과가 자동으로 정확하다는 뜻이 아니다.
이 용도에서는 Harness의 Tool 조합과 재사용성이 특히 잘 맞는다는 평가다.

### 계속 사용하면서 확인해야 할 것

1. **새 Session에서 실제 Recall 품질**
   - Memory에 저장된 내용과 Session search로 찾아야 하는 내용을 Agent가 올바르게 구분하는가.

2. **잘못된 Memory와 Skill의 정정 비용**
   - 오래된 정보나 잘못 학습한 절차를 발견하고 수정하기 쉬운가.

3. **Model별 Tool calling 안정성**
   - Model을 바꿨을 때 같은 Repository 조사 Workflow의 성공률과 검증 품질이 얼마나 달라지는가.

4. **긴 작업의 Context 비용과 압축 손실**
   - Session이 길어졌을 때 중요한 식별자, 결정과 미해결 문제가 얼마나 잘 보존·검색되는가.

5. **Coding Workflow의 완성도**
   - Superpowers 같은 방법론을 붙였을 때 Codex나 Claude Code와 비교해 개발 속도, Review 품질과 운영 부담이 어떻게 달라지는가.

## Sources

[1] Nous Research Releases

<https://nousresearch.com/releases>

[2] Hermes Agent README

<https://github.com/NousResearch/hermes-agent/blob/99d7f17a1c7449e170f2aa3eb6c451b2f5172dfa/README.md>

[3] Nous Portal

<https://github.com/NousResearch/hermes-agent/blob/99d7f17a1c7449e170f2aa3eb6c451b2f5172dfa/website/docs/integrations/nous-portal.md>

[4] Hermes Agent Architecture

<https://hermes-agent.nousresearch.com/docs/developer-guide/architecture>

[5] Agent Loop Internals

<https://hermes-agent.nousresearch.com/docs/developer-guide/agent-loop>

[6] Tools and Toolsets

<https://hermes-agent.nousresearch.com/docs/user-guide/features/tools>

[7] Skills System

<https://hermes-agent.nousresearch.com/docs/user-guide/features/skills>

[8] Subagent Delegation

<https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation>

[9] MCP Integration

<https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp>

[10] Scheduled Tasks

<https://hermes-agent.nousresearch.com/docs/user-guide/features/cron>

[11] Sessions

<https://hermes-agent.nousresearch.com/docs/user-guide/sessions>

[12] Persistent Memory

<https://hermes-agent.nousresearch.com/docs/user-guide/features/memory>

[13] Context Files

<https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files>

[14] Context Compression and Caching

<https://hermes-agent.nousresearch.com/docs/developer-guide/context-compression-and-caching>

[15] Security and Privacy

<https://hermes-agent.nousresearch.com/docs/user-guide/security>

[16] Superpowers README

<https://github.com/obra/superpowers/blob/b36e0829c6d0140e93cfef2ca599b1b07d4a7797/README.md>

[17] Codex Overview

<https://developers.openai.com/codex>

[18] Claude Code Overview

<https://code.claude.com/docs/en/overview>

[19] Codex Advanced Configuration

<https://developers.openai.com/codex/config-file/config-advanced>

[20] Claude Code Enterprise Deployment

<https://code.claude.com/docs/en/third-party-integrations>

[21] Codex Memories

<https://developers.openai.com/codex/customization/memories>

[22] Claude Code Memory

<https://code.claude.com/docs/en/memory>

[23] Codex Skills

<https://developers.openai.com/codex/skills>

[24] Claude Code Skills

<https://code.claude.com/docs/en/skills>

[25] Codex AGENTS.md

<https://developers.openai.com/codex/guides/agents-md>

[26] Claude Code Subagents

<https://code.claude.com/docs/en/sub-agents>
