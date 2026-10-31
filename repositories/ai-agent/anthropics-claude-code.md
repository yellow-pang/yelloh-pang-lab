---
title: "anthropics/claude-code"
repository: "anthropics/claude-code"
url: "https://github.com/anthropics/claude-code"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# anthropics/claude-code

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude Code는 자연어 요청을 받아 코드베이스를 살펴보고 개발 작업과 Git 흐름을 수행하는 코딩 에이전트다.
이 공식 저장소는 사용 안내·변경 기록·확장 예제의 출발점이며, README는 실제 제품의 설치와 별도 공식 문서로 연결한다.[1]

## 답을 설명하는 도구와 행동하는 도구

README는 Claude Code를 터미널뿐 아니라 IDE와 GitHub에서도 사용하는 도구로 소개한다.
코드에 관한 설명만 받는 것과 달리 반복 작업을 실행하도록 맡길 수 있다는 점이 중심이다.
따라서 자연어 요청을 입력하는 편의와 파일·명령 실행의 권한 범위를 함께 이해해야 한다.[1]

저장소를 읽는다고 모델이나 제품 전체 런타임의 내부 구현이 모두 공개되어 있다고 가정하면 안 된다.
이번 조사는 안내와 실제 공개된 Hook 예시를 확인한 것이며, 제품의 전체 의사결정 알고리즘을 분석한 기록은 아니다.

## 실행 전에 개입하는 Hook 예시

`examples/hooks/bash_command_validator_example.py`는 Bash 도구의 PreToolUse Hook 예제다.
Hook은 특정 사건이 발생할 때 호출하는 프로그램이고, PreToolUse는 도구가 실행되기 전에 검사할 기회를 뜻한다.
예시 설정은 Bash 호출에 Python 스크립트를 연결한다.[2]

스크립트는 표준 입력의 JSON에서 `tool_name`과 `tool_input.command`를 읽는다.
Bash가 아니거나 명령이 없으면 정상 종료하고, Bash 명령이면 정규식 규칙을 검사한다.
규칙은 일부 `grep`과 `find -name` 사용을 발견해 `rg` 사용을 권한다.[2]

규칙에 걸리면 이유를 표준 오류로 출력하고 종료 코드 2로 도구 호출을 차단한다.
주석의 표현과 달리 실제 코드는 명령을 직접 `rg`로 고쳐서 실행하지 않는다.
즉, 이 예제의 동작은 자동 치환이 아니라 차단 사유를 에이전트에 돌려주는 것이다.[2]

잘못된 JSON은 종료 코드 1로 처리하며, 파일의 설명은 이 경우 오류가 사용자에게 보이지만 Claude에는 전달되지 않는다고 적는다.
같은 실패라도 종료 코드에 따라 전달 대상과 차단 의미가 달라지는 점을 볼 수 있다.[2]

## 가상 예시: 프로젝트의 검색 명령 규칙

개인 코드 프로젝트에서 파일 검색은 `rg`를 쓰도록 맞추는 상황을 가정한다.
사용자가 코드 탐색을 요청하고 에이전트가 Bash 명령을 제안하면, 실행 직전 Hook이 입력을 읽고 규칙을 검사한다.
검사에 걸리면 실행 대신 사유를 돌려주고, 이후 수정된 행동은 에이전트가 결정해야 한다.[2]

이 흐름은 특정 명령 관례를 유도하는 예일 뿐이다.
두 정규식으로 셸 문법 전체나 모든 위험 명령을 통제할 수 있다는 보안 보장은 없으며, 이 조사에서는 실제 Hook을 설치하거나 호출하지 않았다.

## 공개 예제와 제품 이용 조건의 경계

README는 npm 설치를 deprecated로 표시하고 네이티브 설치 등의 경로를 권한다.
오래된 설치 안내를 그대로 복사하기보다 현재 setup 문서에서 플랫폼별 방식을 확인하는 편이 필요하다.[1]

플러그인 개발의 공식 위치는 별도의 `anthropics/claude-plugins-official`로 안내된다.
여기 있는 예제를 제품에 연결하려면 스크립트 경로와 호스트 설정을 실제 환경에 맞춰야 하며, 파일을 읽는 것만으로 활성화되는 것은 아니다.[1][2]

`LICENSE.md`는 Anthropic의 권리 보유와 Commercial Terms of Service 적용을 명시한다.
공개 GitHub 저장소라는 이유로 MIT나 Apache 계열의 재배포 권리가 주어진다고 설명할 수 없다.
README의 데이터 사용 정책 링크도 실제 제품 이용 전에 확인할 자료다.[3][1]

## 직접 읽어볼 자료

1. README: 현재 설치 경로, 공식 문서와 플러그인 저장소의 역할을 구분한다.[1]
2. `examples/hooks/bash_command_validator_example.py`: 입력 JSON, 정규식, 종료 코드를 차례로 읽는다.[2]
3. `LICENSE.md`: 공개 자료 열람과 제품 이용·재배포 조건을 혼동하지 않도록 확인한다.[3]

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 자료를 확인했다. 설치·빌드·실행은 하지 않았다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] anthropics/claude-code — README.md

<https://github.com/anthropics/claude-code/blob/main/README.md>

[2] anthropics/claude-code — examples/hooks/bash_command_validator_example.py

<https://github.com/anthropics/claude-code/blob/main/examples/hooks/bash_command_validator_example.py>

[3] anthropics/claude-code — LICENSE.md

<https://github.com/anthropics/claude-code/blob/main/LICENSE.md>
