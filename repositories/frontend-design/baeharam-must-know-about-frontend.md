---
title: "baeharam/Must-Know-About-Frontend"
repository: "baeharam/Must-Know-About-Frontend"
url: "https://github.com/baeharam/Must-Know-About-Frontend"
category: "frontend-design"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "frontend-design"
  - "starred-draft"
---

# baeharam/Must-Know-About-Frontend

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Must-Know-About-Frontend는 프론트엔드 취업 준비 과정에서 접할 기초 개념과 질문을 한국어로 정리한 학습 노트 모음이다.
설치해서 사용하는 프레임워크가 아니라, README의 목차에서 주제별 Markdown 문서로 이동하며 읽는 자료다.[1]

## 무엇을 만들기보다 무엇을 설명할 수 있는지 묻는다

작성자는 실제 면접 질문과 검색으로 찾은 필수 지식을 정리했다고 밝힌다.
범위는 컴퓨터공학 전체가 아니라 프론트엔드에 초점을 맞추며, 너무 얕거나 깊지 않은 수준을 의도한다.
이것은 저자가 정한 학습 범위이지 모든 채용 과정에 공통으로 적용되는 공식 시험 범위는 아니다.[1]

README는 프론트엔드 전반, HTML, CSS, JavaScript, 네트워크, 보안으로 나뉜다.
HTML은 페이지의 내용과 구조, CSS는 표현, JavaScript는 동작을 다루는 축으로 이해하면 목차가 읽힌다.
그 위에 브라우저가 화면을 만드는 과정, 서버와 주고받는 통신, 웹 보안 주제가 연결되는 구성이다.[1]

목록에는 화면을 클라이언트와 서버 중 어디서 만드는지 비교하는 CSR/SSR, 브라우저 렌더링, 박스 모델, 클로저, HTTP, 동일 출처 정책 등이 있다.
README만 보면 질문 목록이지만, 각 링크는 `Notes/` 아래의 별도 설명으로 연결된다.
따라서 한 번에 전부 외우는 순서보다 서로 관련된 장을 연결해서 읽는 방식으로 접근할 수 있다.[1]

## 대표 문서에서 확인한 설명 방식

브라우저 렌더링 문서는 HTML을 읽어 DOM을 만들고, CSS로 CSSOM을 만든 다음 화면에 그릴 구조와 위치를 계산하는 흐름을 설명한다.
DOM은 브라우저가 HTML 요소를 다루기 위해 만든 객체 구조이며, CSSOM은 스타일 규칙을 다루는 구조다.
Layout은 요소의 크기와 위치를 계산하는 단계, Paint는 그 계산을 바탕으로 화면에 그리는 단계로 소개된다.[2]

이 장은 용어 정의에서 끝나지 않는다.
빨간 배경의 `body`, 파란색 `div`, 클릭하면 `Click div`를 출력하는 JavaScript를 함께 싣고, 작성자가 관찰한 개발자 도구의 흐름을 설명한다.
이 원고가 직접 관찰한 결과가 아니라 원문에 있는 코드와 설명이다.
독자는 HTML·CSS·JavaScript가 하나의 화면으로 이어지는 관계를 코드에 대응시켜 볼 수 있다.[2]

이벤트 위임 문서는 먼저 버블링과 캡처링을 설명한다.
버블링은 이벤트가 발생한 하위 요소에서 상위 요소 쪽으로 전달되는 흐름이며, 캡처링은 상위에서 대상 요소로 내려가는 흐름이다.
핸들러는 이벤트가 왔을 때 실행할 함수다.
개별 요소에 함수를 다는 예제와 상위 요소 하나에 처리 함수를 두는 예제를 나란히 보여주는 것이 이 장의 핵심이다.[3]

## 예시로 따라가는 흐름

구체적인 읽기 사례는 “목록 항목을 눌렀는데 왜 바깥 요소의 처리 함수도 실행되는가?”라는 질문에서 시작할 수 있다.
README의 JavaScript 목차에서 이벤트 위임 장으로 이동하면 `div` 안에 `ul`, 그 안에 `li`를 둔 예제가 나온다.
각 요소에 클릭 핸들러를 붙이고 `li`를 클릭하는 입력에 대해, 원문은 `li 클릭`, `ul 클릭`, `div 클릭` 순서의 출력을 제시한다.
이를 버블링의 설명과 대조하면 클릭한 요소와 이벤트를 전달받는 요소가 같지 않을 수 있다는 점을 읽을 수 있다.[1][3]

다음에는 `ul`의 핸들러에 `{ capture: true }`를 추가한 예제를 비교한다.
원문에 제시된 출력은 `ul 클릭`, `li 클릭`, `div 클릭`으로 바뀐다.
마지막 예제는 각 `li`가 아니라 `ul` 하나에 `onclick`을 둔다.
여기서 얻을 결과는 새 라이브러리 설치물이 아니라, 왜 부모 요소에서 여러 자식의 이벤트를 처리할 수 있는지 설명하는 문장이다.[3]

