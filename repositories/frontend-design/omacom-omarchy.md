---
title: "omacom/omarchy"
repository: "omacom/omarchy"
url: "https://github.com/omacom/omarchy"
category: "frontend-design"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "frontend-design"
  - "starred-draft"
---

# omacom/omarchy

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Omarchy는 Arch Linux를 기반으로 Hyprland와 Quickshell, 여러 애플리케이션과 기본 설정을 함께 구성한 Linux 배포판이다.
배경화면만 바꾸는 테마 모음이 아니라 창 배치, 터미널, 메뉴, 알림과 프로그램 사용 방식을 포함한 데스크톱 환경이다.
README는 저장소의 `manual/`을 권위 있는 설명서로 지정한다.[1][2]

## 선택지를 모으기보다 하나의 기본 환경을 정하기

Linux 데스크톱을 직접 구성할 때는 창 관리자, 실행 메뉴, 터미널, 편집기, 색상과 단축키를 따로 선택하게 된다.
각 도구는 잘 동작해도 조작 방식과 표현이 어긋날 수 있다.
Omarchy는 이 선택을 한 묶음으로 제안한다.
공식 소개의 omakase라는 표현도 모든 조합을 중립적으로 나열하기보다 특정한 사용 방식을 골라 제공한다는 뜻에 가깝다.[2]

Hyprland는 창을 타일처럼 배치하는 창 관리자이고, Quickshell은 데스크톱 구성 요소를 만드는 기반이다.
소개 문서는 Neovim, Chromium, Obsidian, LibreOffice, 영상 도구 등도 함께 언급한다.
따라서 Omarchy의 정체는 새 운영체제 커널을 직접 구현한 프로젝트보다 기존 Linux 구성 요소를 일관된 환경으로 조합한 배포판으로 이해하는 편이 맞다.[2]

공식 설명은 Windows나 macOS와 최대한 비슷해지는 것을 목표로 하지 않는다고 밝힌다.
터미널 사용과 설정 파일 편집이 자연스러운 환경을 전제로 한다.
화면이 익숙하거나 예쁘다는 인상만으로 전환 비용이 없다고 판단하기보다는 창 조작과 기본 응용 프로그램의 선택까지 하나의 구성으로 봐야 한다.
이 글은 외관에 대한 개인적 호불호를 평가하지 않는다.[2]

## 여러 프로그램의 색상을 연결하는 테마

테마 설명서에 따르면 하나의 테마는 데스크톱·터미널·Neovim·btop·Chromium과 상단 바, 메뉴, 알림, 잠금 화면 등의 표현을 함께 바꾼다.
테마별 배경도 선택할 수 있다.
다만 Obsidian은 앱 안에서 Omarchy 테마를 수동으로 선택해야 한다는 예외가 명시되어 있다.
공통 테마를 제공한다는 것과 모든 앱이 같은 설정을 자동으로 읽는다는 말은 다르다.[3]

대표 스크립트 `omarchy-theme-set`은 이 조정을 실제로 연결한다.
요청받은 테마 이름과 디렉터리를 확인하고, 다음 테마를 준비하는 임시 영역을 만든 뒤 공식 설정에 사용자 설정을 겹쳐 놓는다.
동적 설정을 생성하고 현재 테마 영역을 교체한 다음 데스크톱 셸에 새 팔레트를 전달하며, 프로그램별 후처리 명령도 호출한다.
한 장의 배경을 복사하는 작업보다 넓은 범위다.[4]

동시에 테마가 바뀌는 상황도 코드에 반영되어 있다.
공유된 임시 영역과 현재 테마 상태를 여러 호출이 함께 바꾸면 선택이 뒤섞일 수 있으므로 잠금을 사용한다.
외부 저장소에서 가져온 테마에서는 Lua나 터미널 설정 등 코드 실행과 연결될 수 있는 일부 파일을 제외하는 처리도 있다.
이는 색상 자료와 실행 가능한 설정의 경계를 구별하려는 구현이며, 모든 외부 테마의 안전성을 검증했다는 뜻은 아니다.[4]

## 예시로 따라가는 흐름

공식 설명서의 메뉴 경로를 따라 Tokyo Night 테마를 선택하는 상황을 보자.
문서상 입력은 Omarchy 메뉴의 Style, Theme에서 선택한 테마이며, 스크립트의 사용 예에도 같은 테마 이름이 등장한다.
다음은 문서와 코드를 따라간 흐름으로, 실제 데스크톱에서 테마를 전환한 결과가 아니다.[3][4]

선택이 적용 단계에 들어가면 스크립트는 테마 이름을 정리하고 해당 테마가 존재하는지 확인한다.
다음 테마 영역에는 공식 자료가 먼저 복사되고 사용자 설정이 덧입혀진다.
색상 자료가 오래된 형식이면 변환하는 경로도 거친다.
이어 템플릿으로 앱별 설정을 만들고, 가능한 경우 이전 배경과 다음 배경을 확인해 화면 전환을 준비한다.
여기서 사람은 어떤 사용자 설정이 공식 설정을 덮는지와 배경 파일이 실제 존재하는지를 구분해 볼 수 있다.[4]

