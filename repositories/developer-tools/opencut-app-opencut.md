---
title: "OpenCut-app/OpenCut"
repository: "OpenCut-app/OpenCut"
url: "https://github.com/OpenCut-app/OpenCut"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# OpenCut-app/OpenCut

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 완성된 편집기와 진행 중인 재작성을 구분하기

OpenCut은 웹·데스크톱·모바일을 위한 무료 공개 소스 영상 편집기를 지향하는 프로젝트다.
다만 확인한 현재 저장소의 README는 처음부터 다시 작성하는 중이라고 명확히 밝힌다.
오늘 사용할 이전 버전은 별도의 `opencut-classic` 저장소를 가리키며, 기존 공개 사이트도 classic을 실행한다고 설명한다.
따라서 이 저장소에 과거 버전의 모든 편집 기능이 그대로 들어 있다고 소개하면 상태를 잘못 전달하게 된다.[1]

영상 편집기는 원본 클립을 가져와 시간 순서로 배치하고, 화면과 소리를 조정한 뒤 하나의 결과물로 내보내는 도구다.
사용자가 기대하는 것은 이 전체 흐름이지만, 개발 중인 저장소에서는 화면의 뼈대와 실제 미디어 처리가 서로 다른 완성 단계에 있을 수 있다.
OpenCut을 읽을 때 가장 먼저 해결해야 할 질문도 어떤 기능이 계획이고 어떤 코드가 현재 구현되었는가다.[1][2]

README의 재작성 목표에는 Editor API, 플러그인 우선 구조, Rust 핵심부를 공유하는 여러 플랫폼, AI 에이전트를 위한 MCP 서버, headless 모드, 편집기 안의 스크립트 탭이 있다.
이 목록은 ‘앞으로 올 것’으로 제시된 내용이다.
API는 다른 프로그램이 편집 기능을 호출하는 접점이고, headless는 그래픽 화면 없이 자동 처리하는 방식이지만, 목록에 있다고 지금 바로 사용할 수 있다는 뜻은 아니다.[1]

## 실제로 확인되는 웹과 데스크톱의 상태

웹의 `apps/web/src/routes/editor.tsx`는 `/editor` 경로에 Editor 구성 요소를 연결한다.
그러나 반환하는 화면은 Editor 제목과 `Coming soon.` 문구다.
이 파일에서는 클립 가져오기, 시간선 조작, 영상 내보내기를 확인할 수 없다.
경로가 존재한다는 것과 해당 화면의 편집 동작이 준비되었다는 것은 다르다는 점을 아주 작은 파일이 보여준다.[3]

데스크톱 README 역시 매우 초기 단계이며 현재는 창이 열리는 정도라고 경고한다.
구현에는 Rust의 GPUI를 사용하는 앱 진입점이 있고, `main.rs`가 OpenCut 제목의 창을 만든 뒤 `Shell`을 연결한다.
GPUI는 데스크톱 화면을 구성하고 그리는 라이브러리다.
이는 미디어 처리 성능을 증명하는 자료가 아니라 앱 창과 편집기 외형을 마련하는 단계의 근거다.[2][4]

`Shell`은 Browser, Preview, Inspector, Timeline이라는 패널을 배치한다.
일반적인 편집 화면의 용어로 보면 자료 탐색, 미리보기, 선택 항목 속성, 시간선 영역을 가리킨다.
하지만 이 파일에서 확인되는 것은 패널 객체를 만들고 화면에 배치한다는 사실이다.
각 이름이 암시하는 완성된 기능을 그대로 갖췄다고 읽어서는 안 된다.[5]

패널은 매번 화면을 그릴 때 새로 만들지 않고 처음 생성한 객체를 보관하는 구조다.
소스의 주석도 다시 그릴 때 상태가 유지되도록 이 방식을 사용한다고 설명한다.
화면이 자주 갱신되는 앱에서 구성 요소의 상태를 유지할 자리를 마련한 설계로 볼 수 있다.
그렇다고 현재 패널 안에 실제 편집 상태가 구현되어 있다고 확대할 수는 없다.[5]

