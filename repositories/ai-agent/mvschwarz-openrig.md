---
title: "mvschwarz/openrig"
repository: "mvschwarz/openrig"
url: "https://github.com/mvschwarz/openrig"
category: "ai-agent"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# mvschwarz/openrig

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

OpenRig는 여러 AI 코딩 도구의 세션을 일정한 역할과 주소를 가진 팀으로 운영하는 로컬 관리 시스템이다.
Claude Code나 Codex를 대체하는 모델이 아니라, 그 도구들을 실행하고 연결하며 작업 상태를 이어 가는 상위 계층이다.[1]

## 터미널 여러 개와 팀 운영의 차이

코딩 에이전트는 모델과 도구를 이용해 파일을 읽고 수정하거나 명령을 실행하는 프로그램이다.
여러 세션을 열면 누가 구현을 맡고 누가 검토하는지, 종료한 터미널이 작업 종료를 뜻하는지, 다음 요청을 어디에 보낼지 관리할 문제가 생긴다.
OpenRig는 팀 구성을 YAML 문서로 정의하고 `rig up`으로 시작하며, 통신과 작업 기록을 별도로 관리하는 방식으로 이 문제를 다룬다.[1]

주요 구성은 CLI, 터미널 UI인 TUI, 에이전트가 도구를 호출하는 MCP 서버, 백그라운드에서 계속 동작하는 daemon이다.
daemon 아래에는 SQLite 상태 저장소와 tmux 세션, 각 코딩 도구를 연결하는 어댑터가 있다.
tmux는 보는 터미널 창과 실행 중인 세션을 분리하는 도구다.
기존 React 웹 UI는 유지보수 모드이며, README의 주된 운영 화면은 TUI다.[1]

## 역할 주소와 대화는 다르다

`Seat`는 팀 안의 안정된 역할과 주소다.
예를 들어 `dev-owner@first-project`는 구현 책임자를 가리키며, 그 자리를 차지하는 대화가 바뀌어도 역할의 정체성과 작성한 문맥을 이어 가도록 설계한다.
`Pod`는 관련 역할의 묶음이고 각 에이전트는 여전히 자기 문맥 창을 가진다.
모든 에이전트가 하나의 대화를 공유하는 구조로 이해하면 안 된다.[1]

`RigSpec`은 팀의 자리와 관계를 정하는 YAML 정의다.
실제 `first-project/rig.yaml`에는 같은 저장소에서 동작하는 owner와 check 두 자리가 있고, 둘 다 Codex와 `gpt-6-astra`를 지정한다.
owner에서 check로 향하는 `delegates_to` 관계가 선언되어 있다.
이 설정은 어떤 조합으로 시작하는지 보여주며, 결과가 자동으로 올바르게 검토된다는 증명은 아니다.[2]

같은 폴더의 `CULTURE.md`는 협업 규칙을 설명한다.
작업 전에 저장소 지침과 기존 문맥을 읽고, 첫 의미 있는 변경은 정확한 diff와 약속한 동작을 대상으로 독립 검토를 받도록 한다.
대화 메시지 대신 queue와 프로젝트 산출물을 기록으로 쓰고, 다른 작업 행이 닫혔다는 이유로 현재 변경을 승인하지 말라는 지침도 있다.[3]

## 예시로 따라가는 흐름

공식 Getting started는 CSV 파일을 읽는 프로젝트에서 필수 열이 빠졌을 때의 오류를 개선하는 사례를 제시한다.
입력 요청에는 빠진 열의 이름을 알리고, 기존 데이터는 바꾸지 않으며, 회귀 확인을 추가하고, 정확한 변경 후보를 별도 검토자에게 확인받으라는 조건이 들어 있다.
끝에는 로컬 변경만 하고 공개하지 말라는 경계를 붙인다.
단순히 ‘오류 처리를 개선해 줘’보다 무엇이 관찰되어야 하는지 분명한 요청이다.[4]

