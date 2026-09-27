---
title: "dani-garcia/vaultwarden"
repository: "dani-garcia/vaultwarden"
url: "https://github.com/dani-garcia/vaultwarden"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# dani-garcia/vaultwarden

> https://github.com/dani-garcia/vaultwarden

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Vaultwarden은 공식 Bitwarden 앱과 통신할 수 있도록 Bitwarden Client API를 Rust로 구현한 비공식 비밀번호 관리 서버이다.[1]

## 이 Repository는 무엇인가?

사용자가 서버를 직접 운영하는 자체 호스팅을 목적으로 하며, 기존 Bitwarden 클라이언트를 이용하면서 서버 구현을 선택할 수 있게 한다.[1]

독립적인 프로젝트로서 공식 Bitwarden 서버와 구분되며, 문제 신고와 지원도 Vaultwarden의 커뮤니티 채널에서 받는다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 개인 금고, 첨부파일, 자료 공유 기능인 Send, 긴급 접근과 개인 API 키를 지원 기능으로 제시한다.[1]
- 여러 사용자가 자료를 나누는 조직 기능에는 컬렉션, 비밀번호 공유, 사용자 역할, 그룹, 이벤트 기록과 정책이 포함된다.[1]
- 비밀번호 외에 추가 인증 수단을 쓰는 다중 인증으로 인증 앱, 이메일, FIDO2 WebAuthn, YubiKey와 Duo를 안내한다.[1]

## 어떻게 동작하는가?

```text
HTTPS와 영구 저장 공간을 갖춘 서버 준비.
↓
Bitwarden 클라이언트와 호환 API로 금고 기능 이용.
↓
금고·공유·첨부파일 관리와 서버 데이터 보존.
```

공식 문서는 컨테이너 배포와 호스트 저장 공간 연결을 권장하며, 브라우저용 Web Vault에는 HTTPS 보안 환경이 필요하다고 설명한다.[1]

### 확인한 파일 구성

- `Cargo.toml`은 Rust 패키지와 의존성을 선언하며, 데이터베이스 종류를 빌드 기능으로 선택하고 `macros`를 작업 공간 구성원으로 등록한다.[2]
- `README.md`는 클라이언트 API의 지원 범위, 컨테이너 배포 방식, Web Vault 요구사항과 지원 창구를 정리한다.[1]

## 어떤 기술을 사용하는가?

- `Rust`와 `Rocket`: 서버 구현 언어와 웹 요청을 처리하는 프레임워크이다.[1][2]
- `Diesel`: Rust 코드에서 데이터베이스를 다루는 계층이며, SQLite·MySQL·PostgreSQL용 기능 선택 항목이 선언되어 있다.[2]
- `Docker`·`Podman`: 서버와 수정된 Web Vault 클라이언트를 컨테이너 형태로 배포하는 경로이다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 실습 서버에서 비밀번호 금고와 추가 인증 수단을 구성하는 자체 호스팅 방식을 학습할 수 있다.[1]
- 가족 등 소규모 공유 환경을 가정하여 컬렉션, 역할과 비밀번호 공유 기능의 관계를 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 사용자 앱을 새로 만드는 대신 기존 클라이언트의 API 규약을 구현하여 서버 선택지를 제공한다는 점이 핵심이다.[1]

## 장점

- 개인 금고와 공유·접근 관리 기능을 같은 서버에서 다루는 구성을 제공한다.[1]
- 컨테이너 배포와 직접 빌드를 모두 안내하고, 빌드 설정에는 여러 데이터베이스 선택지를 둔다.[1][2]

## 단점 / 주의점

- Web Vault는 HTTPS를 요구하며, 문서는 역방향 프록시 사용과 파일·데이터베이스의 정기 백업을 권장한다.[1]
- 라이선스는 `AGPL-3.0-only`로 선언되어 있으며 직접 빌드할 때는 데이터베이스 기능을 적어도 하나 활성화해야 한다.[2]
- 공식 Bitwarden 지원 대상과 동일시해서는 안 되며, README의 API 호환성 설명이 모든 클라이언트 버전의 무조건적인 지원 보장은 아니다.[1]

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

Vaultwarden은 기존 Bitwarden 클라이언트에 자체 운영 서버를 연결하는 대안 구현이다.[1] HTTPS, 영구 저장과 백업을 갖추고 독립 프로젝트의 지원 범위를 구분해야 한다.[1]

## Sources

[1] dani-garcia/vaultwarden 공식 README

<https://github.com/dani-garcia/vaultwarden/blob/main/README.md>

[2] dani-garcia/vaultwarden Cargo.toml

<https://github.com/dani-garcia/vaultwarden/blob/main/Cargo.toml>
