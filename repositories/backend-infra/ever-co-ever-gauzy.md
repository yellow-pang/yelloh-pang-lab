---
title: "ever-co/ever-gauzy"
repository: "ever-co/ever-gauzy"
url: "https://github.com/ever-co/ever-gauzy"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# ever-co/ever-gauzy

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Ever Gauzy는 프로젝트, 고객, 인력, 시간과 재무 기록을 하나의 API와 여러 화면에서 다루는 통합 관리 플랫폼이다.
시간을 재는 앱 하나가 아니라 웹 화면, 서버, 전체 데스크톱 앱과 시간 기록 전용 앱을 구분해서 제공하는 프로젝트다.[1]

## 흩어진 기록을 연결하는 플랫폼

개인 실습용으로 작은 프로젝트 기록을 만든다고 생각해 보자.
해야 할 일, 소요 시간, 연락처와 비용을 서로 다른 파일에 넣으면 같은 프로젝트를 기준으로 다시 맞춰야 한다.
Gauzy는 프로젝트·작업 관리와 시간표, 청구서·수입·지출 등의 영역을 한 플랫폼 범위로 제시한다.
실제 자동 정산의 정확성이나 특정 제도 적합성까지 이 소개로 보증되는 것은 아니다.[1]

README에 나오는 ERP는 자원과 재무 관리, CRM은 고객 관계 관리, HRM은 인력 관리, ATS는 지원자 채용 과정의 추적을 뜻한다.
이 명칭들을 모두 외우기보다 어떤 기록이 누구에게 속하고 어떤 화면에서 쓰이는지 보는 편이 이해하기 쉽다.
조직, 팀, 역할과 권한이 함께 등장하는 이유도 같은 데이터를 모든 사용자가 동일하게 다루지 않기 때문이다.[1]

## 화면보다 먼저 서버와 권한을 나누어 본다

Headless API는 기본 웹 화면 없이 다른 프로그램에서도 기능을 호출할 수 있는 접점이다.
Gauzy Server는 API와 데이터베이스 및 화면 제공을 묶고, Desktop App은 전체 구성을 로컬에서 사용하거나 외부 서버에 연결하는 경로를 제공한다.
Desktop Timer는 시간과 활동 기록에 집중하며 서버 연결이 필요한 별도 앱으로 안내된다.[1]

이 구분은 같은 다운로드 페이지에 있는 프로그램들이 동일한 역할을 하지 않음을 뜻한다.
관리 화면이 필요한 사람과 시간을 기록하는 사용자에게 필요한 구성은 다르다.
README의 데모 설명에서도 관리자 계정이 곧 시간을 기록할 수 있는 직원 계정은 아니라고 구분한다.
계정의 권한과 기록 주체의 모델을 분리해서 봐야 하는 이유다.[1]

실제 기능 파일인 `timer.controller.ts`에는 `/timesheet/timer` 아래 상태 조회, 시작, 종료와 전환 API가 있다.
클래스에 테넌트·권한 guard와 `TIME_TRACKER` 권한이 붙는다.
테넌트는 데이터를 구분하는 사용자 집단의 경계이며 guard는 요청이 기능에 도달하기 전 검사하는 장치다.
시간 기록이 단순한 시계 값 전송만으로 끝나지 않는다는 구조를 보여 준다.[2]

시작과 종료는 입력 자료형을 검증한 뒤 각각 `StartTimerCommand`, `StopTimerCommand`를 command bus에 전달한다.
조회는 query bus를 쓰는 경로가 있다.
즉 HTTP 요청을 받는 계층과 실제 작업 처리 계층을 나누고 있다.
이 파일은 API 입구를 설명하는 근거이며, 내부 시간 계산과 모든 권한 규칙이 정확하다고 검증한 것은 아니다.[2]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
공개 학습용 샘플 프로젝트에서 한 사용자가 문서 작성 시간을 기록한다고 가정하자.
먼저 서버와 연결할 앱, 기록할 사용자와 필요한 권한이 준비되어야 한다.
전체 Desktop App을 쓰는지 시간 기록 전용 Timer를 쓰는지에 따라 보이는 기능 범위는 달라도 서버 API라는 접점은 구분해서 볼 수 있다.[1]

시작 요청이 서버에 오면 확인한 컨트롤러는 `StartTimerDTO` 형태로 입력을 받아 검증하고 시작 명령을 전달한다.
종료 요청은 `StopTimerDTO`를 받아 종료 명령으로 넘어간다.
코드의 반환형과 설명은 종료 시 실행 중인 타이머가 없으면 `null`일 수도 있음을 보여 준다.
버튼을 눌렀다는 사실과 새 시간 로그가 실제 생성됐다는 사실은 다르므로 응답을 확인하는 단계가 필요하다.[2]

