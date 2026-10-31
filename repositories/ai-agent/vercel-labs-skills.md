---
title: "vercel-labs/skills"
repository: "vercel-labs/skills"
url: "https://github.com/vercel-labs/skills"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# vercel-labs/skills

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

`skills`는 코딩 에이전트가 읽을 작업 지침인 Skill을 찾아 설치하고 관리하는 명령행 도구다.
새 언어 모델이나 독립 에이전트가 아니며, 실제 작업을 수행하는 호스트에 재사용 가능한 지침을 전달하는 역할이다.[1]

## 지침 내용과 배치 도구를 구분하기

Skill은 코드 검토나 특정 라이브러리 사용법처럼 반복되는 작업 방법을 적어 둔 자료다.
호스트마다 설치 위치가 달라 같은 지침을 여러 도구에서 쓰려면 파일 복사와 갱신을 관리해야 한다.
이 저장소는 그 공통 작업을 CLI로 제공하며, Skill 내용 모음인 `vercel-labs/agent-skills`와는 별개의 프로젝트다.[1]

README는 프로젝트 범위를 기본으로 하고 전역 적용은 `-g`로 선택하도록 안내한다.
설치할 지침과 대상 에이전트를 고르는 단계가 있으므로 지원 목록에 있다는 사실만으로 모든 지침의 실행 호환성이 입증되는 것은 아니다.[1]

## 한 원본을 여러 위치에서 읽게 하는 방법

명령 문서는 기본적인 설치 방식으로 심볼릭 링크와 복사를 구분한다.
심볼릭 링크는 여러 경로가 하나의 실제 원본을 가리키는 방식이고, 복사는 호스트별 독립 파일을 만든다.
`--list`는 설치 후보만 보여주며 `--all`은 모든 Skill과 모든 에이전트를 대상으로 확인 없이 설치하는 옵션이다.[2]

`src/installer.ts`의 `installSkillForAgent`는 전역 지원 여부, 이름, 공통 원본 경로와 에이전트 경로를 확인한다.
원본 위치는 `.agents/skills/<skill-name>`을 중심으로 계산하며, 기본 설치 모드는 `symlink`다.
경로가 허용한 기본 디렉터리 안에 있는지 검사하고, 설치 원본과 대상이 겹치면 원본을 지우지 않도록 건너뛰는 분기가 있다.[3]

복사 모드의 경로 정리는 기존 디렉터리를 제거하고 다시 만드는 함수를 사용한다.
따라서 설치된 지침을 현장에서 직접 수정했다면 갱신 전에 보존 방법을 생각해야 한다.
경로 안전성 검사가 있다는 사실은 가져온 지침의 내용까지 안전하다는 보증과는 다르다.[3]

## 가상 예시: 웹 화면 검토 지침 하나만 추가하기

개인 웹 프로젝트에서 화면 검토용 Skill을 한 코딩 에이전트에만 넣는 상황을 가정한다.
먼저 저장소의 후보를 보고 원하는 Skill과 대상 호스트를 지정한 뒤 프로젝트 범위로 설치한다.
호스트가 읽을 경로가 생성되는 것이 설치 결과이고, 실제 화면 검토 보고서는 그 지침을 사용해 별도로 작업해야 생긴다.[1][2]

계속 보관할 필요가 없다면 `skills use` 경로도 있다.
이 명령은 임시 디렉터리에 지침 파일을 준비하고 프롬프트를 출력하며, `--agent`를 지정하면 지원하는 에이전트를 대화형으로 시작한다.
따라서 설치하지 않는다는 말은 아무 파일도 만들지 않거나 아무 프로세스도 시작하지 않는다는 뜻이 아니다.[2]

## 적용 범위와 운영상 주의점

조회 당시 README의 Node.js 요구 조건은 22.20 이상이다.
`experimental_` 명령은 옵션과 동작이 바뀌거나 없어질 수 있는 미리보기 기능으로 분리되어 있다.[1]

텔레메트리와 보안 감사 요청은 `DISABLE_TELEMETRY=1` 또는 `DO_NOT_TRACK=1`로 끌 수 있다고 안내한다.
전역 설치와 확인 생략을 조합하기 전에 어느 프로젝트와 호스트에 영향을 주는지 확인해야 한다.[1][2]

CLI 자체의 라이선스는 MIT다.
이 조건을 외부 저장소에서 가져오는 모든 Skill의 이용 조건으로 확대 해석하지 말고 각 원본의 고지와 실행 지침을 별도로 읽어야 한다.[4]

## 직접 읽어볼 자료

1. README: 설치 도구와 Skill 콘텐츠, `add`와 `use`의 목적을 구분한다.[1]
2. `docs/commands.md`: 범위·복사·링크·확인 생략 옵션의 차이를 읽는다.[2]
3. `src/installer.ts`: `installSkillForAgent`와 경로 겹침 검사, 기존 경로 정리를 따라간다.[3]

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

[1] vercel-labs/skills — README.md

<https://github.com/vercel-labs/skills/blob/main/README.md>

[2] vercel-labs/skills — docs/commands.md

<https://github.com/vercel-labs/skills/blob/main/docs/commands.md>

[3] vercel-labs/skills — src/installer.ts

<https://github.com/vercel-labs/skills/blob/main/src/installer.ts>

[4] vercel-labs/skills — LICENSE

<https://github.com/vercel-labs/skills/blob/main/LICENSE>
