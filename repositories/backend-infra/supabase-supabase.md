---
title: "supabase/supabase"
repository: "supabase/supabase"
url: "https://github.com/supabase/supabase"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# supabase/supabase

> https://github.com/supabase/supabase

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Supabase는 PostgreSQL 데이터베이스를 중심으로 인증, API, 파일 저장과 실시간 변경 알림을 묶어 제공하는 개발 플랫폼이다.[1]

## 이 Repository는 무엇인가?

앱의 화면 뒤에서 데이터를 저장하고 사용자 접근을 관리하는 백엔드 기능을 여러 공개 소프트웨어의 조합으로 제공한다.[1]

Firebase와 비슷한 개발 경험을 목표로 하지만 기능이 일대일로 대응하는 제품은 아니며 호스팅 서비스, 자체 운영과 로컬 개발 경로를 구분한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 데이터베이스, 사용자 인증·권한 관리, 자동 생성 REST·GraphQL API를 제공한다.[1]
- 데이터 변경을 구독하는 실시간 기능과 파일 저장, 데이터베이스 함수·Edge Functions를 제공한다.[1]
- 관리 대시보드와 여러 언어용 클라이언트 라이브러리, AI용 벡터·임베딩 도구를 제공한다.[1]

## 어떻게 동작하는가?

```text
호스팅 또는 자체 운영 환경에서 데이터베이스와 사용할 기능을 준비한다.
↓
앱이 클라이언트 라이브러리나 API를 통해 인증·데이터·파일 관련 요청을 보낸다.
↓
구성 서비스가 요청을 처리하고 결과를 반환하며, 실시간 서비스는 허가된 클라이언트에 변경 내용을 전달한다.
```

전체 플랫폼의 구성 관계를 요약한 흐름이며, 개별 서비스와 클라이언트 라이브러리는 별도 저장소에도 나뉘어 있다.[1]

### 확인한 파일 구성

- `apps/studio`는 관리 대시보드, `apps/www`는 소개 사이트, `apps/docs`는 사용 안내와 API 참고 문서를 담당한다.[2]
- `packages/`에는 공통 UI, 설정과 공유 데이터가 있고, `docker/`는 로컬 Studio용 서비스 구성을 다루는 경로이다.[2]
- 루트 `package.json`은 Studio·문서·공통 UI 등의 개발·빌드·검사 명령을 Turborepo로 묶고 패키지 관리자를 pnpm으로 지정한다.[3]

## 어떤 기술을 사용하는가?

- `PostgreSQL`과 `PostgREST`: 데이터를 저장하고 데이터베이스에 REST API를 제공하는 핵심 구성 요소이다.[1]
- `Realtime`: PostgreSQL 변경 내용을 JSON으로 변환해 WebSocket 연결의 허가된 클라이언트로 보내는 Elixir 서버이다.[1]
- `Turborepo`와 `pnpm`: 여러 앱과 공유 패키지의 개발 환경을 관리하며 로컬 Studio 구동에는 Docker가 필요하다.[2]

## 실제로 어디에 사용할 수 있는가?

- 개인 앱의 회원 가입·로그인, 데이터 저장과 파일 업로드를 만들며 백엔드 기능의 연결 방식을 학습할 수 있다.[1]
- 데이터 변경을 화면에 반영하는 구독 기능이나 벡터 검색을 사용하는 개인 학습 프로젝트의 기반으로 활용할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 모든 기능을 하나의 서버로 새로 만드는 대신 독립적인 서비스와 클라이언트를 조합하는 설계가 관찰 지점이다.[1]

## 장점

- 호스팅 사용과 자체 운영·로컬 개발을 모두 안내해 사용 형태를 구분할 수 있다.[1]
- 클라이언트의 하위 라이브러리를 기능별 독립 구현으로 나누어 각 외부 시스템에 대응한다.[1]

## 단점 / 주의점

- Firebase와 기능이 정확히 대응한다고 가정하면 안 되며 서비스별 API와 동작 범위를 따로 확인해야 한다.[1]
- 이 저장소의 웹 앱 개발에는 Node.js·pnpm·make 등이 필요하고, 로컬 Studio는 프런트엔드 외에 Docker 서비스도 요구한다.[2]
- 조회한 루트 manifest의 소스 개발 조건은 Node.js `>=22.13`, 패키지 관리자는 `pnpm@11.13.1`이다. 앱 사용과 자체 서버 운영 조건을 이 개발용 선언과 혼동하지 않아야 한다.[3]

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

## 정리

PostgreSQL 주변에 인증·API·실시간·파일 저장 서비스를 조합한 백엔드 플랫폼이다.[1] 플랫폼 전체와 이 저장소의 웹 앱·개발 구성을 구별하고 운영 형태별 요구사항을 확인해야 한다.[1][2]

## Sources

[1] supabase/supabase 공식 README

<https://github.com/supabase/supabase/blob/master/README.md>

[2] supabase/supabase DEVELOPERS.md

<https://github.com/supabase/supabase/blob/master/DEVELOPERS.md>

[3] supabase/supabase package.json

<https://github.com/supabase/supabase/blob/master/package.json>
