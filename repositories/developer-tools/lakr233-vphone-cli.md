---
title: "Lakr233/vphone-cli"
repository: "Lakr233/vphone-cli"
url: "https://github.com/Lakr233/vphone-cli"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# Lakr233/vphone-cli

> https://github.com/Lakr233/vphone-cli

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

vphone-cli는 Apple Silicon Mac에서 펌웨어를 준비하고 가상 iPhone을 생성·실행·관리하는 도구이다.[1]

## 이 Repository는 무엇인가?

실제 휴대전화 대신 Mac 안에서 실행되는 가상 머신을 다루며, Apple의 Virtualization.framework와 PCC 연구용 VM 기반을 이용한다.[1]

2.x에서는 `VPhone.bundle`이 펌웨어 준비·복원·VM 제어를 묶고, `vphone-launchpad`가 번들 설치와 VM 생성을 안내한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 가상 머신의 목록·상태 조회, 실행·정지, 백업 내보내기와 가져오기를 제공한다.[1]
- VM 창에서 앱과 파일 탐색, 클립보드·설정 도구, 화면 캡처·녹화와 진단 기능을 제공한다.[1]
- 게스트 내부 서비스가 선택적인 HTTP·WebSocket API와 창의 제어 기능을 지원하며, API 요청에는 토큰을 요구한다.[1]

## 어떻게 동작하는가?

```text
지원되는 물리 Mac과 호스트 조건, 호환되는 iPhone·cloudOS 펌웨어 조합을 확인한다.
↓
번들이 펌웨어 준비와 복원을 수행하여 가상 머신을 생성한다.
↓
별도 VM 프로세스와 창으로 게스트를 실행하고 관리 기능을 사용한다.
```

위 내용은 구성 요소의 관계를 설명한 연구용 개요이며, 호스트 보안 설정 변경이나 펌웨어 수정의 실행 절차는 포함하지 않는다.[1]

### 확인한 파일 구성

- `VPhoneExecutable/`은 CLI·VM 프로세스·펌웨어 처리·복원 기능을 담고, `VPhoneKit/`은 공통 호스트 라이브러리와 API 클라이언트를 담는다.[1]
- `VPhoneDaemon/`의 `vphoned`는 게스트 내부 제어 서비스이며, `VPhoneGuestComponents/`는 게스트 보조 구성 요소를 담는다.[1]
- `VPhoneExecutable/VPhoneCommand/VPhoneCommand/VPhoneCommand.swift`는 Swift 명령 정의에서 VM·펌웨어·복원·호스트·아카이브 관련 하위 명령을 연결한다.[2]

## 어떤 기술을 사용하는가?

- `Virtualization.framework`: Mac에서 가상 iPhone 실행 기반으로 사용하는 Apple 가상화 프레임워크이다.[1]
- `Swift`와 `ArgumentParser`: 대표 CLI 파일이 명령과 옵션을 선언하는 데 사용하는 언어와 명령행 파서이다.[2]
- `HTTP`와 `WebSocket`: 게스트 제어를 외부의 로컬 자동화 도구에 제공하는 선택적 API 방식이다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 연구 환경에서 호스트 프로그램·가상 머신·게스트 제어 서비스가 역할을 나누는 구조를 학습할 수 있다.[1]
- 자신이 관리하는 가상 기기의 화면·파일·진단 정보를 살펴보고 백업과 복원 흐름을 연구할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 준비와 수명 주기 관리를 맡는 CLI, 창을 가진 VM 실행 프로세스, 게스트 제어 서비스를 분리한 구성을 관찰할 수 있다.[1]

## 장점

- 펌웨어 준비·복원·VM 제어를 자체 포함 번들로 묶어 Launchpad와 터미널 흐름에서 이용한다.[1]
- 화면을 통한 사용과 토큰 기반 API 자동화를 함께 제공한다.[1]

## 단점 / 주의점

- 물리 Apple Silicon Mac과 macOS 15 이상을 요구하며, VM 생성에는 로컬 펌웨어가 있어도 복원 티켓을 위한 네트워크와 상당한 디스크 공간이 필요하다.[1]
- 권장 호스트 설정은 SIP를 유지하되 디버깅 제한을 완화하고 권한 있는 helper를 사용하므로, 일반 앱 실행과 같은 보안 전제로 다루어서는 안 된다.[1]
- 2.x는 `schemaVersion=2`로 생성한 VM만 시작하고 이전 VM은 다시 생성해야 하며, 프로젝트 코드의 라이선스는 MIT이다.[1][3]

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

vphone-cli는 가상 iPhone의 준비·실행·게스트 제어를 여러 구성 요소로 나누어 제공하는 연구 도구이다.[1] 호스트 보안 조건과 펌웨어 호환성, VM 형식 제약을 먼저 구분해야 한다.[1]

## Sources

[1] Lakr233/vphone-cli 공식 README

<https://github.com/Lakr233/vphone-cli/blob/main/README.md>

[2] Lakr233/vphone-cli VPhoneExecutable/VPhoneCommand/VPhoneCommand/VPhoneCommand.swift

<https://github.com/Lakr233/vphone-cli/blob/main/VPhoneExecutable/VPhoneCommand/VPhoneCommand/VPhoneCommand.swift>

[3] Lakr233/vphone-cli LICENSE

<https://github.com/Lakr233/vphone-cli/blob/main/LICENSE>