이후 사용자는 상태 조회와 시간표 등 제공되는 화면에서 기록이 의도한 대상에 연결되었는지 확인할 수 있다.
이 설명은 기능 간 관계를 이해하기 위한 것이며, 임의의 근무 시간이나 청구 금액을 계산해 결과로 제시하지 않는다.
특히 활동 기록과 스크린샷을 켜는 경우에는 기록 목적과 수집 범위를 먼저 정해야 한다.
시간 측정 기능을 이해하는 데 다른 사람의 실제 화면이나 개인정보는 필요하지 않다.[1][2]

## 데모와 운영은 같은 설정이 아니다

README는 데모 데이터베이스가 재설정된다는 점과 공개 SaaS가 Alpha·테스트 상태라는 점을 밝힌다.
데모 계정을 실제 운영 계정으로 가져다 쓰면 안 된다.
운영 안내는 인증용 비밀값과 초기 계정 암호를 고유하게 설정하도록 요구하며, 기본값을 그대로 둔 경우 서버가 시작이나 초기 데이터 생성을 거부한다고 설명한다.[1]

프록시를 거치는 구성에서는 실제 접속 주소 판단이 로그인 제한과 연결되고, 여러 API 인스턴스를 쓰면 제한 상태를 공유하는 설정도 필요하다.
이는 기능이 많은 웹 화면을 켜는 일과 안전하게 운영하는 일이 다름을 보여 준다.
본문에서는 이 설정을 적용하거나 외부 서비스를 배포하지 않았다.[1]

운영 환경에서는 REST API, GraphQL과 Socket.io를 포함한 클라이언트–서버 통신을 HTTPS/WSS/SSL로 암호화해야 한다고 안내한다.
웹 화면이 HTTPS로 열린다는 사실만 보지 말고 API 요청과 실시간 연결에도 보호된 통신이 적용되는지 구분해 확인해야 한다.[1]

라이선스 문서는 Community Edition의 AGPL v3와 별도 상용 계약을 구분한다.
수정한 버전을 네트워크 서비스로 제공할 때의 소스 공개 조건, 하위 구성 요소의 라이선스와 상표 조건도 별도로 안내한다.
소스가 공개되어 있다는 사실만으로 모든 재배포·브랜딩 방식이 제한 없이 허용된다고 보아서는 안 된다.[3]

## 직접 읽어볼 자료

- [공식 README](https://github.com/ever-co/ever-gauzy/blob/develop/README.md)
  Features를 훑은 뒤 Server & Desktop Apps를 읽는다.
  필요한 것이 전체 관리 화면인지 시간 기록 전용 화면인지 먼저 정리하면 비슷한 이름의 배포물을 혼동하지 않을 수 있다.
- [Timer API 컨트롤러](https://github.com/ever-co/ever-gauzy/blob/develop/packages/core/src/lib/time-tracking/timer/timer.controller.ts)
  권한 선언에서 시작해 시작·종료 함수와 반환형을 비교한다.
  요청 검증과 실제 명령 처리가 어디서 나뉘는지, 종료 요청에 로그가 없을 수도 있는지를 확인하는 짧은 경로다.
- [공식 라이선스 안내](https://github.com/ever-co/ever-gauzy/blob/develop/LICENSES.md)
  Community Edition과 별도 계약을 구분해서 읽는다.
  기능을 수정하거나 서비스로 제공하려면 어떤 코드 공개·고지 조건을 더 확인해야 하는지 살펴볼 수 있다.

## 정리

Gauzy는 여러 관리 기록을 서버 API와 웹·데스크톱 화면으로 연결하는 플랫폼이다.
대표 Timer 경로에서도 사용자 경계, 입력 검증과 명령 처리가 구분되며, 데모·운영 설정과 수집 정보의 범위를 별도로 판단해야 한다.[1][2]

## 자료 확인 범위

2026-09-27에 공식 README, 루트 구성, 라이선스 안내와 Timer 컨트롤러를 읽었다.
서버나 데스크톱 앱을 실행하지 않았으며 시간 집계·청구·접근 통제의 실제 결과는 시험하지 않았다.

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

## 관련 Repository

- [makeplane/plane](makeplane-plane.md)

## Sources

[1] ever-co/ever-gauzy — README.md

<https://github.com/ever-co/ever-gauzy/blob/develop/README.md>

[2] ever-co/ever-gauzy — packages/core/src/lib/time-tracking/timer/timer.controller.ts

<https://github.com/ever-co/ever-gauzy/blob/develop/packages/core/src/lib/time-tracking/timer/timer.controller.ts>

[3] ever-co/ever-gauzy — LICENSES.md

<https://github.com/ever-co/ever-gauzy/blob/develop/LICENSES.md>
