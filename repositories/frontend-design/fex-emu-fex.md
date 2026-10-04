---
title: "FEX-Emu/FEX"
repository: "FEX-Emu/FEX"
url: "https://github.com/FEX-Emu/FEX"
category: "frontend-design"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "frontend-design"
  - "starred-draft"
---

# FEX-Emu/FEX

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

FEX는 ARM64 Linux 장치에서 x86 및 x86-64 애플리케이션을 실행하기 위한 사용자 공간 에뮬레이터다.
CPU가 이해하는 명령 체계가 다른 프로그램을 실행하도록 중간에서 변환한다.
운영체제 전체를 가상 컴퓨터로 재현하는 도구와는 범위가 다르며, Windows 프로그램에는 Wine이나 Proton 같은 별도의 호환 계층을 함께 쓰는 경로를 안내한다.[1]

## 실행 파일이 있어도 CPU가 다르면

같은 Linux용 프로그램이어도 x86-64용으로 배포된 실행 파일을 ARM64 CPU가 그대로 이해할 수 있는 것은 아니다.
소스 코드를 다시 빌드할 수 없는 프로그램이라면 명령어 수준의 차이를 연결할 방법이 필요하다.
FEX는 이 문제를 사용자 프로그램 실행 범위에서 다룬다.
여기서 사용자 공간은 운영체제의 핵심인 커널이 아니라 일반 애플리케이션이 실행되는 영역을 뜻한다.[1][2]

FEXCore 문서는 커널 공간과 IRQ, 주기 단위로 정확한 하드웨어 재현 등을 목표 밖으로 둔다.
그러므로 FEX를 PC 전체를 부팅하는 가상 머신이나 커널 드라이버 실행기로 소개하면 틀린 범위를 전달하게 된다.
Linux용 프로그램의 CPU 명령 변환과 Windows API 호환성 역시 별개의 문제다.
Wine/Proton을 함께 언급하는 이유도 이 두 층을 구별해야 이해할 수 있다.[1][2]

## 명령어를 번역하는 층과 라이브러리를 연결하는 층

공식 Source Outline은 x86 명령 분석, 중간 표현 생성, 최적화, ARM64 코드 생성으로 소스 구조를 나눈다.
중간 표현인 IR은 어느 한 기계어를 그대로 유지하는 대신 변환과 최적화에 쓰는 내부 명령 형태다.
입력 명령을 해석한 뒤 IR로 바꾸고, 실행 장치에 맞는 코드를 생성하는 역할이 각 영역에 분리되어 있다. `Core.cpp`는 이 구성과 조회 캐시, 실행 진입점을 연결하는 파일로 설명된다.[3]

모든 동작을 CPU 명령 하나씩 번역해야 하는 것은 아니다.
FEX의 Thunklibs는 게스트 쪽 라이브러리 호출을 호스트 쪽 실제 라이브러리로 넘기는 연결 계층이다.
게스트는 원래 x86 프로그램이 기대하는 환경, 호스트는 실제 ARM64 시스템이다.
README는 OpenGL·Vulkan 같은 호스트 라이브러리로 호출을 전달해 에뮬레이션 부담을 줄이는 기능을 설명한다.
이는 프로젝트의 설계 설명이지 이 글에서 측정한 속도 향상 수치가 아니다.[1][4]

Thunklibs 문서에 따르면 호출 인자를 묶고, 호스트로 전환하고, 다시 인자를 풀어 실제 함수를 부른 후 반환 값을 전달하는 단계가 있다.
호스트에서 게스트 함수를 되부르는 콜백 경로도 구별한다.
단순 이름 바꾸기가 아니라 서로 다른 실행 환경 사이에서 호출과 반환 규약을 연결하는 것이다. `libasound_Guest.cpp`는 ALSA 헤더와 생성된 thunk 코드 파일을 포함하고 라이브러리를 로드하는 작은 실제 예다.
짧은 파일만 보아도 반복되는 연결 코드를 생성 도구에 맡긴다는 문서 설명과 연결된다.[4][5]

## 예시로 따라가는 흐름

다음은 소유하거나 실행 허가를 받은 x86-64 Linux 프로그램을 ARM64 Linux 장치에서 열려는 이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
입력은 이미 빌드된 프로그램과 그 프로그램이 필요로 하는 x86-64 사용자 환경이다.
먼저 장치의 명령 확장이 요구 조건에 맞는지 확인하고, 필요한 라이브러리를 담은 RootFS를 준비해야 한다.
RootFS는 여기서 게스트 프로그램이 찾을 파일과 라이브러리의 기본 파일 시스템을 뜻한다.[1]

프로그램이 실행 경로에 들어가면 FEX의 프런트엔드는 x86 명령과 블록을 분석하고, IR 변환과 최적화 뒤 ARM64 코드를 만드는 흐름으로 이어진다.
외부 라이브러리 호출에서는 구성에 따라 호스트 라이브러리 연결 계층을 이용할 수 있다.
예컨대 오디오 라이브러리 쪽 구조를 읽으려면 `libasound_Guest.cpp`의 생성 코드 포함과 Thunklibs의 인자 포장·반환 설명을 함께 보면 된다.
이 사례에서 특정 프로그램이 실제로 소리를 재생했다거나 완전히 호환되었다고 가정하지 않는다.[3][4][5]

