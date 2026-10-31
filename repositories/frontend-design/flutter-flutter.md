---
title: "flutter/flutter"
repository: "flutter/flutter"
url: "https://github.com/flutter/flutter"
category: "frontend-design"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["frontend-design", "starred-draft"]
---

# flutter/flutter

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Flutter는 Dart로 모바일·웹·데스크톱 화면을 만드는 UI 개발 SDK다.
앱 화면과 상호작용을 구현하는 도구이며, 서버나 데이터 저장 서비스를 자동으로 제공하는 완성형 서비스는 아니다.[1]

## 여러 화면을 하나의 구성 방식으로 표현한다

각 플랫폼마다 화면을 별도로 작성하면 공통 디자인과 동작을 맞추는 작업이 반복된다.
Flutter는 하나의 코드베이스로 여러 플랫폼을 대상으로 삼고 Material·Cupertino 위젯과 사용자 정의 화면 요소를 제공한다.[1]
위젯은 버튼·레이아웃처럼 화면 일부를 설명하는 구성 단위다.
`packages/flutter/lib/src/widgets/framework.dart`는 Widget을 변경 불가능한 UI 설명으로 정의하고, 실제 트리 관리는 Element가 맡는다고 설명한다.[2]

## 상태가 변하면 화면 설명을 다시 만든다

같은 파일에서 StatefulWidget의 변경 가능한 값은 별도 State 객체에 둔다.
즉 버튼을 누를 때 기존 위젯 설명을 직접 수정하기보다, 상태를 바꾸고 그 상태에 맞는 화면을 다시 구성하는 방식이다.[2]
`setState` 구현은 전달된 콜백을 실행한 다음 `markNeedsBuild()`를 호출한다.
이 메서드는 새 화면 구성이 필요하다는 사실을 프레임워크에 알리는 연결점이다.[2]

Dart는 앱의 로직과 화면 구성을 표현하고, Flutter는 위젯·렌더링 계층을 제공한다.
README는 그래픽 처리에 Skia와 Impeller를 소개하고, 네이티브 기능 연결에 FFI와 platform channels를 안내한다.
FFI는 다른 언어로 만든 함수를 부르는 통로이며, platform channel은 플랫폼별 기능과 메시지를 주고받는 통로다.[1]
따라서 공통 UI 코드가 있다는 것과 모든 장치 기능이 플랫폼 구분 없이 동작한다는 것은 다르다.

## 가상 예시: 개인 독서 목록 앱

읽은 책 목록과 완료 표시 버튼을 가진 앱을 만든다고 가정하자.
개발자는 목록 데이터를 State에 두고, 현재 값에서 목록 위젯을 구성한다.
버튼을 누르면 해당 책의 완료 값을 `setState` 콜백 안에서 바꾸고, 프레임워크가 다시 구성하도록 알린다.[2]
README의 hot reload는 코드를 바꾼 뒤 앱 상태를 유지하며 결과를 살펴보는 개발 기능이다.[1]
책 데이터를 다음 실행에도 남기려면 별도의 저장 방법을 설계해야 한다.
이 예시는 화면 갱신을 설명하기 위한 것이며, 앱 제작이나 장치별 실행을 수행한 기록은 아니다.

## 생명주기와 배포 환경의 주의점

화면이 제거된 뒤 타이머나 네트워크 요청이 완료될 수 있다.
`setState`의 진단 코드는 `dispose` 이후 호출을 오류로 설명하며, 타이머·구독 해제나 `mounted` 확인을 안내한다.[2]
또한 상태 변경 콜백이 Future를 반환하면 안 된다는 진단이 있다.
비동기 작업과 화면 상태 갱신을 구분하는 이유를 이 구현에서 읽을 수 있다.[2]

README는 GitHub에서 가져온 SDK를 처음 실행하거나 업그레이드할 때 Dart SDK 등 자원을 내려받을 수 있다고 안내한다.
소스만 받았다고 모든 빌드 준비가 끝나는 것은 아니며, 대상 플랫폼의 설치 안내도 별도로 확인해야 한다.[1]
루트 LICENSE는 저작권·조건 보존과 이름을 이용한 무단 보증 금지 등을 명시하는 BSD 3-Clause 조건이다.
외부 패키지의 라이선스나 앱 콘텐츠 권리까지 이 파일 하나로 판단할 수는 없다.[3]

## 직접 읽어볼 자료

- [1] README의 About Flutter에서 UI 구성, hot reload, 네이티브 연동의 역할을 나눠 읽는다.
- [2] `framework.dart`의 Widget·StatefulWidget 설명부터 읽고 `setState`로 이동한다.
- [2] `dispose` 이후 호출과 비동기 콜백 진단은 화면 생명주기 오류를 이해하는 출발점이다.
- [3] LICENSE의 배포 조건을 확인한 뒤 사용할 패키지의 조건을 따로 확인한다.

## 자료 확인 범위

2026-10-28 공개 README·루트 구조·위에 인용한 코드 또는 문서를 확인했다.
설치·프로그램 실행·성능 측정은 하지 않았다.

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

[1] flutter/flutter — README.md

<https://github.com/flutter/flutter/blob/master/README.md>

[2] flutter/flutter — packages/flutter/lib/src/widgets/framework.dart

<https://github.com/flutter/flutter/blob/master/packages/flutter/lib/src/widgets/framework.dart>

[3] flutter/flutter — LICENSE

<https://github.com/flutter/flutter/blob/master/LICENSE>