시작 경로는 선택한 starter의 `rig specs preview`, `rig up ... --plan`, 실제 실행, 자리별 준비 상태 확인으로 나뉜다.
`rig send`로 owner에게 요청을 전달하면 owner가 영속적인 작업을 만들고 맡은 뒤 구현과 확인을 진행하도록 안내한다.
메시지 전송 자체가 queue 작업을 생성하는 것은 아니다.
README와 CULTURE는 대화가 전달된 것과 책임이 기록된 것을 분리한다.[1][3][4]

owner는 저장소 경로, 정확한 commit이나 diff, 수행한 확인과 한계를 검토자에게 넘긴다.
검토자는 그 후보에 대한 관찰을 기록하고, owner가 지적을 해결한 뒤 최종 결과와 이어 할 지점을 남기는 흐름이다.
예제가 기대하는 산출물은 수정된 코드만이 아니라 어떤 동작을 확인했고 사용자가 어떻게 재현할 수 있는지에 대한 기록까지 포함한다.[3]

사람은 TUI의 상태나 메시지 전달 표시만 보고 완료로 판단해서는 안 된다.
필수 열이 빠진 CSV에 열 이름이 표시되는지, 실패 뒤 데이터가 유지되는지, 검토자가 본 diff가 최종 후보와 같은지 확인하는 것이 이 사례의 핵심이다.
공식 안내도 작업 행과 전이 기록을 읽고 실제 산출물을 확인하라고 한다.
이 글은 해당 프로젝트를 준비하거나 에이전트 팀을 시작하지 않았으며, 예시 결과를 실제 성공 사례로 제시하지 않는다.[4]

## 실행 전 알아야 할 변경 범위

OpenRig는 읽기 전용 대시보드가 아니다.
README는 시작 과정에서 provider 설정, 작업 폴더의 지침과 실행 가능한 hook, 신뢰 설정을 기록한다고 명시한다.
hook은 특정 사건이 발생할 때 실행되는 명령이다.
Claude 설정·프로젝트의 `.claude/settings.local.json`, Codex의 `config.toml`, `.openrig` 자원 등은 instance 저장소와 별개로 영향을 받을 수 있다.
`OPENRIG_HOME`만 바꿔도 provider 설정까지 격리되는 것은 아니다.[1]

`rig setup --dry-run`은 setup 계획을 미리 보는 명령이지 이후 daemon과 자리 시작 과정의 모든 쓰기를 미리 보여주는 기능이 아니다.
기존 status-line 명령이나 선택된 설정이 바뀔 수 있고, 완전한 보존·롤백도 보장하지 않는다고 README가 밝힌다.
따라서 관련 설정을 백업하고, 선택한 자원이 무엇을 바꾸는지 먼저 확인해야 한다.[1]

기본 실행은 Claude의 `acceptEdits` 또는 Codex의 `workspace-write`를 사용하며, 전체 권한 우회인 YOLO는 기본값이 아니다.
그러나 제한이 남는다는 말과 변경이 없다는 말은 다르다.
Getting started에 따르면 일반 Codex 시작은 기존 설정이나 관리 제한이 막지 않는 조건에서 sandbox 안의 네트워크 접근을 켤 수 있다.
README는 Codex 실행에 `.git`과 공유 queue-state 경로 쓰기 접근도 추가한다고 설명한다.[1][4]

별도의 명령 허용 질문에 Yes를 선택하면 `rig` 명령군 전체에 대한 반복 승인 생략 규칙을 설정하는 경로가 있다.
이는 시작·중지·설정과 프로세스 실행까지 포함하며, 특정 조회 명령만 허용하는 뜻이 아니다.
No 또는 응답 없음은 기존 설정을 유지하도록 안내한다.
이 선택, native sandbox, 승인 정책은 서로 다른 층이며 문서상의 선택 표시가 실제 권한 강제를 입증하지도 않는다.[1][4]

