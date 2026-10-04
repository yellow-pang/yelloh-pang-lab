---
title: "tashfeenahmed/freellmapi"
repository: "tashfeenahmed/freellmapi"
url: "https://github.com/tashfeenahmed/freellmapi"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# tashfeenahmed/freellmapi

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

FreeLLMAPI는 사용자가 보유한 여러 모델 제공자의 키를 한 OpenAI 호환 endpoint 뒤에 연결하는 개인용 게이트웨이다.
무료 모델을 새로 제공하거나 모든 제공자의 할당량을 보장하는 서비스가 아니다.
README는 운영용 기반이 아니라 개인 실험과 학습을 위한 프로젝트라고 명시한다.[1]

## 다른 제한을 가진 API들을 한곳으로

여러 무료 API를 시험하면 호출 형식뿐 아니라 분당 요청 수, 하루 토큰 수와 오류 처리 방식이 달라진다.
FreeLLMAPI는 클라이언트가 한 주소를 사용하도록 하고 내부에서 사용 가능한 모델·키를 선택하는 방식으로 이 차이를 감싼다.
endpoint는 요청을 받는 API 주소이며, proxy는 요청을 다른 서비스로 전달하는 중간 계층이다.[1][2]

이 프로젝트의 “무료”는 각 제공자의 무료 구간에 기대는 표현이다.
키를 추가하지 않은 서비스까지 자동으로 사용할 권리가 생기는 것도 아니고, 다른 제공자의 사용 조건이 사라지는 것도 아니다.
README의 거대한 합산 토큰 소개를 특정 사용자가 매달 안정적으로 쓸 수 있는 용량으로 옮겨 적으면 부정확하다.[1]

## 라우팅과 사용량 장부의 역할

공식 아키텍처는 Express proxy가 요청을 받고 라우터가 정상 상태의 키와 남은 한도를 확인해 모델을 선택한다고 설명한다. 429는 과도한 요청 같은 제한 응답이고 5xx는 서버 측 실패 범주다.
이런 실패를 만나면 해당 키를 잠시 쉬게 하는 cooldown과 다음 후보로 넘어가는 fallback을 사용한다.
응답을 조금씩 전달하는 streaming도 이 중간 계층을 통과한다.[2]

사용량은 요청 수와 토큰 수를 분·일 단위로 나눠 추적한다.
토큰은 모델이 텍스트를 처리하는 단위다.
대표 구현 `ratelimit.ts`에는 SQLite에 기록한 수치와 메모리의 시간 창을 사용하는 경로가 있다.
시간 창은 최근 일정 기간에 일어난 사용만 남겨 현재 제한과 비교하는 방식이다.
제공자 계정 단위의 일일 제한처럼 UTC 자정 경계를 쓰는 경우도 별도로 설명되어 있다.[3]

또 중요한 장치는 진행 중 요청의 lease다.
사용량을 성공 후에만 적으면 동시에 들어온 요청들이 모두 “아직 여유 있음”을 보고 한도를 넘길 수 있다.
코드 주석과 함수는 진행 중 요청과 예상 토큰도 임시 사용량으로 반영하는 구조를 보여 준다.
lease는 이 프로세스에서 살아 있는 요청의 임시 점유 기록이라 영구 저장하지 않는다고 설명한다.[3]

키별 동시 요청 수 제한은 별도 선택 설정이며, 아무 설정이 없으면 무제한으로 취급하는 함수가 있다.
따라서 “사용량 추적을 한다”를 “모든 동시성 제한이 기본으로 강제된다”로 바꾸면 안 된다.
공개 코드가 어떤 제한을 아는지와 제공자가 실제로 적용하는 정책이 일치하는지도 확인 대상이다.[3]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
개인 학습용 요약 앱이 게이트웨이에 짧은 텍스트를 보내고, 사용자가 정상적으로 발급받은 두 제공자의 키를 등록해 두었다고 하자.
클라이언트는 같은 호환 API로 요청하지만 라우터는 우선순위, 키 상태와 남은 요청·토큰 제한을 확인해 실제 제공자를 고른다.
키는 저장 시 암호화되고 호출할 때 메모리에서 복호화하는 방식으로 문서화되어 있다.[2]

첫 제공자가 제한 오류를 반환하면 문서상의 흐름은 cooldown을 기록하고 다음 후보를 시도한다.
동시에 다른 요약 요청이 들어와도 진행 중 lease가 임시 사용량에 반영되어 이미 약속한 처리량을 무시하지 않도록 한다.
결과가 돌아오면 클라이언트는 호환 응답을 받지만, 실제 처리한 모델이 처음 기대한 모델과 달라질 수 있다.
사람이 확인할 것은 성공 여부뿐 아니라 선택된 모델, 사용량 기록과 fallback 발생 여부다.[2][3]

