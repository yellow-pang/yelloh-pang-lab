---
title: "embabel/embabel-agent"
repository: "embabel/embabel-agent"
url: "https://github.com/embabel/embabel-agent"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# embabel/embabel-agent

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Embabel Agent는 Java와 Kotlin에서 언어 모델 호출, 일반 코드와 도메인 객체를 조합해 에이전트 흐름을 만드는 JVM 프레임워크다.
JVM은 Java 계열 프로그램을 실행하는 환경이다.
이 프로젝트의 중심은 미리 적은 순서를 그대로 실행하는 데만 있지 않고, 현재 상태에서 목표에 이르는 행동 경로를 계획하는 데 있다.[1]

## 긴 프롬프트 대신 작은 행동과 데이터

자료를 받아 정보를 추출하고 외부 정보를 조회한 뒤 글을 작성하는 작업을 한 프롬프트에 모두 맡길 수 있다.
그러나 중간 결과가 무엇인지, 어느 단계가 실패했는지, 일반 코드로 처리할 부분이 어디인지 구분하기 어려워진다.
Embabel은 Action, Goal, Condition과 도메인 객체로 이 흐름을 표현한다.[1]

Action은 한 단계의 행동, Goal은 도달하려는 결과, Condition은 실행 전후에 확인할 조건이다.
도메인 객체는 사람·기사·요약처럼 작업에서 다루는 정보를 타입이 있는 구조로 표현한 것이다.
함수가 무엇을 받아 무엇을 반환하는지에서 조건을 추론할 수 있으므로, 모든 연결을 문자열 키와 임의 사전으로 전달하지 않아도 되는 설계를 제시한다.[1]

## 모델이 모든 순서를 결정하는 것은 아니다

README는 기본 계획 방식으로 GOAP를 소개한다.
이는 목표와 현재 상태, 가능한 행동의 전후 조건을 보고 경로를 고르는 계획 알고리즘이다.
계획 자체는 언어 모델만의 추론에 의존하는 것이 아니며, 한 행동이 끝난 뒤 바뀐 상태를 보고 다시 계획한다고 설명한다.[1]

Utility AI라는 다른 선택 방식도 소개한다.
이 방식은 엄격한 최종 목표로 가는 조건보다 행동의 효용 점수를 기준으로 선택한다.
두 접근이 모두 지원된다는 사실을 모든 상황에서 자동으로 최적 경로를 보장한다는 뜻으로 읽으면 안 된다.
개발자가 어떤 행동과 조건을 정의했는지가 가능한 작업의 범위를 정한다.[1]

실행 범위도 Focused·Closed·Open으로 나뉜다.
코드가 특정 에이전트를 부를 수도 있고, 입력 의도에 맞는 에이전트를 고를 수도 있으며, 여러 행동을 모아 목표 경로를 구성할 수도 있다.
Open이 더 유연하지만 덜 결정적이라고 README가 명시한다.
그래도 개별 단계는 정의된 행동 안에서 실행되며 없는 기능을 임의로 발명하는 권한은 아니다.[1]

## 구현에서 드러나는 행동의 경계

추가로 확인한 `Action.kt`는 행동의 입력·출력과 사전 조건·효과 외에 `canRerun`, `readOnly`, `qos`를 표현한다. `canRerun`은 같은 에이전트 실행에서 다시 수행할 수 있는지, `readOnly`는 외부 API·데이터베이스·파일을 변경하지 않는지를 나타낸다.
읽기 전용의 기본값은 false다.[2]

이 선언은 실제 코드를 자동으로 무해하게 만드는 샌드박스가 아니라 프레임워크가 사용하는 행동 정보다.
파일 설명도 일반 애플리케이션이 이 내부 인터페이스를 직접 쓰기보다 애너테이션이나 Kotlin DSL을 사용하도록 한다.
애너테이션은 코드에 역할을 표시하는 표식이고 DSL은 해당 작업을 간결하게 적는 전용 표현 방식이다.[2]

재시도 정책의 별도 파일은 한 번만 실행하는 `FIRE_ONCE`와 기본 정책을 구분한다.
외부에 글을 게시하거나 데이터를 변경하는 행동은 실패 후 반복했을 때 결과가 중복될 수 있으므로, 다시 실행할 수 있음과 안전하게 반복할 수 있음은 다른 질문이다.
정책을 선언한 사실만으로 모든 외부 시스템의 중복 처리가 해결되지는 않는다.[3]

## 예시로 따라가는 흐름

루트 README의 `StarNewsFinder`는 이름과 별자리 정보를 받아 관련 뉴스를 엮는 공식 예제다.
점성술의 타당성을 검증하는 도구가 아니라 일반 코드와 LLM 단계를 섞는 설명용 사례로 읽어야 한다.
먼저 사용자 입력에서 이름·별자리를 구조화된 `StarPerson`으로 추출한다.
다음 행동은 일반 서비스 호출로 운세 문자열을 받아 `Horoscope` 객체로 만든다.[1]