결과를 확인할 사람은 창이 열리는지와 계산·그래픽·오디오가 의도대로 동작하는지를 별도로 살펴야 한다.
어떤 실패가 명령 변환 문제인지, 게스트 라이브러리 누락인지, 호스트 그래픽 환경 문제인지도 구분해야 한다.
Windows 게임을 대상으로 바꾸면 Wine/Proton까지 추가되므로 FEX만의 성공 여부로 전체 호환성을 설명할 수 없다.
이처럼 입력 CPU 형식, 게스트 파일 환경, 호스트 라이브러리, 운영체제 API를 구별하는 것이 사례의 핵심이다.[1][4]

## 호환성과 성능을 단정하지 말아야 하는 이유

루트 README는 ARMv8.0-a 이상과 `FEAT_FP`, `FEAT_CRC32` 확장을 요구하며 x86-64 RootFS도 명시한다.
반면 FEXCore의 목표 설명에는 ARMv8.1을 대상으로 한 문구가 남아 있다.
사용 조건은 루트 안내를 출발점으로 삼되, 하위 설계 문서의 목표와 현재 설치 조건을 같은 의미로 읽지 않는 편이 정확하다.
이 글은 두 문서의 차이를 하드웨어 실험으로 해소하지 않았다.[1][2]

실험적인 코드 캐시와 프로그램별 설정도 README에 소개되어 있다.
메모리 모델 에뮬레이션을 생략하는 등의 조정은 속도 선택지로 언급되지만, 모든 프로그램에 같은 설정을 적용해도 된다는 보장은 아니다.
FEXCore에 적힌 네이티브 대비 성능 목표 역시 개발 목표이며 현재 측정 결과가 아니다.
지원 기능 목록이나 목표 문구로 특정 게임의 프레임 수와 실행 성공을 예측하지 않아야 한다.[1][2]

또한 에뮬레이션은 출처 불명의 실행 파일을 안전하게 만드는 기능과 다르다.
읽은 자료의 목적은 다른 CPU용 사용자 프로그램 실행이며, 보안 격리 제품으로서의 보호 수준을 검증한 자료는 아니다.
소유·이용 권한이 있는 프로그램을 대상으로 하고, 실행할 파일의 신뢰와 접근 권한은 별도로 관리해야 한다.

## 직접 읽어볼 자료

- [루트 README](https://github.com/FEX-Emu/FEX/blob/main/Readme.md)
  먼저 하드웨어 조건과 RootFS 요구를 확인한다.
  Windows 게임 언급에서는 FEX와 Wine/Proton이 서로 다른 문제를 맡는다는 점을 구분하고, 실험적 기능을 기본 호환성 보장으로 읽지 않는다.
- [Source Outline](https://github.com/FEX-Emu/FEX/blob/main/docs/SourceOutline.md)
  frontend의 x86-to-ir, backend의 ARM64, glue의 driver 순으로 읽으면 명령 변환의 전체 경로를 잡을 수 있다.
  모든 파일을 읽기 전에 각 단계가 입력과 출력 중 무엇을 바꾸는지 질문해 볼 수 있다.
- [FEXCore의 목표와 비목표](https://github.com/FEX-Emu/FEX/blob/main/FEXCore/Readme.md)
  Goals와 Not desired를 나란히 비교한다.
  성능 목표를 실측 수치와 구분하고 사용자 공간만 다룬다는 제한을 먼저 파악하는 자료다.
- [Thunklibs 설명](https://github.com/FEX-Emu/FEX/blob/main/ThunkLibs/README.md)과 [오디오 라이브러리의 게스트 진입 파일](https://github.com/FEX-Emu/FEX/blob/main/ThunkLibs/libasound/libasound_Guest.cpp)
  인자가 어떤 순서로 포장되고 반환되는지 문서로 읽은 다음 생성 코드가 포함되는 지점을 본다.
  작은 파일에 전체 함수 구현이 없는 이유를 이해하는 경로다.

## 정리

FEX는 ARM64 Linux에서 x86 사용자 프로그램을 실행하는 변환 계층이다.
CPU 명령 번역과 호스트 라이브러리 연결을 함께 사용하지만, 운영체제 전체의 재현이나 모든 프로그램의 호환성을 약속하는 도구는 아니다.

## 자료 확인 범위

2026-09-27에 공식 README, 소스 구조와 FEXCore·Thunklibs 문서, 오디오 라이브러리 게스트 파일을 확인했다.
FEX 설치나 프로그램 실행은 하지 않았고 호환성·성능을 측정하지 않았다.

## 사용자 생각

아래는 실제 판단을 넣기 전까지 비워두는 영역입니다.

- [ ] 지금 보려는 이유가 분명한가?
- [ ] 내 작업 환경에서 바로 적용할 수 있을까?
- [ ] 실험 10~20분으로 검증 가능한 값이 있는가?

## 나중에 할 일

- [ ] README 전체 읽기
- [ ] 설치/실행 예시가 있는지 확인
- [ ] 장단점, 주의점, 대체안 비교
- [ ] 블로그 글 제목/개인 결론 반영

## Sources

[1] FEX-Emu/FEX — Readme.md

<https://github.com/FEX-Emu/FEX/blob/main/Readme.md>

[2] FEX-Emu/FEX — FEXCore/Readme.md

<https://github.com/FEX-Emu/FEX/blob/main/FEXCore/Readme.md>

[3] FEX-Emu/FEX — docs/SourceOutline.md

<https://github.com/FEX-Emu/FEX/blob/main/docs/SourceOutline.md>

[4] FEX-Emu/FEX — ThunkLibs/README.md

<https://github.com/FEX-Emu/FEX/blob/main/ThunkLibs/README.md>

[5] FEX-Emu/FEX — ThunkLibs/libasound/libasound_Guest.cpp

<https://github.com/FEX-Emu/FEX/blob/main/ThunkLibs/libasound/libasound_Guest.cpp>