## Timeline이라는 이름을 코드로 확인하기

대표 기능 영역인 `timeline.rs`는 Timeline이라는 구조체와 그리기 함수를 정의한다.
함수는 테마 색상을 얻고 높이·정렬을 지정한 영역에 `Timeline`이라는 문자열을 넣는다.
확인한 파일에는 클립 목록, 재생 위치, 트랙 편집, 프레임 계산이 들어 있지 않다.
화면에 시간선 자리가 있다는 사실만으로 영상을 잘라 붙일 수 있다고 설명할 수 없는 이유다.[6]

이 상태 설명은 기능이 영원히 없을 것이라는 예측이 아니다.
재작성 중인 프로젝트의 확인 시점과 확인 파일에 한정한 관찰이다.
반대로 과거 제품의 소개나 화면을 가져와 현재 코드의 기능 증거로 사용하는 것도 부적절하다.
별도 classic 버전과 재작성 저장소를 구분해 두어야 이후 변화가 생겨도 무엇을 비교하는지 분명해진다.[1][3][6]

## 예시로 따라가는 흐름

현재 저장소의 데스크톱 시작 경로를 문서와 소스로 따라가는 사례다.
영상을 넣고 완성 파일을 내보내는 성공담 대신, 실제로 확인할 수 있는 입력과 결과의 경계를 살펴본다.
앱이 시작될 때 `main.rs`는 GPUI 애플리케이션을 만들고 창 옵션을 정한 뒤 `Shell::new`로 화면 내용을 연결하도록 되어 있다.
이 흐름은 코드를 읽어 설명한 것이며 직접 실행한 결과가 아니다.[4]

`Shell::new`는 네 패널을 각각 생성해 보관한다.
이어지는 렌더링에서는 윗부분에 Browser·Preview·Inspector를 나란히 두고 아랫부분에 Timeline을 놓는다.
여기서 처리되는 입력은 실제 영상 파일이 아니라 앱의 화면 구성과 테마 상태다.
각 패널의 배치를 코드로 분리해 놓았다는 것을 확인할 수 있지만, 화면 크기 조절이나 파일 끌어 놓기가 실제로 어떤 결과를 내는지는 이 파일만으로 판단할 수 없다.[5]

마지막으로 Timeline 패널까지 내려가면 가운데 정렬된 이름 표시가 확인된다.
따라서 이 경로에서 근거를 갖고 설명할 수 있는 결과는 편집기 형태의 패널 구성이며, 영상 가져오기·편집·렌더링은 확인한 흐름에 나타나지 않는다.
사람은 ‘패널이 보일 자리’, ‘입력 이벤트 처리’, ‘미디어 상태 변경’, ‘최종 파일 생성’을 따로 점검해야 한다.
이름이 있는 영역과 완성된 사용 기능 사이에 남아 있는 작업을 구체적으로 읽는 사례다.[5][6]

웹도 같은 방식으로 확인할 수 있다. `/editor` 파일의 존재를 발견한 뒤 실제 반환 내용을 읽으면 준비 중 문구가 나온다.
실제 편집이 필요하다면 README가 별도로 안내하는 이전 버전의 문서와 상태를 다시 조사해야 한다.
이번 원고는 그 다른 저장소를 대신 분석하거나 사용할 수 있는 기능 목록을 끌어오지 않는다.[1][3]

## 개발 목표와 사용 가능 범위 사이

Rust 핵심부 공유와 플러그인 우선 구조는 여러 환경에서 기능을 연결하려는 재작성 방향이다.
플러그인은 본체에 추가 기능을 연결하는 확장 단위이며, Editor API나 스크립팅과 결합하면 자동화의 접점이 될 수 있다.
그러나 이번에 읽은 파일로는 플러그인 인터페이스 안정성, MCP 도구 목록, 배치 렌더링 성공을 확인하지 못했다.
모두 README의 계획으로만 소개해야 한다.[1]

