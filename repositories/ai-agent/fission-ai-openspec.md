---
title: "Fission-AI/OpenSpec"
repository: "Fission-AI/OpenSpec"
url: "https://github.com/Fission-AI/OpenSpec"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# Fission-AI/OpenSpec

> https://github.com/Fission-AI/OpenSpec

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

OpenSpec은 AI가 코드를 작성하기 전에 요구사항과 설계를 문서로 정리하도록 돕는 명세 중심 개발 도구이다.[1]

## 이 Repository는 무엇인가?

명세는 만들 기능과 동작 조건을 적은 문서이며, OpenSpec은 이를 대화 기록 밖의 Markdown 파일로 남긴다.[1]

AI 모델 자체가 아니라 기존 코딩 도우미에 명령과 작업 지침을 연결하는 도구로, 새 프로젝트뿐 아니라 기존 코드의 변경도 다룬다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- `explore`로 코드와 선택지를 검토하고, `propose`로 제안·요구사항·설계·구현 목록을 변경별 폴더에 만든다.[1]
- `apply`로 구현 작업을 진행하고 `archive`로 완료한 변경을 보관하며 명세를 갱신하는 흐름을 제공한다.[1]
- 문서를 고정된 단계 순서에 가두지 않고 수정할 수 있으며, 별도 저장소에서 명세를 공유하는 Stores 기능도 베타로 제공한다.[1]

## 어떻게 동작하는가?

```text
프로젝트와 사용할 코딩 도우미를 초기화하고 변경 아이디어를 입력한다.
↓
AI가 명세·설계·작업 목록을 작성하면 사람이 검토하고 구현을 진행한다.
↓
완료한 변경을 보관하고 다음 작업이 참고할 명세를 갱신한다.
```

터미널의 초기화·갱신 명령과 AI 도우미 안의 작업 명령은 구별되며, 명령 표기는 사용하는 도구에 따라 달라진다.[1]

### 확인한 파일 구성

- `package.json`은 `bin/openspec.js`를 CLI 진입점으로 지정하고 `dist`, `bin`, `schemas`를 배포 파일에 포함한다.[2]
- README는 실제 명세 사례를 `openspec/specs`, 진행 중 변경을 `openspec/changes`에서 볼 수 있다고 안내한다.[1]

## 어떤 기술을 사용하는가?

- `Node.js`는 CLI 실행 기반이고 `TypeScript`는 개발 언어이며, manifest에 컴파일과 `Vitest` 테스트 명령이 선언되어 있다.[2]
- `Markdown`은 요구사항과 구체적인 동작 시나리오를 저장하는 문서 형식이다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 웹 프로젝트에 기능을 추가하기 전, AI와 변경 범위 및 예상 동작을 문서로 검토하는 데 사용할 수 있다.[1]
- 기존 코드 수정의 이유와 구현 목록을 변경별로 남겨 후속 대화에서도 참고하는 흐름에 사용할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 AI 대화 자체보다 제안·설계·명세·작업 목록이라는 검토 가능한 산출물을 중심에 둔 구성이 관찰점이다.[1]

## 장점

- 변경 이유와 구현 항목을 같은 폴더에 모아 계획과 작업의 관계를 살펴볼 수 있다.[1]
- 특정 편집기 하나에 한정하지 않고 여러 코딩 도우미의 명령 체계에 연결한다.[1]

## 단점 / 주의점

- Node.js 20.19.0 이상이 필요하고 MIT 라이선스이며, Stores는 일반 기능과 구별되는 베타 상태이다.[1]
- 익명 명령명·버전 통계 수집이 기본 활성화되어 있으나 설정이나 환경변수로 끌 수 있고, CI에서는 자동 비활성화된다.[1]

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

- [github/spec-kit](github-spec-kit.md)

## 정리

OpenSpec은 명세 문서를 매개로 사람과 AI의 계획·구현·보관을 연결한다.[1] 명세 검토와 구현은 기존 코딩 도우미를 통해 진행되며, 선택한 도구와 프로필에 따라 제공 명령이 달라진다.[1]

## Sources

[1] Fission-AI/OpenSpec 공식 README

<https://github.com/Fission-AI/OpenSpec/blob/main/README.md>

[2] Fission-AI/OpenSpec package.json

<https://github.com/Fission-AI/OpenSpec/blob/main/package.json>
