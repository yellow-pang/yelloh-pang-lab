---
title: "cloudflare/quiche"
repository: "cloudflare/quiche"
url: "https://github.com/cloudflare/quiche"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# cloudflare/quiche

> https://github.com/cloudflare/quiche

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

quiche는 인터넷 통신 규약인 QUIC과 그 위에서 웹 요청을 주고받는 HTTP/3를 구현한 저수준 네트워크 라이브러리이다.[1]

## 이 Repository는 무엇인가?

라이브러리는 QUIC 패킷 처리와 연결 상태 관리를 맡고, 실제 소켓 입출력과 시간을 처리하는 이벤트 반복 구조는 사용하는 앱이 제공한다.[1]

완성된 웹 서버 하나라기보다 앱이 자체 네트워크 구조에 끼워 넣는 통신 구현이며, 별도 HTTP/3 계층이 요청·응답용 API를 제공한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 연결 설정에서 전송량 제한, 혼잡 제어, 유휴 시간 제한, TLS 암호화 설정 등을 조절할 수 있다.[1]
- 연결의 `recv`·`send`로 패킷을 처리하고, 여러 데이터 흐름인 stream 단위로 앱 데이터를 송수신한다.[1]
- 패킷 전송 간격을 조절하는 pacing 힌트와 연결 시간 초과 처리를 제공하고, C·C++에서 호출할 C API도 갖춘다.[1]

## 어떻게 동작하는가?

```text
앱이 연결 설정과 소켓·타이머를 준비하고 클라이언트 또는 서버 연결을 만든다.
↓
수신 패킷을 라이브러리에 전달하고 연결 상태·stream·시간 초과를 처리한다.
↓
라이브러리가 만든 송신 패킷을 앱이 네트워크로 보내고 수신 데이터를 사용한다.
```

quiche가 패킷을 만든다는 것과 실제 전송을 수행한다는 것은 다르며, 타이머 만료 통지와 송신 일정 적용도 앱의 책임이다.[1]

### 확인한 파일 구성

- 루트 `Cargo.toml`은 `quiche`, `apps`, `tokio-quiche`, `qlog` 등을 묶는 Rust 작업 공간과 공통 의존성을 선언한다.[2]
- `apps/`는 README가 소개하는 클라이언트·서버 명령행 예제의 `quiche-apps` 패키지 위치이다.[1]
- `quiche/examples/`는 Rust 및 C·C++ API 사용 예제를 안내하는 경로이고, `quiche/include/quiche.h`는 C API 진입 헤더로 연결되어 있다.[1]

## 어떤 기술을 사용하는가?

- `Rust·Cargo`: 통신 라이브러리 구현과 패키지 빌드를 구성하며, 작업 공간의 라이선스는 BSD-2-Clause로 선언되어 있다.[1][2]
- `BoringSSL·TLS`: QUIC 연결의 암호화 초기 협상에 사용하며 기본 빌드 과정에서 함께 연결된다.[1]
- `C API·FFI`: 다른 언어에서 함수를 호출하는 연결 방식을 제공하여 C·C++ 앱과 통합할 수 있다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 네트워크 학습에서 패킷 입출력과 프로토콜 상태 관리를 나누는 라이브러리 구조를 읽을 수 있다.[1]
- Rust 또는 C·C++ 앱에 QUIC·HTTP/3를 연결하는 API 사용 예제를 살펴볼 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 프로토콜 처리를 제공하면서 소켓·이벤트 반복·타이머를 앱에 남기는 명확한 책임 분리가 핵심이다.[1]

## 장점

- 앱이 선택한 운영체제·네트워크 프레임워크의 입출력과 타이머 구조에 결합할 수 있도록 API 경계를 둔다.[1]
- QUIC 전송 API 위에 HTTP/3 API를 제공하고, Rust 외에 C 언어 호출 경로도 문서화한다.[1]

## 단점 / 주의점

- README의 빌드 조건은 Rust 1.88 이상과 CMake이며, Windows에서는 BoringSSL 빌드에 NASM도 필요하다.[1]
- 일부 전송량·stream 기본값은 0이므로 앱에 맞게 정해야 하고, C API의 `ffi` 기능은 기본 비활성 상태이다.[1]
- 제공된 클라이언트·서버 예제와 자체 서명 인증서는 운영 환경용이 아니며, 예제의 성능·보안·신뢰성을 보장하지 않는다고 명시한다.[1]

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

quiche는 앱이 제공한 입출력 위에서 QUIC 연결과 HTTP/3 통신을 처리하는 라이브러리이다.[1] 연결 설정과 이벤트 처리는 통합하는 앱의 책임이며, 예제 프로그램을 그대로 운영용 서비스로 간주해서는 안 된다.[1]

## Sources

[1] cloudflare/quiche 공식 README

<https://github.com/cloudflare/quiche/blob/master/README.md>

[2] cloudflare/quiche Cargo.toml

<https://github.com/cloudflare/quiche/blob/master/Cargo.toml>
