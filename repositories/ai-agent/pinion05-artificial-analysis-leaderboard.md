---
title: "pinion05/artificial-analysis-leaderboard"
repository: "pinion05/artificial-analysis-leaderboard"
url: "https://github.com/pinion05/artificial-analysis-leaderboard"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# pinion05/artificial-analysis-leaderboard

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

모델 성능표를 에이전트가 읽을 수 있는 표와 JSON으로 가져오는 Skill이다.
자체 벤치마크나 모델 실행 서비스가 아니라 Artificial Analysis의 공개 페이지에 들어 있는 평가·가격·속도 정보를 추출하는 도구다.[1]

## 순위보다 비교 기준을 먼저 정하는 도구

“코딩에 좋은 모델”과 “첫 응답이 빠른 모델”은 같은 질문이 아니다.
이 프로젝트는 Intelligence, Coding, Agentic이라는 서로 다른 종합 지표에 가격과 응답 속도를 나란히 붙여, 목적별로 후보를 좁히게 한다.
Agentic은 여기서 도구 사용과 에이전트 과제에 관한 평가 묶음이며, 특정 개인 프로젝트의 성공률을 뜻하지 않는다.[1]

README는 이 기능을 `SKILL.md`를 읽는 에이전트에 넣을 수 있는 휴대 가능한 Skill로 소개한다.
Skill은 새 모델이 아니라 에이전트에게 언제 어떤 도구를 사용할지 알려주는 지침이다.
실제 데이터 수집은 Node.js 스크립트가 맡으므로 에이전트 없이도 실행 가능한 구조다.
다만 에이전트마다 Skill 탐색 경로는 다르므로, 디렉터리 형식이 같다는 이유만으로 모든 환경에서 자동 인식된다고 단정할 수는 없다.[1]

## 공개 HTML에서 비교표까지

수집기는 브라우저 화면을 클릭하지 않는다.
HTTPS 요청으로 받은 페이지 안에는 서버가 미리 만든 모델 데이터가 포함되어 있고, `parse()`는 이스케이프된 JSON 조각을 찾아 모델별 레코드로 나눈다.
JSON은 프로그램이 읽기 쉬운 구조화된 데이터 형식이며, 이스케이프는 따옴표 같은 문자를 다른 문자열 안에 안전하게 담는 표현이다.[2]

`field()`가 지표 이름에 해당하는 값을 꺼내고, 같은 slug가 여러 번 나타나면 유효한 값이 더 많은 레코드를 남긴다.
slug는 화면의 긴 이름 대신 모델을 구별하는 문자열이다.
이 과정을 통해 HTML의 중복 표현이 최종 비교표의 중복 행이 되는 것을 줄인다.
자료가 없는 값은 `--`로 표시하며 지능 지수가 추정값이면 별표를 붙인다.
누락과 낮은 성능을 구별해서 읽어야 하는 이유다.[1][2]

정렬 방향도 지표에 따라 다르다.
코딩·에이전트·속도는 높은 값부터, 가격·첫 응답 대기는 낮은 값부터 보도록 구현되어 있다.
필터와 정렬 이후 `--top`을 적용하므로 “특정 제공자의 상위 모델”을 뽑는 과정과 전체 상위 목록에서 제공자를 찾는 과정은 다르다.
표에 쓰는 혼합 가격은 `price1mBlended7To2To1` 필드이며, 모든 사용 패턴의 실제 청구액을 계산하는 비용 계산기는 아니다.[2]

`--deep`은 여러 모델을 얇게 비교하는 대신 한 모델의 원래 데이터 객체를 더 깊게 해독한다.
괄호의 짝을 찾아 JSON 범위를 정하고 두 번 파싱해 세부 벤치마크, 입력·출력·캐시 가격, 속도 분포를 드러낸다.
중앙값 하나에 가려지는 응답 지연 차이를 더 살펴보는 입구지만, 데이터가 없는 항목까지 새로 측정해 채우지는 않는다.[1][2]

## 예시로 따라가는 흐름

공식 사용 예인 `node aa-leaderboard.js --sort coding --top 10`을 문서와 코드로 따라가 보자.
이는 직접 실행한 결과가 아니라 제공된 명령의 처리 흐름을 읽은 것이다.
입력은 코딩 지표 정렬과 최대 행 수다.
스크립트는 공개 페이지를 가져와 모델 레코드를 만들고, 값이 없는 항목을 뒤로 보내며 코딩 점수 순으로 정렬한 다음 선택된 행을 표로 출력하도록 되어 있다.
여기서 나온 첫 번째 모델을 곧바로 “모든 코드 작업의 최선”이라고 해석하지 않는 것이 중요하다.[1][2]

