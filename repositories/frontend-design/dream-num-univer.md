---
title: "dream-num/univer"
repository: "dream-num/univer"
url: "https://github.com/dream-num/univer"
category: "frontend-design"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "frontend-design"
  - "starred-draft"
---

# dream-num/univer

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Univer는 웹 애플리케이션 안에 스프레드시트와 문서 편집기를 구성하는 Office SDK다.
SDK는 완성된 서비스에 가입하는 대신 개발자가 자기 프로그램에 넣어 사용하는 부품 모음이다.
Univer는 파일을 보여주는 뷰어에 머무르지 않고 문서 모델, 계산 엔진, 화면 표시, 사용자 조작을 함께 제공한다.
이 저장소의 공개 코어와 별도로 개발되는 Univer Pro, 이를 이용한 AI 연동 제품은 같은 범위가 아니다.[1]

## 표를 그리는 것과 편집기를 만드는 것의 차이

웹 화면에 행과 열을 그리는 일만으로 스프레드시트가 완성되지는 않는다.
셀 하나의 값이 바뀌면 다른 시트의 합계를 다시 계산해야 하고, 숫자는 금액이나 날짜로 표시될 수 있으며, 잘못된 입력을 제한하거나 조건에 맞는 셀을 강조해야 한다.
Univer의 Sheets 영역은 이러한 편집·계산·표시를 플러그인으로 조합한다.
README는 Sheets를 가장 성숙한 영역으로 설명하며 Docs와 Slides는 같은 구조를 사용하면서 발전 중이라고 구분한다.[1]

여기서 플러그인은 특정 기능을 추가하는 모듈이다.
필요한 플러그인을 직접 등록하는 Plugin Mode에서는 패키지, 스타일, 언어 자료와 API 확장을 직접 선택한다.
Preset Mode는 자주 쓰는 조합과 필요한 스타일·API 등록을 묶어 시작점을 줄여준다.
두 방식은 서로 다른 문서 형식이 아니라 같은 SDK를 구성하는 방법의 차이다.
README의 기본 예제도 언어와 화면 컨테이너를 정하고 인스턴스를 만든 뒤 통합 문서를 생성하는 순서다.[1]

## 계산, 화면, 호출 인터페이스를 나누는 구조

Univer의 렌더링 엔진은 Canvas, 즉 브라우저의 그림 그리기 영역을 사용한다.
수식 엔진은 셀 사이의 계산을 맡고 UI 플러그인은 메뉴나 편집 조작을 맡는다.
Facade API는 이러한 내부 구조를 감싸 통합 문서, 시트, 셀 범위 등을 일관된 방식으로 다루게 하는 상위 인터페이스다.
화면의 버튼을 일일이 흉내 내지 않고도 프로그램에서 문서를 읽거나 변경하는 경로를 제공한다.[1]

브라우저와 Node.js에서 같은 문서 논리를 쓰려면 이 분리가 실제 코드 규칙으로 이어져야 한다.
공식 `ISOMORPHIC.md`는 기반 논리와 UI를 최소 두 플러그인으로 나누라고 설명한다.
예를 들어 필터는 `sheets-filter`와 `sheets-filter-ui`로 구별된다.
데이터 변경 명령이 현재 화면의 선택 상태를 직접 읽도록 만들면 화면 없는 서버에서 재사용하기 어렵기 때문에, 문서 변경 논리는 UI에 의존하지 않아야 한다.[2]

Node.js에서 화면 없이 처리하는 Headless는 이 설계의 다른 사용 경로다.
다만 공개 Headless 기반 기능이 있다는 사실을 Pro의 협업 서버나 서버 계산 상품까지 무료로 제공한다는 뜻으로 확대하면 안 된다.
README는 공개 런타임과 상용 서버 확장을 따로 나열한다.
동일한 API 계열이라도 기능을 공급하는 패키지와 라이선스를 함께 봐야 한다.[1]

## 예시로 따라가는 흐름

공식 `create-sheet-fixture.ts`는 개발용 통합 문서를 만드는 예제 자료다.
Notebook, Marker, Keyboard 같은 품목과 수량·가격·할인·상태를 준비하고, Table 시트의 자료를 Core 시트의 수식에서 참조한다.
여기서는 파일에 정의된 입력과 관계를 따라가며, 브라우저에서 실행해 얻은 결과를 제시하는 것은 아니다.[3]

