---
title: "ghostty-org/ghostty"
repository: "ghostty-org/ghostty"
url: "https://github.com/ghostty-org/ghostty"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# ghostty-org/ghostty

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Ghostty는 명령행 프로그램의 입력과 출력을 창 안에 보여 주는 터미널 에뮬레이터다.
명령을 해석하는 셸이나 개발 언어 자체가 아니라, 그런 프로그램과 사용자를 연결하는 화면·입출력 계층이다.
독립 GUI와 다른 프로그램에 넣을 수 있는 `libghostty`를 함께 소개한다.[1]

## 화면만 바꾸는 도구와 다른 점

명령행 도구는 글자뿐 아니라 색, 커서 이동, 이미지 같은 제어 정보를 보낸다.
터미널은 이를 해석하고 현재 화면 상태를 유지하면서 키 입력을 프로그램으로 전달해야 한다.
README는 표준과 xterm 동작을 우선하는 호환성, 다중 창·탭·분할, 운영체제에 맞는 UI를 주요 목표로 설명한다.[1]

공통 코어는 Zig이며 macOS 앱은 SwiftUI, Linux 앱은 GTK를 사용한다고 안내한다.
렌더링은 화면을 실제 픽셀로 그리는 단계로, README는 macOS의 Metal과 Linux의 OpenGL을 명시한다.
프로젝트의 성능 비교 표현은 이번 조사에서 측정한 벤치마크가 아니다.[1]

## 셸 실행과 화면 처리가 만나는 지점

`src/termio/Exec.zig`는 하위 프로세스를 PTY와 함께 시작·종료하고 읽기 스레드에서 출력을 가져오는 역할을 설명한다.
PTY는 프로그램이 실제 터미널에 연결된 것처럼 입출력을 주고받게 하는 가상 단말이다.
읽은 데이터는 Termio 인스턴스로 전달되므로 프로그램 실행과 터미널 데이터 처리가 분리된 구조를 볼 수 있다.[2]

이 파일의 `init()`은 실행에 필요한 내부 상태만 준비하며 프로세스를 시작하지 않는다고 명시한다.
초기화, 실행, 정리 단계를 구분하면 창이 생겼다는 사실과 셸이 실제 동작한다는 사실을 혼동하지 않게 된다.
`deinit()`은 보관한 하위 프로세스 상태의 정리를 위임한다.[2]

## 가상 예시: 같은 셸을 다른 화면에서 쓰기

다음은 설치나 속도 시험을 수행한 기록이 아니다.
개인 프로젝트에서 한 창에는 편집기를, 다른 분할에는 테스트 명령을 띄운다고 가정한다.
실제 명령 실행은 셸과 하위 프로그램이 맡고, Ghostty는 입력을 전달하고 프로그램 출력을 해석해 표시한다.[1][2]

색이 깨지는 문제와 명령 자체가 실패하는 문제는 원인이 다를 수 있다.
전자는 터미널 제어 정보와 표시 경로를, 후자는 실행 환경과 프로그램 로그를 나누어 보는 관점이 필요하다.
탭과 분할은 작업을 정리하는 UI이지 명령을 자동으로 올바르게 만들어 주는 기능은 아니다.

## GUI와 라이브러리의 지원 범위를 구분하기

README는 `libghostty-vt`가 터미널 시퀀스 해석과 상태 유지에 초점을 둔다고 설명한다.
라이브러리의 Windows·WebAssembly 지원을 곧바로 동일한 데스크톱 GUI 지원으로 읽으면 안 된다.
또한 기능의 안정성과 별개로 라이브러리 API 서명은 변화 중이며 아직 버전 태그가 없다고 안내한다.[1]

충돌 보고서는 로컬에 저장되고 자동 전송되지 않는다고 명시되어 있다.
다만 스레드의 스택 메모리를 포함할 수 있으므로 수동 공유 전 내용 검토가 필요하다.[1]
루트 라이선스는 MIT이며 소프트웨어 재사용 시 저작권과 허가 고지 보존 조건이 있다.[3]

## 직접 읽어볼 자료

- README의 Native Platform Experiences와 libghostty 설명을 비교해 앱과 내장 라이브러리의 경계를 읽는다.[1]
- `src/termio/Exec.zig` 첫 주석과 `init()`을 읽고 상태 준비와 실제 프로세스 실행을 구분한다.[2]
- README의 Crash Reports는 오류 보고서를 외부에 공유하기 전에 확인할 자료다.[1]

## 정리

Ghostty는 운영체제에 맞는 창과 공통 터미널 코어를 함께 설계하는 프로젝트다.
사용자에게 보이는 UI, 셸 프로세스, 재사용 가능한 터미널 라이브러리를 나누어 보면 구조가 명확해진다.[1][2]

## 자료 확인 범위

2026-10-28 공식 저장소 README, 루트 구조와 위 대표 파일을 읽었다.
설치·빌드·서비스 실행은 하지 않았으며 개인 사용 경험이나 채택 평가는 작성하지 않았다.

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

## Sources

[1] ghostty-org/ghostty — README.md

<https://github.com/ghostty-org/ghostty/blob/main/README.md>

[2] ghostty-org/ghostty — src/termio/Exec.zig

<https://github.com/ghostty-org/ghostty/blob/main/src/termio/Exec.zig>

[3] ghostty-org/ghostty — LICENSE

<https://github.com/ghostty-org/ghostty/blob/main/LICENSE>
