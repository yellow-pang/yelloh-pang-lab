---
title: "tt-a1i/archify"
repository: "tt-a1i/archify"
url: "https://github.com/tt-a1i/archify"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# tt-a1i/archify

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Archify는 에이전트가 구조·절차·호출 순서·데이터 흐름·상태 전이를 대화형 HTML로 작성하도록 돕는 Skill과 렌더링 도구다.
범용 그림 편집기나 Mermaid에 색상만 입히는 테마는 아니다.
설명 또는 조사한 코드의 관계를 명시적인 JSON에 담고, 검사한 뒤 독립 HTML 파일로 전달하는 방식이다.[1][2]

## 예쁜 화살표보다 먼저 정할 질문

같은 시스템도 “무엇으로 이루어졌나”, “어떤 순서로 호출하나”, “어떤 조건에서 재시도하나”에 따라 필요한 그림이 달라진다.
Archify는 구성 요소를 보여 주는 architecture, 단계 흐름인 workflow, 호출 시간 순서인 sequence, 자료 이동인 dataflow와 상태 변화인 lifecycle로 유형을 나눈다.
유형을 고르는 것은 모양 선택보다 설명할 질문을 정하는 일에 가깝다.[2]

입력은 꼭 저장소일 필요가 없다.
README의 예처럼 브라우저가 API를 호출하고 캐시가 없으면 데이터베이스를 조회한다는 설명으로 시작할 수 있다.
반면 실제 코드와 일치하는 구조도를 원한다면 해당 저장소 근거를 조사해야 한다.
상상한 설계와 확인한 구현은 다른 출처이며, Skill은 예제의 필드 모양을 참고하되 그 예제의 사실을 그대로 복사하지 말라고 규정한다.[1][2]

## 그림의 원본과 검증 영수증

중간 표현인 typed JSON IR은 노드·관계·라벨·배치 같은 의미를 일정한 스키마로 적은 원본이다.
렌더러는 이를 HTML과 SVG로 바꾸고 검증기는 형식과 배치·경로·라벨 간 간격 등을 검사한다.
스키마는 허용 필드와 값의 규칙이며, SVG는 크기를 바꿔도 선과 문자를 선명하게 그리는 벡터 형식이다.[1][2]

`deliver`는 후보를 같은 디렉터리의 별도 파일로 만들고 검사한 뒤 통과한 HTML만 최종 경로에 원자적으로 바꾸는 경로다.
원자적 교체는 중간에 불완전한 파일을 최종 산출물처럼 남기지 않기 위한 방식이다.
실패하면 이전에 통과한 산출물이 그대로 남을 수 있으므로 출력 파일이 존재한다는 것만으로 새 후보가 성공했다고 할 수 없다.[2]

검사 보고서는 문제의 대상, 측정 근거와 지원되는 수정 방법을 돌려주도록 설명된다.
다만 형식·배치 검사, 실제 브라우저 동작 확인, 눈으로 읽기 좋은지의 검토를 별도 주장으로 나눈다. `deliver`의 통과만으로 브라우저 상호작용을 실행했거나 사람이 그림을 검토했다고 쓰지 않는 것이 이 프로젝트의 중요한 계약이다.[2]

## 예시로 따라가는 흐름

대표 공식 파일 `cache-miss-request.sequence.json`은 캐시에 값이 없는 웹 요청을 설명한다.
직접 렌더링하거나 서비스를 호출한 결과가 아니라 저장된 예제의 의미를 따라가는 것이다.
입력 JSON에는 User, Web App, API, Auth, Redis, Postgres와 Trace가 참여자로 명시되어 있다. `messages`는 페이지 열기, 대시보드 요청, JWT 확인, 캐시 조회와 miss 응답을 차례로 적는다.
JWT는 인증 정보를 전달하는 토큰 형식이며 그림에서는 인증 확인 단계의 역할을 한다.[3]

캐시 miss 뒤 API는 Postgres에서 프로필과 지표를 조회하고 결과를 받은 다음 캐시를 채우고 trace 이벤트를 보낸 뒤 JSON 응답을 돌려준다.
예제는 Trace를 보조적인 비동기 경로로 설명하고 요청·fallback·응답 구간을 나누어 놓았다.
화면에서 보이는 시간 순서는 작성된 메시지 순서이지 실제 응답 시간을 계측한 trace가 아니다.[3]

