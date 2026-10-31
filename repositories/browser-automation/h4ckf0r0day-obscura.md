---
title: "h4ckf0r0day/obscura"
repository: "h4ckf0r0day/obscura"
url: "https://github.com/h4ckf0r0day/obscura"
category: "browser-automation"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["browser-automation", "starred-draft"]
---

# h4ckf0r0day/obscura

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Obscura는 Rust로 만든 독립 headless 브라우저 엔진으로, V8 JavaScript 실행과 CDP 연결을 통해 수집·Agent 자동화를 지원한다.[1]
Headless는 일반 브라우저 창을 띄우지 않고 프로그램으로 페이지를 다룬다는 뜻이다.
Chromium을 뒤에서 실행하는 래퍼가 아니며, 개발 중인 호스팅 서비스 Obscura Cloud와 공개 엔진도 구분해야 한다.[1]

## 기존 자동화 API를 별도 엔진에 연결하기

웹 문서가 JavaScript로 내용을 바꾸면 HTML 다운로드만으로는 최종 화면 정보를 얻기 어렵다.
Obscura는 페이지 실행과 DOM 접근을 제공하고, CDP라는 브라우저 제어 프로토콜을 구현해 Puppeteer·Playwright와 연결한다.
README의 대체 가능성 주장은 이런 연결 표면에 관한 것으로, 모든 웹 표준과 Chromium 렌더링이 같다는 보장은 아니다.[1]

루트에는 Rust workspace 설정인 `Cargo.toml`과 `crates`, `docs`, `vendor`, 재현 자료인 `render-repros`가 있다.
한 파일짜리 수집 스크립트가 아니라 엔진·도구·문서와 외부 코드가 함께 있는 구조다.[2]

## Playwright 연결 문서에 드러나는 실제 경계

`docs/Use-with-Playwright.md`는 `chromium.connectOverCDP`로 실행 중인 서버에 연결하도록 안내한다.
Playwright 자체 프로토콜을 쓰는 `connect`와는 다르다고 명시하므로 메서드 이름을 바꾸어 쓰면 안 된다.
이후 context와 page를 만들고 이동·DOM 평가·locator 조작을 수행하는 흐름이다.[3]

같은 문서는 여러 페이지가 하나의 V8 isolate를 공유한다고 밝힌다.
Isolate는 JavaScript 실행 상태를 담는 독립 단위이며, 여기서는 한 페이지의 CPU 집약적 코드가 다른 페이지도 막을 수 있다.
여러 page 객체를 만들었다는 사실을 각 페이지의 계산이 별도 CPU에서 병렬 실행된다는 뜻으로 해석해서는 안 된다.[3]

스크린샷은 현재 viewport 또는 전체 문서 공간을 캡처하고 PDF는 raster-backed 출력이다.
CDP screencast는 활동에 따라 만들어지는 페이지 캡처이며 고정 프레임률 데스크톱 영상이 아니다.
캡처 파일이 생기는 것과 원래 브라우저의 화면·인쇄 품질을 완전히 재현하는 것은 별개다.[3]

## 가상 시나리오: 공개 예제 페이지의 링크 모으기

수집이 허용된 공개 예제 페이지에서 JavaScript가 만든 링크를 모으는 상황을 가정한다.
Obscura 서버에 CDP로 연결하고 페이지를 연 뒤 필요한 요소가 준비될 때까지 기다린다.
DOM 평가로 링크의 문구와 주소를 추출하고, 필요하면 스크린샷을 별도 확인 자료로 남기는 흐름이다.[1][3]

추출 결과가 비었다면 정말 링크가 없는지, 아직 페이지가 준비되지 않았는지, 필요한 Web API가 지원되지 않는지 나누어 확인해야 한다.
이 시나리오에서는 서버 기동·네트워크 수집·렌더링 검증을 실제로 수행하지 않았다.

## 제어 포트와 호환성을 먼저 확인하기

README는 일부 CSS·Web API·미디어·폰트 렌더링이 Chromium과 다를 수 있다고 명시한다.
메모리·속도 표는 프로젝트의 주장으로만 읽고 여기서는 벤치마크를 재현하지 않았다.[1]

Docker 예시는 host loopback에 포트를 공개하고 `OBSCURA_CDP_TOKEN`을 설정한다.
모든 인터페이스로 포트를 열면 브라우저 제어 경로가 외부에 노출될 수 있다.
사설 IP 접근은 기본 차단되며 로컬 개발 서버에 접근하는 허용 옵션은 필요한 범위에서만 검토해야 한다.[1]
Apache-2.0 엔진을 사용할 권리와 대상 사이트에 접근·수집할 권한은 별개이며, 기능 소개가 접근 제한 우회를 승인하지 않는다.[4]

## 직접 읽어볼 자료

- [1] Rendering의 한계와 Docker·사설 네트워크 설명을 설치 예제보다 먼저 확인한다.
- [3] Connect와 Multiple pages를 읽어 프로토콜과 JavaScript 실행 공유 범위를 이해한다.
- [3] Screenshot·PDF·Screencasting에서 각 산출물의 표현 방식과 한계를 확인한다.

## 자료 확인 범위

2026-10-28 공식 README, 루트 목록, 아래 대표 구현·문서와 라이선스를 확인했다.
설치·빌드·프로그램 실행은 하지 않았으며, 예시는 이해를 위한 가상 시나리오다.

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

[1] h4ckf0r0day/obscura — README

<https://github.com/h4ckf0r0day/obscura/blob/main/README.md>

[2] h4ckf0r0day/obscura — 루트 파일 목록

<https://api.github.com/repos/h4ckf0r0day/obscura/contents>

[3] h4ckf0r0day/obscura — docs/Use-with-Playwright.md

<https://github.com/h4ckf0r0day/obscura/blob/main/docs/Use-with-Playwright.md>

[4] h4ckf0r0day/obscura — LICENSE

<https://github.com/h4ckf0r0day/obscura/blob/main/LICENSE>
