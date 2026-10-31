---
title: "HKUDS/CLI-Anything"
repository: "HKUDS/CLI-Anything"
url: "https://github.com/HKUDS/CLI-Anything"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# HKUDS/CLI-Anything

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

CLI-Anything은 기존 소프트웨어를 에이전트가 명령으로 조작하도록 CLI를 만드는 지침과, 이미 만들어진 CLI를 찾는 생태계다.
화면을 대신 클릭하는 범용 에이전트라기보다 실제 앱의 기능을 명령과 구조화된 결과로 연결한다.[1]

## 기존 CLI 사용과 새 CLI 제작을 나눈다

README는 CLI-Hub로 기존 도구를 찾아 설치하는 경로와 플러그인·Skill로 새 harness를 만드는 경로를 나눈다.
여기서 harness는 외부 앱의 기능을 일정한 명령과 상태 관리 방식으로 감싼 연결 코드다.
GIMP·Blender 같은 원래 앱이 필요한 경우 그 앱도 설치해야 하므로 CLI 패키지만으로 모든 기능이 완성되지는 않는다.[1]

## 제작 지침은 화면보다 내부 기능을 먼저 읽는다

`cli-anything-plugin/HARNESS.md`는 원본 코드에서 backend, 데이터 형식, 기존 CLI와 명령·되돌리기 구조를 분석하는 것으로 시작한다.
이후 일회성 하위 명령과 상태를 유지하는 REPL, JSON 출력, 프로젝트 저장을 설계한다.
REPL은 명령을 반복 입력하면서 같은 작업 상태를 이어가는 대화형 명령 환경이다.[2]

지침은 테스트를 만들기 전에 `TEST.md`에 단위 시험과 실제 사용 시나리오를 기록하도록 요구한다.
중간 파일의 형식 검사와 실제 backend를 호출하는 끝까지의 시험을 구분하며, 종료 코드만 보고 내보내기 성공으로 판단하지 말라고 명시한다.
이는 생성 절차의 요구 조건이지 모든 등록 도구가 동일한 품질을 이미 입증했다는 뜻은 아니다.[2]

## Blender 연결 코드가 실제로 하는 일

대표 파일 `blender_backend.py`는 실행 경로에서 Blender를 찾고, 없으면 설치 안내와 함께 오류를 낸다.
`render_script`는 Blender를 background 모드로 실행해 Python 스크립트를 전달한다.
`render_scene_headless`는 임시 스크립트를 쓰고 실행 결과를 확인한 뒤, 출력 파일 또는 프레임 번호가 붙은 후보를 찾는다.[3]

반환값에는 실제 경로, 확장자, Blender 버전과 파일 크기가 담긴다.
이 함수의 파일 존재 검사는 렌더링 내용이 의도한 장면인지까지 검사하는 시각적 품질 평가와 다르다.[3]

## 가상 제작 시나리오

개인 3D 장면을 수정하고 미리보기 이미지를 얻는 작업을 가정한다.
에이전트는 기존 Blender CLI를 찾고 필요한 backend가 준비됐는지 확인한 뒤 장면 작업과 렌더링을 이어가는 흐름을 만들 수 있다.[1][3]
CLI-Hub meta-skill은 여러 도구가 필요한 경우에도 preflight로 준비 상태를 확인하고 필요한 capability만 설치하라고 안내한다.
전체 도구 묶음을 무조건 설치하는 대신 한 작업에 필요한 범위를 정하는 것이 문서의 권장 흐름이다.[4]

## 자동 실행과 검증의 경계

실제 backend 호출은 파일을 생성·변경하고 스크립트를 실행한다. JSON 출력은 권한 격리나 안전한 실행을 뜻하지 않는다.[2][3]
설치 지침을 읽는 단계와 패키지 설치·앱 실행을 승인하는 단계는 구분해야 한다.[4]
README의 테스트 수나 성공 표현은 프로젝트 보고이며 이 조사에서 재현한 결과가 아니다.[1]
저장소는 Apache-2.0이지만 연결하는 앱과 설치되는 다른 패키지의 라이선스·실행 조건은 별도 확인 대상이다.[5]

## 직접 읽어볼 순서

1. README의 두 시작 경로를 읽어 기존 도구 사용인지 새 harness 제작인지 정한다.[1]
2. HARNESS의 분석·구현·시험 단계를 읽고 실제 산출물 검증 요구를 확인한다.[2]
3. Blender backend의 subprocess와 출력 경로 검사, meta-skill의 preflight·범위 제한을 함께 읽는다.[3][4]

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

[1] HKUDS/CLI-Anything — README.md

<https://github.com/HKUDS/CLI-Anything/blob/main/README.md>

[2] HKUDS/CLI-Anything — cli-anything-plugin/HARNESS.md

<https://github.com/HKUDS/CLI-Anything/blob/main/cli-anything-plugin/HARNESS.md>

[3] HKUDS/CLI-Anything — blender/agent-harness/cli_anything/blender/utils/blender_backend.py

<https://github.com/HKUDS/CLI-Anything/blob/main/blender/agent-harness/cli_anything/blender/utils/blender_backend.py>

[4] HKUDS/CLI-Anything — skills/cli-hub-meta-skill/SKILL.md

<https://github.com/HKUDS/CLI-Anything/blob/main/skills/cli-hub-meta-skill/SKILL.md>

[5] HKUDS/CLI-Anything — LICENSE

<https://github.com/HKUDS/CLI-Anything/blob/main/LICENSE>
