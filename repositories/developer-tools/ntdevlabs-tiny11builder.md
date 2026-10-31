---
title: "ntdevlabs/tiny11builder"
repository: "ntdevlabs/tiny11builder"
url: "https://github.com/ntdevlabs/tiny11builder"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# ntdevlabs/tiny11builder

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Tiny11 Builder는 공식 Windows 설치 이미지에서 선택한 앱·구성 요소를 제거해 더 작은 설치 ISO를 만드는 도구다.
현재 기본 브랜치는 WinUI 앱을 중심으로 안내하며, 원래 PowerShell 빌더도 보존한다.
완성된 Windows 배포판이나 Windows 사용권을 제공하는 프로젝트는 아니다.[1]

## 용량을 줄일 때 무엇을 남길지 결정한다

Tiny11 모드는 업데이트·Defender·복구를 유지하면서 기본 앱과 원치 않는 기능을 제거한다.
Core는 서비스 유지 관리와 복구 지원을 더 제거하고 Defender를 비활성화하며, Custom은 개별 항목을 선택한다.
따라서 가장 작은 이미지가 항상 일상 사용에 적합하다는 결론은 성립하지 않는다.[1]

## 이미지 조사와 실제 빌드를 분리한다

`src/Tiny11Builder.Core/BuildEngine.cs`의 `InspectAsync`는 원본 경로를 검사하고 DISM을 통해 이미지 목록을 얻는다.
DISM은 Windows 이미지의 구성 요소를 조사·수정하는 도구다.
`InventoryAsync`는 작업 공간과 도구를 검증한 뒤 이미지 구성 요소를 읽기 전용으로 마운트하고, 읽은 뒤 버린다.[2]

ESD 입력이면 작업용 WIM으로 내보내는 경로가 있고, 실패 시에는 자신이 연 마운트를 정리하면서 작업 로그를 남기도록 작성돼 있다.
반면 `BuildAsync`는 ISO 생성에 필요한 oscdimg 존재와 원본 이미지·부팅 자료를 먼저 검사한다.
파일을 조사하는 단계, 이미지를 수정하는 단계, 부팅 ISO를 만드는 단계는 서로 다른 결과다.[2]

## 결과물과 재개 기능의 의미

사용 문서는 작업 레시피 입출력, 진행 로그, 저장된 작업 상태, 출력 SHA-256과 JSON 보고서를 안내한다.
중단 뒤 재개는 확인된 저장 진행 상태를 바탕으로 하며, 아무 지점에서나 안전하게 이어지는 마법 같은 기능으로 보면 안 된다.[3]
해시가 있다는 사실은 파일 식별에 도움이 되지만 해당 ISO의 모든 설치·부팅 기능이 정상이라는 증거는 아니다.

## 가상 제작 시나리오

개인 가상 머신에서 기본 앱이 적은 Windows 환경을 시험하려는 상황을 가정한다.
사용자는 원본 ISO와 에디션을 선택하고 Tiny11 또는 Custom에서 제거 범위를 검토한 뒤 작업 공간과 출력 위치를 정한다.[1][3]
빌더가 내는 결과는 수정된 설치 ISO와 작업 기록이며, 실제 VM에서 설치·부팅·업데이트를 확인하는 단계가 뒤따른다.
원본 보존과 실험용 VM 사용을 전제로 하며 이 문서에서 이미지를 만들거나 설치한 것은 아니다.

## 되돌리기 어려운 선택과 배포 권리

Core는 업데이트나 servicing이 불가능하다고 README가 경고한다.
Custom의 보호된 구성 요소 제거도 Windows를 망가뜨릴 수 있어, 실사용 장치에 설치하기 전에 VM 시험이 필요하다.[1]
앱 실행은 관리자 승인을 요구하고 x64·ARM64 패키지가 구분된다.[1]
`NOTICE.md`는 독립된 라이선스 파일이 없으며 새 라이선스를 부여하지 않았다고 명시한다.
공개 GitHub 저장소라는 사실만으로 재배포 권한이 생기지 않으며, Windows 이미지와 도구 사용은 Microsoft 조건을 별도로 따른다.[4]

## 직접 읽어볼 순서

1. README의 모드 표와 Core 경고부터 읽는다. 삭제량보다 유지할 기능을 먼저 결정하는 자료다.[1]
2. BuildEngine의 InventoryAsync와 BuildAsync를 비교해 읽기 전용 조사와 실제 변경의 경계를 본다.[2]
3. usage의 취소·재개·산출물 설명을 읽은 뒤 NOTICE에서 재배포 조건을 확인한다.[3][4]

## 자료 확인 범위

2026-10-28 공개 README, 루트 파일 트리와 아래 대표 자료를 확인했다.
설치·실행·성능 측정은 하지 않았으며, 활용 예시는 동작을 설명하기 위한 가상 상황이다.

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

[1] ntdevlabs/tiny11builder — README.md

<https://github.com/ntdevlabs/tiny11builder/blob/winui-preview/README.md>

[2] ntdevlabs/tiny11builder — src/Tiny11Builder.Core/BuildEngine.cs

<https://github.com/ntdevlabs/tiny11builder/blob/winui-preview/src/Tiny11Builder.Core/BuildEngine.cs>

[3] ntdevlabs/tiny11builder — docs/usage.md

<https://github.com/ntdevlabs/tiny11builder/blob/winui-preview/docs/usage.md>

[4] ntdevlabs/tiny11builder — NOTICE.md

<https://github.com/ntdevlabs/tiny11builder/blob/winui-preview/NOTICE.md>
