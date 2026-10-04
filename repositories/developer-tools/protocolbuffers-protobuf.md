---
title: "protocolbuffers/protobuf"
repository: "protocolbuffers/protobuf"
url: "https://github.com/protocolbuffers/protobuf"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# protocolbuffers/protobuf

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

서로 다른 언어로 만든 프로그램이 같은 자료를 주고받으려면, 값뿐 아니라 그 값의 구조도 약속해야 한다.
Protocol Buffers는 구조화된 데이터를 저장하거나 전달할 수 있는 바이트 형태로 바꾸는 직렬화 방식이며, 이 저장소는 그 형식을 다루는 컴파일러와 여러 언어의 런타임을 제공한다.
데이터베이스나 통신 서버 자체는 아니다.[1]

## 주소록을 파일로 옮길 때 필요한 약속

이름과 전화번호를 화면에 표시하는 일과, 그 주소록을 다른 프로그램이 다시 읽게 저장하는 일은 다르다.
작성하는 쪽과 읽는 쪽이 어떤 항목이 문자열인지, 사람이 여러 명 들어갈 수 있는지에 합의해야 한다.
protobuf에서는 이 합의를 `.proto` 파일로 적는다.
공식 예제의 `addressbook.proto`는 `Person`과 `AddressBook`이라는 메시지를 정의한다.
여기서 메시지는 채팅 문장이 아니라 이름 붙은 데이터 묶음이다.[2]

README는 사용에 필요한 구성요소를 둘로 나눈다. `protoc`는 `.proto` 정의를 처리하는 프로토콜 컴파일러이고, 언어별 런타임은 프로그램이 생성된 자료형을 읽고 쓰는 데 필요한 라이브러리다.
Python으로 이용한다고 해서 Python 코드만 있으면 준비가 끝나는 것은 아니다.
구조 정의를 해당 언어에서 사용할 수 있게 만드는 단계와, 실제 프로그램 안에서 값을 다루는 단계를 구분해야 한다.[1]

## 정의 파일과 프로그램이 나누어 맡는 일

공식 주소록 정의에서 `Person`에는 `name`, `id`, `email`이 있으며 각 항목에 타입과 번호가 붙는다. `string name = 1`을 읽을 때는 이름이라는 항목의 값이 문자열이고 번호가 1이라는 점을 함께 본다.
이는 이름을 임의의 순서로 텍스트에 써 넣는 방식과 다르다.
항목의 이름뿐 아니라 스키마, 즉 데이터 구조에 관한 명시적 약속을 먼저 둔다.[2]

전화번호는 사람 안에 들어가는 `PhoneNumber`라는 별도의 묶음이다.
그 안에 번호 문자열과 `PhoneType`이 있고, 종류는 `MOBILE`, `HOME`, `WORK`로 정의된다. `repeated PhoneNumber phones`는 한 사람에게 전화번호가 여러 개 연결될 수 있음을 표현한다.
바깥쪽 `AddressBook`도 `repeated Person people`로 여러 사람을 담는다.
같은 반복 규칙이 다른 층위에 적용되는 예다.[2]

정의에는 `google.protobuf.Timestamp`도 포함된다.
다만 구조에 시간이 들어갈 자리가 있다는 사실과 예제 프로그램이 그 시간을 실제로 채운다는 것은 별개다.
확인한 Python 추가 예제는 이름·ID·이메일·전화번호를 입력받지만 `last_updated`에 값을 넣는 부분은 보이지 않는다.
정의 파일만 읽고 모든 필드가 자동으로 계산되거나 채워진다고 이해하면 안 된다.[2][3]

Python 예제는 `addressbook_pb2`를 가져와 `AddressBook()`을 만든다.
기존 파일은 `ParseFromString`으로 읽고, 저장할 때는 `SerializeToString`을 사용한다.
개발자는 문자열 분리 규칙을 직접 만드는 대신 메시지 객체의 항목을 다룬다.
하지만 사람이 입력한 값이 실제 연락처인지, 같은 ID가 이미 있는지 판단하는 업무 규칙까지 이 형식이 대신 정하지는 않는다.[3]

## 예시로 따라가는 흐름

공식 `add_person.py`를 코드 순서대로 따라가 보자.
이는 문서와 소스의 흐름을 설명하는 것이며 직접 실행한 결과가 아니다.
프로그램의 입력은 주소록 파일 경로 하나와 사용자가 입력하는 연락처다.
먼저 새 `AddressBook` 객체를 만든 뒤, 지정한 파일이 있으면 바이너리 모드로 읽어 메시지로 복원한다.
파일 읽기에서 `IOError`가 발생하면 새 파일을 만든다는 안내를 출력하도록 되어 있다.[3]

