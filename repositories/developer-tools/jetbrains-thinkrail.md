---
title: "JetBrains/thinkrail"
repository: "JetBrains/thinkrail"
url: "https://github.com/JetBrains/thinkrail"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# JetBrains/thinkrail

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

pi 코딩 에이전트에 편집기·작업공간·변경 검토와 명세 문서를 붙인 개발 환경이다.[1]

## 채팅 옆에 개발 맥락을 놓는다

ThinkRail은 pi 에이전트를 중심으로 편집기, 터미널, 파일 트리와 변경 검토를 제공하는 개발 환경이다.
에이전트가 작성한 코드를 대화 밖에서 다시 찾아보는 대신 같은 작업공간에서 확인하게 한다.
데스크톱 앱과 브라우저를 여는 CLI가 같은 엔진 호스트를 공유한다.[1]

README의 작업공간은 기본적으로 Git worktree를 이용해 별도 브랜치와 작업 디렉터리를 만든다.
worktree는 같은 저장소 이력을 쓰면서 파일 작업 폴더를 따로 두는 Git 기능이다.
여러 에이전트가 같은 체크아웃을 동시에 바꾸는 충돌을 줄이는 방식이지만, 코드 폴더 분리가 운영체제 보안 격리를 뜻하지는 않는다.[1]

## 실행 호스트·통신 계약·화면의 세 층

공식 `architecture.md`는 엔진 호스트, 통신 계약, UI 클라이언트를 세 부분으로 나눈다.
호스트는 pi를 같은 프로세스 안에서 실행하고 세션 상태와 영속 저장을 소유한다.
`packages/contracts`는 화면과 호스트 사이의 타입·버전이 있는 통신 규약을 맡는다.[2]

`apps/web`은 화면을 제공하고, `apps/cli`와 `apps/desktop`은 공통 호스트 라이브러리를 실행하는 얇은 진입점이다.
화면이 서버 내부 패키지를 직접 참조하지 않게 의존 방향을 제한하는 것이 핵심 설계 규칙이다.
두 실행 형태에서 기능별 구현을 중복하지 않으려는 구조로 읽을 수 있다.[2]

README는 코드 옆 `SPEC.md`의 연결 그래프와 이를 읽고 유지하는 `spec_*` 도구를 소개한다.
명세는 어떤 동작을 의도했는지 기록한 문서이며, 대화가 끝난 뒤에도 의도가 남게 하려는 목적이다.
화면의 명세 탐색기와 에이전트가 명세를 수정하는 도구는 읽기·쓰기 역할이 다르다.[1]

## 가상 사례: 화면 오류를 별도 브랜치에서 수정

입력은 자신의 공개 개인 저장소와 “검색 결과가 없을 때 안내 화면을 추가해 달라”는 요청이라고 가정한다.
별도 worktree 작업공간을 만들고 pi 세션에서 변경을 진행한 뒤, 편집기와 diff에서 결과를 확인한다.
diff는 이전 내용과 바뀐 내용을 비교하는 화면이다.[1]

기존 파일에 검토 의견을 고정해 남기고 대화의 구조화된 맥락으로 전달할 수 있다.
예상 산출물은 별도 브랜치의 변경과 검토 기록이며, 필요하면 의도를 설명하는 명세도 정리할 수 있다.
이 조사에서는 앱 실행·코드 수정·테스트·PR 생성을 수행하지 않았다.[1]

## 편의 기능이 갖는 외부 부작용

README는 PR 전송 워크플로가 사용자의 `gh`를 통해 브랜치를 push하고 GitHub PR을 만들거나 갱신한다고 설명한다.
따라서 워크플로 호출을 모두 로컬 읽기 작업으로 취급하면 안 된다.
단계별 커밋이나 원격 병합 기능도 사용자의 승인 범위와 저장소 규칙에 맞춰 구분해야 한다.[1]

모델 제공자 연결과 Git 실행 환경이 필요하며 pi의 기존 설정을 읽는다.
MCP 서버는 외부 도구를 연결하는 경로이고, README는 비읽기 전용 호출의 확인 및 저장소가 정의한 서버의 승인 절차를 설명한다.
문서상의 승인 모델을 확인했을 뿐 실제 권한 통제를 검증한 것은 아니다.[1]

라이선스는 Apache-2.0이며 모델 제공자의 이용 조건·요금과는 별개다.[3]
아키텍처에는 후속 공유 클라이언트 형태처럼 미래 계획을 나타내는 문장도 있으므로 현재 데스크톱 로컬 실행과 혼동하지 않아야 한다.[2]

## 정리

ThinkRail은 에이전트 대화, 코드 변경, 명세와 Git 작업공간을 한 개발 환경에 모은다.
읽을 핵심은 pi 자체를 다시 구현하는 대신 공통 호스트와 통신 계약 위에 검토 가능한 화면을 놓는 방식이다.

## 직접 읽어볼 자료

1. [README의 What you get](https://github.com/JetBrains/thinkrail/blob/main/README.md)
   worktree, 명세, PR 작업의 역할과 부작용을 구분한다.
2. [최상위 아키텍처](https://github.com/JetBrains/thinkrail/blob/main/architecture.md)
   호스트와 UI의 소유권, 공통 실행 경로, 현재 기능과 후속 계획을 읽는다.

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

[1] JetBrains/thinkrail — README.md

<https://github.com/JetBrains/thinkrail/blob/main/README.md>

[2] JetBrains/thinkrail — architecture.md

<https://github.com/JetBrains/thinkrail/blob/main/architecture.md>

[3] JetBrains/thinkrail — LICENSE

<https://github.com/JetBrains/thinkrail/blob/main/LICENSE>

[4] JetBrains/thinkrail — 공식 루트 트리

<https://github.com/JetBrains/thinkrail/tree/main>
