---
title: "vercel-labs/scriptc"
repository: "vercel-labs/scriptc"
url: "https://github.com/vercel-labs/scriptc"
category: "developer-tools"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# vercel-labs/scriptc

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

scriptc는 TypeScript와 JavaScript에서 지원되는 코드를 네이티브 실행 파일이나 WebAssembly로 컴파일하는 실험적 도구다.
정적 빌드는 Node.js와 JavaScript 실행 엔진 없이 동작하며, npm 의존성이나 일부 동적 코드는 별도 내장 엔진을 선택해 처리한다.[1]

## JavaScript를 다른 JavaScript로 바꾸는 도구가 아니다

컴파일은 소스 코드를 실행 가능한 형태로 미리 변환하는 과정이다.
scriptc는 TypeScript의 타입 정보를 이용해 지원되는 연산을 기계 명령으로 바꾼다.
타입은 문자열·숫자처럼 값의 종류에 대한 정보다.
TypeScript 문법을 지우고 Node.js에서 실행하는 흐름과 달리, 정적 빌드 결과에 JavaScript 엔진을 넣지 않는다는 점이 출발점이다.[1][2]

그렇다고 기존 Node.js 프로그램을 그대로 포장하는 만능 배포 도구는 아니다.
README는 JavaScript·TypeScript·Node.js API의 일부만 지원한다고 명시한다.
API는 프로그램이 호출할 수 있는 기능의 접점이다.
프로젝트가 원래 TypeScript 검사를 통과했더라도 scriptc가 아직 구현하지 않은 호출을 사용한다면 컴파일할 수 없다.[1][3]

## 타입 검사에서 실행 파일까지

공식 구조 설명은 타입 검사 → 타입이 붙은 중간 표현 → LLVM → 실행 파일 연결이라는 흐름을 보여준다.
중간 표현은 사람의 소스 코드와 기계 명령 사이에서 컴파일러가 다루기 쉬운 형식이다.
LLVM은 이 표현을 바탕으로 대상 플랫폼의 명령을 만드는 쪽을 맡는다.
마지막에는 생성된 코드와 C로 작성된 네이티브 런타임을 링커가 묶는다.[2]

저장소에서는 `packages/compiler`가 분석과 변환, `native/llvm-codegen`이 LLVM 도구 연결, `packages/runtime`이 메모리·이벤트 루프·입출력 등을 맡는다.
`packages/cli`는 사용자가 `build`, `run`, `coverage` 명령으로 이 기능을 호출하는 입구다.
지원되지 않는 연산을 만났을 때는 그대로 Node.js로 넘기는 것이 아니라 진단을 내는 경로가 있다.[2]

`--emit=ir`, `--emit=llvm`, `--emit=asm`, `--emit=obj`는 서로 다른 중간 결과를 보는 공식 옵션이다.
구조 설명의 재귀 함수 예제는 같은 소스가 JSON 형태의 중간 표현, LLVM 코드, 어셈블리, 오브젝트 파일로 이어지는 모습을 보여준다.
최종 프로그램 배포뿐 아니라 타입 정보가 실행 형태로 변하는 과정을 읽을 때도 사용할 수 있는 관찰 지점이다.[2]

## 예시로 따라가는 흐름

공식 Quickstart의 `hello.ts`는 첫 번째 사용자 인수가 있으면 그 값을 쓰고, 없으면 `world`를 선택해 인사한다.
여기서 입력은 `./hello scriptc`의 `scriptc`라는 문자열이다.
예제 코드가 `process.argv[2]`를 읽는 이유를 먼저 확인한 다음 `scriptc build hello.ts -o hello`가 소스에서 실행 파일을 만드는 단계임을 구분한다.
문서가 보여주는 결과는 `hello, scriptc`이며, 이것은 공식 예시 출력이지 이 원고에서 얻은 실행 로그가 아니다.[4]

`scriptc run hello.ts`는 컴파일 뒤 실행까지 한다.
그러나 추가 프로그램 인수를 넘기는 경로는 아니다.
Quickstart는 인수를 전달하려면 실행 파일을 만든 뒤 직접 호출하라고 안내한다.
따라서 명령 하나로 시험하는 편의 기능과, 인수를 받아 배포할 프로그램을 만드는 기능을 같은 것으로 읽으면 안 된다.[4][3]

이어 `scriptc coverage hello.ts`의 보고서를 본다.
공식 예제는 실행문 두 개를 모두 정적으로 컴파일할 수 있다고 보여준다.
coverage는 실행 파일을 만들지 않고 정적 처리, 동적 처리가 필요한 위치, 지원하지 않는 연산을 나눠 알려준다.
독자가 확인할 점은 ‘100%’라는 숫자보다 그 숫자가 바로 이 입력 프로그램의 실행문에만 적용된다는 사실이다.
의존 패키지 내부 구현의 문장 수는 집계 대상이 아니며, 이 결과로 Node.js 전체 호환성을 판단할 수 없다.[5]

다음 비교 대상은 `picocolors`를 가져와 문자열을 녹색으로 출력하는 예제다.
패키지를 준비하고 coverage를 보면 내장 엔진이 필요하다는 `SC2013` 진단이 나온다.
`--dynamic`을 선택하면 패키지 JavaScript를 빌드 시 포함하며, 실행 시 `node_modules`를 읽지 않는 결과물을 만든다고 설명한다.
사람은 패키지 포함 여부와 함께 남은 blockers, 즉 여전히 컴파일할 수 없는 연산이 있는지를 확인해야 한다.[5][4]