렌더링에 전달할 원본에는 요청과 인증, 캐시 fallback, 반환과 trace라는 읽기 장면도 들어 있다.
완성 HTML에서는 이런 작성된 관계를 강조하고 확대·탐색할 수 있지만 없는 통신 경로를 새로 발견하는 것은 아니다.
사람이 확인할 부분은 캐시 실패 분기와 반환 방향이 의도와 맞는지, 비동기 표현이 실제 코드와 일치하는지, 라벨과 화살표가 서로 가리지 않는지다.
실서비스 설명에 재사용할 때는 예제 참여자를 그대로 사실로 옮기지 않고 자신의 공개 근거로 다시 작성해야 한다.[1][2][3]

## 탐색은 분석 결과를 대신하지 않는다

HTML의 노드 검색, 상·하류 관계 탐색과 route 강조는 작성된 연결을 재사용한다.
README는 Architecture Delta도 검증한 전후 스냅샷의 작성 사실을 비교할 뿐 영향·위험·병합 안전성을 추론하지 않는다고 밝힌다.
그림에서 연결이 보인다고 실운영 장애 전파나 보안 적합성을 증명할 수는 없다.[1]

소스 근거 연결은 요청한 경우의 추가 기능이며 공개 커밋에 고정한 파일·줄 범위를 표시하는 방식으로 소개된다.
일반 다이어그램은 소스 근거가 없는 상태로도 만들 수 있다.
따라서 `verified`라는 표현을 읽을 때 그림 형식이 검사를 통과했다는 뜻인지 원래 코드와 대조했다는 뜻인지 구분해야 한다.[1][2]

결과는 기본적으로 자체 포함 HTML이라 보는 사람에게 Archify 설치를 요구하지 않는다.
그러나 연결한 외부 사이트나 지도는 네트워크가 필요하다.
애니메이션은 명시적으로 요청할 때 켜는 선택 기능이며, 움직임이 실제 시스템의 현재 상태나 실행 로그를 나타내는 것도 아니다.[1][2]

한국어 설명을 넣는 것과 뷰어 자체의 UI 언어도 다르다.
Skill은 렌더러 소유 UI의 locale을 영어·중국어로 제한하고 다른 언어의 본문은 보존하되 고정 UI와 HTML 언어 표시가 영어로 돌아감을 알리도록 한다.
한글 라벨을 넣었다고 모든 버튼과 접근성 안내까지 번역되는 것은 아니다.[2]

## 직접 읽어볼 자료

- [README의 Choose the right diagram과 How it works](https://github.com/tt-a1i/archify/blob/main/README.md)
  설명할 질문에 따라 유형을 고른 뒤 생성·검사·미리보기·전달의 구분을 읽는다.
  특히 작성된 연결 탐색과 실제 시스템 분석의 차이를 확인한다.
- [Archify Skill의 Delivery](https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md)
  후보 통과, 브라우저 증거와 시각 검토를 별도로 보고하라는 계약을 본다.
  실패 때 남은 이전 HTML을 새 결과로 검사하면 안 되는 이유도 설명되어 있다.
- [캐시 miss Sequence 원본](https://github.com/tt-a1i/archify/blob/main/archify/examples/cache-miss-request.sequence.json)
  `participants`, `messages`, `views`를 순서대로 따라간다.
  배치 숫자부터 읽기보다 누가 무엇을 요청하고 어떤 반환·비동기 관계를 그리는지 먼저 확인할 수 있다.

## 정리

Archify는 설명 가능한 구조를 편집 가능한 원본과 전달 가능한 HTML로 만든다.
검증된 산출물이라는 말의 범위는 형식·브라우저·시각·소스 근거 중 실제 확인한 층으로 제한해야 한다.

## 자료 확인 범위

2026-09-27 기준 README 핵심 절, 루트 구성, Skill 전체와 캐시 miss Sequence JSON을 읽었다.
CLI 실행, HTML 생성, 브라우저 시각 검사와 소스 커밋 검증은 하지 않았다.

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

[1] tt-a1i/archify — README.md

<https://github.com/tt-a1i/archify/blob/main/README.md>

[2] tt-a1i/archify — archify/SKILL.md

<https://github.com/tt-a1i/archify/blob/main/archify/SKILL.md>

[3] tt-a1i/archify — archify/examples/cache-miss-request.sequence.json

<https://github.com/tt-a1i/archify/blob/main/archify/examples/cache-miss-request.sequence.json>