사람이 확인할 질문도 나눌 수 있다.
“어느 핸들러에 캡처 옵션을 붙였나?”, “클릭 대상은 무엇인가?”, “모든 핸들러를 지운 것과 부모 하나에 모은 것은 어떻게 다른가?”를 코드에 표시해 보는 식이다.
원문 마지막 예제는 이벤트 종류를 알리는 간단한 형태이므로, 어떤 목록 항목을 눌렀는지 구분하는 완성된 목록 UI라고 해석하지 않는다.
여기서 설명한 출력은 원문에 실린 예시이며 이 원고에서 브라우저를 열어 재현한 실행 결과는 아니다.[3]

## 연관 주제로 이동하는 읽기 순서

화면 구성부터 이해하려면 렌더링 장에서 DOM·CSSOM·Layout·Paint의 역할을 구분한 뒤 HTML 목차의 `script` 장으로 넘어갈 수 있다.
이 장은 일반 스크립트와 `async`, `defer`를 HTML 파싱과 연관 지어 비교한다.
파싱은 입력 문서를 읽어 브라우저가 다룰 구조로 바꾸는 작업이다.
여기서는 네트워크로 파일을 가져오는 시점과 코드를 실행하는 시점을 따로 표시하며 읽는 것이 중요하다.[2][4]

다만 해당 `script` 장은 `defer` 설명에서 “로드”라는 말을 여러 의미로 사용하고, `<body>` 직전에 넣는 방식을 권하는 짧은 설명을 싣고 있다.
그 권고를 모든 브라우저와 스크립트 유형에 통하는 최신 표준 지침으로 확대하지 않는다.
이 원고는 해당 장의 문구를 현재 웹 표준과 대조 검증하지 않았으므로, 세부 동작을 구현에 적용하기 전 별도 확인이 필요하다.[4]

이벤트 처리로 관심이 이동했다면 렌더링 장의 `div` 클릭 예제와 이벤트 위임 장의 중첩 목록 예제를 연결해서 읽을 수 있다.
앞의 예제는 요소를 선택해 함수를 붙이는 모습이고, 뒤의 예제는 이벤트가 여러 요소 사이를 어떻게 지나가는지 설명한다.
같은 `addEventListener`를 보더라도 두 문서가 답하는 질문은 다르다.[2][3]

## 학습 노트와 정답집의 경계

README는 개인적으로 정리한 내용에 틀린 부분이 있을 수 있으며 PR과 이슈를 받는다고 명시한다.
따라서 이 자료는 질문과 개념을 찾는 출발점이지 표준 명세나 브라우저 구현을 대신하는 최종 근거는 아니다.
특정 프레임워크의 최신 사용법을 포괄하는 문서라고도 소개하지 않는다.[1]

대표 장들에는 별도의 참고 링크가 있다.
설명에 의문이 생기면 저자의 요약과 참고 원문을 구분하여 따라갈 수 있지만, 링크가 실려 있다는 것만으로 그 외부 자료를 이 원고가 검증한 것은 아니다.
또 이벤트 위임 장의 성능상 이점은 저자의 설명이며, 특정 페이지의 메모리 사용량이나 실행 속도를 측정한 보증으로 읽지 않는다.[2][4][3]

## 직접 읽어볼 자료

1. [README와 전체 목차](https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/README.md)

   소개의 범위와 오류 가능성 안내를 먼저 읽는다.
   이후 자신이 설명하기 어려운 질문 하나를 골라 관련 장으로 이동하면 단순한 목록 암기와 구분할 수 있다.
2. [브라우저 렌더링 원리](https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/frontend/browser-rendering.md)

   단계 이름을 외우기 전에 HTML·CSS·JavaScript 예제의 역할을 대응시킨다.
   작성자의 관찰 설명과 자신이 아직 확인하지 않은 동작도 구분해 둔다.
3. [이벤트 위임](https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/javascript/event-delegation.md)

   같은 중첩 구조에서 핸들러 옵션만 달라졌을 때 원문 출력이 어떻게 바뀌는지 읽는다.
   마지막 부모 핸들러 예제가 보여주는 범위와 생략한 UI 처리를 나눈다.
4. [script, async, defer 비교](https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/html/script-tag-type.md)

   파일을 가져오는 단계와 실행 단계를 따로 표시한다.
   짧게 압축된 설명의 전제와 현재 표준에서 다시 확인할 부분을 찾는 용도로 읽는다.

## 자료 확인 범위

2026-10-04 고정 커밋 `8dee553abd9f1268f1b70ee793bdf8209f66ec74`의 README와 렌더링, 스크립트 태그, 이벤트 위임 문서를 읽었다.
전체 장의 정확성 검토, 외부 참고 문헌의 재검증, 설치·실행·성능 검증은 하지 않았다.
자료 속 작성자의 경험을 이 원고 작성자의 경험으로 바꾸지 않았다.

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

[1] baeharam/Must-Know-About-Frontend — README.md

<https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/README.md>

[2] baeharam/Must-Know-About-Frontend — Notes/frontend/browser-rendering.md

<https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/frontend/browser-rendering.md>

[3] baeharam/Must-Know-About-Frontend — Notes/javascript/event-delegation.md

<https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/javascript/event-delegation.md>

[4] baeharam/Must-Know-About-Frontend — Notes/html/script-tag-type.md

<https://github.com/baeharam/Must-Know-About-Frontend/blob/8dee553abd9f1268f1b70ee793bdf8209f66ec74/Notes/html/script-tag-type.md>
