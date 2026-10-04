---
title: "firebase/firebase-ios-sdk"
repository: "firebase/firebase-ios-sdk"
url: "https://github.com/firebase/firebase-ios-sdk"
category: "backend-infra"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# firebase/firebase-ios-sdk

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

firebase-ios-sdk는 Apple 앱에서 인증, 데이터 저장, 메시징 같은 Firebase 기능을 쓰도록 연결하는 SDK, 즉 앱 개발용 라이브러리 모음이다.
이름에 iOS가 있지만 여러 Apple 플랫폼을 다루며, Firebase 서버 전체를 자체 운영하는 패키지는 아니다.[1]

## 앱 안의 코드와 서비스의 경계

앱에 로그인이나 공유 데이터 기능을 붙일 때 화면만 만들어서는 다른 기기와 상태를 나눌 수 없다.
이 저장소는 그 연결을 담당하는 Apple 쪽 코드를 제공한다.
`FirebaseAuth`, `FirebaseFirestore`, `FirebaseStorage`, `FirebaseMessaging` 등 제품별 라이브러리가 나뉘므로 모든 기능을 하나의 덩어리로 이해하기보다 필요한 서비스와 클라이언트의 관계를 먼저 보는 편이 명확하다.[1]

공개 범위에도 예외가 있다.
README는 `FirebaseAnalytics`를 제외한 Apple 플랫폼 Firebase 라이브러리의 소스를 포함한다고 설명한다.
Analytics는 오픈소스가 아니며 Swift Package Manager나 CocoaPods 경로에서 사전 컴파일된 바이너리를 제공한다.
따라서 패키지로 받을 수 있다는 사실과 내부 소스를 검토할 수 있다는 사실은 다르다.[1]

## 예제로 보는 구성의 역할

전체 제품을 나열하는 대신 저장소의 FirestoreSample을 읽으면 연결 과정을 좁혀 볼 수 있다.
Firestore는 여기서 문서 형태의 데이터를 조회하는 서비스이고, 이 예제는 `@FirestoreQuery` 사용법을 보여주도록 작성돼 있다.
예제 README는 로컬 Firebase Emulator Suite와 iOS Simulator가 통신하는 경로를 안내한다.
에뮬레이터는 실제 서비스 대신 개발 중 동작을 확인하도록 사용하는 로컬 실행 환경이다.[2]

앱 진입점은 `FirebaseApp.configure()`를 호출하고 Firestore 주소를 `localhost:8080`으로 바꾼다.
이어서 영속 저장과 SSL을 끈다.
이는 해당 로컬 예제의 설정이지 모든 Firebase 앱의 권장 설정이라는 뜻이 아니다.
연결 주소와 보안 설정을 함께 읽어야 샘플이 실제 클라우드 데이터를 읽는 코드라고 오해하지 않는다.[3]

화면의 `Fruit` 구조체는 이름과 즐겨찾기 여부를 담고 `@DocumentID`로 문서 식별자를 연결한다.
`Codable`은 저장된 데이터를 Swift 값으로 바꾸는 규약이다.
`@FirestoreQuery`는 `fruits` 컬렉션, 즉 과일 문서가 모인 위치를 조회하고 결과를 `Result<[Fruit], Error>`로 받는다.
화면은 성공이면 이름 목록을 표시하고 변환 실패이면 오류 문구를 보여준다.[4]

## 예시로 따라가는 흐름

이해를 위해 로컬 에뮬레이터의 `fruits`에 이름과 `isFavourite` 값이 있는 과일 문서들을 준비했다고 가정하자.
이는 원고에서 삽입한 데이터가 아니라 공식 화면 코드를 설명하기 위한 입력 가정이다.
앱이 시작되면 초기화 코드가 로컬 Firestore로 연결하고, 화면은 `isFavourite`가 `true`인 문서만 요청한다.
정상적으로 Swift의 `Fruit` 배열로 변환되면 사용자는 즐겨찾기 과일 이름 목록을 보게 된다.[3][4]

도구 모음 버튼을 누르면 `toggleFilter()`가 상태를 뒤집는다.
즐겨찾기만 보려는 상태에서는 조건을 다시 넣고, 전체를 보려는 상태에서는 조건 목록을 비운다.
새 조회 결과를 같은 목록 화면이 표현한다.
따라서 핵심은 버튼이 데이터베이스의 즐겨찾기 값을 수정하는 것이 아니라 조회 조건을 바꾼다는 점이다.
이 화면에는 문서를 생성하거나 즐겨찾기를 저장하는 쓰기 코드가 없다.[4]

