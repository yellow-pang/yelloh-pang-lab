---
title: "ranxianglei/billion-context"
repository: "ranxianglei/billion-context"
url: "https://github.com/ranxianglei/billion-context"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# ranxianglei/billion-context

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

billion-context는 에이전트와 모델 API 사이에서 대화 내용을 압축하는 프록시·플러그인이다.
모델의 고정된 입력 창을 실제로 무한대로 늘리는 것이 아니라, 오래된 내용을 요약해 한 번에 보내는 분량을 관리한다.[1]

## 긴 대화의 기록과 현재 입력을 나누기

코딩 대화가 길어지면 읽은 파일과 도구 출력이 누적되어 입력 비용과 문맥 한계가 문제가 된다.
이 프로젝트는 사용한 대화 범위를 층별 요약으로 접고 필요하면 다시 펼치는 방식을 제시한다.
모델이 `compress`, `decompress`, `search_context`, `acp_status` 등의 도구를 사용하고 프록시가 해당 처리를 이어간다.[1]

README의 수십억 토큰은 긴 기간에 걸쳐 처리한 누적량이라는 맥락으로 읽어야 한다.
“5배 적은 토큰”이나 높은 캐시 적중률은 프로젝트의 성능 주장이지, 이 조사에서 재현한 측정값이나 모든 모델에 대한 보장이 아니다.[1]

## 요약을 쓰는 모델과 상태를 관리하는 커널

`kernel/README.md`는 모델이 요약을 쓰고 커널은 그 주변의 범위·상태·추천·복원을 관리한다고 설명한다.
커널 자체는 모델을 호출하지 않으며, 상태를 함수 입력과 출력으로 전달하고 저장은 호스트에 맡긴다.
이 구분 덕분에 같은 압축 논리를 서로 다른 에이전트 연결 방식에서 사용할 수 있다.[2]

`kernel/src/compress.ts`의 `applyCompression`은 시작 참조·끝 참조·요약이 들어 있는 범위 목록을 받는다.
상태를 복제한 다음 기본적으로 최근 메시지와 마지막 사용자 메시지 등 보호 범위를 계산한다.
요청한 범위는 정상·이미 소비됨·알 수 없음·잘못된 참조 등으로 분류되어 오류와 처리 흐름이 나뉜다.[3]

따라서 아무 텍스트를 짧게 자르는 함수라기보다 메시지 식별자와 압축 블록을 유지하는 상태 관리에 가깝다.
보호와 참조 검사 코드가 있다고 해도 모델이 쓴 요약이 중요한 세부사항을 빠짐없이 보존한다는 의미는 아니다.[2][3]

## 원문 보관과 의미 요약은 같지 않다

커널 문서는 선택 기능인 CCR을 도구 결과의 무손실 보관·검색 경로로 설명한다.
큰 도구 출력을 저장소에 보관하고 표시 내용을 참조로 바꾸며, 원문은 `acp_retrieve`로 다시 읽는다.
이 기능은 기본 비활성이고 저장 책임은 호스트에 있으므로 모든 사용 모드에서 자동 보관된다고 가정하면 안 된다.[2]

요약으로 충분한 상황과 정확한 오류 로그·코드 원문이 필요한 상황을 나누어 읽는 것이 핵심이다.
복원 가능성은 요약 문장이 완전하다는 성질이 아니라, 연결된 상태와 원문 보관 경로가 유지된다는 조건과 관련된다.[1][2]

## 가상 예시: 여러 날 이어지는 개인 코드 탐색

공개 개인 프로젝트의 파일을 차례로 읽으며 긴 대화를 이어가는 상황을 가정한다.
이미 정리한 파일 조사 구간은 요약으로 접고 현재 수정할 파일과 최근 요청을 남겨 모델 입력을 줄인다.
나중에 이전 오류의 정확한 문장이 필요해지면 검색이나 복원 경로로 원문을 확인한다.[1][2]

이 예시의 성공 기준은 대화가 계속된다는 사실뿐 아니라 요구사항·결정·코드 위치가 제대로 이어지는지다.
토큰 절감과 요약 정확도는 따로 확인해야 하며, 실제 세션을 연결해 비교한 기록은 아니다.

## 연결 방식과 라이선스 주의점

README는 네이티브 플러그인·런처·URL 변경을 구분하고 클라이언트별 지원 차이를 안내한다.
일부 런처는 인증서를 이용한 중간 프록시 경로를 사용하므로 요청 경로와 인증서 신뢰 변경을 먼저 확인해야 한다.
기존 독립 확장과 네이티브 플러그인을 함께 켜면 이중 압축이 생길 수 있다는 경고도 있다.[1]

커널 README에는 MIT라는 표현이 있지만 루트 LICENSE에는 제품 화면의 출처 표시를 요구하는 추가 조항이 있다.
사용자에게 보이는 제품·서비스라면 홈페이지·문서·About 등 정해진 위치에 billion-context와 해당 시 acp-kernel 사용 사실과 링크를 명시하도록 한다.
따라서 저장소 전체를 아무 추가 조건 없는 MIT로 요약하지 않고 실제 적용할 구성 요소의 조건을 확인해야 한다.[2][4]

## 직접 읽어볼 자료

1. README의 How it works와 Which do I need?: 모델 API 사이의 위치와 클라이언트별 연결 차이를 읽는다.[1]
2. `kernel/README.md`: 모델이 만든 요약, 함수 상태, CCR 원문 보관의 책임을 구분한다.[2]
3. `kernel/src/compress.ts`: `applyCompression`의 보호 범위와 참조 분류를 따라간다.[3]
4. 루트 LICENSE: MIT 본문 뒤에 붙은 출처 표시 조항까지 확인한다.[4]

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 자료를 확인했다. 설치·빌드·실행은 하지 않았다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] ranxianglei/billion-context — README.md

<https://github.com/ranxianglei/billion-context/blob/master/README.md>

[2] ranxianglei/billion-context — kernel/README.md

<https://github.com/ranxianglei/billion-context/blob/master/kernel/README.md>

[3] ranxianglei/billion-context — kernel/src/compress.ts

<https://github.com/ranxianglei/billion-context/blob/master/kernel/src/compress.ts>

[4] ranxianglei/billion-context — LICENSE

<https://github.com/ranxianglei/billion-context/blob/master/LICENSE>
