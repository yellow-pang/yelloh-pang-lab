---
title: "ever-co/ever-gauzy"
repository: "ever-co/ever-gauzy"
url: "https://github.com/ever-co/ever-gauzy"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# ever-co/ever-gauzy

> https://github.com/ever-co/ever-gauzy

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Ever Gauzy는 고객·인력·프로젝트·시간·재무 기록을 웹 화면과 API로 함께 관리하는 공개 소스 통합 플랫폼이다.[1]

## 이 Repository는 무엇인가?

ERP는 자원·재무 관리, CRM은 고객 관계 관리, HRM은 인력 관리이며 이 밖에 지원자 채용 추적과 프로젝트 관리 기능을 하나의 플랫폼에 모은다.[1]

브라우저에서 쓰는 화면뿐 아니라 서버, 전체 기능을 담은 데스크톱 앱, 시간 기록에 집중한 Desktop Timer를 구분하여 제공한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 프로젝트와 작업, 시간표·활동 기록, 연락처·판매 단계, 청구서·수입·지출 등의 관리 기능을 제시한다.[1]
- 여러 조직, 부서·팀, 역할·권한과 함께 다국어·다중 통화, 데이터 가져오기·내보내기를 지원 기능으로 안내한다.[1]
- 화면 없이 다른 프로그램에서 호출할 수 있는 Headless API와 대시보드·보고서·분석 기능을 제공한다.[1]

## 어떻게 동작하는가?

```text
서버·데이터베이스와 사용자·권한 구성.
↓
웹 화면·데스크톱 앱·API로 관리 항목과 시간 기록 입력.
↓
대시보드·청구서·보고서와 데이터 내보내기로 확인.
```

전체 기능을 로컬에 포함하는 데스크톱 방식과 별도 서버에 연결하는 클라이언트·서버 방식이 모두 안내되며, Timer는 서버와 연결하는 별도 앱이다.[1]

### 확인한 파일 구성

- 루트 `package.json`은 `apps/*`, `packages/*`, `packages/plugins/*`와 `tools`를 Yarn 작업 공간으로 묶는 모노레포 구성을 선언한다.[2]
- 같은 파일의 시작 스크립트는 API와 Gauzy 화면을 함께 구동하도록 나누고, Nx를 호출하는 개발·빌드 항목을 제공한다.[2]
- `LICENSES.md`는 Community Edition과 별도 계약에 따른 상용 라이선스, 포함 라이브러리와 상표 관련 조건을 구분한다.[3]

## 어떤 기술을 사용하는가?

- `TypeScript`·`NestJS`·`Angular`: API 서버와 웹 화면을 구성하는 주요 기술로 소개되며 manifest에서도 관련 의존성을 확인할 수 있다.[1][2]
- `Nx`·`Lerna`·`Yarn workspaces`: 여러 앱과 패키지를 하나의 저장소에서 관리하는 도구 구성이다.[1][2]
- `SQLite`·`PostgreSQL`: 로컬·데모 구성과 외부 데이터베이스 연결 등 배포 형태에 맞춰 사용하도록 안내한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 실습 데이터로 작업·시간 기록·청구서·보고서가 연결되는 관리 시스템의 흐름을 학습할 수 있다.[1]
- 공개 API와 웹·데스크톱 앱 구성을 비교하여 동일한 서버에 여러 종류의 사용자 화면을 연결하는 방식을 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 전체 플랫폼용 데스크톱 앱과 시간 기록 전용 앱을 나누고, 둘 다 서버 구성과 연결할 수 있게 한 점이 관찰점이다.[1]

## 장점

- 시간 기록만이 아니라 프로젝트·고객·재무와 보고서 기능까지 같은 플랫폼의 범위로 제시한다.[1]
- 로컬 전체 구성, 별도 서버 연결과 화면 없는 API 사용 등 서로 다른 접근 방식을 제공한다.[1]

## 단점 / 주의점

- Community Edition의 기본 라이선스는 AGPL v3이며 별도 상용 계약과 구분되고, 하위 구성요소의 라이선스도 확인하도록 안내한다.[3]
- 공식 SaaS는 Alpha·테스트 상태로 명시되어 있고 데모와 실제 운영 구성을 구분해야 하며, 운영 환경에는 고유한 인증 비밀값·초기 계정 암호·암호화 통신이 필요하다.[1]
- Desktop Timer에는 스크린샷과 활동 기록 기능이 포함되므로, 개인정보가 들어가는 자료의 수집 범위를 정한 뒤 다뤄야 한다.[1]

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

- [makeplane/plane](makeplane-plane.md)

## 정리

Ever Gauzy는 관리 API와 웹·데스크톱 화면을 결합한 통합 관리 플랫폼이다.[1] 라이선스 종류, 데모와 운영 설정의 차이, 활동 기록에 포함될 정보를 구분하여 이해해야 한다.[1][3]

## Sources

[1] ever-co/ever-gauzy 공식 README

<https://github.com/ever-co/ever-gauzy/blob/develop/README.md>

[2] ever-co/ever-gauzy package.json

<https://github.com/ever-co/ever-gauzy/blob/develop/package.json>

[3] ever-co/ever-gauzy LICENSES.md

<https://github.com/ever-co/ever-gauzy/blob/develop/LICENSES.md>
