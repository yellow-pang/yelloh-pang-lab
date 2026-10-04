---
title: "cursor/plugins"
repository: "cursor/plugins"
url: "https://github.com/cursor/plugins"
category: "ai-agent"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# cursor/plugins

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

`cursor/plugins`는 Cursor 에이전트에 작업 절차, 규칙, 자동 실행 hook, 외부 도구 연결을 추가하는 공식 플러그인 모음이다.
Cursor 편집기 자체의 소스나 모든 기능을 한 번에 실행하는 독립 프로그램은 아니다.[1]

## 목록보다 중요한 플러그인의 경계

루트의 marketplace 목록은 어떤 플러그인이 있는지 알려 주고, 각 플러그인의 `.cursor-plugin/plugin.json`은 이름과 구성 정보를 기술한다.
Skill은 에이전트가 읽는 작업 절차, rule은 작업 중 참고할 규칙, hook은 대화 종료 같은 사건에 반응하는 실행 연결이다.
MCP는 에이전트가 외부 도구와 정해진 방식으로 통신하는 연결 규약이다.[1][2][3]

루트 README의 대표 구조에는 `skills/`, `rules/`, `mcp.json` 등이 나온다.
다만 실제 목록에는 루트의 독립 폴더뿐 아니라 `third_party/` 아래 서비스 연결도 있으므로 한 가지 폴더 모양이나 권한으로 모두를 이해하면 안 된다.
예를 들어 Continual Learning은 대화 기록을 읽어 로컬 문서를 갱신하는 반면, Gmail은 로그인한 외부 계정에 접근하는 연결이다.[1][2][3]

이 모음의 범위는 단순한 코드 작성 조언보다 넓다.
학습 회고, 저장소 호환성 확인, PR 검토, 문서 화면 생성, 외부 서비스 작업이 서로 다른 플러그인에 나뉜다.
따라서 설치할 단위를 먼저 정하고 그 안의 README와 실제 Skill·hook을 함께 읽는 것이 동작과 권한을 이해하는 출발점이다.[1]

## 대화를 다음 작업의 문서로 바꾸는 구성

대표 사례인 Continual Learning은 모델을 다시 훈련하는 기능이 아니다.
대화에서 반복되는 사용자 수정과 오래 유지할 작업 공간 사실을 추려 `AGENTS.md`에 남긴다.
이 파일은 이후 에이전트가 읽는 지침·문맥 문서이므로 “학습”의 결과는 새 모델 가중치가 아니라 로컬 문서다.[2][4]

역할은 세 단계로 나뉜다.
stop hook이 실행 시점을 판단하고, `continual-learning` Skill이 작업을 연결하며, `agents-memory-updater` 보조 에이전트가 실제 기록을 읽고 문서를 갱신한다.
Skill에는 일반 모델 호출에서 자동 선택하지 않도록 하는 `disable-model-invocation: true`가 있지만, 조건을 만족한 hook은 후속 메시지로 실행을 요청할 수 있다.[2][5]

실제 hook은 완료된 응답 중 `loop_count`가 0인 경우를 세고, 같은 generation의 중복 처리를 피한다.
누적 차례 수, 이전 실행 뒤 경과 시간, 대화 파일 수정 시각의 증가를 함께 검사한다.
조건을 만족하면 상태를 먼저 저장하고 학습 Skill을 실행하라는 후속 메시지를 반환한다.
이 코드는 갱신을 요청하는 장치이지 문서 변경의 내용이 정확한지 검증하는 장치가 아니다.[6]

## 예시로 따라가는 흐름

동일 작업 공간의 여러 대화에서 사용자가 “테스트 결과를 확인한 뒤 완료라고 적어 달라”고 반복 수정했다고 가정하자.
이는 흐름 이해를 위한 가정이며 실제 사용자의 기록을 조사한 예가 아니다.
일반 기본 주기는 완료된 대화 차례 10회 이상과 이전 실행 뒤 120분 이상이며 대화 파일도 바뀌어야 한다.
README는 플러그인 설정의 trial mode에서는 초기 주기가 더 짧고 일정 시간이 지나면 기본값으로 돌아간다고 설명한다.[2][6]

조건이 맞으면 Skill은 보조 에이전트에게 전체 갱신 작업을 넘긴다.
보조 에이전트는 기존 `AGENTS.md`와 증분 인덱스를 읽고, 해당 작업 공간의 대화 기록 중 새 파일이나 수정 시각이 더 최신인 파일만 검사하도록 되어 있다.
증분 처리는 이미 처리한 자료를 매번 전부 읽는 대신 바뀐 부분을 추적하는 방식이다.[5][4]

앞의 반복 수정이 지속적으로 쓸 만한 선호인지 판단되면 기존 항목을 제자리에서 고치거나 새 항목을 추가한다.
비슷한 항목을 중복 제거하고 학습한 각 섹션을 최대 12개 bullet로 제한하며, 일회성 요청이나 비밀·사적 정보는 제외하도록 지시한다.
결과는 `Learned User Preferences`와 `Learned Workspace Facts`의 간단한 항목, 처리한 파일을 기록하는 인덱스다.
의미 있는 변경이 없으면 `AGENTS.md`는 그대로 두되 인덱스는 갱신한다.[4]