개발 환경은 proto로 고정된 도구를 준비하고 moon 작업으로 웹·API·데스크톱을 나누어 실행하도록 안내한다.
데스크톱은 플랫폼별 그래픽 환경 조건도 있다.
macOS에는 Xcode 명령행 도구, Linux에는 Vulkan과 관련 시스템 구성 요소를 안내하며 WSL의 창 시스템 호환성 주의도 적혀 있다.
이런 환경 준비 안내는 배포된 편집기의 안정성 검증 자료가 아니다.[1][2]

README는 아키텍처를 설계하는 동안 외부 기여를 받을 준비가 아직 되지 않았다고 적는다.
소스는 MIT로 공개되어 있지만 유지보수자의 기여 절차와 제품의 완성도는 라이선스와 다른 문제다.
이 문서는 향후 일정, 성능, 다른 편집기와의 우열을 추측하지 않는다.[1]

## 직접 읽어볼 자료

1. [루트 README의 Status](https://github.com/OpenCut-app/OpenCut/blob/main/README.md)
   계획 목록보다 먼저 재작성 선언과 classic 안내를 읽는다.
   어느 저장소와 어느 사이트를 설명하는지 구분한 뒤 API·플러그인·headless가 현재 기능인지 미래 목표인지 표시하며 읽는 순서다.
2. [데스크톱 README](https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/README.md)와 [앱 진입점](https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/main.rs)
   초기 단계 경고를 코드의 창 생성 흐름과 대조한다.
   실행 조건과 화면을 여는 구조를 확인하되, 설치 지침이 있다는 이유로 영상 편집 기능까지 시험된 것으로 판단하지 않는다.
3. [화면 Shell](https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/shell.rs)과 [Timeline 패널](https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/panels/timeline.rs)
   패널 객체를 보관하는 설계와 패널 내부의 현재 내용을 이어 읽는다.
   상태를 유지할 자리가 있다는 사실과 실제 클립 상태가 존재한다는 주장을 구분하는 데 적합한 짧은 파일들이다.
4. [웹 Editor 경로](https://github.com/OpenCut-app/OpenCut/blob/main/apps/web/src/routes/editor.tsx)
   경로 등록과 준비 중 화면을 직접 확인한다.
   웹·데스크톱을 한 이름으로 부르더라도 각 실행 대상의 구현 상태는 따로 읽어야 한다는 점을 점검한다.

## 정리

현재 OpenCut 저장소는 새 영상 편집기를 위한 재작성과 화면 구조를 공개하는 단계다.
기존 편집기 사용 안내와 미래 설계 목표, 실제 확인한 초기 코드를 구분하는 것이 이 프로젝트를 오해하지 않고 읽는 핵심이다.

## 자료 확인 범위

2026-09-27에 확보한 공식 README와 루트 구성, 데스크톱 안내·창 진입점·화면 Shell·Timeline 패널, 웹 Editor 경로를 읽었다.
설치하거나 실행하지 않았고 영상을 불러오거나 편집·출력하지 않았다.
classic 저장소의 기능은 이번 조사 범위에 포함하지 않았다.

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

[1] OpenCut-app/OpenCut — README.md

<https://github.com/OpenCut-app/OpenCut/blob/main/README.md>

[2] OpenCut-app/OpenCut — apps/desktop/README.md

<https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/README.md>

[3] OpenCut-app/OpenCut — apps/web/src/routes/editor.tsx

<https://github.com/OpenCut-app/OpenCut/blob/main/apps/web/src/routes/editor.tsx>

[4] OpenCut-app/OpenCut — apps/desktop/src/main.rs

<https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/main.rs>

[5] OpenCut-app/OpenCut — apps/desktop/src/shell.rs

<https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/shell.rs>

[6] OpenCut-app/OpenCut — apps/desktop/src/panels/timeline.rs

<https://github.com/OpenCut-app/OpenCut/blob/main/apps/desktop/src/panels/timeline.rs>