이 두 객체가 준비되면 웹 도구를 요구하는 행동이 관련 뉴스를 찾고 URL과 요약을 모은다.
마지막 행동은 사람 정보·운세·뉴스를 받아 링크가 있는 Markdown 글을 생성하고 목표 달성 표식을 갖는다.
검색과 글쓰기는 서로 다른 모델 설정을 사용할 수 있으며, 중간 데이터가 명시적인 타입으로 오간다는 점이 중요하다.
사람은 추출된 이름이 맞는지, 기사 URL이 실제 근거인지, 뉴스와 생성된 연결 문장이 구분되는지 확인해야 한다.
예제에 포함된 테스트는 프롬프트가 필요한 정보를 담는지와 불필요한 도구 그룹을 주지 않았는지를 검사한다.
그것이 생성된 뉴스 내용의 사실성을 증명하는 것은 아니다.
이 글에서는 예제를 빌드하거나 외부 뉴스를 조회하지 않았다.[1]

## 타입 안전성과 답변의 진실성은 다르다

타입이 맞으면 다음 행동이 필요한 모양의 객체를 받을 수 있다.
그러나 요약 문자열이 정확한지, 링크가 주장을 뒷받침하는지까지 타입 검사가 보장하지는 않는다.
README도 모델이 수행하는 변환의 결과는 비결정적이라고 설명한다.
같은 입력이라도 결과가 달라질 수 있으므로 단위 테스트, 외부 호출 검증과 결과 검토를 구분해야 한다.[1]

여러 모델을 섞는 구조도 자동 비용 절감을 증명하는 것은 아니다.
각 모델의 인증과 요금, 필요한 도구의 권한, 실패와 재시도 설정을 갖춰야 한다.
특히 동적 계획에서 외부 쓰기 행동을 허용한다면 목표 선택 범위와 행동 조건을 신중하게 정의할 필요가 있다.[1][2][3]

README에는 향후 모드와 계획도 함께 적혀 있다.
현재 설명하는 Focused·Closed·Open 경로와 “가능한 미래 모드”를 같은 완성 기능 목록으로 묶지 않아야 한다.
예시 코드 또한 선택한 릴리스의 API와 함께 확인해야 하며, 이 원고는 main 브랜치 자료를 읽은 시점의 설명이다.[1]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/embabel/embabel-agent/blob/main/README.md)
   Key Concepts에서 행동과 목표, 조건을 읽고 Show Me The Code로 넘어간다.
   예제 함수의 인자·반환 타입이 다음 행동을 어떻게 준비하는지 따라가는 것이 중심이다.
2. [Action 핵심 모델](https://github.com/embabel/embabel-agent/blob/main/embabel-agent-api/src/main/kotlin/com/embabel/agent/core/Action.kt)
   `readOnly`, `canRerun`, 입력·출력과 효과 메타데이터를 살핀다.
   선언된 행동 정보와 OS가 강제하는 실행 제한을 혼동하지 않도록 읽는다.
3. [행동 재시도 정책](https://github.com/embabel/embabel-agent/blob/main/embabel-agent-api/src/main/kotlin/com/embabel/agent/core/ActionRetryPolicy.kt)
   한 번 실행과 기본 재시도 정책을 비교한다.
   일반 함수 호출과 외부 상태 변경을 같은 재시도 조건으로 다뤄도 되는지 생각하며 읽는 자료다.

## 정리

Embabel은 타입이 있는 행동과 데이터를 바탕으로 목표 경로를 구성한다.
일반 코드와 모델을 섞고 계획을 다시 세울 수 있지만, 행동의 권한과 반복 안전성, 모델 결과의 사실 검증은 개발자가 별도로 정의해야 한다.

## 자료 확인 범위

2026-09-27 공식 README의 개념·계획·실행 모드·StarNewsFinder 예제와 테스트 설명, `Action.kt` 및 `ActionRetryPolicy.kt`를 확인했다.
Maven 빌드, 모델 호출, 도구 연결, 예제 실행이나 테스트는 수행하지 않았다.

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

[1] embabel/embabel-agent — README.md

<https://github.com/embabel/embabel-agent/blob/main/README.md>

[2] embabel/embabel-agent — embabel-agent-api/src/main/kotlin/com/embabel/agent/core/Action.kt

<https://github.com/embabel/embabel-agent/blob/main/embabel-agent-api/src/main/kotlin/com/embabel/agent/core/Action.kt>

[3] embabel/embabel-agent — embabel-agent-api/src/main/kotlin/com/embabel/agent/core/ActionRetryPolicy.kt

<https://github.com/embabel/embabel-agent/blob/main/embabel-agent-api/src/main/kotlin/com/embabel/agent/core/ActionRetryPolicy.kt>
