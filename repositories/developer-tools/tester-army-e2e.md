---
title: "tester-army/e2e"
repository: "tester-army/e2e"
url: "https://github.com/tester-army/e2e"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# tester-army/e2e

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

e2e는 자연어로 목표를 주는 Agent 행동과 코드로 명시하는 결과 검증을 같은 테스트에 넣는 웹·모바일 종단 간 테스트 프레임워크다.[1]
종단 간 테스트란 사용자가 앱을 조작하는 경로를 따라 결과를 확인하는 방식이다. 아무 검증 없이 Agent가 성공했다고 말하면 끝나는 채팅 도구도, 특정 모델 하나를 묶어 판매하는 실행기만도 아니다.[1]

## 의도를 말하되 성공 조건은 남겨두기

README의 예시는 `agent.act`로 요금제 변경을 요청하고 `expect`로 화면 상태를 확인한다.
자연어는 행동 목표를 지정하고 locator는 화면 요소를 찾으며 assertion은 기대 조건을 검사한다.
Agent가 없는 테스트에는 모델이 필요 없고, 모델을 쓰는 경우에는 구독·API key·로컬 모델을 선택할 수 있다고 안내한다.[1]

루트에는 `packages`, `apps`, `examples`, `docs`, `plugins`가 있다.
README는 SDK·runner·CLI와 Playwright 기반 웹 엔진, agent-device 기반 모바일 엔진을 구분한다.
테스트 작성 API와 실제 브라우저·기기 조작 계층이 분리된 구성이다.[2][1]

## 검증된 행동만 재생 캐시에 남긴다

대표 문서 `docs/cache.mdx`는 `agent.act()` 뒤의 검증이 통과하면 행동 기록을 저장한다고 설명한다.
다음 실행은 기록된 조작을 모델 호출 없이 재생하고, 화면 요소나 최종 상태가 맞지 않으면 Agent에 제어를 넘긴다.
캐시는 테스트 결과를 무조건 성공으로 재사용하는 기능이 아니라 조작 경로를 재사용하는 기능이다.[3]

화면 locator에 대한 `expect` 등은 검증으로 인정되지만 일반 값 비교, 단순 텍스트 읽기, 또 다른 `act`만으로는 저장 조건을 충족하지 않는다.
`agent.assert`, `agent.waitFor`, `agent.extract`는 항상 실시간으로 실행된다.
따라서 캐시가 켜졌다는 이유로 전체 테스트의 모델 호출이 없어지는 것은 아니다.[3]

기록에는 테스트·대상·지시·매개변수·Agent 등의 정체성이 연결된다.
테스트 이름이나 지시를 바꾸면 cache miss가 나지만 모델 변경만으로는 miss가 나지 않는다.
새 모델을 평가할 때 `--no-cache`를 사용하라는 지침은 이 차이에서 나온다.[3]

## 가상 시나리오: 장바구니 테스트

개인 데모 쇼핑 앱에서 “상품 하나를 장바구니에 추가”하는 목표를 테스트한다고 가정한다.
먼저 테스트용 페이지를 열고 `agent.act`로 행동을 요청한 다음 상태 요소가 `1 item`을 표시하는지 검사한다.
이는 공식 캐시 문서의 장바구니 예제 흐름이며, 실제 구매를 수행하는 시나리오가 아니다.[3]

처음 검증된 기록은 다음 실행에서 조작 재생에 사용될 수 있다.
버튼을 찾을 수 없거나 최종 상태가 달라지면 handoff 원인을 보고 UI 변경인지 테스트 데이터 문제인지 확인한다.
재생 성공률과 새 모델의 문제 해결 능력은 다른 지표이므로 마지막에는 캐시 없는 실행도 별도 평가 대상으로 둔다.[3]

## 변하는 API와 외부 실행 비용

README는 1.0을 향한 개발 중이며 minor release 사이에도 API·설정이 바뀔 수 있다고 밝힌다.
웹·모바일 엔진과 호스팅 어댑터가 여러 개이므로 예제의 플랫폼과 자신의 실행 환경을 맞춰야 한다.[1]

기본 캐시 모드는 로컬에서 read-write이고 `CI`가 설정되면 지정하지 않은 모드가 read-only다.
실패한 시도도 앞서 검증된 단계는 보존할 수 있으므로 실패 결과와 캐시 변경 여부를 함께 읽어야 한다.[3]
CLI는 익명 사용량 수집을 안내하며 `E2E_TELEMETRY_DISABLED=1` 등 거부 방법도 제공한다.[1]
라이선스는 Apache-2.0이며 모델·호스팅·기기 제공자의 비용과 조건은 별도다.[4][1]

## 직접 읽어볼 자료

- [1] 첫 테스트 예제에서 자연어 행동과 코드 검증이 담당하는 부분을 나눈다.
- [3] When a recording is written과 How a replay works를 읽어 저장·재생 조건을 확인한다.
- [3] `replayed`, `handed off`, `missed`와 `--no-cache`를 비교해 보고서를 해석한다.

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

[1] tester-army/e2e — README

<https://github.com/tester-army/e2e/blob/main/README.md>

[2] tester-army/e2e — 루트 파일 목록

<https://api.github.com/repos/tester-army/e2e/contents>

[3] tester-army/e2e — docs/cache.mdx

<https://github.com/tester-army/e2e/blob/main/docs/cache.mdx>

[4] tester-army/e2e — LICENSE

<https://github.com/tester-army/e2e/blob/main/LICENSE>
