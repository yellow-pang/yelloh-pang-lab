---
title: "thedotmack/claude-mem"
repository: "thedotmack/claude-mem"
url: "https://github.com/thedotmack/claude-mem"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# thedotmack/claude-mem

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude-Mem은 에이전트 세션에서 일어난 도구 사용을 관찰하고 요약해 이후 세션의 맥락으로 돌려주는 메모리 시스템이다.
모델 자체를 재학습시키는 방식이 아니라 별도 기록 저장소와 검색·삽입 경로를 연결한다.
Claude Code의 플러그인 경로가 중심에 있으며 README는 다른 에이전트용 설치 경로도 소개한다.[1]

## 세션이 끝나도 남겨야 하는 정보

코딩 대화가 길어지면 어떤 오류를 고쳤는지, 특정 구조를 왜 선택했는지 다시 설명해야 할 수 있다.
모든 대화를 매번 통째로 전달하면 모델이 읽는 입력도 길어진다.
Claude-Mem은 작업 관찰과 세션 요약을 저장하고 처음에는 짧은 색인이나 최근 맥락을 제공한 뒤 필요한 상세 기록을 찾게 하는 접근을 사용한다.[1]

관찰은 도구 결과를 그대로 모두 복사한 로그와 같은 개념이 아니다.
공식 아키텍처는 도구 사용 이벤트가 작업 대기열에 들어가고 SDK 에이전트의 응답 처리기가 관찰을 SQLite에 저장하며 검색용 벡터를 ChromaDB로 동기화하는 흐름을 제시한다.
SQLite는 구조화된 기록을 보관하는 데이터베이스이고 벡터는 의미가 비슷한 내용을 찾기 위한 숫자 표현이다.[2]

## 기록하는 시점과 다시 읽는 시점

Hook은 세션 시작이나 도구 사용처럼 정해진 사건에 실행되는 연결 지점이다.
문서에는 세션 시작 때 worker를 준비하고 맥락을 넣으며, 사용자 입력 시 세션을 등록하고, 도구 사용 뒤 관찰을 전송하며, 끝날 때 요약과 남은 메시지를 처리하는 경로가 나온다.
worker는 기록·검색을 처리하는 별도 로컬 서비스다.[2]

기록을 다시 찾을 때는 `search`로 짧은 결과 색인과 ID를 받고, `timeline`으로 주변 작업의 시간 맥락을 살핀 뒤 `get_observations`로 선택한 ID의 상세 내용을 가져오는 단계가 소개된다.
검색 결과 전체를 곧바로 펼치는 대신 어떤 기록이 필요한지 먼저 좁히는 구조다.
README의 토큰 절감 수치는 저자가 설명한 효과이며 이번에 측정한 결과는 아니다.[1]

대표 구현 `ContextBuilder.ts`도 저장된 기억을 무제한으로 붙이지 않는다.
프로젝트 범위로 관찰과 요약을 조회하고 시간 순서 출력을 구성한 뒤 `fitContextForDelivery`에서 출력 예산에 맞춘다.
예산은 전달할 수 있는 길이의 상한이며, 실제로 남은 관찰과 요약만 통계에 반영하도록 구현되어 있다.[3]

## 예시로 따라가는 흐름

공식 README의 인증 버그 검색 예를 따라가 보자.
직접 실행한 결과가 아니라 문서상의 호출 흐름을 설명한다.
이전에 기록된 버그 수정 중 `authentication bug`를 검색어로 삼고 유형을 `bugfix`로 제한하면 먼저 작은 색인을 받는 방식이다.
입력은 과거 소스 전체가 아니라 현재 알고 싶은 주제와 필터이며, 색인에서 관련 관찰 ID를 고르는 단계가 이어진다.[1]

색인 제목만으로 당시의 원인을 확정하지 않고 주변 시간 기록을 보면 같은 시점에 어떤 변경과 시도가 있었는지 살필 수 있다.
이후 고른 ID만 상세 조회해 원인 설명과 관찰 내용을 읽는다.
새 세션의 기본 맥락도 이와 별도로 프로젝트의 최근 관찰·요약을 조합하지만 출력 길이를 맞추기 위해 일부가 줄어들 수 있다.
“저장된 기억”과 “이번 요청에 실제 전달된 기억”은 다른 집합일 수 있다.[1][3]

