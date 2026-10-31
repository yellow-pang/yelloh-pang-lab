---
title: "headroomlabs-ai/headroom"
repository: "headroomlabs-ai/headroom"
url: "https://github.com/headroomlabs-ai/headroom"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# headroomlabs-ai/headroom

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Headroom은 도구 결과·로그·파일·대화 기록이 언어 모델에 전달되기 전에 줄이는 문맥 압축 계층이다.
자체 코딩 에이전트나 대체 언어 모델이 아니라 기존 앱과 모델 제공자 사이에서 입력을 가공한다.
라이브러리, 로컬 프록시, 에이전트 래퍼, MCP 서버 등 여러 연결 방식을 제공한다.[1]

## 반복되는 도구 출력이 문맥을 채울 때

코드 검색과 로그 조회를 반복하면 비슷한 JSON 항목과 이미 본 문장이 대화에 계속 쌓인다.
Headroom은 콘텐츠 유형에 맞는 압축을 적용하고 CCR로 원본을 로컬에 보관해 필요할 때 다시 조회하도록 한다.
CCR의 가역성은 축약문 자체로 모든 원문을 복구한다는 뜻이 아니라, 보관된 원본을 가져오는 경로가 있다는 의미다.[1]

README는 JSON, 코드, 일반 텍스트에 서로 다른 압축기를 배치한다.
반복적인 데이터와 짧고 정보 밀도가 높은 문장은 절감 폭이 다르다고 명시한다.
따라서 소개 문구의 ‘같은 답변’을 모든 질문에서 정보 손실이 없다는 보증으로 읽으면 안 된다.[1]

## 분류를 잘못하면 압축도 잘못된다

루트에는 Python 패키지 `headroom/`, Rust 구현 영역 `crates/`, SDK와 벤치마크·문서가 함께 있다.
대표 파일 `headroom/transforms/content_router.py`의 `_determine_strategy`는 혼합 콘텐츠 여부와 탐지 결과를 재사용한다.
예를 들어 코드 안의 사전·목록이 JSON처럼 보여도 탐지기가 충분히 확신하는 소스 코드이면 코드 경로를 우선한다.
문자열에 중괄호가 있다는 이유만으로 일반 문장처럼 압축하지 않으려는 조건이다.[2]

`_strategy_from_detection`은 소스 코드, JSON 배열, 검색 결과, 빌드 출력, diff 등을 서로 다른 전략에 대응시킨다.
코드 전용 처리를 선호하지 않도록 설정하면 일반 텍스트 압축기로 넘기지 않고 PASSTHROUGH를 선택한다.
PASSTHROUGH는 변형하지 않고 통과시키는 방식이다. 코드 의미를 손상시키지 않기 위한 명시적 분기다.[2]

혼합 콘텐츠 처리에서는 사용자 정의 태그를 보호한 뒤 부분별로 나누는 코드도 확인된다.
이를 통해 지침 블록이 일반 문장 구간으로 쪼개져 손실 압축되는 문제를 피하려 한다.
이 방어 로직의 존재와 모든 입력에 대한 완전한 보존 검증은 구별해야 한다.[2]

## 가상 시나리오: 긴 테스트 로그에서 실패 찾기

공개 개인 프로젝트의 긴 테스트 로그를 에이전트가 읽는 상황을 가정한다.
먼저 기존 메시지에 압축 계층을 적용하고 로그 유형에 맞는 결과를 모델에 전달한다.
오류 맥락이 부족하면 모델이 `headroom_retrieve`로 원본을 다시 읽을 수 있는 경로를 마련한다.[1]

비교할 산출물은 줄어든 메시지뿐 아니라 최종 답변과 재조회 성공 여부다.
입력 토큰 감소가 재조회 비용과 오류 분석 정확도까지 개선했는지는 원본 전달 기준선과 별도 비교해야 한다.
이 문서는 그 비교를 수행한 성능 보고서가 아니다.

## 프록시를 연결하면 설정과 데이터 흐름도 바뀐다

압축은 로컬에서 실행되지만 압축 후 요청은 설정한 LLM 제공자로 전송된다.
원본 재조회는 설정한 보관 기간에 의존하며 영구적인 원문 접근을 뜻하지 않는다.[1]
`wrap`은 프록시 시작과 에이전트 설정뿐 아니라 Serena 연결도 수행할 수 있다.
외부 OpenAI 호환 제공자는 upstream 주소를 명시하지 않으면 기본 OpenAI 대상으로 키가 전달될 수 있다는 경고가 있다.
`learn`은 지침 파일에 보정 내용을 쓸 수 있으므로 읽기 전용 분석 도구로만 취급하면 안 된다.[1]
저장소는 Apache-2.0이며, 별도 모델·연결 서비스의 이용 조건과 비용은 독립적으로 확인해야 한다.[3]

## 직접 읽어볼 자료

- [1] README의 How it works, Agent compatibility, When to skip에서 연결 경로와 제외 조건을 읽는다.
- [2] `_determine_strategy`와 `_strategy_from_detection`으로 코드·혼합 입력의 예외 처리를 따라간다.
- [3] LICENSE를 읽고 사용할 배포물의 고지와 외부 의존성 조건을 구분한다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 문서·코드의 관련 부분을 확인했다.
설치·모델 다운로드·서비스 실행은 하지 않았으며, 시나리오는 이해를 위한 가정이다.

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

[1] headroomlabs-ai/headroom — README.md

<https://github.com/headroomlabs-ai/headroom/blob/main/README.md>

[2] headroomlabs-ai/headroom — headroom/transforms/content_router.py

<https://github.com/headroomlabs-ai/headroom/blob/main/headroom/transforms/content_router.py>

[3] headroomlabs-ai/headroom — LICENSE

<https://github.com/headroomlabs-ai/headroom/blob/main/LICENSE>