사람이 확인할 부분은 코딩 점수뿐 아니라 가격, 속도, 누락 표시와 모델의 정확한 이름이다.
후보 하나를 더 살필 때는 `--deep`과 구체적인 모델 이름을 함께 주는 경로가 마련되어 있다.
부분 문자열만 맞으면 다른 변형이 선택될 수 있어 구현은 정확히 일치하지 않은 경우 안내를 출력한다.
그다음 세부 지표와 가격 구성을 읽어 선택 근거를 좁힐 수 있다.
JSON 출력은 이 자료를 후속 분석에 넘기기 위한 형식일 뿐, 실제 후보를 호출하거나 코드를 채점한 결과가 아니다.[2]

## 최신 자료를 가져오는 방식의 한계

README는 Node.js 14 이상과 대상 사이트에 대한 인터넷 접근을 요구하며, 표준 라이브러리만 사용하고 API 키는 필요 없다고 설명한다.
이 조건은 데이터 제공 사이트의 유료 Data API를 사용하지 않는다는 뜻이지, 영구적으로 안정된 공식 API 계약이 있다는 뜻은 아니다.
사이트가 HTML 내부 데이터 모양을 바꾸면 `parse()`와 `field()`를 수정해야 한다고 명시한다.[1]

현재 구현을 읽을 때도 문서와 코드의 범위를 구별해야 한다.
README의 `--model` 설명은 이름 또는 slug 검색을 말하지만 일반 목록 경로의 필터는 `d.name`을 검사한다.
반면 심층 경로는 이름·짧은 이름·slug를 비교한다.
따라서 slug를 쓰는 심층 예와 일반 목록의 이름 필터를 같은 동작으로 설명하면 부정확하다.
이 차이는 정적 코드 확인 사항이며 실제 장애를 재현한 보고는 아니다.[2]

조회 때마다 바뀌는 자료를 다루므로 특정 모델 수나 순위를 이 글의 고정 사실로 제시하지 않는다.
비교 결과를 기록하려면 조회 시각과 정렬 기준도 함께 보존해야 의미가 남는다.
수집 성공 여부와 원래 평가의 적절성은 별개의 문제다.

## 직접 읽어볼 자료

- [README의 What it returns와 Flags](https://github.com/pinion05/artificial-analysis-leaderboard/blob/main/README.md)
  먼저 지표별 단위와 정렬 방향을 읽는다.
  특히 `--`와 별표의 뜻을 확인하면 “자료 없음”을 낮은 점수로 오독하지 않을 수 있다.
- [수집기의 parse와 main](https://github.com/pinion05/artificial-analysis-leaderboard/blob/main/skills/artificial-analysis-leaderboard/aa-leaderboard.js)
  HTML에서 어떤 필드가 선택되고 중복이 어떻게 제거되는지 따라간다.
  일반 검색과 심층 검색이 어느 속성을 검사하는지도 직접 비교할 수 있다.
- [README의 Deep profile과 Reliability](https://github.com/pinion05/artificial-analysis-leaderboard/blob/main/README.md)
  표에서 생략한 가격·분포 정보와 사이트 구조 변경에 따른 유지보수 경계를 연결해서 읽는다.
  필드가 많다는 것과 모든 필드가 채워진다는 것은 다르다.

## 정리

이 프로젝트의 역할은 공개 모델 평가를 다시 측정하는 것이 아니라, 이미 공개된 데이터를 목적별로 정리해 사람이 비교하도록 돕는 것이다.
순위의 의미와 수집 방식의 취약성을 함께 읽어야 도구의 범위가 분명해진다.

## 자료 확인 범위

2026-09-27 수집 기준으로 공식 README, 루트 구성, 실제 수집 스크립트를 확인했다.
모델 조회 명령과 Skill 설치는 실행하지 않았으며 실시간 순위와 파서의 현재 성공 여부를 검증한 것은 아니다.

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

[1] pinion05/artificial-analysis-leaderboard — README.md

<https://github.com/pinion05/artificial-analysis-leaderboard/blob/main/README.md>

[2] pinion05/artificial-analysis-leaderboard — skills/artificial-analysis-leaderboard/aa-leaderboard.js

<https://github.com/pinion05/artificial-analysis-leaderboard/blob/main/skills/artificial-analysis-leaderboard/aa-leaderboard.js>