## 정적 코드와 동적 코드의 경계

`--dynamic`은 quickjs-ng라는 JavaScript 엔진을 실행 파일에 포함한다.
이 엔진은 자신의 메모리 영역과 작업 큐를 가지며, 정적 코드와 값을 주고받을 때 변환과 타입 확인이 일어난다.
따라서 ‘단일 실행 파일’과 ‘JavaScript 엔진을 전혀 사용하지 않음’은 다른 속성이다.[2]

동적 실행을 켜도 지원되지 않는 정적 타입 호출은 해결되지 않는다.
내장 엔진에서 Node.js 기능을 제공하는 shim 역시 원래 Node.js 모듈 자체가 아니라 제한된 대체 구현이다.
coverage의 `runs with --dynamic`과 `blockers`를 나누어 읽어야 하는 이유다.
또 타입 검사 오류가 먼저 발생하면 문장별 집계 자체가 나오지 않으므로, 보고서를 비교하기 전에 그 오류부터 구분해야 한다.[3][5]

## 호환성과 실행 권한의 주의점

컴파일 성공은 같은 동작을 보장하는 결론이 아니다.
공식 Limitations는 동적으로 얻은 값을 `JSON.parse(s) as Config`로 바꿀 때 실제 자료형을 검사하며, 맞지 않으면 잡을 수 있는 오류가 발생한다고 설명한다.
반면 숫자 typed array의 잘못된 인덱스를 정적 타입으로 읽는 경우에는 프로세스를 종료하는 trap이 발생할 수 있다.
Node.js에서 기대하던 오류 처리와 다르므로 정상 입력뿐 아니라 잘못된 입력도 따로 확인해야 한다.[3]

Node-API나 V8 기반 `.node` 애드온은 필요한 Node 런타임이 포함되지 않아 그대로 사용할 수 없다.
WASI 대상에도 별도 제약이 있다.
WASI는 WebAssembly 프로그램이 파일 등 호스트 기능과 연결되는 인터페이스이며, 여기서 사용하는 Preview 1 대상은 소켓·자식 프로세스·운영체제 시그널 같은 기능을 제공하지 않는다.
네이티브에서 지원되는 서버 예제를 WebAssembly에서도 그대로 실행할 수 있다고 생각하면 안 된다.[3]

npm 설치에는 Node.js 24 이상이 필요하지만, 설치된 네이티브 컴파일러와 출력물 실행에는 Node.js가 필요 없다는 구분도 중요하다.
실행 파일 빌드에는 플랫폼 링커와 SDK가 필요하고, macOS 안내는 Xcode Command Line Tools를 요구한다.
Quickstart는 네이티브 명령을 설치하기 위해 npm의 optional dependencies와 설치 스크립트를 허용하라고 한다.
따라서 설치 스크립트와 입력 소스의 신뢰를 확인해야 하며, `run`을 읽기 전용 분석처럼 취급해서는 안 된다.[4]

README는 Apache-2.0 라이선스를 표시하지만, 그것이 모든 의존 패키지나 외부 서비스의 조건까지 대신하지는 않는다.
이 원고는 sandbox 격리, 특정 서비스 비용, 실제 바이너리 성능을 검증하지 않았다.
공식 문서 역시 테스트가 다룬 사례의 증거와 완전한 Node.js 호환성은 구분한다.[1][2]

## 직접 읽어볼 자료

1. [Quickstart](https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/quickstart.mdx)

   인사 예제를 따라 소스, 빌드 결과, 프로그램 인수를 나눠 읽는다.
   설치에 필요한 Node.js와 결과물 실행 조건을 혼동하지 않는 데 초점을 둔다.

2. [Coverage Reports](https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/coverage.mdx)

   순수한 인사 예제와 npm 패키지 예제의 보고서를 비교한다.
   문장 수와 문제 위치 수가 왜 다를 수 있는지, blockers가 남는지 확인한다.

3. [How It Works](https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/how-it-works.mdx)

   중간 표현부터 링커까지 각 단계의 책임을 확인한다.
   `--emit` 예제는 최종 실행 파일만 보지 않고 변환 중간 결과를 읽는 길을 제공한다.

4. [Limitations](https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/limitations.mdx)

   사용하는 API와 자료형 변환에 해당하는 항목부터 읽는다.
   특히 오류 처리, 내장 엔진, 네이티브 애드온, WASI 제약을 일반적인 Node.js 동작과 분리한다.

## 자료 확인 범위

2026-10-04 기준 커밋 `b97ea1d929593690e8d05f263b6f55b5502532c9`의 README와 공식 Quickstart, Coverage Reports, 구조 설명, Limitations의 관련 항목을 읽었다.
설치, 컴파일, 예제 실행, 호환성 테스트 및 성능 검증은 하지 않았다.
공식 출력과 지원 설명은 해당 문서의 주장으로 기록했으며 모든 API를 확인한 것은 아니다.

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

[1] vercel-labs/scriptc — README.md

<https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/README.md>

[2] vercel-labs/scriptc — docs/content/docs/how-it-works.mdx

<https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/how-it-works.mdx>

[3] vercel-labs/scriptc — docs/content/docs/limitations.mdx

<https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/limitations.mdx>

[4] vercel-labs/scriptc — docs/content/docs/quickstart.mdx

<https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/quickstart.mdx>

[5] vercel-labs/scriptc — docs/content/docs/coverage.mdx

<https://github.com/vercel-labs/scriptc/blob/b97ea1d929593690e8d05f263b6f55b5502532c9/docs/content/docs/coverage.mdx>
