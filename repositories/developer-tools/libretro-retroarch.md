---
title: "libretro/RetroArch"
repository: "libretro/RetroArch"
url: "https://github.com/libretro/RetroArch"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# libretro/RetroArch

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

RetroArch는 libretro API를 구현한 코어를 불러와 실행하는 공통 프런트엔드다.
코어는 에뮬레이터나 게임 엔진 등을 담은 라이브러리이며 RetroArch는 화면·소리·입력과 실행 생명주기를 담당한다.
모든 기종의 동작을 하나의 엔진으로 직접 구현하거나 게임 콘텐츠를 제공하는 저장소로 이해하면 안 된다.[1]

## 실행 로직과 장치 처리를 분리하기

서로 다른 에뮬레이터마다 창, 오디오 출력, 컨트롤러 처리를 다시 만들면 플랫폼별 작업이 반복된다.
libretro는 이 사이에서 오디오·비디오·입력을 주고받을 공통 인터페이스를 정의한다.
RetroArch는 그 인터페이스의 대표 프런트엔드로 코어 바깥의 공통 기능을 맡는다.[1]

README는 게임패드·터치 중심 UI, 명령행 인터페이스와 여러 플랫폼 이식을 설명한다.
셰이더, 되감기, 녹화, run-ahead 같은 기능도 안내하지만 모든 코어와 장치에서 같은 결과를 보장한다는 뜻은 아니다.[1]

## 콜백으로 연결되는 프런트엔드와 코어

`libretro-common/include/libretro.h`는 양쪽이 따라야 할 함수와 자료 구조를 정의하는 헤더다.
콜백은 한쪽이 미리 등록해 두었다가 필요한 순간 다른 쪽에서 부르는 함수다.
`retro_set_video_refresh()`와 오디오·입력 콜백 등록 함수는 첫 `retro_run()` 전에 호출되어야 한다고 명시한다.[2]

`retro_load_game()`은 콘텐츠를 넘겨받아 로딩 성공 여부를 반환한다.
`retro_unload_game()`은 현재 콘텐츠를 해제하며 코어 정리 함수보다 먼저 호출한다고 문서화되어 있다.
이처럼 파일을 여는 일, 반복 실행, 화면 전달과 종료는 서로 다른 계약이다.[2]

헤더의 `retro_init()` 설명은 동적 라이브러리가 이전 세션부터 남아 있을 수 있으므로 전역 변수가 초기값이라고 가정하지 말라고 경고한다.
한 프로세스 안에서 코어를 다시 사용하는 경우 초기화와 정리가 중요한 이유를 보여 준다.[2]

## 가상 예시: 직접 만든 테스트 콘텐츠 실행

다음은 에뮬레이터나 게임을 실제 실행한 기록이 아니다.
사용 권한이 있는 테스트 콘텐츠와 이를 지원하는 코어를 준비했다고 가정한다.
프런트엔드는 코어와 영상·음성·입력 연결을 준비하고 콘텐츠를 로드한 뒤 실행 과정의 출력을 표시한다.[1][2]

컨트롤러 입력은 공통 프런트엔드에서 코어로 전달되고, 코어가 만든 결과는 다시 출력 경로로 돌아온다.
게임 실행 로직과 화면 출력 드라이버가 다르므로 검은 화면을 곧바로 콘텐츠 손상으로 단정할 수는 없다.
코어 호환성, 로딩 결과, 영상 드라이버 설정을 단계별로 나누어 볼 수 있다.[1][2]

## 플랫폼 수와 실제 호환성은 다르다

README는 실행에 libretro 구현이 필요하며 동적 로딩 때문에 빌드 시 반드시 포함될 필요는 없다고 설명한다.
따라서 RetroArch 실행 파일만 갖추었다고 원하는 콘텐츠까지 실행 준비가 끝나는 것은 아니다.[1]
OpenGL·Vulkan 등 영상 드라이버별로 요구 사항과 셰이더 지원이 다르므로 장치에 맞는 경로를 확인해야 한다.[1]

루트 `COPYING`은 GPLv3를 담고 있어 수정·재배포 시 해당 조건과 소스 제공 의무를 검토해야 한다.[3]
프런트엔드의 라이선스가 별도 코어나 게임·BIOS 콘텐츠의 사용 권한까지 대신 허용하지는 않는다.
코어와 콘텐츠를 가져올 때는 각각의 배포 조건과 출처를 따로 확인해야 한다.

## 직접 읽어볼 자료

- README의 libretro 설명과 Dependencies를 함께 읽어 프런트엔드와 코어의 의존 관계를 확인한다.[1]
- `libretro.h`에서 콜백 등록, `retro_load_game()`, `retro_run()`, 해제 함수 순서로 API 계약을 읽는다.[2]
- README의 그래픽 드라이버별 Requirements는 셰이더나 화면 문제가 있을 때 확인할 출발점이다.[1]

## 정리

RetroArch는 여러 코어의 실행을 공통 입출력과 UI로 연결하는 구조다.
코어 실행 능력, 플랫폼 출력 지원, 콘텐츠 사용 권한을 분리해서 이해해야 범위를 정확히 파악할 수 있다.[1][2]

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

[1] libretro/RetroArch — README.md

<https://github.com/libretro/RetroArch/blob/master/README.md>

[2] libretro/RetroArch — libretro-common/include/libretro.h

<https://github.com/libretro/RetroArch/blob/master/libretro-common/include/libretro.h>

[3] libretro/RetroArch — COPYING

<https://github.com/libretro/RetroArch/blob/master/COPYING>
