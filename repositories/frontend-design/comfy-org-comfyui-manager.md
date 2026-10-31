---
title: "Comfy-Org/ComfyUI-Manager"
repository: "Comfy-Org/ComfyUI-Manager"
url: "https://github.com/Comfy-Org/ComfyUI-Manager"
category: "frontend-design"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["frontend-design", "starred-draft"]
---

# Comfy-Org/ComfyUI-Manager

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

ComfyUI-Manager는 ComfyUI의 추가 노드와 모델을 찾고 설치·활성화·비활성화·제거하는 관리 확장이다.[1]
이미지를 생성하는 모델 자체나 ComfyUI를 대체하는 독립 편집기가 아니다.
여러 노드로 구성된 작업 흐름을 실행하기 전에 필요한 구성 요소를 준비하고 유지하는 역할을 맡는다.[1]

## 공유 워크플로보다 설치 환경이 먼저 달라지는 문제

ComfyUI에서 노드는 이미지 처리 등의 작업 단위이고, 커스텀 노드는 외부에서 추가하는 확장이다.
다른 사람이 만든 워크플로를 열어도 해당 노드가 없거나 서로 충돌하면 그대로 실행하기 어렵다.
Manager는 설치 목록, 업데이트 정보, 충돌 표시와 스냅샷 기능을 통해 이런 환경 차이를 다루도록 한다.[1]

노드 목록은 로컬 자료, 하루 캐시를 쓰는 채널, 원격 채널이라는 조회 모드를 가진다.
원격 연결에 실패하면 로컬 자료를 사용하는 경로가 있으므로 화면의 목록이 항상 최신이라는 뜻은 아니다.
또한 `Fetch Updates`는 정보를 가져오는 동작이고 실제 업데이트 적용과 구별된다.[1]

## 설치 버튼 뒤에서 실제로 일어나는 일

루트에는 `glob`, `js`, `docs`와 노드 목록 JSON 등이 있다.
관리 화면, Python 처리, 배포 목록이 함께 존재하며 카탈로그 표시만 하는 저장소는 아니다.[2]

`glob/manager_core.py`의 `execute_install_script`는 대상 확장 안의 `requirements.txt`와 `install.py`를 확인한다.
의존성 설치가 허용되면 패키지 항목을 읽어 설치 명령을 만들고, 설치 스크립트가 있으면 Python으로 실행하도록 넘긴다.
즉 확장 설치는 이름을 목록에 추가하는 행위가 아니라 로컬 환경과 코드를 바꾸는 행위다.[3]

같은 파일의 `restore_snapshot`은 스냅샷을 읽고 활성 노드와 기록된 버전을 비교한다.
기록에 없는 노드를 비활성화하거나 버전 전환 대상으로 분류하는 처리도 있으므로 복원 역시 환경 변경이다.
README는 Git으로 관리되지 않는 노드의 스냅샷 지원은 불완전하며 복원 적용에는 재시작이 필요하다고 설명한다.[3][1]

## 가상 시나리오: 공개 이미지 워크플로를 열기

공개 예제 워크플로에 필요한 노드가 현재 환경에 없는 상황을 가정한다.
먼저 누락된 확장의 출처·요구 패키지를 확인한 뒤 Manager에서 해당 항목을 찾는다.
설치 전 상태를 기록하고, 설치·재시작 뒤 노드가 등록되었는지와 워크플로가 실제 작동하는지를 따로 확인한다.[1][3]

노드 설치 성공과 이미지 생성 성공은 같은 결과가 아니다.
모델 파일, 장치 메모리, 노드 간 입력 규격까지 맞아야 하므로 Manager의 목록만으로 실행 가능성을 확정하지 않는다.
이 시나리오에서는 모델 다운로드나 확장 설치를 수행하지 않았다.

## 경로 이동과 공급망을 주의하기

README는 V3.38에서 관리 데이터를 보호 경로로 옮겼고, ComfyUI 버전에 따라 `__manager` 또는 이전 사용자 경로를 사용한다고 안내한다.
같은 README의 스냅샷 설명에는 이전 경로도 남아 있으므로 현재 ComfyUI 버전과 Paths·Migration Guide를 우선 대조해야 한다.[1]

외부 노드 설치 스크립트가 실행될 수 있으므로 익숙한 관리 화면이라는 이유로 모든 항목을 신뢰해서는 안 된다.[3]
공유 메뉴는 외부 서비스로 워크플로를 보낼 수 있어 설치 관리와 공개 공유를 별도 행동으로 구분해야 한다.[1]
루트 라이선스는 GPLv3이며, 수정·배포 시 소스 제공 등 적용 의무를 확인해야 한다. 개별 노드와 모델의 조건까지 이 라이선스로 통일되지는 않는다.[4][1]

## 직접 읽어볼 자료

- [1] DB 모드, Paths, Snapshot-Manager를 순서대로 읽어 조회·설치·복원의 차이를 확인한다.
- [3] `execute_install_script`에서 의존성·설치 스크립트 실행 경로를 먼저 본다.
- [3][4] `restore_snapshot`의 상태 변경과 GPLv3 배포 조건을 함께 검토한다.

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

[1] Comfy-Org/ComfyUI-Manager — README

<https://github.com/Comfy-Org/ComfyUI-Manager/blob/main/README.md>

[2] Comfy-Org/ComfyUI-Manager — 루트 파일 목록

<https://api.github.com/repos/Comfy-Org/ComfyUI-Manager/contents>

[3] Comfy-Org/ComfyUI-Manager — glob/manager_core.py

<https://github.com/Comfy-Org/ComfyUI-Manager/blob/main/glob/manager_core.py>

[4] Comfy-Org/ComfyUI-Manager — LICENSE.txt

<https://github.com/Comfy-Org/ComfyUI-Manager/blob/main/LICENSE.txt>
