---
title: "toeverything/AFFiNE"
repository: "toeverything/AFFiNE"
url: "https://github.com/toeverything/AFFiNE"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# toeverything/AFFiNE

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

AFFiNE은 문서·화이트보드·표를 블록 기반으로 함께 다루는 지식 작업 공간이다.
편집기 라이브러리만 제공하는 프로젝트가 아니라 사용자용 앱이며, README는 로컬 우선 저장과 실시간 협업을 함께 소개한다.[1]

## 문서와 캔버스를 오가는 작업

설명을 쓰는 문서와 관계를 그리는 화이트보드를 따로 쓰면 같은 정보를 옮겨 적게 된다.
AFFiNE은 텍스트·메모·도형·연결 페이지·데이터베이스 등을 경계 없는 캔버스에 놓는 접근을 취한다.[1]
블록은 문서나 화면을 구성하는 작은 콘텐츠 단위이고, 로컬 우선은 클라우드만을 유일한 데이터 보관 장소로 삼지 않는 방향이다.
그렇다고 동기화·AI·모든 협업 동작이 항상 오프라인이라는 뜻은 아니다.[1]

## Workspace 구현에서 보이는 문서 관리

`packages/frontend/core/src/modules/workspace/impls/workspace.ts`의 WorkspaceImpl은 문서들을 Map으로 관리한다.
문서 메타데이터가 추가되면 DocImpl을 생성해 등록하고, 변경되면 문서 목록 갱신 신호를 보낸다.[2]
메타데이터는 제목·생성 시각·태그처럼 문서 자체를 설명하는 정보다.
`createDoc`은 별도 생성 콜백이 있으면 이를 사용하고, 기본 경로에서는 중복 ID를 검사한 뒤 메타데이터를 등록한다.[2]

같은 클래스는 Yjs 문서와 BlobEngine을 받아들인다.
Blob은 이미지처럼 덩어리로 저장하는 자료를 뜻하며, 문서 목록과 별도 저장·동기화 경계를 가진다는 점을 읽을 수 있다.[2]
특히 이 생성자의 기본 BlobSource는 MemoryBlobSource다.
이 파일 하나만 보고 실제 앱의 디스크 저장·클라우드 동기화가 모두 증명되었다고 결론 내릴 수는 없다.[2]

## 가상 예시: 독서 메모를 발표 구조로 바꾸기

공개 책 소개 자료를 읽고 요약·인용·질문을 개인 문서에 모은다고 가정하자.
문서의 블록을 캔버스에서 관계가 보이도록 배치하고, 연결 페이지로 세부 설명을 나누는 활용을 생각할 수 있다.[1]
구현 관점에서는 새 문서의 식별자와 메타데이터가 등록되고 목록 갱신 신호를 통해 주변 기능이 변화를 알게 된다.[2]
완성 결과는 정리한 문서와 시각적 배치이지, 출처 검증까지 끝난 지식이 자동 생성되는 것은 아니다.
이는 활용 흐름을 설명하는 가상 예시이며 앱 편집·저장·동기화·AI 호출을 수행하지 않았다.

## 공개 소스와 라이선스 범위를 함께 읽기

루트 LICENSE는 `packages/backend`와 `packages/common/native`를 별도 라이선스 대상으로 지정한다.
그 밖의 범위는 MIT이며 외부 구성 요소는 원래 조건을 따른다고 적혀 있다.[3]
따라서 README의 “open-source”라는 표현만으로 저장소 전체를 MIT라고 요약하면 잘못이다.

서버 LICENSE는 CE에 포함되지 않은 부분에 EE 조건을 적용한다.
EE의 운영 사용은 유효한 구독과 관련 약관 준수를 요구하며, 개발·테스트 복사와 수정은 구독 없이 허용한다고 명시한다.[4]
같은 문서는 CE 배포 부분 또는 명시한 클라이언트 제공 부분에 MPL2.0 조건을 적용한다고 구분한다.
실제 재배포나 자체 운영 전에는 사용할 에디션·파일·배포물의 경계를 따로 확인해야 한다.[3][4]

## 직접 읽어볼 자료

- [1] README의 Features에서 문서·캔버스 통합과 로컬 우선·협업을 별개의 기능으로 읽는다.
- [2] `WorkspaceImpl`의 생성자, `_bindDocMetaEvents`, `createDoc`, `removeDoc` 순서로 문서 수명주기를 확인한다.
- [3] 루트 LICENSE에서 디렉터리별 적용 조건을 먼저 찾는다.
- [4] 서버 LICENSE에서 EE의 운영 조건과 CE·클라이언트 부분의 예외를 확인한다.

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

[1] toeverything/AFFiNE — README.md

<https://github.com/toeverything/AFFiNE/blob/canary/README.md>

[2] toeverything/AFFiNE — packages/frontend/core/src/modules/workspace/impls/workspace.ts

<https://github.com/toeverything/AFFiNE/blob/canary/packages/frontend/core/src/modules/workspace/impls/workspace.ts>

[3] toeverything/AFFiNE — LICENSE

<https://github.com/toeverything/AFFiNE/blob/canary/LICENSE>

[4] toeverything/AFFiNE — packages/backend/server/LICENSE

<https://github.com/toeverything/AFFiNE/blob/canary/packages/backend/server/LICENSE>
