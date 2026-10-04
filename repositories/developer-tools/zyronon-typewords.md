---
title: "zyronon/TypeWords"
repository: "zyronon/TypeWords"
url: "https://github.com/zyronon/TypeWords"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# zyronon/TypeWords

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

TypeWords는 영어 단어와 글을 키보드로 입력하며 연습하는 웹 도구다.
타자 속도만 재는 시험이 아니라 따라 쓰기·받아쓰기·자기 시험·기억해서 철자 쓰기 등 학습 방식을 제공하고, 틀린 단어와 익힌 단어를 나누어 관리한다.
입력이라는 행동을 어휘·문장 연습과 연결한 앱으로 이해할 수 있다.[1]

## 알고 있는 단어와 써낼 수 있는 단어의 차이

단어를 보고 뜻을 알아도 소리만 듣거나 힌트 없이 철자를 적으면 틀릴 수 있다.
TypeWords는 단어 연습에 발음 기호, 미국식·영국식 발음, 예문, 구절, 동의어 등 여러 정보를 연결한다고 설명한다.
읽는 정보와 직접 입력하는 과제를 번갈아 사용하도록 구성되어 있다.
다만 제공되는 자료가 많다는 사실이 모든 학습자의 기억 향상을 증명하는 실험 결과는 아니다.[1]

Smart 모드는 기억 곡선에 따라 학습할 단어를 계산한다고 소개하고, Free 모드는 사용자가 학습량을 자유롭게 정하도록 한다.
여기서 기억 곡선은 시간이 지나며 기억이 달라지는 양상을 이용한다는 설명의 일부다.
이번 조사에서는 추천 알고리즘의 효과를 시험하지 않았으므로 특정 시간 안에 암기된다는 약속으로 해석하지 않는다.[1]

## 단어 목록과 글 연습을 구분하는 화면

단어 연습에서 틀린 단어는 오답 단어장에 추가되고, 익힌 것으로 표시한 단어는 이후 연습에서 자동으로 건너뛰며, 즐겨찾기는 따로 복습할 항목을 모으는 용도로 설명된다.
오답·숙달·즐겨찾기는 비슷한 저장 목록처럼 보여도 의도가 다르다.
오답은 입력 결과, 숙달은 향후 연습 제외 판단, 즐겨찾기는 다시 보고 싶은 선택에 가깝다.[1]

글 암기에서는 기본 교재와 사용자가 추가·가져온 글을 다루며 문장 단위로 따라 쓰기와 받아쓰기를 수행한다고 안내한다.
문장별 자동 발음, 번역과 양언어 비교도 소개된다.
어휘 하나의 철자를 맞히는 과제와 문장의 순서·표현을 이어서 입력하는 과제가 분리되어 있는 셈이다.
어떤 연습을 했는지 구분해야 오답 통계나 진행 위치도 의미 있게 읽을 수 있다.[1]

단축키·키보드 소리·세부 설정을 조절할 수 있고, 기본 어휘 목록에는 여러 영어 시험의 단어장이 포함된다.
그러나 시험 이름은 제공 목록의 범주이지 실제 시험 점수를 보장하는 표시가 아니다.
학습 자료의 수준과 자신이 연습하려는 내용을 먼저 맞춰 보는 것이 기능 목록을 읽는 적절한 방법이다.[1]

## 진행 상태는 어떻게 남는가

대표 파일 `practice-sentence-cache.ts`는 문장 연습 상태를 저장하는 자료형을 보여준다.
단어장 ID와 이름, 연습 항목 목록, 현재 인덱스, 틀린 항목 ID, 완료 항목 ID, 연습 모드를 함께 담는다.
화면에서 보이는 '진행 중'을 단순 숫자 하나가 아니라 여러 상태의 묶음으로 다룬다는 점이 드러난다.[2]

읽기와 쓰기는 `idb-keyval`의 `get`·`set`으로 연결되며 버전 정보와 갱신 시각을 붙인 JSON 문자열을 저장한다.
읽을 때 자료가 없거나 JSON 해석이 실패하거나 버전이 맞지 않으면 사용할 캐시가 없다는 뜻의 `null`을 돌려준다.
이 처리는 오래되거나 잘못된 자료를 무조건 정상 진행으로 받아들이지 않는 장치다.
반면 다른 버전의 자료를 자동 변환해 복구하는 기능까지 이 파일에서 확인된 것은 아니다.[2]

`usePracticeSentencePersistence.ts`는 load·fetch·save·clear라는 짧은 인터페이스로 이 저장 기능을 감싼다.
특히 여기서 `fetch`는 원격 서버에 가져오러 가는 요청이 아니라 `load()`를 호출한다.
함수 이름만 보고 계정 간 동기화가 있다고 추정할 수 없는 예다.
README도 독립 실행 시 자료를 로컬에 저장하며 기기를 바꿀 때 수동 백업이 필요하다고 밝힌다.[1][3]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
짧은 영어 글을 가져와 먼저 문장을 보며 따라 쓰고, 이어 소리를 듣고 받아쓰는 상황을 생각해 보자.
입력은 연습할 글과 선택한 모드, 사용자의 키 입력이다.
README가 설명한 문장 단위 연습에서는 단어 하나만 외우는 대신 이어지는 표현을 순서대로 써 보는 과제가 된다.[1]

