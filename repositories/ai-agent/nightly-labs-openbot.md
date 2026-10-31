---
title: "nightly-labs/openbot"
repository: "nightly-labs/openbot"
url: "https://github.com/nightly-labs/openbot"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-agent", "starred-draft"]
---

# nightly-labs/openbot

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

여러 AI 에이전트의 지속 대화·작업 큐·파일 전달을 한곳에서 관리하는 로컬 우선 데스크톱 작업공간이다.[1]

## 대화를 닫아도 이어지는 작업공간

OpenBot은 Codex, Claude Code와 여러 CLI 기반 제공자를 한 데스크톱 앱에 연결한다.
각 에이전트에 별도 작업공간과 대화를 두고, 작업 큐·파일 전달·내장 브라우저·에이전트 간 메시지를 제공한다.
새 언어 모델이 아니라 기존 실행 엔진을 관리하는 작업 환경이다.[1]

한 에이전트에게 초안을 맡기고 다른 에이전트에게 검토를 맡기면 대화, 파일, 진행 상태가 흩어지기 쉽다.
README는 공동 채널의 작업 소유자, 명시적 위임, 중지·재개·재할당과 공유 이력을 설명한다.
작업 큐는 먼저 들어온 메시지를 먼저 처리하는 FIFO 방식이며 일시정지·취소와 복구를 지원한다고 안내한다.[1]

## 도구를 한꺼번에 넣지 않는 방식

공식 Providers 문서는 기본 직접 도구와 필요할 때 찾는 도구 카탈로그를 구분한다.
`tool_search`는 로컬 키워드 일치로 도구를 찾고, `tool_describe`는 상세 스키마와 안내를 제공한다.
스키마는 도구 이름만으로 알 수 없는 인자 형식과 호출 계약을 뜻한다.[2]

강한 일치 결과에는 스키마를 함께 실어 별도 조회 단계를 줄일 수 있다.
`tool_call`은 원래 도구 이름으로 라우팅하며 요청·대화·턴 식별자와 취소 신호를 유지한다.
도구 검색이 기존 권한 처리와 결과 처리 경로를 새로 우회하는 별도 실행기가 되지 않도록 설계한 설명이다.[2]

세션 교체 시에는 제한된 크기의 인계 자료를 만들고, 빠진 대화는 이력 검색·읽기 도구로 복구한다.
문서는 발췌문을 의미 요약과 구분하며, 저장된 원본 대화가 그대로 전부 모델에 들어간다고 주장하지 않는다.
따라서 지속 대화 기능과 무한한 문맥 기억은 같은 개념이 아니다.[2]

## 가상 사례: 초안과 검토 결과 주고받기

자신의 공개 개인 프로젝트 소개 문서와 “설명 초안을 작성하고 다른 에이전트에게 검토를 요청하라”는 요청이 입력이라고 가정한다.
첫 에이전트가 자기 작업공간에서 파일을 만들고, 메시지나 관리되는 파일 전달로 두 번째 에이전트에게 넘긴다.
공동 채널에서는 누가 작업을 맡았는지 확인하고 필요하면 큐를 멈추거나 재할당하는 식이다.[1]

예상 결과는 초안 파일과 검토 메시지, 작업 이력이다.
이것은 설명용 시나리오이며 실제 파일 생성이나 제공자 간 협업을 실행한 결과가 아니다.
검토자를 하나 더 두었다는 사실만으로 사실 검증이나 파일 접근 통제가 완성되지는 않는다.

## 로컬 우선과 안전한 격리는 다르다

README는 개발 프리뷰라고 명시하고, 최초 동의 뒤 에이전트가 개별 확인 없이 파일·명령·네트워크·브라우저를 다룰 수 있다고 경고한다.
현재 설정은 `danger-full-access`와 `approvalPolicy: never`이며, 로컬 우선은 오프라인 전용이라는 뜻도 아니다.[1]

라이선스는 MIT나 Apache가 아닌 PolyForm Noncommercial 1.0.0이다.
비상업적 목적과 상업적 적용을 예상하지 않는 개인 연구·취미 등의 허용 범위를 규정하므로, 소스가 공개되었다는 이유로 상업 이용까지 자유롭다고 보면 안 된다.[3]

SECURITY 문서는 사용자 대신 호스트를 샌드박스로 격리하지 않는다고 명시한다.
원격 페이지와 특권 IPC의 분리, 첨부 파일 경로 경계 등은 지켜야 할 보안 경계로 제시되지만 여기서 실제 검증하지 않았다.
이전 릴리스의 보안 수정 역이식도 보장하지 않으므로 중요한 자료와 계정이 있는 환경에서는 별도 주의가 필요하다.[4]

## 직접 읽어볼 자료

1. [README의 What works·경고문](https://github.com/nightly-labs/openbot/blob/main/README.md)
   협업 기능과 최초 동의 이후 실행 권한을 함께 확인한다.
2. [Providers 구조 문서](https://github.com/nightly-labs/openbot/blob/main/docs/architecture/providers.md)
   도구 발견과 세션 교체 때 보존되는 정보, 생략되는 정보를 구분한다.
3. [SECURITY.md](https://github.com/nightly-labs/openbot/blob/main/SECURITY.md)
   로컬 저장과 호스트 격리가 다른 이유를 확인한다.

## 자료 확인 범위

2026-10-28 공식 README·루트 트리와 위 대표 문서·코드를 읽었다.[5]
설치·실행·외부 계정 연결은 하지 않았으며, 예시는 직접 사용 경험이 아닌 가상 흐름이다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] nightly-labs/openbot — README.md

<https://github.com/nightly-labs/openbot/blob/main/README.md>

[2] nightly-labs/openbot — docs/architecture/providers.md

<https://github.com/nightly-labs/openbot/blob/main/docs/architecture/providers.md>

[3] nightly-labs/openbot — LICENSE

<https://github.com/nightly-labs/openbot/blob/main/LICENSE>

[4] nightly-labs/openbot — SECURITY.md

<https://github.com/nightly-labs/openbot/blob/main/SECURITY.md>

[5] nightly-labs/openbot — 공식 루트 트리

<https://github.com/nightly-labs/openbot/tree/main>
