---
title: "twentyhq/twenty"
repository: "twentyhq/twenty"
url: "https://github.com/twentyhq/twenty"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# twentyhq/twenty

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Twenty는 고객·연락처·진행 상태 같은 관계 데이터를 관리하고, 앱으로 기능을 확장하는 CRM 플랫폼이다.
CRM은 관계와 후속 활동을 한곳에 정리하는 시스템이다. 이 저장소는 UI 부품 모음이나 독립 에이전트만을 배포하는 프로젝트가 아니다.[1]

## 표를 넘어서 데이터와 행동을 연결하기

README는 객체, 보기, 워크플로와 에이전트를 기본 구성 요소로 소개한다.
객체는 관리할 데이터 종류, 필드는 각 항목의 속성, 보기는 데이터를 어떤 방식으로 보여줄지 정한 설정이다.
클라우드 서비스와 자체 호스팅 경로를 함께 제공하며, SDK로 이러한 구성을 코드에 담을 수 있다.[1]

루트 `packages/`를 중심으로 서버·화면·SDK·앱 개발 도구가 함께 있는 모노레포 구조다.
README의 기술 구성은 TypeScript, Nx, NestJS, PostgreSQL·Redis·BullMQ, React 등을 포함한다.
따라서 앱 확장 코드를 만드는 일과 CRM 서버 전체를 운영하는 일은 요구하는 범위가 다르다.[1]

## 확장 앱은 별도 웹 서버가 아니다

`packages/twenty-agent-skills/references/concepts/how-apps-work.md`는 앱을 독립 npm 패키지로 설명한다.
하지만 앱이 자체 서버를 띄우는 것은 아니다. 실행 중인 Twenty에 설치되어 객체·필드·화면·로직을 추가한다.
`twenty-sdk/define`은 이런 요소를 선언하고 `twenty-client-sdk`는 작업 공간 데이터와 메타데이터 접근을 담당한다.[2]

개발 흐름은 생성 → 수정 → 동기화 → 검증이다.
`twenty app apply`는 활성 remote에 빌드·배포·설치를 수행하므로 로컬 문법 검사와 다르다.
remote는 연결할 Twenty 인스턴스와 인증 정보를 묶은 이름이며, 실제 대상 작업 공간을 결정한다.
`publish`는 다른 인스턴스나 마켓플레이스에 배포하기 위한 별도 단계다.[2]

프런트 컴포넌트는 격리된 Remote DOM 안에서 렌더링된다.
일반 React 앱처럼 `document`나 `window`에 직접 접근하거나 외부 DOM을 조작하는 라이브러리를 그대로 사용할 수 없다.
엔티티의 UUID는 이름을 바꾸어도 유지해야 하므로 파일명 변경과 데이터 정체성 변경을 구분해야 한다.[2]

## 가상 시나리오: 공개 커뮤니티 연락 기록

개인 오픈소스 모임의 공개 연락 창구와 후속 활동을 정리하는 작은 앱을 가정한다.
연락 대상과 상태를 객체·필드로 정의하고 진행 상태별 보기를 만든 뒤, 별도의 테스트 작업 공간으로 동기화한다.
화면이 나타나는지만 보지 않고 수정 내용이 올바른 객체에 저장되는지 확인하는 흐름이다.[1][2]

최종 결과는 Twenty 안에서 동작하는 확장 앱이며, 서버와 인증 없이 열리는 독립 HTML 문서는 아니다.
예제 앱을 만들었다고 실제 연락 정보를 수집하거나 외부 메시지를 보냈다는 뜻도 아니다.[2]

## 배포 대상과 라이선스의 경계

자체 호스팅은 데이터베이스와 서버 운영이 필요하며 CLI가 컨테이너를 자동으로 준비해 주는 것은 아니다.
활성 remote를 확인하지 않고 `apply`하면 원하지 않는 작업 공간을 변경할 수 있다.[2]
루트 LICENSE는 대부분 AGPLv3이지만 `@license Enterprise` 파일은 상용 조건, 일부 SDK·도구·앱 패키지는 MIT라고 구분한다.
Application Exception은 정해진 인터페이스만 이용하는 앱의 자체 라이선스를 허용하되 Twenty 자체 수정에는 적용하지 않는다.
따라서 ‘전부 MIT’나 ‘모든 확장 앱을 무조건 AGPL로 공개해야 함’이라는 요약은 모두 부정확하다.[3]

## 직접 읽어볼 자료

- [1] README의 Build an app과 Self-hosting을 비교해 개발할 앱과 운영할 서버를 나눈다.
- [2] How Twenty Apps Work에서 remote, apply, publish와 Remote DOM 제한을 먼저 읽는다.
- [3] LICENSE의 첫머리와 Twenty Application Exception으로 실제 재사용 파일의 조건을 확인한다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 문서·코드의 관련 부분을 확인했다.
설치·모델 다운로드·서비스 실행은 하지 않았으며, 시나리오는 이해를 위한 가정이다.

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

[1] twentyhq/twenty — README.md

<https://github.com/twentyhq/twenty/blob/main/README.md>

[2] twentyhq/twenty — packages/twenty-agent-skills/references/concepts/how-apps-work.md

<https://github.com/twentyhq/twenty/blob/main/packages/twenty-agent-skills/references/concepts/how-apps-work.md>

[3] twentyhq/twenty — LICENSE

<https://github.com/twentyhq/twenty/blob/main/LICENSE>
