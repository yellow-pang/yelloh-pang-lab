---
title: "peetzweg/opendisplay"
repository: "peetzweg/opendisplay"
url: "https://github.com/peetzweg/opendisplay"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# peetzweg/opendisplay

> https://github.com/peetzweg/opendisplay

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

OpenDisplay는 iPhone, iPad 또는 다른 Mac을 Mac의 추가 모니터로 활용하도록 화면과 입력을 기기 사이에서 전송하는 도구이다.[1]

## 이 Repository는 무엇인가?

같은 화면을 복제하는 미러링뿐 아니라 창을 옮길 수 있는 실제 확장 화면을 만들며, 별도의 송신 앱과 수신 앱으로 구성된다.[1]

화면 데이터는 케이블이나 로컬 네트워크의 직접 연결로 전달되며, 중앙 화면 중계 서버나 사용자 계정 없이 사용하는 방식을 제시한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- iPhone·iPad에는 USB 또는 WiFi로 연결하고, Mac 수신기에는 WiFi나 유선 네트워크로 화면을 보낼 수 있다.[1]
- 고해상도 화면 배율, 세로·가로 회전, 확장·미러링 모드와 여러 기기에 각각의 확장 화면을 제공하는 구성을 안내한다.[1]
- iOS 수신기의 터치를 클릭·끌기·두 손가락 스크롤로 전달하지만, Mac 수신기의 키보드·마우스 전달은 후속 과제로 구분한다.[1]

## 어떻게 동작하는가?

```text
송신 Mac과 수신 기기 연결 및 필요한 권한 허용.
↓
가상 화면 생성·화면 캡처·H.264 영상 압축.
↓
수신 화면 표시와 지원되는 터치 입력의 역방향 전달.
```

수신 기기가 연결을 기다리고 Mac이 접속하는 구조이므로, 같은 화면 전송 흐름을 USB와 WiFi 양쪽에서 사용할 수 있다.[1]

### 확인한 파일 구성

- `project.yml`은 송신 Mac, iOS 수신기와 Mac 수신기를 별도 앱 대상으로 정의하고, 각각 `Mac`, `iOS`, `MacReceiver` 및 공통 `Shared` 소스를 연결한다.[2]
- `PROTOCOL.md`는 기기 사이에 오가는 영상·제어 메시지의 전송 규약을 정의하고, 화면 생성이나 렌더링 같은 내부 구현은 규약 범위와 구분한다.[3]

## 어떤 기술을 사용하는가?

- `Swift`와 `XcodeGen`: 앱 언어 설정과 플랫폼별 빌드 대상 생성에 사용한다.[1][2]
- `ScreenCaptureKit`·`VideoToolbox`: Mac 화면을 캡처하고 H.264 영상으로 인코딩하며, 수신 측은 `AVSampleBufferDisplayLayer`로 표시한다.[1]
- `usbmuxd`·`Bonjour`: 각각 USB 통신 연결과 로컬 네트워크 기기 발견을 맡는다.[1]

## 실제로 어디에 사용할 수 있는가?

- 보유한 iPhone이나 iPad를 개인 Mac의 추가 작업 화면으로 사용하는 구성을 살펴볼 수 있다.[1]
- 송신·수신 앱과 공개 프로토콜을 비교하여 화면 스트리밍과 입력 전달이 분리되는 방식을 학습할 수 있다.[1][3]

## 왜 주목할 만한가?

학습 관점에서는 화면 생성과 인코딩은 송신 측에 두고, 수신 측이 화면 정보와 입력을 되돌려주는 양방향 구조가 관찰점이다.[1]

## 장점

- 동일한 프로토콜을 공유하는 별도 수신 앱으로 iOS 기기와 Mac을 추가 화면으로 활용한다.[1][2]
- 전송 규약을 문서로 공개하여 앱 내부 구현과 기기 간 호환성 규칙을 분리한다.[3]

## 단점 / 주의점

- 송신기는 macOS 14+, Mac 수신기는 macOS 12+, iOS 수신기는 iOS 15+를 대상으로 하며 화면 기록·손쉬운 사용 등의 권한이 필요하다.[1][2]
- 프로토콜 버전 3에는 TLS 암호화와 인증이 없으므로 신뢰할 수 있는 로컬 네트워크나 직접 케이블을 전제로 한다.[3]
- 가상 화면 생성에 비공개 `CGVirtualDisplay` API를 사용하여 OS 업데이트 영향이 가능하며, 현재 라이선스는 GPL-3.0이다.[1]

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

OpenDisplay는 가상 화면을 압축 전송하여 보조 기기를 확장 모니터로 만드는 앱이다.[1] 지원 OS·권한과 함께 비공개 API 및 전송 보안 제약을 구분해야 한다.[1][3]

## Sources

[1] peetzweg/opendisplay 공식 README

<https://github.com/peetzweg/opendisplay/blob/main/README.md>

[2] peetzweg/opendisplay project.yml

<https://github.com/peetzweg/opendisplay/blob/main/project.yml>

[3] peetzweg/opendisplay PROTOCOL.md

<https://github.com/peetzweg/opendisplay/blob/main/PROTOCOL.md>