연습을 이어가기 위해 필요한 상태는 대표 캐시 자료형에서 확인할 수 있다.
어느 자료 묶음을 연습했는지 나타내는 ID, 현재 항목 위치, 틀린 항목과 완료한 항목 목록, 선택한 모드가 저장 대상이다.
다음에 읽을 때는 저장 형식의 버전이 맞는지도 확인한다.
소스에 이런 상태가 정의되어 있다는 사실은 확인했지만 모든 화면 이벤트가 어느 시점에 저장을 호출하는지까지 추적한 것은 아니다.[3][2]

사람이 확인할 결과는 입력이 맞았는지와 오답을 다시 연습할 필요가 있는지다.
완료 표시를 이해도나 장기 기억의 증거와 동일시하지 않는다.
다른 기기에서 이어 하려면 로컬 저장 자료가 자동으로 따라온다고 기대하지 말고 백업 조건을 확인해야 한다.
이처럼 연습 모드, 진행 기록, 실제 학습 판단을 구분하면 앱이 맡는 일과 학습자가 맡는 일이 선명해진다.[1]

## 읽을 때 조심할 구현의 시간차

추가로 열어본 `useSentenceTypingFlow.ts`에는 반응성 연결 문제로 다른 구성요소의 내부 상태 방식에 대체되었으며 새 코드에서 사용하지 말라는 deprecated 주석이 있다.
deprecated는 이전 방식이 남아 있지만 새 사용은 권하지 않는다는 뜻이다.
파일이 저장소에 있다는 이유로 현재 화면의 핵심 입력 처리라고 소개하면 부정확하므로, 이 글은 해당 코드를 현행 동작 근거로 사용하지 않았다.[4]

README는 프로젝트가 초기 개발 중이라고 밝힌다.
자체 실행은 Nuxt와 Node.js 환경을 필요로 하고, 자료를 로컬에 저장할 수 있다는 설명은 모델이나 모든 부가 서비스까지 완전한 오프라인이라는 뜻으로 확대하지 않는다.
브라우저 자료 정리나 기기 이동 전에는 연습 기록의 보존 방법을 확인할 필요가 있다.[1]

## 직접 읽어볼 자료

- [공식 README](https://github.com/zyronon/TypeWords/blob/master/README.md)
  Word Practice와 Article Memorization을 비교하고 오답·숙달·즐겨찾기의 차이를 읽는다.
  실행 절의 로컬 저장·수동 백업 안내도 기능 설명과 연결해서 확인한다.
- [문장 연습 캐시](https://github.com/zyronon/TypeWords/blob/master/app/composables/practice-sentences/practice-sentence-cache.ts)
  자료형에서 어떤 상태를 보존하는지 보고 읽기 함수의 버전 검사로 내려간다.
  저장된 값이 있다는 사실만으로 사용 가능한 진행 자료라고 볼 수 없는 이유를 확인할 수 있다.
- [저장 인터페이스](https://github.com/zyronon/TypeWords/blob/master/app/composables/practice-sentences/usePracticeSentencePersistence.ts)
  `fetch`와 `load`의 관계, `clear`가 저장 계층에 전달하는 값을 본다.
  이름에서 원격 동기화를 추측하지 않고 실제 호출을 읽는 작은 사례다.

## 정리

TypeWords는 영어 자료를 보는 일과 직접 입력하는 연습을 연결하고, 오답과 진행을 관리한다.
로컬 상태 저장은 연습을 이어가는 기반이지만 기기 간 동기화나 학습 효과의 보증은 아니다.
오래된 구현과 현재 기능 설명도 구별해서 읽어야 한다.

## 자료 확인 범위

2026-09-27 기준 README, 루트 구성, 문장 연습 저장 파일들과 deprecated 입력 훅을 확인했다.
웹 앱 설치·실행이나 학습 기록 생성은 하지 않았고 기억 곡선 기반 추천의 효과도 측정하지 않았다.

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

[1] zyronon/TypeWords — README.md

<https://github.com/zyronon/TypeWords/blob/master/README.md>

[2] zyronon/TypeWords — app/composables/practice-sentences/practice-sentence-cache.ts

<https://github.com/zyronon/TypeWords/blob/master/app/composables/practice-sentences/practice-sentence-cache.ts>

[3] zyronon/TypeWords — app/composables/practice-sentences/usePracticeSentencePersistence.ts

<https://github.com/zyronon/TypeWords/blob/master/app/composables/practice-sentences/usePracticeSentencePersistence.ts>

[4] zyronon/TypeWords — app/composables/practice-sentences/useSentenceTypingFlow.ts

<https://github.com/zyronon/TypeWords/blob/master/app/composables/practice-sentences/useSentenceTypingFlow.ts>
