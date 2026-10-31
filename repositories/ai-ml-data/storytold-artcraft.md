---
title: "storytold/artcraft"
repository: "storytold/artcraft"
url: "https://github.com/storytold/artcraft"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["ai-ml-data", "starred-draft"]
---

# storytold/artcraft

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

ArtCraft는 이미지·영상 생성에 2D 합성과 3D 장면 배치를 결합하는 창작용 데스크톱 도구다.
새로운 단일 생성 모델이 아니라 구도·카메라·참조 자료와 여러 모델을 연결하는 작업 환경이다.[1]

## 프롬프트만으로 정하기 어려운 구도

글로 인물의 위치와 카메라 각도를 반복 설명하는 대신 장면을 먼저 구성하는 것이 프로젝트의 방향이다.
README는 이미지 레이어, 배경 제거, 캐릭터 포즈, 3D 오브젝트 배치, 이미지에서 영상 생성 등의 기능을 소개한다.[1]
화면에 보이는 합성 작업과 외부 모델의 생성 결과는 구분해야 한다.
로컬 데스크톱 앱이라는 사실만으로 모든 생성이 로컬 모델에서 처리된다고 볼 수는 없다.[1][2]

## 대표 경로: URL 입력에서 비동기 영상 작업으로

`_docs/artcraft_omni_api.md`는 API 키로 인증하는 프로그래밍용 영상 생성 경로를 설명한다.
이미지·영상·음성 참조 URL을 전달하면 서버가 파일을 내려받아 계정의 미디어로 저장한 뒤 생성에 사용한다.[2]
API는 소프트웨어끼리 요청을 주고받는 통로이며, 이 경로는 브라우저 세션 쿠키 대신 키를 사용한다.
생성 요청은 모델·프롬프트·길이·비율 등의 값을 받고 즉시 완성 영상 대신 작업 토큰을 반환한다.[2]

작업 토큰은 후속 상태 조회에 사용하는 식별자다.
`idempotency_token`은 의도치 않은 중복 요청을 방지하는 필수 값으로 안내된다.
동일 종류의 입력을 URL과 미디어 토큰으로 동시에 보내면 안 된다는 제약도 있다.[2]
따라서 “요청을 받았다”, “작업이 끝났다”, “사용할 결과가 나왔다”를 나누어 다뤄야 한다.

## 데스크톱과 서비스는 같은 저장소가 아니다

`_docs/dev_setup.md`는 Rust·Tauri 앱과 JavaScript·TypeScript·React 프런트엔드를 안내한다.
Tauri는 웹 기술로 만든 화면을 데스크톱 앱의 네이티브 기능과 연결하는 틀이다.[3]
개발 문서는 백엔드 서비스와 웹사이트 빌드를 별도 `artcraft-services` 저장소로 구분한다.
따라서 현재 저장소를 빌드하면 생성 서비스까지 모두 자체 운영할 수 있다고 설명해서는 안 된다.[3]

## 가상 예시: 같은 장면의 짧은 영상 만들기

사용 권한을 가진 풍경 이미지로 구도를 잡고 짧은 영상으로 확장한다고 가정하자.
화면에서는 2D·3D 배치 도구로 의도를 다듬고, 프로그램 연동에서는 시작 이미지 URL과 프롬프트를 영상 API에 전달하는 경로를 생각할 수 있다.[1][2]
API가 돌려준 토큰으로 완료 상태를 확인한 다음 결과의 인물·구도·움직임을 검토한다.
이는 README와 API 문서를 연결해 이해하는 예시이며, 데스크톱 내부의 모든 호출이 이 공개 API를 쓴다고 검증한 것은 아니다.
앱 설치·로그인·이미지 업로드·유료 생성 요청은 수행하지 않았다.

## 사용 조건과 배포물의 권리

API 키 기능은 계정별 활성화가 필요하다고 문서에 명시되어 있다.
외부 생성 서비스의 접근 조건과 비용을 확인해야 하며, 모델 목록의 비활성·제한 표시도 현재 이용 가능 여부와 구분해야 한다.[1][2]
README와 NOTICE는 MIT 또는 Apache-2.0 중 선택하는 이중 라이선스를 안내한다.[1][4]
그러나 NOTICE는 이름·로고의 상표와 외부 구성 요소를 별도로 다루고, 일부 효과음·이미지·텍스처·모션 자료에는 두 라이선스의 권리를 부여하지 않는다고 명시한다.
코드 공개를 모든 번들 자산의 자유로운 재배포 허용으로 해석하면 안 된다.[4]

## 직접 읽어볼 자료

- [1] README에서 장면 구성 기능과 생성 모델 기능, 현재 제한 표시를 나누어 읽는다.
- [2] Omni API 문서의 인증·URL 입력 제약·작업 토큰 응답에서 실행 경계를 확인한다.
- [3] Dev Setup에서 데스크톱 구성과 별도 서비스 저장소의 역할을 확인한다.
- [4] NOTICE의 Material not covered 구간을 배포 전에 반드시 읽는다.

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

[1] storytold/artcraft — README.md

<https://github.com/storytold/artcraft/blob/main/README.md>

[2] storytold/artcraft — _docs/artcraft_omni_api.md

<https://github.com/storytold/artcraft/blob/main/_docs/artcraft_omni_api.md>

[3] storytold/artcraft — _docs/dev_setup.md

<https://github.com/storytold/artcraft/blob/main/_docs/dev_setup.md>

[4] storytold/artcraft — NOTICE

<https://github.com/storytold/artcraft/blob/main/NOTICE>
