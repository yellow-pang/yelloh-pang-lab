---
title: "supermemoryai/supermemory"
repository: "supermemoryai/supermemory"
url: "https://github.com/supermemoryai/supermemory"
category: "ai-ml-data"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# supermemoryai/supermemory

> https://github.com/supermemoryai/supermemory

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Supermemory는 대화에서 얻은 사실과 문서를 저장하고 다음 AI 요청에 필요한 맥락을 돌려주는 기억·검색 계층이다.[1]

## 이 Repository는 무엇인가?

AI의 대화가 바뀔 때 취향과 이전 논의가 사라지는 문제를 다루며, 기억 추출·프로필·문서 검색을 하나의 API로 제공한다.[1]

README는 서비스와 로컬 실행 제품을 함께 소개하고, 공개 저장소는 여러 앱과 공통 패키지를 묶은 모노레포로 구성된다.[1][2]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 대화에서 사실을 추출하고 변경·모순·유효기간을 다루며, 사용자 프로필을 장기 정보와 최근 활동으로 구분한다.[1]
- RAG, 즉 관련 문서 조각을 찾아 AI에 제공하는 검색 방식과 개인 기억 검색을 한 요청에서 결합한다.[1]
- 파일 처리와 외부 문서 연결을 제공하고, 플러그인 또는 AI 도구 연결 규약인 MCP를 통해 기억 저장·회상을 노출한다.[1]

## 어떻게 동작하는가?

```text
대화나 문서를 사용자·프로젝트를 구분하는 `containerTag`와 함께 저장한다.
↓
기억 추출과 프로필 갱신을 거친 뒤 질문에 맞는 기억 및 문서를 검색한다.
↓
프로필과 검색 결과를 다음 AI 대화에 사용할 맥락으로 반환한다.
```

API 직접 호출과 도우미 플러그인 방식은 진입점이 다르며, README의 로컬 실행 예시는 클라이언트의 서버 주소를 바꾸어 연결한다.[1]

### 확인한 파일 구성

- 루트 `package.json`은 `apps/*`, `packages/*`를 작업 공간으로 묶고 공통 빌드·형식 검사·타입 검사 명령을 정의한다.[2]
- `apps/mcp/package.json`은 MCP 앱을 정의하며 MCP SDK, Supermemory 클라이언트와 Wrangler 개발·배포 명령을 포함한다.[3]

## 어떤 기술을 사용하는가?

- `Bun`과 `Turborepo`는 모노레포의 패키지 관리 및 여러 패키지의 개발·빌드 실행을 담당한다.[2]
- `Wrangler`는 MCP 앱의 Cloudflare 개발·배포 도구이고, `TypeScript` 타입 검사와 `Vitest` 테스트도 선언되어 있다.[3]

## 실제로 어디에 사용할 수 있는가?

- 개인 AI 도우미가 이전 대화의 선호와 프로젝트 맥락을 참고하게 하는 기억 기능을 구성할 수 있다.[1]
- 개인 학습 문서 검색 결과와 사용자 취향을 함께 반환하는 AI 앱의 데이터 흐름을 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 문서 검색과 시간에 따라 바뀌는 사용자 사실을 구별하면서도, 프로필·검색 API에서 함께 반환하는 설계가 관찰점이다.[1]

## 장점

- 저장·프로필·검색을 API로 나누어 기존 AI 앱에서 필요한 기능을 선택해 연결할 수 있다.[1]
- 프로젝트 태그로 기억의 범위를 구분하고 여러 도우미에 연결할 공식 플러그인 경로를 안내한다.[1]

## 단점 / 주의점

- 저장소 LICENSE는 MIT이며 저작권 및 허가 고지 보존을 요구하지만, 이 파일만으로 외부 서비스의 이용 조건까지 판단할 수는 없다.[4]
- 로컬 구성도 선택한 모델 제공자에 따라 외부 API를 사용할 수 있고, README는 Ollama를 사용하는 오프라인 구성을 별도로 설명한다.[1]

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

- [thedotmack/claude-mem](../ai-agent/thedotmack-claude-mem.md)

## 정리

Supermemory는 기억과 문서를 검색 가능한 맥락으로 바꾸어 AI 앱에 공급하는 제품과 연동 도구를 제공한다.[1] 공개 코드에서 확인한 MCP 앱 구성과 README가 설명하는 전체 기억 엔진의 범위는 구별해 읽어야 한다.[1][3]

## Sources

[1] supermemoryai/supermemory 공식 README

<https://github.com/supermemoryai/supermemory/blob/main/README.md>

[2] supermemoryai/supermemory package.json

<https://github.com/supermemoryai/supermemory/blob/main/package.json>

[3] supermemoryai/supermemory apps/mcp/package.json

<https://github.com/supermemoryai/supermemory/blob/main/apps/mcp/package.json>

[4] supermemoryai/supermemory LICENSE

<https://github.com/supermemoryai/supermemory/blob/main/LICENSE>