다음으로 `address_book.people.add()`로 사람 하나를 추가하고 ID와 이름을 입력받는다.
이메일은 빈 입력이면 건너뛰고, 전화번호는 빈 문자열이 들어올 때까지 반복해서 추가한다.
전화번호 종류가 mobile·home·work 중 무엇인지에 따라 스키마의 열거형 값을 지정한다.
모르는 종류가 입력되면 예제는 기본값을 남긴다고 안내한다.
입력 문자열이 곧바로 완전한 데이터 검증을 통과했다는 의미는 아니다.[2][3]

마지막 단계는 변경된 주소록 전체를 `SerializeToString`으로 바꾸어 같은 경로에 쓰는 것이다.
결과물은 사람이 읽는 연락처 목록 화면이 아니라 직렬화된 주소록 파일이다.
독자가 확인할 부분은 기존 사람들을 읽은 다음 새 사람을 더하는 순서, 전화번호의 반복 구조, 그리고 저장 대상이 원래 파일과 같다는 점이다.
이 예제는 구조 정의와 읽기·수정·쓰기의 연결을 보여주지만, 동시 편집이나 안전한 백업 절차를 갖춘 주소록 제품을 제시하지는 않는다.[3]

## 형식이 해결하는 것과 남겨두는 것

protobuf의 언어 중립성은 모든 언어 구현이 이 저장소의 같은 위치에 있다는 뜻이 아니다.
README는 C++·Java·Python 등의 소스 경로를 안내하는 한편 Go, Dart, JavaScript는 별도 저장소로 연결한다.
사용할 언어의 런타임 안내를 따로 읽어야 하며, 이번에 읽은 Python 예제를 다른 언어의 설치 조건으로 그대로 옮겨서는 안 된다.[1]

소스의 최신 상태와 안정적으로 사용할 배포판도 구분해야 한다.
README는 대부분의 사용자에게 지원되는 릴리스를 권하며, `main`의 최신 코드는 소스 호환성을 깨는 변경이나 충분히 시험되지 않은 동작 때문에 빌드가 깨질 수 있다고 경고한다.
특히 소스 빌드가 필요한 경우 릴리스 브랜치의 릴리스 커밋에 고정하라고 안내한다.
이 경고는 프로젝트의 데이터 형식 설명과 별도로 실제 도입 시 확인할 조건이다.[1]

## 직접 읽어볼 자료

- [공식 README](https://github.com/protocolbuffers/protobuf/blob/main/README.md)
  먼저 컴파일러와 런타임을 구분하는 Overview를 읽고 언어별 설치 안내가 어디로 연결되는지 확인한다.
  최신 소스를 읽는 일과 지원 릴리스를 사용하는 일이 왜 다른지도 함께 살핀다.
- [주소록 구조 정의](https://github.com/protocolbuffers/protobuf/blob/main/examples/addressbook.proto)
  `Person` 안의 전화번호와 `AddressBook` 안의 사람 목록을 번갈아 읽는다.
  타입·필드 번호·반복 표시가 어떻게 데이터의 모양을 정하는지, 정의만 있고 예제에서는 채우지 않는 항목이 무엇인지 찾아볼 수 있다.
- [Python 연락처 추가 예제](https://github.com/protocolbuffers/protobuf/blob/main/examples/add_person.py)
  파일 읽기에서 `people.add()`를 거쳐 파일 쓰기로 이어지는 부분을 따라간다.
  protobuf 라이브러리가 수행하는 변환과 예제 작성자가 직접 둔 입력 분기·오류 처리를 나누어 읽으면 역할 경계가 드러난다.

## 정리

protobuf는 공유할 자료의 구조를 먼저 정의하고, 언어별 프로그램이 그 구조를 같은 형식으로 읽고 쓰도록 돕는다.
주소록 예제의 핵심은 메시지 정의와 실제 입력·저장 코드의 연결이다.
값의 의미, 유효성, 파일을 안전하게 관리하는 정책은 형식 바깥에서 따로 설계해야 한다.

## 자료 확인 범위

2026-09-27 기준으로 공식 README, 루트 구성, `addressbook.proto`와 `add_person.py`를 읽었다.
컴파일러 설치, 코드 생성, 주소록 실행이나 언어 간 호환성 시험은 수행하지 않았다.

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

[1] protocolbuffers/protobuf — README.md

<https://github.com/protocolbuffers/protobuf/blob/main/README.md>

[2] protocolbuffers/protobuf — examples/addressbook.proto

<https://github.com/protocolbuffers/protobuf/blob/main/examples/addressbook.proto>

[3] protocolbuffers/protobuf — examples/add_person.py

<https://github.com/protocolbuffers/protobuf/blob/main/examples/add_person.py>