사람이 확인할 부분은 검색된 기록이 현재 프로젝트와 같은 범위인지, 이후 수정으로 과거 설명이 낡지 않았는지, 기록 생성 또는 동기화 실패 경고가 없는지다.
구현은 observer 장애·할당량 대기·클라우드 동기화 상태의 경고를 맥락 끝에 붙이고 그 경고도 길이 예산에 포함한다.
단순히 기록이 없다는 결과와 기록 수집이 잠시 멈췄다는 상황을 구별하려는 처리다.
실제 코드 상태의 검증은 기억 조회와 별도로 남는다.[3]

## 자동 동작의 전제와 남는 책임

공식 설치 설명은 `npm install -g claude-mem`이 SDK·라이브러리만 설치하며 플러그인 Hook과 worker를 등록하는 설치는 아니라고 경고한다.
패키지가 존재한다는 것과 현재 에이전트가 관찰을 보내고 있다는 것은 다르다.
아키텍처 문서에는 설치 시 Bun과 uv 등을 준비하는 경로도 설명되므로 읽기 전용 도구처럼 생각해서는 안 된다.[1][2]

README의 현재 설치 안내에는 브라우저 로그인과 호스팅 observer 선택, 사용자 제공 API 키 또는 기존 모델 이용 경로가 함께 있다.
계정 상호작용을 건너뛰는 옵션도 소개한다.
따라서 로컬 데이터베이스를 쓴다는 이유만으로 요약 처리·선택적 동기화까지 모두 장치 안에서 끝난다고 판단할 수 없다.
선택한 제공자와 전송·보관 설정을 확인해야 한다.[1]

`<private>` 태그로 민감한 내용을 저장에서 제외하는 기능을 소개하지만, 모든 비밀을 자동으로 알아서 찾아 준다는 보장은 아니다.
어떤 도구 결과와 입력이 관찰 대상인지 먼저 확인하고 민감한 자료는 입력 자체를 최소화해야 한다.
저장된 요약은 모델이 만든 파생 자료이므로 중요한 결정을 재사용할 때 원문·현재 코드와 대조할 필요가 있다.[1][2]

아키텍처는 worker 연결 실패나 timeout을 호스트 세션의 비차단 실패로 처리하고, 일부 클라이언트 오류는 차단되는 오류로 분리한다고 설명한다.
에이전트 대화가 계속된다고 메모리 수집도 정상이라는 뜻은 아니다.
이 문서는 모든 통합 환경의 Hook 차이를 시험하지 않았으므로 Claude Code 경로에서 확인한 흐름을 다른 호스트의 구현과 동일하다고 단정하지 않는다.[2]

## 직접 읽어볼 자료

- [README의 Quick Start와 MCP Search Tools](https://github.com/thedotmack/claude-mem/blob/main/README.md)
  플러그인 설치와 SDK 설치의 차이를 확인한 뒤 검색 색인에서 상세 관찰로 좁히는 예를 읽는다.
  설치 완료와 실제 기억 수집을 구별하는 출발점이다.
- [Architecture Overview](https://github.com/thedotmack/claude-mem/blob/main/docs/architecture-overview.md)
  Hook에서 대기열·요약 처리·SQLite·ChromaDB로 이어지는 경로를 따라간다.
  어떤 이벤트가 기록되고 worker 장애가 호스트에 어떻게 전달되는지 확인할 수 있다.
- [ContextBuilder.ts](https://github.com/thedotmack/claude-mem/blob/main/src/services/context/ContextBuilder.ts)
  `generateContextWithStats`와 `fitContextForDelivery`를 읽는다.
  조회한 기록과 실제 전달된 기록을 구별하고 경고까지 출력 예산에 포함하는 이유가 주석에 설명되어 있다.

## 정리

Claude-Mem은 이전 작업을 다시 찾을 수 있도록 기록과 검색을 연결한다.
기억의 존재, 현재성, 수집 상태와 실제 전달 범위를 따로 확인해야 과거 요약을 현재 사실로 오해하지 않을 수 있다.

## 자료 확인 범위

2026-09-27 기준 README 핵심 절, 루트 구성, 아키텍처 문서와 맥락 조립 구현을 읽었다.
플러그인 설치, observer 호출, 기록 검색과 메모리 절감 측정은 실행하지 않았다.

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

[1] thedotmack/claude-mem — README.md

<https://github.com/thedotmack/claude-mem/blob/main/README.md>

[2] thedotmack/claude-mem — docs/architecture-overview.md

<https://github.com/thedotmack/claude-mem/blob/main/docs/architecture-overview.md>

[3] thedotmack/claude-mem — src/services/context/ContextBuilder.ts

<https://github.com/thedotmack/claude-mem/blob/main/src/services/context/ContextBuilder.ts>