읽기 시작점은 화면 색상보다 `DATA_ROWS`와 `CORE_CELL_DATA`다. `=SUM(TableRevenue)`는 이름 붙인 셀 범위의 합계를, `SUMIFS`는 Hardware 분류에 맞는 값을 모으는 계산을 표현한다.
상태 열에 Blocked가 있는지를 `COUNTIF`로 확인한 뒤 `IF`로 문구를 선택하는 수식도 있다.
즉 품목 표와 요약 표를 따로 만들어도 셀 참조가 연결 고리가 된다.
입력을 수정했을 때 확인할 것은 숫자 하나가 바뀌었는지뿐 아니라 참조한 범위와 조건이 의도에 맞는지다.[3]

같은 파일은 수량·할인에 범위 검사를 붙이고, 상태에 선택 목록을 지정하며, Blocked 문구를 강조하는 조건부 서식을 구성한다.
마지막에는 시트, 스타일, 이름 정의와 각 플러그인의 자료를 통합 문서 스냅샷으로 묶는다.
스냅샷은 어느 순간의 문서 상태를 담은 객체다.
이 흐름에서 사람은 입력 제한이 계산식과 맞는지, 금액 서식이 원래 값을 바꾸지는 않는지, 다른 시트의 참조가 끊기지 않았는지를 나누어 확인할 수 있다.
예제의 핵심은 예쁜 표 하나보다 값·수식·규칙·표시가 같은 문서에 함께 기록된다는 점이다.[3]

## 공개 기능과 통합 책임의 경계

README 기준 공개 SDK는 Apache-2.0이며, 실시간 협업·편집 이력·파일 가져오기와 내보내기·차트·피벗 같은 기능은 Pro 확장으로 구분한다.
따라서 화면에서 셀을 편집할 수 있다는 것과 기존 Office 파일을 원하는 수준으로 가져오고 다시 저장할 수 있다는 것은 별개다.
필요한 결과물 형식을 먼저 정한 뒤 공개 패키지 범위와 상용 확장을 대조해야 한다.[1]

패키지 조합에도 조건이 있다.
같은 배포 계열의 `@univerjs/*`는 버전을 맞추고, 독립 배포 패키지는 명세에 맞는 호환 버전을 사용하도록 안내한다.
README는 Headless 실행 환경과 저장소 자체를 개발하는 Node.js 요구 버전도 구별한다.
브라우저에서는 `Intl.Segmenter` 지원 여부와 필요시 보완 코드도 확인해야 한다.
이 조건들은 설치 명령 하나가 성공하는 것과 편집기가 정상 통합되는 것이 다름을 보여준다.[1]

## 직접 읽어볼 자료

- [공식 README](https://github.com/dream-num/univer/blob/dev/README.md)
  Quick Start의 두 구성 방법을 먼저 대조한 뒤 Open Source and Pro 표를 읽는다.
  원하는 기능이 코어 플러그인인지 상용 확장인지, 기본 예제가 실제로 어떤 화면과 계산 모듈을 등록하는지 확인하는 순서가 유용하다.
- [브라우저와 서버 구조 안내](https://github.com/dream-num/univer/blob/dev/docs/ISOMORPHIC.md)
  필터 기능의 논리·UI 분리 예를 통해 화면 없는 실행에서 무엇을 빼야 하는지 살핀다.
  Facade API와 데이터 변경 명령이 브라우저 상태를 직접 읽지 않아야 하는 이유를 연결해서 읽을 수 있다.
- [Sheets 예제 자료 생성기](https://github.com/dream-num/univer/blob/dev/examples/src/sheets/create-sheet-fixture.ts)
  `DATA_ROWS`에서 시작해 수식, 입력 검증 규칙, 마지막 `createSheetFixture` 순으로 이동한다.
  같은 품목 자료가 여러 기능을 검토하는 표로 어떻게 구성되는지 추적하면 플러그인 목록보다 구체적으로 이해할 수 있다.

## 정리

Univer는 문서 편집기를 만드는 부품과 공통 호출 인터페이스를 제공한다.
표의 값과 계산을 화면에서 분리하는 구조가 중심이며, 공개 SDK와 Pro의 기능 경계를 구분해야 정확한 통합 범위를 정할 수 있다.

## 자료 확인 범위

2026-09-27 기준 공식 README, 구조 문서와 Sheets 예제 자료 생성 파일을 읽었다.
설치·실행하지 않았고 수식 결과, 브라우저 호환성, 대규모 문서 성능은 실측하지 않았다.

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

[1] dream-num/univer — README.md

<https://github.com/dream-num/univer/blob/dev/README.md>

[2] dream-num/univer — docs/ISOMORPHIC.md

<https://github.com/dream-num/univer/blob/dev/docs/ISOMORPHIC.md>

[3] dream-num/univer — examples/src/sheets/create-sheet-fixture.ts

<https://github.com/dream-num/univer/blob/dev/examples/src/sheets/create-sheet-fixture.ts>
