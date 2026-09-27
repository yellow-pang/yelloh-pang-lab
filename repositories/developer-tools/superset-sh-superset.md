---
title: "superset-sh/superset"
repository: "superset-sh/superset"
url: "https://github.com/superset-sh/superset"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# superset-sh/superset

> https://github.com/superset-sh/superset

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Superset은 CLI 코딩 도우미, 터미널, 코드 변경 검토와 브라우저 미리보기를 한 작업 공간에 모은 개발 도구이다.[1]

## 이 Repository는 무엇인가?

사용자가 선택한 Claude Code·Codex 등의 도우미를 실행하며, 도우미가 만든 코드를 검토하고 수정하는 화면을 제공한다.[1]

각 작업을 Git worktree, 즉 같은 저장소에서 분리한 작업 폴더와 브랜치에 배치해 독립적인 과제를 병렬로 다룬다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 변경 전후를 비교하는 diff 화면에서 코드를 검토·수정하고, PR의 선택한 줄에 대한 의견을 실행 중이거나 새 도우미 세션으로 전달한다.[1]
- 작업 공간별 개발 서버 포트를 감지해 브라우저 미리보기를 제공하며, Design 모드로 선택한 화면 요소의 정보를 도우미에 전달한다.[1]
- 도우미 상태 표시·완료 알림·분할 터미널을 제공하고 CLI·SDK·MCP로 작업 공간 관리도 자동화할 수 있다.[1]

## 어떻게 동작하는가?

```text
Git 저장소를 추가하고 작업 공간과 사용할 CLI 도우미를 선택한다.
↓
도우미에게 변경을 요청하고 해당 작업 공간에서 수정 결과를 생성한다.
↓
변경 화면·테스트·브라우저로 결과를 검토한 뒤 커밋 여부를 결정한다.
```

README의 시작 흐름은 생성된 패치를 사람이 검토하도록 안내하며, 작업 폴더 분리는 실행 프로세스 격리나 병합 충돌 방지를 뜻하지 않는다.[1]

### 확인한 파일 구성

- 루트 `package.json`은 `apps/*`, `packages/*`, `tooling/*`를 모으고 데스크톱·API·웹 개발 작업을 연결한다.[2]
- `apps/desktop/package.json`은 Electron 진입점과 앱 빌드·패키징을 정의하며, 터미널·화면·로컬 데이터용 패키지 의존성을 선언한다.[3]

## 어떤 기술을 사용하는가?

- `Electron`과 `React`는 데스크톱 앱 구성 요소이며, 앱 manifest에는 `electron-vite`와 `electron-builder` 빌드 경로가 있다.[3]
- `Bun`과 `Turborepo`는 모노레포 작업 실행을 관리하고, `TypeScript`는 타입 검사 도구로 선언되어 있다.[2]

## 실제로 어디에 사용할 수 있는가?

- 개인 프로젝트의 서로 다른 수정안을 별도 worktree에서 진행하고 결과를 비교·검토할 수 있다.[1]
- 웹 화면의 개선점을 요소 정보와 함께 도우미에 보내고 미리보기와 코드 차이를 함께 확인할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 도우미를 많이 실행하는 기능뿐 아니라, 변경 줄·화면 요소를 다시 도우미에게 전달하는 검토 경로를 통합한 점이 관찰점이다.[1]

## 장점

- 터미널·코드 차이·브라우저 미리보기를 같은 작업 공간에서 확인할 수 있다.[1]
- 별도 CLI와 TypeScript SDK를 제공해 수동 사용과 프로그램 제어를 같은 작업 공간에 연결한다.[1]

## 단점 / 주의점

- macOS가 주 대상이고 Linux 빌드는 실험적이며 Windows 앱은 아직 제공하지 않는다고 안내한다.[1]
- worktree는 보안 격리 환경이 아니며, 모바일은 Pro·iOS 26 이상·컴퓨터의 Remote Access 활성화 조건이 있다.[1]
- Elastic License 2.0은 주요 기능을 제3자에게 호스팅·관리 서비스로 제공하는 행위와 라이선스 키 우회, 고지 제거 등을 제한한다.[4]

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

## 관련 Repository

- [manaflow-ai/cmux](manaflow-ai-cmux.md)

## 정리

Superset은 기존 코딩 도우미를 작업별 폴더에서 실행하고 변경 내용을 검토하는 개발 환경이다.[1] 작업 분리를 보안 샌드박스로 이해해서는 안 되며 플랫폼·부가 서비스·라이선스 조건을 구별해야 한다.[1][4]

## Sources

[1] superset-sh/superset 공식 README

<https://github.com/superset-sh/superset/blob/main/README.md>

[2] superset-sh/superset package.json

<https://github.com/superset-sh/superset/blob/main/package.json>

[3] superset-sh/superset apps/desktop/package.json

<https://github.com/superset-sh/superset/blob/main/apps/desktop/package.json>

[4] superset-sh/superset LICENSE.md

<https://github.com/superset-sh/superset/blob/main/LICENSE.md>
