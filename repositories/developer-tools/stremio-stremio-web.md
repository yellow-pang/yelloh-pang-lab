---
title: "Stremio/stremio-web"
repository: "Stremio/stremio-web"
url: "https://github.com/Stremio/stremio-web"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# Stremio/stremio-web

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Stremio Web은 영상 콘텐츠를 탐색하고 재생하는 Stremio의 공식 웹 사용자 화면이다.
React로 만들어진 화면이지만 계정·라이브러리·애드온 관련 상태 계산은 별도의 `stremio-core`가 맡는다.
이 저장소만으로 모든 영상이 제공되는 독립 콘텐츠 서비스라고 이해하면 구조를 놓치게 된다.[1]

## 탐색 화면과 재생 엔진을 분리하는 이유

영상 목록에서 작품을 고르고, 시청하던 위치를 확인하고, 자막을 바꾸는 일은 한 화면에서 일어난다.
그러나 그 뒤에는 목록 제공, 계정 상태, 재생 가능한 형식 선택처럼 다른 종류의 처리가 있다.
README는 애드온이 제공하는 카탈로그로 영화·시리즈·채널을 찾고, Stremio 계정을 통해 라이브러리와 Continue Watching 상태를 기기 사이에서 동기화한다고 설명한다.
화면은 이 여러 정보를 사용자가 다룰 수 있는 형태로 보여준다.[1]

애드온은 본체에 연결해 자료나 기능을 보태는 확장 구성요소다.
작품을 찾는 카탈로그와 재생 관련 정보의 출처가 화면 코드 자체와 같지는 않다.
그래서 UI가 열렸다는 사실만으로 특정 작품이 존재하거나 모든 재생 경로가 작동한다고 결론 내릴 수 없다.
콘텐츠의 이용 권한 역시 소프트웨어 화면의 공개 여부와 별개의 문제다.[1]

## 화면이 상태를 계산하지 않는 구조

README의 구조 설명은 역할을 명확하게 나눈다.
UI는 상태를 화면에 표시하고, Rust로 만든 `stremio-core`는 WebAssembly로 컴파일되어 Web Worker에서 실행된다.
WebAssembly는 웹 환경에서 실행할 수 있는 코드 형식이며, Web Worker는 화면을 그리는 작업과 분리된 실행 공간이다.
재생은 별도 `stremio-video`가 환경에 맞는 플레이어 구현을 고르는 구조로 설명한다.[1]

실제 연결부인 `createTransport.ts`는 Worker를 만들고 `Bridge`로 창과 Worker를 잇는다. `init`, `getState`, `dispatch`가 각각 초기화, 상태 읽기, 동작 전달을 맡는 함수로 드러난다. `dispatch`에는 action과 model뿐 아니라 현재 `location.hash`도 전달된다.
이를 보면 화면의 클릭을 받은 쪽이 모든 규칙을 자체 계산하기보다는 메시지를 건너편 코어에 전달하도록 인터페이스를 마련했음을 확인할 수 있다.[2]

같은 파일에는 스트림의 인코딩·디코딩과 analytics 호출도 있다.
함수가 존재한다는 사실은 각 호출이 Bridge를 통해 전달된다는 근거다.
다만 이 작은 파일은 코어 내부 구현이나 수집되는 모든 데이터의 범위를 보여주는 개인정보 정책 문서는 아니다.
함수 이름만으로 전체 재생 방식이나 데이터 취급 정책을 단정해서는 안 된다.[2]

`CoreProvider.tsx`는 코어를 화면의 하위 구성요소에 연결한다.
상태·일반 이벤트·오류의 수신자를 별도로 관리하고, `NewState`가 오면 상태 수신자들에게 전달한다.
초기화가 성공해야 자식 화면을 표시하며 실패하면 오류 화면을 선택하는 분기도 있다.
코어가 준비되지 않았는데 정상 화면을 계속 그리는 방식이 아니라 준비 여부를 표시 조건으로 삼는 것이다.[3]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
이용 권한이 있는 영상을 애드온 카탈로그에서 찾아 재생하려는 상황을 생각해 보자.
우선 앱은 자신에 관한 정보를 코어 초기화에 전달한다. `CoreProvider` 코드에서는 그 요청이 성공했을 때 `ready`를 참으로 바꾸고 하위 화면을 보여준다.
실패한 경우 오류를 보이게 되어 있으므로, 첫 화면이 안 나오는 문제와 특정 영상만 재생되지 않는 문제는 조사 지점부터 다르다.[1][3]