후보가 바뀌면 문체·문맥 길이·도구 호출 품질이 달라질 수 있으므로 같은 주소를 사용했다는 이유로 결과를 동일한 모델의 반복 실험으로 취급할 수 없다.
모델 고정이 필요한 비교와 가용 후보를 우선하는 개인 실험은 다른 목적이다.
이 예시는 할당량을 우회하거나 여러 계정으로 제한을 피하는 절차가 아니며 각 제공자의 계정·사용 약관 안에서만 생각해야 한다.[1][2]

## 경계와 문서 사이의 차이

아키텍처는 단일 사용자 설계로 다중 사용자 인증·청구가 없고 인터넷에 공개하지 말라고 안내한다.
API 키 암호화도 전체 시스템의 안전성을 자동으로 보장하지 않는다.
관리 화면, 로그와 데이터 디렉터리의 접근 권한은 별도 관리 대상이다.
무료 서비스의 지연과 가용성에는 SLA, 즉 계약된 서비스 수준 보장이 없다고 문서가 명시한다.[2]

README의 제한 절은 frontier 모델이 없다고 표현하지만 추가 아키텍처 문서는 일부 상위 모델 항목이 존재하되 작은 무료 할당량과 불안정한 가용성이 제한이라고 설명한다.
이 불일치를 감추고 특정 모델 급을 보장하지 않는다.
실제 이용 가능 여부는 카탈로그·제공자 조건·등록한 키의 상태를 함께 확인해야 한다.[1][2]

카탈로그도 코드와 분리해 서명된 피드로 갱신하며 무료 설치와 유료 live catalog의 전달 시점이 다르다고 소개한다.
서명된 업데이트가 있다고 제공자가 갑자기 바꾼 약관과 한도가 즉시 모든 설치에 반영되는 것은 아니다.
프로젝트의 약관 검토 표는 작성자의 해석이지 각 제공자가 현재 허가했다는 공식 확인을 대신하지 않는다.[1][2]

## 직접 읽어볼 자료

- [README의 Why this exists와 Disclaimer](https://github.com/tashfeenahmed/freellmapi/blob/main/README.md)
  통합 API가 해결하는 편의 문제와 개인 실험 전용이라는 범위를 함께 읽는다.
  합산 무료 용량 소개를 자신의 보장 용량으로 해석하지 않는 출발점이다.
- [Architecture & internals](https://github.com/tashfeenahmed/freellmapi/blob/main/docs/en/architecture/00-high-level-index.md)
  정상 키 선택, cooldown, fallback과 단일 사용자 제한을 연결해서 본다.
  지원하지 않는 기능과 모델 가용성의 경고도 같은 페이지에 있다.
- [ratelimit.ts](https://github.com/tashfeenahmed/freellmapi/blob/main/server/src/services/ratelimit.ts)
  상단의 In-flight leases와 Provisional usage 뒤 `canMakeRequest`, `canUseTokens`를 읽는다.
  완료된 요청만 세면 병렬 호출에서 왜 한도를 잘못 판단할 수 있는지 보여 주는 구현이다.

## 정리

FreeLLMAPI는 여러 개인용 API의 차이를 숨기는 라우터이되 한도와 조건을 없애지는 않는다.
같은 주소 뒤에서 모델이 바뀔 수 있고, 서비스 안정성과 외부 제공자의 규칙은 계속 남는다.

## 자료 확인 범위

2026-09-27 기준 README 관련 절, 루트 구성, 공식 아키텍처와 사용량 제한 구현의 핵심 구간을 확인했다.
설치·키 등록·API 호출은 실행하지 않았으며 실제 무료 가용량과 암호화 안전성을 시험하지 않았다.

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

[1] tashfeenahmed/freellmapi — README.md

<https://github.com/tashfeenahmed/freellmapi/blob/main/README.md>

[2] tashfeenahmed/freellmapi — docs/en/architecture/00-high-level-index.md

<https://github.com/tashfeenahmed/freellmapi/blob/main/docs/en/architecture/00-high-level-index.md>

[3] tashfeenahmed/freellmapi — server/src/services/ratelimit.ts

<https://github.com/tashfeenahmed/freellmapi/blob/main/server/src/services/ratelimit.ts>
