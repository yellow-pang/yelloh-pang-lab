---
title: "reconurge/flowsint"
repository: "reconurge/flowsint"
url: "https://github.com/reconurge/flowsint"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# reconurge/flowsint

> https://github.com/reconurge/flowsint

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Flowsint는 공개된 정보로 조사하는 OSINT에서 도메인·주소·인물 같은 항목의 관계를 점과 선으로 살펴보는 그래프 탐색 도구이다.[1]

## 이 Repository는 무엇인가?

개별 검색 결과를 따로 읽는 대신 여러 개체의 관계를 시각적으로 연결하고, 추가 정보를 붙이는 enricher로 탐색을 확장한다.[1]

공개 정보의 검증과 합법적인 연구를 목표로 하며, 무단 수집이나 개인을 겨냥한 괴롭힘을 위한 도구로 사용해서는 안 된다고 명시한다.[1][2]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 도메인과 IP 관련 정보, 웹사이트 내용, 공개 프로필, 가상자산 거래 등 여러 종류의 개체와 정보 보강 모듈을 제시한다.[1]
- API에는 인증·사용자 관리, 그래프 데이터베이스 연결과 실시간 이벤트 전달 기능이 포함된다.[1]
- 사용자가 처음 계정을 등록하도록 하며, 조사 데이터는 사용자의 기기에 저장하는 구성을 안내한다.[1]

## 어떻게 동작하는가?

```text
허가된 조사 범위와 출발 개체 정의.
↓
정보 보강 모듈을 통해 관련 데이터와 관계 연결.
↓
화면의 그래프로 관계를 탐색하고 조사 내용 확인.
```

이는 README가 설명하는 그래프 중심의 개념적 흐름이며, 데이터의 로컬 저장과 외부 정보원에 대한 조회는 구분해야 한다.[1]

### 확인한 파일 구성

- `flowsint-app`은 화면, `flowsint-api`는 API, `flowsint-core`는 작업 조정·인증·저장 관련 공통 기능을 맡는다.[1]
- `flowsint-enrichers`는 정보 보강 모듈, `flowsint-types`는 데이터 형식을 맡고 루트 `pyproject.toml`이 Python 모듈을 작업 공간으로 묶는다.[1][3]
- `docker-compose.prod.yml`은 PostgreSQL·Redis·Neo4j 등을 서비스로 정의하며, 데이터베이스 포트를 호스트 내부 주소에 연결한다.[4]

## 어떤 기술을 사용하는가?

- `Python`·`uv`: 여러 백엔드 모듈과 의존성을 하나의 작업 공간에서 관리하며 Python 3.12 이상, 4.0 미만을 선언한다.[3]
- `FastAPI`·`Pydantic`: 각각 API 서버와 개체의 데이터 형식 정의에 사용한다.[1]
- `Neo4j`·`PostgreSQL`: 그래프를 포함한 조사 데이터 저장 구성에 포함되는 데이터베이스이다.[1][4]

## 실제로 어디에 사용할 수 있는가?

- 허가된 실습 자료를 사용하여 공개 정보의 개체·관계를 그래프로 표현하는 방식을 공부할 수 있다.[1][2]
- 개인 학습 관점에서 데이터 형식, 정보 보강 모듈과 API를 분리한 Python 프로젝트 구조를 살펴볼 수 있다.[1][3]

## 왜 주목할 만한가?

학습 관점에서는 새 데이터 유형, 정보 보강 모듈과 API를 서로 다른 계층에 추가하도록 구성한 점이 확장 구조의 관찰점이다.[1]

## 장점

- 시각적 관계 탐색과 자동 정보 보강을 같은 조사 흐름으로 연결한다.[1]
- 화면·API·공통 기능·보강 모듈·데이터 형식을 나누어 각 영역의 역할을 설명한다.[1]

## 단점 / 주의점

- 인권·개인정보를 존중해야 하며, 무단 감시·수집·신상 공개·괴롭힘을 금지하는 윤리 지침을 별도로 둔다.[2]
- 네트워크에 공개하기 전 인증 비밀값·저장된 API 키의 암호화 키·데이터베이스 비밀번호와 호스트 허용 목록을 변경하도록 안내한다.[1]
- 현재 Apache-2.0이며 윤리 원칙에서 다른 라이선스를 참조하는 것과 실제 배포 라이선스는 구분해야 한다.[2] 초기 개발 단계이고 테스트 모음이 불완전하다는 설명도 있다.[1]

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

- [kaifcodec/user-scanner](../ai-agent/kaifcodec-user-scanner.md)

## 정리

Flowsint는 공개 정보의 보강과 관계 시각화를 결합한 모듈형 조사 도구이다.[1] 로컬 저장만으로 사용의 적법성이나 접근 보안이 보장되지는 않으므로 허가·개인정보·네트워크 설정을 확인해야 한다.[1][2]

## Sources

[1] reconurge/flowsint 공식 README

<https://github.com/reconurge/flowsint/blob/main/README.md>

[2] reconurge/flowsint ETHICS.md

<https://github.com/reconurge/flowsint/blob/main/ETHICS.md>

[3] reconurge/flowsint pyproject.toml

<https://github.com/reconurge/flowsint/blob/main/pyproject.toml>

[4] reconurge/flowsint docker-compose.prod.yml

<https://github.com/reconurge/flowsint/blob/main/docker-compose.prod.yml>
