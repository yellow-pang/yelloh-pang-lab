---
title: "android/skills"
repository: "android/skills"
url: "https://github.com/android/skills"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# android/skills

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Android 개발에서 에이전트가 자주 틀리는 작업을 공식 지침과 예제로 보완하는 모듈형 Skill 모음이다.
새 AI 모델이나 Android 앱 런타임이 아니라 호스트 에이전트가 읽고 따라가는 작업 명세다.[1]

## 범용 답변에 Android의 조건을 더한다

README는 이미 모델이 잘하는 기초 영역보다 평가에서 부족함이 드러난 사용 사례와 작업 흐름에 집중한다고 설명한다.
Skill은 관련 요청에 맞춰 불러오는 Markdown 지침이며, 설치한 에이전트가 적절한 항목을 찾아 활성화하는 방식이다.
따라서 모든 Android 지식을 대체하는 사전보다 특정 문제의 조건과 순서를 전달하는 보조 자료로 이해하는 편이 맞다.[1]

## 화면 가림 문제를 다루는 실제 지침

`system/edge-to-edge/SKILL.md`는 Compose 앱에서 시스템 표시줄과 키보드가 내용을 가리는 문제를 다룬다.
전제는 Jetpack Compose 사용과 target SDK 35 이상이며, 더 낮으면 SDK를 올리도록 요구한다.
처음에는 Activity, 목록, 화면 위에 떠 있는 동작 버튼, 텍스트 입력 필드를 찾아 기존 처리를 분석하도록 한다.[2]

이후 `enableEdgeToEdge`를 적용하고 키보드가 있는 Activity의 `adjustResize` 설정을 점검한다.
Insets는 상태 표시줄이나 키보드가 차지하는 공간 정보다.
지침은 Scaffold가 넘겨준 여백과 별도 padding을 무작정 더하지 않도록, 중복 여백을 만드는 잘못된 예제까지 보여준다.[2]

## 도구 안내와 수정 지침은 역할이 다르다

`devtools/android-cli/SKILL.md`는 SDK 관리, 앱 실행, 기기·에뮬레이터 조작 등을 위한 Android CLI 사용법을 담는다.
도구가 없으면 설치하라는 단계도 있으므로 Skill은 읽기 전용 참고 자료로만 작동한다고 가정할 수 없다.[3]
README의 전체 설치 옵션은 감지된 여러 에이전트 디렉터리에 영향을 줄 수 있어 개별 Skill 선택과 전체 설치를 구분해야 한다.[1]

## 가상 수정 시나리오

개인 Compose 앱에서 키보드를 열면 입력 칸이 가려지는 상황을 가정한다.
에이전트는 edge-to-edge 지침을 선택하고 Activity와 Manifest, Scaffold 및 입력 컨테이너의 여백 전달을 읽는다.[2]
이미 상위에서 키보드 여백을 적용했다면 하위에 같은 여백을 추가하는 대신 중복 여부를 확인한다.
결과 후보는 수정된 Kotlin·Manifest이며, 실제 기기에서 입력 필드 가시성과 조작 가능성을 확인해야 수정 성공을 판단할 수 있다.[2]

## 적용 전에 확인할 경계

이 Skill의 Compose 전제를 XML View 앱에 그대로 적용하면 분석 대상부터 달라질 수 있다.
SDK 상향은 단순 여백 수정보다 범위가 큰 변경이므로 프로젝트 호환성 검토 없이 자동 승인하지 않는 편이 안전하다.[2]
README도 AI 결과를 다시 확인하라고 안내한다. 설치 자체가 UI 동작 검증은 아니다.[1]
저장소는 Apache-2.0이며 사용·재배포 시 라이선스 조건을 확인해야 한다.[4]

## 직접 읽어볼 순서

1. README의 설치 범위와 자동 활성화 설명을 읽어 호스트 에이전트의 역할을 구분한다.[1]
2. edge-to-edge Skill의 Prerequisites, Step 1, IME의 RIGHT/WRONG 예제를 차례로 비교한다.[2]
3. android-cli Skill에서 실제 실행될 설치·기기 조작 명령을 살펴본다. 지침 선택과 도구 권한은 별도 판단이다.[3]

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

[1] android/skills — README.md

<https://github.com/android/skills/blob/main/README.md>

[2] android/skills — system/edge-to-edge/SKILL.md

<https://github.com/android/skills/blob/main/system/edge-to-edge/SKILL.md>

[3] android/skills — devtools/android-cli/SKILL.md

<https://github.com/android/skills/blob/main/devtools/android-cli/SKILL.md>

[4] android/skills — LICENSE.txt

<https://github.com/android/skills/blob/main/LICENSE.txt>
