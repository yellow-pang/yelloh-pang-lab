---
title: "cs341-illinois/coursebook"
repository: "cs341-illinois/coursebook"
url: "https://github.com/cs341-illinois/coursebook"
category: "system-design"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "system-design"
  - "starred-draft"
---

# cs341-illinois/coursebook

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Coursebook은 C 언어를 통해 프로그램과 운영체제가 어떻게 상호작용하는지 배우는 공개 시스템 프로그래밍 교재이며, Illinois CS 341 수업에서 사용된다.
실행 도구나 문제 풀이 모음이 아니라 설명·코드·그림·확인 질문을 함께 읽는 책이다.[1]

## 어떤 기초를 연결하는 책인가?

시스템 프로그래밍은 화면에 보이는 기능 뒤에서 메모리, 파일, 실행 중인 프로그램과 통신을 다루는 일이다.
이 책은 프로그래밍 언어 수업을 이수했고 어셈블리 명령에 익숙한 독자를 가정하며, 코드와 설명은 C를 중심으로 한다.
따라서 ‘입문 교재’라는 소개를 프로그래밍을 처음 접하는 사람을 위한 문법책이라는 뜻으로 받아들이면 진입 난도를 잘못 판단하기 쉽다.[1]

README는 원래의 공개 wikibook을 바탕으로 사실 근거, 각주, 추가 읽을거리와 용어 설명을 보강하려는 목적을 밝힌다.
PDF뿐 아니라 HTML·Wiki·EPUB 링크도 제공하므로 책을 읽기 위해 LaTeX 환경부터 설치할 필요는 없다.
다만 README에 연결된 최신 배포본과 이 원고의 고정 커밋 자료가 완전히 같은 내용인지는 확인하지 않았다.[1]

## 목차를 읽기 경로로 바꾸기

실제 장 순서는 `order.yaml`에 적혀 있다.
도입과 배경 뒤에 C 입문, 프로세스, 메모리 할당, 스레드, 동기화, 교착상태가 이어진다.
후반에는 프로세스 사이의 통신, 스케줄링, 네트워킹, 파일시스템, 시그널과 보안이 놓인다.
프로세스는 실행 중인 프로그램의 단위이고, 스레드는 그 안에서 실행 흐름을 나누는 단위로 구분하며 읽을 수 있다.[2]

처음 읽는 경로는 `introc/introc`에서 C 표현을 익힌 뒤 `processes/processes`로 넘어가는 것이다.
이번에 실제 내용을 확인한 Processes 장은 파일 디스크립터, 프로세스의 메모리, `fork`, 기다리기, `exec`, 결합 패턴, 연습문제 순으로 전개된다.
파일 디스크립터는 프로그램이 열린 파일이나 입출력 대상을 가리킬 때 쓰는 번호다.
함수 이름만 외우기보다 ‘무엇을 복제하고 무엇을 바꾸는가’를 중심 질문으로 삼으면 각 절이 연결된다.[2][3]

원고의 구성도 책과 배포 도구를 구분한다.
장별 원문은 `.tex` 파일이며, 기여 안내는 `order.yaml`을 읽어 Markdown을 만드는 Python 스크립트와 PDF 빌드를 별도로 설명한다.
독자에게 필요한 것은 장을 고르는 일이고, 문서를 수정·재배포하는 사람에게 필요한 것은 그 생성 경로다.
빌드 명령을 학습의 첫 단계로 삼을 이유는 없다.[4]

## 예시로 따라가는 흐름

Processes 장의 `The fork-exec-wait Pattern`을 읽는 상황을 보자.
입력은 사용자가 프로그램에 넣는 데이터가 아니라, 자식 프로세스에서 `/bin/ls`를 실행하고 부모가 그 종료를 기다리는 C 예제다.
먼저 `fork()`의 반환값을 기준으로 종이에 세 갈래를 그린다.
음수이면 생성 실패, 양수이면 부모, 0이면 자식이라는 구분이다.
부모는 반환받은 자식의 식별 번호를 `waitpid`에 넘기고, 자식은 `execl`로 자신이 실행할 프로그램을 바꾼다.[3]

이때 `exec`는 자식 하나를 더 만드는 함수가 아니다.
기존 프로세스의 실행 내용을 지정한 프로그램으로 교체한다.
따라서 성공하면 그 뒤의 원래 코드를 계속 실행하지 않으며, 예제의 `exit(1)`은 교체가 실패해 돌아왔을 때를 위한 경로다.
예제가 의도한 결과는 자식에서 파일 목록을 출력한 뒤 부모가 기다림을 끝내는 흐름이다.
구체적인 파일 목록은 실행 디렉터리마다 달라지므로 이 글에서 출력을 만들어 제시하지 않는다.[3]

사람이 확인할 지점은 출력 여부보다 제어 흐름이다.
‘누가 `waitpid`를 호출하는가’, ‘`execl` 뒤에 도달했다면 무엇이 실패했는가’, ‘종료 상태 정수와 실제 종료 코드는 같은가’를 순서대로 묻는다.
같은 장의 Exit statuses 절은 `WIFEXITED`로 정상 종료를 확인한 뒤에만 `WEXITSTATUS`로 코드를 읽으라고 설명한다.
마지막 연습문제도 정상 종료와 시그널 종료를 구분하는 함수를 요구한다.
이렇게 예제와 확인 질문을 연결해야 함수 호출 순서 암기에서 상태 해석으로 넘어갈 수 있다.[3]