새 상태가 현재 테마가 되면 셸에 색상 자료를 전달하고 터미널·브라우저·편집기 등의 후처리 함수를 호출한다.
의도한 결과는 서로 다른 프로그램의 표현이 선택한 팔레트로 이어지는 것이다.
확인할 때는 바탕화면만 보지 말고 터미널 가독성, 알림과 잠금 화면, 앱별 예외를 함께 살펴야 한다.
Obsidian처럼 추가 선택이 필요한 경우는 실패와 구분해야 한다.
이 사례는 테마 전환이라는 작은 조작이 여러 구성 요소의 상태 변경을 조정하는 작업임을 보여준다.[3][4]

## 설치는 외관 변경이 아니라 시스템 변경

Getting Started는 ISO를 이용한 설치를 설명하며 전체 디스크 설치와 남은 공간 설치를 구별한다.
전체 디스크 옵션은 선택한 드라이브를 지우므로 기존 자료가 있는 경우 백업하라고 명시한다.
기본 암호화와 부팅 시 암호 입력도 안내하며, Bluetooth 키보드 대신 유선 또는 동글 방식 키보드가 필요한 조건을 설명한다.
이 글에서는 드라이브 선택이나 설치 절차를 실행하지 않았다.[5]

공식 설치 안내에는 펌웨어의 Secure Boot·TPM 설정 변경 요구도 포함되어 있다.
이런 설정은 외관이나 편의성의 문제가 아니라 장치의 보안·부팅 정책과 관계되므로, 안내를 그대로 따라 하기 전에 자기 하드웨어와 현재 운영체제의 보호 기능을 별도로 검토해야 한다.
특정 장치가 지원되는지, 기존 암호화·복구 설정에 어떤 영향이 있는지는 이번 자료 읽기로 확인하지 않았다.[5]

추가 테마 역시 단순 이미지로만 취급하지 않는 편이 안전하다.
읽은 스크립트는 저장소에서 설치된 테마를 구별하여 제한하지만, 사용자가 직접 만든 디렉터리와 연결된 작업 사본에는 다른 처리를 적용한다.
신뢰 경계가 경로와 설치 방식에 연결되어 있으므로, 제한 코드가 있다는 사실만으로 임의 자료를 안심하고 넣을 수 있다고 결론 내릴 수 없다.[4]

## 직접 읽어볼 자료

- [README와 매뉴얼 목차](https://github.com/omacom/omarchy/blob/quattro/README.md), [Welcome 문서](https://github.com/omacom/omarchy/blob/quattro/manual/01-welcome-to-omarchy.md)
  먼저 배포판의 구성과 지향하는 조작 방식을 읽는다.
  어떤 앱을 포함하는지뿐 아니라 터미널·타일형 창 관리가 기본이라는 점을 확인한 뒤 필요한 매뉴얼 장으로 이동한다.
- [Themes 안내](https://github.com/omacom/omarchy/blob/quattro/manual/06-themes.md)
  한 테마가 적용되는 화면 범위와 Obsidian의 예외를 확인한다.
  배경 선택과 부팅 잠금 해제 화면의 스타일은 메뉴가 구별되어 있다는 점도 함께 볼 수 있다.
- [테마 적용 스크립트](https://github.com/omacom/omarchy/blob/quattro/bin/omarchy-theme-set)
  이름 검증, 임시 영역 준비, 현재 상태 교체, 후처리 순으로 읽는다.
  외부 테마에서 제외하는 파일과 잠금 사용 부분을 보면 편의 기능에 어떤 안전·동시성 조건이 붙는지 드러난다.
- [Getting Started](https://github.com/omacom/omarchy/blob/quattro/manual/02-getting-started.md)
  설치 속도 홍보보다 디스크 삭제와 암호화, 입력 장치 조건을 먼저 확인한다.
  설치 방식이 기존 운영체제와 자료에 미치는 차이를 판단하기 위한 자료다.

## 정리

Omarchy는 여러 Linux 도구를 특정한 사용 방식으로 엮은 데스크톱 배포판이다.
테마의 일관성 뒤에는 앱별 설정 조정이 있고, 도입에는 화면 변경을 넘어 디스크·부팅·입력 장치 조건을 확인하는 일이 따른다.

## 자료 확인 범위

2026-09-27 기준 README, Welcome·Themes·Getting Started 매뉴얼과 테마 적용 스크립트를 읽었다.
ISO 설치나 데스크톱 실행, 테마 변경, 하드웨어 호환성 시험은 하지 않았다.

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

[1] omacom/omarchy — README.md

<https://github.com/omacom/omarchy/blob/quattro/README.md>

[2] omacom/omarchy — manual/01-welcome-to-omarchy.md

<https://github.com/omacom/omarchy/blob/quattro/manual/01-welcome-to-omarchy.md>

[3] omacom/omarchy — manual/06-themes.md

<https://github.com/omacom/omarchy/blob/quattro/manual/06-themes.md>

[4] omacom/omarchy — bin/omarchy-theme-set

<https://github.com/omacom/omarchy/blob/quattro/bin/omarchy-theme-set>

[5] omacom/omarchy — manual/02-getting-started.md

<https://github.com/omacom/omarchy/blob/quattro/manual/02-getting-started.md>