## 공급자·비용·운영 경계

요구 환경은 Node.js 22 또는 24와 tmux, macOS 또는 Linux다.
Apple silicon Mac은 Node.js 22를 사용하라고 안내하고, native Windows는 미지원, WSL2는 미검증으로 구분한다.
선택한 코딩 도구의 로그인과 모델 이용 권한도 필요하다.
오픈소스 라이선스는 Apache 2.0이지만 선택한 모델 공급자의 사용 비용은 별개다.[1]

프로젝트 starter를 한 공급자로 골랐다고 전체 시스템도 그 공급자만 사용하는 것은 아니다.
운영 지원을 맡는 kernel의 자동 시작은 별도로 인증 상태를 조사하므로, 두 계정이 모두 인증되어 있으면 양쪽을 사용할 수 있다고 공식 안내가 명시한다.
프로젝트의 두 자리 외에 어떤 지원 세션이 시작되는지와 사용량도 확인할 대상이다.[4]

보안 정책상 개인 머신이나 신뢰하는 사설 네트워크가 대상이며 공개 인터넷 노출은 지원 배포 방식이 아니다.
또 활동 hook의 전송 내용에서 프롬프트와 도구 인수를 제외한다는 README 설명을 전체 시스템의 외부 전송 금지로 확대할 수 없다.
선택한 모델 공급자와 MCP 연결은 각각 별도 데이터 흐름을 가진다.[5][1]

## 직접 읽어볼 자료

1. [README와 머신 변경 안내](https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/README.md)

   팀 운영 기능보다 먼저 trust, hook, provider 설정의 변경 표를 읽는다.
   dry-run이 보여주는 범위와 daemon 시작 이후의 효과를 나누어 확인한다.

2. [first-project 팀 정의](https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/packages/daemon/specs/rigs/launch/first-project/rig.yaml)

   두 자리의 모델·실행 폴더·관계를 확인한다.
   팀 이름만 보고 에이전트 수나 사용하는 공급자를 추측하지 않게 하는 실제 설정이다.

3. [first-project 협업 규칙](https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/packages/daemon/specs/rigs/launch/first-project/CULTURE.md)

   정확한 후보 검토, queue 인계, 게시·파괴 작업의 별도 허가 조건을 읽는다.
   협업 지침과 실제 실행 권한이 같은 것이 아님을 염두에 둔다.

4. [Getting started](https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/docs/reference/getting-started.md)

   CSV 오류 개선 사례를 요청 조건부터 결과 확인까지 따라간다.
   자리 준비 상태, kernel의 별도 공급자 선택, 권한 설정을 함께 읽어야 한다.

## 자료 확인 범위

2026-10-04 기준 커밋 `d2707daafa946325205c1c67aa65bd80089ba547`의 README, first-project YAML·CULTURE, Getting started의 시작·예제·권한 설명과 SECURITY를 확인했다.
설치, 로그인, 설정 변경, daemon 및 팀 실행, 코드 변경과 성능 검증은 하지 않았다.
이 설명은 고정 소스 기준이며 배포된 npm 버전과의 일치 여부는 확인하지 않았다.

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

[1] mvschwarz/openrig — README.md

<https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/README.md>

[2] mvschwarz/openrig — packages/daemon/specs/rigs/launch/first-project/rig.yaml

<https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/packages/daemon/specs/rigs/launch/first-project/rig.yaml>

[3] mvschwarz/openrig — packages/daemon/specs/rigs/launch/first-project/CULTURE.md

<https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/packages/daemon/specs/rigs/launch/first-project/CULTURE.md>

[4] mvschwarz/openrig — docs/reference/getting-started.md

<https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/docs/reference/getting-started.md>

[5] mvschwarz/openrig — SECURITY.md

<https://github.com/mvschwarz/openrig/blob/d2707daafa946325205c1c67aa65bd80089ba547/SECURITY.md>
