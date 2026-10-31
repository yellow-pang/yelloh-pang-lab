---
title: "chrislgarry/Apollo-11"
repository: "chrislgarry/Apollo-11"
url: "https://github.com/chrislgarry/Apollo-11"
category: "system-design"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["system-design", "starred-draft"]
---

# chrislgarry/Apollo-11

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Apollo-11은 아폴로 11호 유도 컴퓨터 AGC의 사령선·달착륙선 소스를 디지털 형태로 보존하는 저장소다.
현대 컴퓨터용 비행 앱이나 독립 실행 시뮬레이터가 아니라 원본 출력물을 옮긴 역사적 코드 자료다.[1]

## 보존 대상과 실행 도구를 구분한다

README는 Comanche055를 사령선, Luminary099를 달착륙선 코드로 구분한다.
소스는 MIT Museum의 종이 출력물 이미지에서 전사되었으며, 컴파일에 관심이 있으면 Virtual AGC를 보도록 안내한다.[1]
따라서 이 저장소의 코드를 읽는 것과 당시 컴퓨터 환경을 재현해 실행하는 것은 별도 작업이다.
어셈블리는 하드웨어 명령에 가까운 언어이므로, 현대 고급 언어의 문법이나 라이브러리와 바로 비교하기 어렵다.

## EXECUTIVE에서 읽는 작업 우선순위

대표 파일 `Comanche055/EXECUTIVE.agc`는 작업 등록·중단·재개·종료와 우선순위 변경을 다룬다.
`NOVAC`과 `FINDVAC`은 작업용 VAC 영역 필요 여부가 다른 등록 진입점으로 주석에 설명되어 있다.[2]
VAC 영역은 이 코드가 특정 작업에 배정하는 작업 공간이며, 구체적인 메모리 배치는 관련 정의와 함께 읽어야 한다.
`CHANG1` 주석은 더 높은 우선순위의 작업을 처리하기 위해 기본 작업을 중단하는 경우를 설명한다.[2]

`JOBSLEEP`은 예상한 사건이 완료될 때까지 자발적으로 기다리는 진입점이다.
`JOBWAKE` 쪽은 잠든 작업을 다시 깨우며, `PRIOCHNG`는 실행 중인 작업의 우선순위를 바꾼다.[2]
파일 후반의 `EJSCAN`은 우선순위 레지스터를 검사하여 활성 작업 중 높은 우선순위를 찾는 흐름을 보여 준다.
이 부분은 제한된 자원에서 여러 작업을 조정하는 구조를 읽는 자료이지, 현대 운영체제 스케줄러와 완전히 같다는 증거는 아니다.[2]

## 가상 읽기 시나리오: 기다리는 작업은 어떻게 재개될까?

학습자가 입출력을 기다리는 작업과 바로 처리할 작업의 관계를 이해하고 싶다고 가정하자.
먼저 `JOBSLEEP`의 주석과 위치 정보 저장 부분을 읽고, `JOBWAKE2`의 잠든 작업 탐색으로 이동한다.[2]
그다음 작업 주소가 일치할 때 우선순위 상태와 재개 주소를 어떻게 준비하는지 비교한다.
산출물은 작업 등록·대기·재개를 연결한 개인 학습 그림이 될 수 있다.
이는 코드를 읽는 가상 경로이며, 실제 우주선 동작 재현이나 프로그램 실행 결과가 아니다.

## 원본의 오타도 자료의 일부다

CONTRIBUTING은 수동 전사 과정의 오류를 원본 스캔과 대조해 수정하라고 한다.
주석은 원본과 정확히 맞아야 하므로, 원본에 있던 오타를 보기 좋게 고치는 변경도 허용 목적과 어긋날 수 있다.[3]
일반적인 코드 개선 저장소라면 문장 교정이 개선이지만, 여기서는 원본 충실도가 우선인 셈이다.
이 차이를 이해해야 낡은 표현이나 특이한 이름을 버그라고 단정하지 않는다.

LICENSE.md는 Public Domain Mark 1.0으로 알려진 저작권 제한이 없다고 표시한다.
동시에 관할권에 따른 차이, 저작권 외 권리, 무보증, 저자의 보증을 암시하지 말라는 주의를 담고 있다.[4]
공개된 역사 자료라는 사실은 현재의 안전 필수 시스템에 그대로 사용할 수 있다는 인증이 아니다.
대표 파일 일부를 읽었다는 사실만으로 전체 항법 알고리즘이나 임무 안전성을 검증했다고 주장할 수 없다.

## 직접 읽어볼 자료

- [1] README의 Attribution에서 두 프로그램의 대상과 전사 출처를 먼저 확인한다.
- [2] EXECUTIVE의 진입점 주석부터 읽고 작업 등록·대기·재개·우선순위 탐색을 연결한다.
- [3] CONTRIBUTING의 Comments와 Typographic Errors에서 보존 프로젝트의 수정 원칙을 읽는다.
- [4] LICENSE.md의 Other Information에서 Public Domain 표시의 주의점을 확인한다.

## 자료 확인 범위

2026-10-28 공개 README·루트 구조·위에 인용한 코드 또는 문서를 확인했다.
설치·프로그램 실행·성능 측정은 하지 않았다.

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

[1] chrislgarry/Apollo-11 — README.md

<https://github.com/chrislgarry/Apollo-11/blob/master/README.md>

[2] chrislgarry/Apollo-11 — Comanche055/EXECUTIVE.agc

<https://github.com/chrislgarry/Apollo-11/blob/master/Comanche055/EXECUTIVE.agc>

[3] chrislgarry/Apollo-11 — CONTRIBUTING.md

<https://github.com/chrislgarry/Apollo-11/blob/master/CONTRIBUTING.md>

[4] chrislgarry/Apollo-11 — LICENSE.md

<https://github.com/chrislgarry/Apollo-11/blob/master/LICENSE.md>
