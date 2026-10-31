---
title: "EpicGames/raddebugger"
repository: "EpicGames/raddebugger"
url: "https://github.com/EpicGames/raddebugger"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# EpicGames/raddebugger

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

RAD Debugger는 로컬 네이티브 프로그램의 실행 상태를 살펴보는 그래픽 디버거이며, 디버그 정보 형식 RDI와 링커 개발도 같은 프로젝트에서 다룬다.[1]
브라우저용 JavaScript 디버거나 모든 운영체제에 연결되는 원격 디버깅 서비스는 아니다.
현재 README는 Windows·Linux의 로컬 x64를 지원 범위로 적고 Linux 지원은 초기 단계라고 밝힌다.[1]

## 실행 파일을 사람이 읽을 수 있는 정보와 연결하기

컴파일된 프로그램에서 실행 주소만 보면 어느 함수나 소스 줄에 해당하는지 알기 어렵다.
디버그 정보는 이런 기계 수준의 위치와 사람이 작성한 코드 사이를 연결한다.
RAD Debugger는 도구마다 다른 PDB·DWARF 정보를 직접 공통 사용하기보다 자체 RDI 형식으로 필요할 때 변환한다.[1]

README는 디버거, 변환·덤프 도구 `radbin`, 대형 프로그램 링크를 겨냥한 RAD Linker를 구분한다.
링커는 여러 컴파일 결과를 실행 파일로 합치는 도구로, 디버거의 화면 조작과는 다른 단계다.
RAD Linker는 일반 PDB와 선택적 RDI 생성을 설명하지만 링크 시점 최적화는 아직 지원하지 않는다고 명시한다.[1]

## 대표 파서에서 확인한 형식 검사

루트의 `src`는 소스 코드, `data`는 빌드에 사용하는 작은 바이너리 자료를 담는 구역이다.
README는 소스를 비교적 평평한 계층으로 나누고 짧은 접두어로 소속을 표현하는 방식을 설명한다.[2][1]

`src/lib_rdi/rdi_parse.c`의 `rdi_parse`는 입력 바이트와 크기를 받아 RDI 헤더를 먼저 확인한다.
헤더 크기와 magic 값이 맞지 않으면 형식 불일치 상태를, encoding version이 다르면 지원하지 않는 버전 상태를 반환한다.[3]

그다음 데이터 섹션 표의 끝이 입력 크기 안에 있는지 검사한다.
검사가 통과하면 원본 데이터·크기·섹션 표·섹션 수를 결과 구조에 연결한다.
이 순서는 RDI가 단순한 파일 확장자가 아니라 버전과 구조를 가진 이진 계약이라는 점을 보여준다.[3]

## 가상 시나리오: 작은 C 프로그램의 오류 위치 찾기

자신이 만든 공개 C 프로그램이 예상과 다르게 종료되는 상황을 가정한다.
지원하는 x64 환경에서 디버그 정보를 포함한 실행 파일을 준비하고 RAD Debugger로 상태를 관찰한다.
기존 PDB·DWARF를 RDI로 변환하는 경로가 실행 주소와 소스 정보를 연결하는 바탕이 된다.[1]

형식 자체를 공부하고 싶다면 `radbin`의 변환·텍스트 덤프와 RDI 파서의 헤더 검사 부분을 비교해 볼 수 있다.
이 과정은 디버거를 사용해 버그를 찾는 학습과 디버그 도구 내부 형식을 배우는 학습을 구분해 준다.
실제 오류 재현이나 실행 파일 로딩을 수행한 기록은 아니다.[1][3]

## 아직 확정된 지원과 계획을 구분하기

프로젝트는 ALPHA이며 원격·다중 머신 및 다른 아키텍처 지원은 계획으로 소개한다.
루트 README는 기술 개요이며 실제 조작법은 릴리스에 함께 들어가는 별도 README를 보라고 안내한다.[1]

링커의 large pages 옵션은 기본적으로 꺼져 있고, Windows 환경의 메모리 단편화와 재부팅 위험 때문에 초기화 가능한 VM·컨테이너 사용을 권고한다.
README의 성능 수치는 특정 테스트 조건의 주장으로 읽어야 하며 이 초안에서 재측정하지 않았다.[1]
라이선스는 MIT 조건으로 저작권·허가 고지 유지와 무보증 조항을 포함한다.[4]

## 직접 읽어볼 자료

- [1] 첫 지원 범위와 RDI·RAD Linker 항목을 먼저 읽어 세 도구의 목적을 구분한다.
- [3] `rdi_parse`의 헤더 검사 → 섹션 표 범위 검사 → 결과 연결을 따라간다.
- [1][4] 릴리스 조작 안내와 빌드 요구사항, 배포 시 고지 조건을 확인한다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 목록, 아래 대표 구현·문서와 라이선스를 확인했다.
설치·빌드·프로그램 실행은 하지 않았으며, 예시는 이해를 위한 가상 시나리오다.

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

[1] EpicGames/raddebugger — README

<https://github.com/EpicGames/raddebugger/blob/master/README.md>

[2] EpicGames/raddebugger — 루트 파일 목록

<https://api.github.com/repos/EpicGames/raddebugger/contents>

[3] EpicGames/raddebugger — src/lib_rdi/rdi_parse.c

<https://github.com/EpicGames/raddebugger/blob/master/src/lib_rdi/rdi_parse.c>

[4] EpicGames/raddebugger — LICENSE

<https://github.com/EpicGames/raddebugger/blob/master/LICENSE>