준비된 화면에서 사용자가 콘텐츠 관련 동작을 선택하면, 연결 계층은 동작을 코어로 전달할 수 있다.
코어가 계산한 새 상태는 `NewState` 이벤트를 통해 등록된 수신자에게 전해진다.
이 일반적인 메시지 경로와 카탈로그·재생 역할은 확인했지만, 이번에 읽은 두 파일만으로 특정 작품 선택 버튼의 모든 함수 호출 순서를 확인한 것은 아니다.
실제 재생 구현 선택은 README가 연결한 `stremio-video`의 책임으로 설명된다.[1][2][3]

독자가 최종 화면에서 확인할 것은 원하는 콘텐츠의 출처와 이용 권한, 재생 환경, 자막 출처다.
README는 애드온 제공 자막이나 로컬 자막, 키보드 재생 제어, Chromecast를 기능으로 소개한다.
그러나 각 기능의 존재와 모든 기기·영상 조합의 성공은 같지 않다.
이 시나리오의 핵심은 한 번의 사용자 선택 뒤에 UI·코어·애드온·플레이어가 나누어 맡는 작업을 구별하는 데 있다.[1]

## PWA와 개발 환경을 혼동하지 않기

README는 앱이 독립 실행 형태로 설치 가능한 PWA라고 안내한다.
PWA는 웹 기술로 만든 앱을 별도 창과 앱 아이콘처럼 사용할 수 있게 하는 방식이다.
반면 소스 개발 안내에는 Node.js 22 이상과 pnpm 11 이상이 필요하다고 적혀 있다.
웹 앱을 이용하는 독자의 준비와 이 저장소를 수정해 개발 서버를 띄우려는 독자의 준비는 서로 다른 범위다.[1]

또한 생태계 표는 `stremio-core`, `stremio-video`, 번역 저장소, 애드온 SDK를 별개로 안내한다.
UI 소스만 수정해도 되는 문제가 있고, 코어의 상태 처리나 애드온 응답을 확인해야 하는 문제가 있다.
버그를 읽거나 구조를 공부할 때도 문제가 발생한 화면과 원인 계층을 무조건 동일시하지 않는 편이 정확하다.
이 문서에서는 연결부 소스까지만 확인했으며 별도 엔진 내부까지 검증하지 않았다.[1]

배포 라이선스는 README에서 GPL-2.0으로 안내한다.
수정·배포를 생각한다면 라이선스 전문과 관련 구성요소 조건을 따로 확인해야 한다.
콘텐츠의 저작권과 앱 코드의 라이선스는 다른 권리이므로, 소스가 공개되어 있다는 이유로 모든 콘텐츠 이용이 허용되는 것은 아니다.[1]

## 직접 읽어볼 자료

- [공식 README](https://github.com/Stremio/stremio-web/blob/development/README.md)
  Features를 읽은 뒤 How it works의 역할 분리를 확인한다.
  탐색·동기화·재생 중 어느 부분이 이 React 저장소에 있고 어느 부분이 외부 구성요소에 있는지 구분하는 순서로 읽는다.
- [코어 연결 계층](https://github.com/Stremio/stremio-web/blob/development/src/core/createTransport.ts)
  Worker 생성과 Bridge 호출을 따라가며 `getState`와 `dispatch`가 받는 인자가 어떻게 다른지 살핀다.
  짧은 파일이므로 함수 이름보다 실제로 전달하는 값에 주목하기 좋다.
- [CoreProvider](https://github.com/Stremio/stremio-web/blob/development/src/core/CoreProvider.tsx)
  초기화 성공·실패 분기부터 보고 `NewState`와 `CoreEvent` 처리로 이동한다.
  화면을 그려도 되는 시점과 업데이트를 알리는 방식이 각각 어디에 표현되어 있는지 확인할 수 있다.

## 정리

Stremio Web은 콘텐츠와 상태를 한 화면에서 조작하도록 연결하는 웹 클라이언트다.
이 저장소의 특징은 모든 기능을 React 안에 넣지 않고 코어와 플레이어를 분리한 데 있다.
영상의 제공 여부, 재생 호환성, 이용 권한은 화면의 기능 목록만으로 확정할 수 없다.

## 자료 확인 범위

2026-09-27 기준 공식 README와 루트 파일 목록, `createTransport.ts`, `CoreProvider.tsx`를 읽었다.
앱을 설치하거나 영상을 재생하지 않았고, 별도 코어·플레이어 저장소의 내부 동작과 기기별 호환성은 시험하지 않았다.

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

[1] Stremio/stremio-web — README.md

<https://github.com/Stremio/stremio-web/blob/development/README.md>

[2] Stremio/stremio-web — src/core/createTransport.ts

<https://github.com/Stremio/stremio-web/blob/development/src/core/createTransport.ts>

[3] Stremio/stremio-web — src/core/CoreProvider.tsx

<https://github.com/Stremio/stremio-web/blob/development/src/core/CoreProvider.tsx>
