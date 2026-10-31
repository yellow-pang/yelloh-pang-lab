---
title: "xtool-org/xtool"
repository: "xtool-org/xtool"
url: "https://github.com/xtool-org/xtool"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# xtool-org/xtool

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

xtool은 SwiftPM 기반 프로젝트를 iOS 앱으로 빌드하고 서명·기기 설치까지 연결하는 도구다.
Linux·WSL·macOS에서 Xcode의 일부 개발 기능을 대신하는 CLI이며, Apple SDK와 서명 체계 자체를 없애는 프로젝트는 아니다.[1][2]

## 앱 코드를 실행 가능한 묶음으로 만들기

SwiftPM은 Swift 코드와 의존성을 패키지로 관리하는 도구다.
iOS 기기에 앱을 올리려면 코드 컴파일뿐 아니라 앱 구조 구성, 서명, 기기 연결이 필요하다.
xtool은 이 단계를 묶고 Apple Developer Services와 기기 제어에 사용할 `XKit` 라이브러리도 제공한다.[1]

README의 Xcode replacement라는 표현을 IDE의 모든 기능을 동일하게 제공한다는 뜻으로 읽으면 범위가 넓어진다.
확인한 중심 경로는 SwiftPM 앱 제작과 배포이며, 기존의 임의 Xcode 프로젝트가 수정 없이 호환된다고 입증한 것은 아니다.[1][3]

## 빌드와 실행의 실제 경계

`Sources/XToolSupport/DevCommand.swift`의 `PackOperation`은 현재 디렉터리에 `Package.swift` 계열 파일이 있는지 검사한다.
SDK를 확인하고 `xtool.yml`을 읽어 설정을 얻으며, 설정 파일이 없으면 기본 조직 식별자를 사용한다는 경고를 낸다.
이어 `BuildSettings`와 `Planner`로 계획을 만들고 `Packer.pack`으로 앱 묶음을 생성한다.[3]

`DevBuildCommand`는 서명 여부와 `.ipa` 출력 여부를 따로 받는다.
서명을 선택했는데 저장된 인증 정보가 없으면 실패하며, IPA가 필요하면 앱을 Payload 디렉터리에 넣고 압축한다.
앱 파일을 만드는 것과 기기에 설치할 수 있도록 서명하는 것은 별도 선택이라는 뜻이다.[3]

`DevRunCommand`는 앱 묶음을 만든 뒤 기기 연결을 얻고 `IntegratedInstaller.install`을 호출한다.
`dev`의 기본 하위 명령은 이 실행 경로다.
macOS에서만 컴파일되는 Simulator 옵션도 있으므로 플랫폼별 지원을 하나로 뭉뚱그리면 안 된다.[3]

## 가상 예시: Linux에서 작은 개인 iOS 앱 만들기

SwiftPM 기반의 작은 메모 앱을 연결된 iPhone에 올리는 상황을 가정한다.
개발자는 먼저 Swift 도구와 기기 통신, SDK, 인증을 준비하고 프로젝트 디렉터리에서 개발 명령을 사용한다.
소스와 설정에서 앱 묶음이 만들어지고, 인증과 기기 연결을 거쳐 설치하는 흐름이다.[1][3][2]

컴파일 실패, 서명 실패, USB 연결 실패는 서로 다른 단계의 문제다.
최종 앱이 실제로 실행되고 화면이 의도대로 동작하는지는 별도의 기기 확인이 필요하며, 이 글은 배포를 직접 수행한 기록이 아니다.

## Xcode를 실행하지 않아도 필요한 것

Linux/Windows 설치 문서는 Windows에 WSL과 USB 전달 설정이 필요하다고 설명한다.
Linux 쪽은 Swift 도구 체인과 `usbmuxd`가 필요하며, Apple에서 받은 `Xcode.xip`로 iOS Swift SDK를 생성한다.
Xcode 파일 다운로드에는 Apple ID 로그인과 라이선스 동의가 필요하다.[2]

인증은 유료 Apple Developer Program의 API Key 방식과 비공개 Apple API를 사용하는 암호 방식을 구분한다.
따라서 무료 계정 지원을 공식 공개 API만 사용한다는 의미로 받아들이면 안 된다.
문서의 도구 체인·Xcode 버전은 바뀔 수 있으므로 설치 시점의 조합을 다시 확인해야 한다.[2]

xtool 코드는 MIT 라이선스다.
Apple SDK와 계정 서비스의 이용 조건까지 MIT로 바뀌는 것은 아니므로 배포 조건은 각 원본 정책을 별도로 확인해야 한다.[2][4]

## 직접 읽어볼 자료

1. README: CLI의 setup·auth·sdk와 dev·install 역할을 나누어 본다.[1]
2. `Sources/XToolSupport/DevCommand.swift`: PackOperation → build/run의 분기를 읽는다.[3]
3. `Documentation/xtool.docc/Installation-Linux.md`: WSL, 기기 통신, SDK와 인증의 전제 조건을 확인한다.[2]

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 자료를 확인했다. 설치·빌드·실행은 하지 않았다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] xtool-org/xtool — README.md

<https://github.com/xtool-org/xtool/blob/main/README.md>

[2] xtool-org/xtool — Documentation/xtool.docc/Installation-Linux.md

<https://github.com/xtool-org/xtool/blob/main/Documentation/xtool.docc/Installation-Linux.md>

[3] xtool-org/xtool — Sources/XToolSupport/DevCommand.swift

<https://github.com/xtool-org/xtool/blob/main/Sources/XToolSupport/DevCommand.swift>

[4] xtool-org/xtool — LICENSE.md

<https://github.com/xtool-org/xtool/blob/main/LICENSE.md>