사람이 확인할 부분은 화면에 값이 나오는지뿐만이 아니다.
문서의 필드 이름과 자료형이 `Fruit`에 맞는지, 오류 분기가 표시되는지, 연결이 정말 로컬 에뮬레이터인지 구분해야 한다.
샘플의 규칙 파일은 모든 문서에 대해 `allow read, write: if true;`로 읽기와 쓰기를 허용한다.
이 규칙을 공개 서비스로 옮기면 사용자별 권한을 제한하지 못하므로, 예제의 편의 설정과 실제 서비스의 접근 규칙을 분리해서 검토해야 한다.[3][4][5]

## 배포 경로와 플랫폼 조건

자료 확인 시점의 README에는 2026년 10월 이후 새 버전을 CocoaPods에 게시하지 않는다는 경고가 있다.
기존 버전의 설치는 계속 가능하다고 설명하지만, 오래된 설치 예제를 따라가는 것과 이후 SDK 업데이트를 받는 것은 다른 문제다.
Swift Package Manager 설치 안내와 CocoaPods 전환 안내를 함께 확인해야 한다.[1]

여러 Apple 플랫폼이 같은 수준으로 지원되는 것도 아니다.
README는 macOS·Catalyst·tvOS를 공식 베타, visionOS·watchOS를 커뮤니티 지원으로 구분한다.
특히 watchOS Crashlytics에는 mach exception과 signal crash를 기록하지 못하는 제한이 명시돼 있다.
`FirebaseCombineSwift`도 개발 중이며 production 용도로 지원되지 않는다고 적혀 있으므로 이름만 보고 일반 제품과 같은 보장을 기대하면 안 된다.[1]

## 라이선스와 비용을 나누어 읽기

저장소의 기본 라이선스는 Apache-2.0이고, LICENSE는 재배포 조건과 무보증 조항을 담는다.
README는 Firebase 서비스 이용에는 별도의 서비스 약관이 적용된다고 명시한다.
소스 이용 허락이 서비스 운영비까지 없애 주는 것은 아니다.
이 조사에서는 각 제품의 요금표나 무료 한도를 확인하지 않았으므로 인증·저장·전송·외부 모델 사용을 모두 무료라고 판단하지 않는다.[1][6]

또 로컬 에뮬레이터 예제만 읽어서는 운영 프로젝트의 인증 상태, 데이터 접근 규칙, 네트워크 전송 범위나 과금 설정을 검증할 수 없다.
예제의 SSL 비활성화와 전면 허용 규칙은 오히려 로컬 학습 경로를 실제 서비스 설정과 구분해야 하는 구체적인 이유다.[2][3][5]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/README.md)

   제품 목록보다 먼저 공개 소스의 예외와 설치 경로 경고를 읽는다.
   이후 목표 플랫폼이 공식 지원인지 베타·커뮤니티 지원인지 확인하면 같은 SDK 이름 아래의 차이를 놓치지 않는다.
2. [FirestoreSample 안내](https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/README.md)와 [앱 초기화 코드](https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/FirestoreSample/App/FirestoreSampleApp.swift)

   예제가 어디에 연결되는지부터 확인한다.
   로컬 에뮬레이터 사용, 포트, SSL·영속 저장 설정을 연결해 읽으면 실제 클라우드 접근과 예제 실행의 경계가 드러난다.
3. [과일 목록 화면](https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/FirestoreSample/Views/FavouriteFruitsView.swift)과 [예제 접근 규칙](https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/firestore.rules)

   데이터 모델, 조회 조건, 성공·오류 분기, 버튼 동작 순서로 따라간다.
   화면에 쓰기 기능이 없다는 사실과 서버 규칙이 쓰기까지 허용한다는 사실을 따로 확인한다.

## 자료 확인 범위

2026-10-04, 고정 커밋 `030f8b19e14c75f3b52baf9cfd544ad833a4eb16`의 README, FirestoreSample 설명·초기화·화면·규칙과 LICENSE를 읽었다.
설치, 앱·에뮬레이터 실행, Firebase 프로젝트 생성, 성능·보안·과금 검증은 하지 않았다.
전체 SDK 구현을 감사한 결과가 아니라 대표 예제를 통한 읽기용 설명이다.

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

[1] firebase/firebase-ios-sdk — README.md

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/README.md>

[2] firebase/firebase-ios-sdk — Example/FirestoreSample/README.md

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/README.md>

[3] firebase/firebase-ios-sdk — Example/FirestoreSample/FirestoreSample/App/FirestoreSampleApp.swift

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/FirestoreSample/App/FirestoreSampleApp.swift>

[4] firebase/firebase-ios-sdk — Example/FirestoreSample/FirestoreSample/Views/FavouriteFruitsView.swift

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/FirestoreSample/Views/FavouriteFruitsView.swift>

[5] firebase/firebase-ios-sdk — Example/FirestoreSample/firestore.rules

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/Example/FirestoreSample/firestore.rules>

[6] firebase/firebase-ios-sdk — LICENSE

<https://github.com/firebase/firebase-ios-sdk/blob/030f8b19e14c75f3b52baf9cfd544ad833a4eb16/LICENSE>