사람은 기록된 문장이 원래 대화의 뜻을 보존하는지, 특정 상황의 요청을 영구 규칙으로 일반화하지 않았는지, 기존 수동 작성 지침이 남아 있는지 확인해야 한다.
특히 지침은 결과 항목에 근거·확신도 태그를 쓰지 못하게 하므로 요약 파일만 보고 추출의 타당성을 증명할 수 없다.
비밀 제외 역시 에이전트에 대한 지침이며 민감 정보 유출 방지를 강제하는 독립 필터를 이번에 검증한 것은 아니다.[4]

## 플러그인마다 달라지는 쓰기와 외부 연결

`cursor-team-kit`의 `review-and-ship`은 변경 검토에서 끝나지 않는다.
테스트, 중요 문제 수정, 선택한 파일의 커밋, 브랜치 push, PR 생성·갱신까지 절차에 포함한다.
따라서 이름의 “review”만 보고 읽기 전용 작업이라고 생각하면 안 된다.
검토만 원하는 경우에는 허용 범위를 분명히 정하고 원격 변경 단계와 분리해야 한다.[7]

Gmail은 다른 형태의 사례다.
README는 Google OAuth 2.0 로그인과 원격 MCP 서버를 안내하며, 실제 `mcp.json`도 HTTP 방식의 Google 서버 주소를 지정한다.
기능 소개에는 검색·읽기뿐 아니라 라벨과 초안 관리, 메일 작성이 포함된다.
로컬 파일에만 머무는 작업이 아니므로 연결 계정과 승인 권한, 초안 변경 범위를 따로 확인해야 한다.[3][8]

Continual Learning hook에는 Bun 입력 API를 사용하는 실행 코드가 있고 상태 JSON을 로컬에 기록한다.
Skill 텍스트 복사만으로 모든 자동화가 작동한다고 가정할 수 없으며, Cursor의 플러그인·hook 환경이 전제다.
읽은 자료는 이 모음을 다른 에이전트에 그대로 설치했을 때의 동일한 동작까지 입증하지 않는다.[2][6]

## 라이선스와 비용

루트 README는 MIT로 표시하고, 대표적으로 확인한 Continual Learning의 LICENSE도 MIT다.
코드 재사용·수정·배포 시 저작권·허가 고지를 유지해야 하며 보증 없이 제공된다.
전체 목록의 모든 서비스와 자산에 같은 조건이 적용되는지까지 조사한 것은 아니므로 각 플러그인의 고지를 별도로 확인해야 한다.[1][9]

플러그인 코드의 공개 라이선스는 Cursor 모델 사용료나 연결 서비스의 요금·계정 조건을 없애지 않는다.
Gmail 예에서도 계정 로그인은 필요하지만 읽은 문서에 전체 과금 조건은 제시되어 있지 않다.
자동 후속 대화와 보조 에이전트 호출이 어느 정도의 비용을 만드는지도 이번 조사에서는 측정하지 않았다.[2][3]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/README.md)

   플러그인을 전부 외우기보다 관심 있는 작업 하나를 고르고 해당 디렉터리로 내려간다.
   로컬 절차와 외부 서비스 연결이 같은 목록에 섞여 있음을 먼저 구분한다.
2. [Continual Learning README](https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/README.md)

   hook·Skill·보조 에이전트의 역할과 실행 주기를 읽는다.
   모델 재학습이 아니라 대화 기록을 통한 문서 갱신임을 이해하는 출발점이다.
3. [메모리 갱신 지침](https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/agents/agents-memory-updater.md)

   입력 경로, 변경 파일 선별, 기존 항목 수정, 비밀 제외 규칙을 확인한다.
   결과에 근거 태그를 남기지 않는다는 조건과 사람이 검토할 범위를 함께 읽는다.

## 자료 확인 범위

2026-10-04 기준 고정 커밋 `e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a`의 README, Continual Learning의 안내·Skill·hook·보조 에이전트·LICENSE, review-and-ship Skill, Gmail README와 MCP 설정을 읽었다.
전체 플러그인을 전수 검증한 것은 아니며 설치, 프로젝트 실행, 실제 대화 기록 열람, 계정 연결, 커밋·push, 성능 검증은 하지 않았다.

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

[1] cursor/plugins — README.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/README.md>

[2] cursor/plugins — continual-learning/README.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/README.md>

[3] cursor/plugins — third_party/gmail/README.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/third_party/gmail/README.md>

[4] cursor/plugins — continual-learning/agents/agents-memory-updater.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/agents/agents-memory-updater.md>

[5] cursor/plugins — continual-learning/skills/continual-learning/SKILL.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/skills/continual-learning/SKILL.md>

[6] cursor/plugins — continual-learning/hooks/continual-learning-stop.ts

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/hooks/continual-learning-stop.ts>

[7] cursor/plugins — cursor-team-kit/skills/review-and-ship/SKILL.md

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/cursor-team-kit/skills/review-and-ship/SKILL.md>

[8] cursor/plugins — third_party/gmail/mcp.json

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/third_party/gmail/mcp.json>

[9] cursor/plugins — continual-learning/LICENSE

<https://github.com/cursor/plugins/blob/e43c7ee26e0038c6c1fa8380dd34ce86ff94cb2a/continual-learning/LICENSE>