## 짧은 코드에 숨은 조건도 읽기

같은 장에는 줄바꿈 없이 `printf`를 호출한 다음 `fork`하는 예제가 있다.
출력 함수 호출은 한 번이지만, 아직 화면으로 보내지 않은 출력 버퍼가 복제되면 같은 텍스트가 두 번 나올 수 있다고 설명한다.
버퍼는 내보내기 전에 데이터를 임시로 모아 두는 공간이다.
이 사례는 ‘코드를 몇 번 지났는가’와 ‘나중에 무엇이 출력되는가’가 다를 수 있음을 보여준다.[3]

따라서 예제는 완성된 응용 프로그램이 아니라 특정 조건을 드러내는 읽기 자료로 다룬다.
어떤 호출의 오류를 생략했는지, 출력 버퍼가 언제 비워지는지, 부모와 자식이 어떤 자원을 이어받는지를 확인해야 한다.
장은 더 자세한 규칙을 함수별 매뉴얼과 POSIX 문서에서 확인하라고 안내한다.
POSIX는 여러 운영체제에서 프로그램이 공통으로 사용할 인터페이스를 정한 표준이다.[3]

## 주의점과 재사용 조건

프로세스 생성 예제를 무심코 반복 실행하면 시스템의 자원을 고갈시킬 수 있다.
장은 fork bomb의 위험을 경고하고 연습문제에도 만들되 실행하지 말라고 적는다.
프로세스 수 제한이나 복구 설명이 있다고 해서 공유 머신에서 위험한 예제를 시험해도 된다는 뜻은 아니다.
이 원고는 해당 코드를 실행하지 않고 분기와 상태만 읽었다.[3]

공개 자료라는 사실과 재사용 조건이 없는 것은 다르다.
라이선스 안내는 소프트웨어 기여분을 University of Illinois/NCSA, PDF·Markdown 산출물의 안내를 CC BY 4.0으로 구분한다.
원래 wikibook 자료에는 CC BY 3.0 안내가 따로 남아 있다.
전체를 하나의 코드 라이선스로 뭉뚱그리지 말고 복사할 부분과 저작자 표시 조건을 확인해야 한다.[5][6]

PDF 라이선스 본문은 저작자·라이선스 안내를 유지하고 수정했다면 그 사실을 표시하도록 한다.
책의 설명이나 그림을 학습 노트에 재사용할 때에도 출처와 수정 여부를 남기는 문제가 포함된다.[7]

## 직접 읽어볼 자료

1. [README와 배포본 안내](https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/README.md)

   대상 독자와 선행 지식부터 확인한다.
   단순 열람에는 PDF나 HTML을 선택하고, 소스 수정 환경이 필요한 상황과 구분한다.

2. [실제 장 순서](https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/order.yaml)

   C 입문 다음에 프로세스와 메모리 할당이 놓이는 순서를 확인한다.
   궁금한 주제가 뒤쪽에 있더라도 어떤 기초 장을 먼저 읽을지 정하는 지도다.

3. [Processes 장 원문](https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/processes/processes.tex)

   `Fork Functionality`의 반환값, `Exit statuses`의 조건, `The fork-exec-wait Pattern`을 연결한다.
   마지막 질문에 자신의 말로 답할 수 있는지 확인하고, 모르는 규칙만 해당 매뉴얼로 확장한다.

4. [라이선스 구분 안내](https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/LICENSE/README.md)

   코드, 원래 교재, 생성 산출물이 나뉘는 이유를 확인한다.
   번역이나 그림 인용을 하기 전에 연결된 개별 라이선스의 표시 조건을 읽는다.

## 자료 확인 범위

2026-10-04 기준 커밋 `442c258aa42f498ccfc2f793f9f55bcbcd2d8f91`의 README, 장 순서, Processes 장의 대표 예제·주의문·질문, 기여 안내와 라이선스 자료를 확인했다.
다른 장 전체의 정확성을 검토한 것은 아니다.
교재 빌드 도구 설치, C 예제 실행, 배포본 대조 및 성능 검증은 하지 않았다.

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

[1] cs341-illinois/coursebook — README.md

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/README.md>

[2] cs341-illinois/coursebook — order.yaml

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/order.yaml>

[3] cs341-illinois/coursebook — processes/processes.tex

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/processes/processes.tex>

[4] cs341-illinois/coursebook — CONTRIBUTING.md

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/CONTRIBUTING.md>

[5] cs341-illinois/coursebook — LICENSE/README.md

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/LICENSE/README.md>

[6] cs341-illinois/coursebook — LICENSE/LICENSE.original

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/LICENSE/LICENSE.original>

[7] cs341-illinois/coursebook — LICENSE/LICENSE.output

<https://github.com/cs341-illinois/coursebook/blob/442c258aa42f498ccfc2f793f9f55bcbcd2d8f91/LICENSE/LICENSE.output>
