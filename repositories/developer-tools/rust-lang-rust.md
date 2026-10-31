---
title: "rust-lang/rust"
repository: "rust-lang/rust"
url: "https://github.com/rust-lang/rust"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# rust-lang/rust

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Rust 언어의 컴파일러, 표준 라이브러리와 문서를 개발하는 중심 저장소다.
Rust로 만든 완성 앱이나 입문 예제 모음이 아니라, Rust 프로그램을 검사하고 실행 가능한 결과로 바꾸는 도구 자체의 소스다.[1]

## 실행 전에 오류를 줄이는 언어

Rust는 성능과 메모리 효율을 유지하면서 메모리·스레드 안전성을 컴파일 시점에 확인하는 것을 강조한다.
소유권은 값의 관리 책임을 정하는 규칙이고, 빌림은 그 값을 다른 코드가 참조하도록 하는 방식이다.
README는 타입 시스템과 소유권 모델을 신뢰성의 근거로 설명하며 Cargo, rustfmt, Clippy 등의 주변 도구도 연결한다.[1]

이 저장소를 읽는 목적은 Rust를 사용하는 것과 Rust가 사용자의 코드를 어떻게 판단하는지 이해하는 것을 구분하는 데 있다.
일반 앱 개발을 시작하려면 README가 연결하는 설치 안내와 The Book이 먼저이며, 컴파일러 전체를 직접 빌드할 필요는 없다.[1]

## 빌림 검사가 코드에 자리 잡는 방식

`compiler/rustc_borrowck/src/lib.rs`는 MIR 타입 검사와 빌림 검사를 담당한다고 명시한다.
MIR은 컴파일러가 소스 코드를 분석하기 좋게 바꾼 중간 표현이다.
`provide`는 컴파일러의 질의 처리 표에 `mir_borrowck`를 등록한다.[2]

`mir_borrowck`는 대상의 MIR을 얻고, 이미 오류에 오염된 본문인지 또는 검사 제외 대상인지 확인한다.
검사가 필요한 본문에는 `BorrowCheckRootCtxt`를 만들고 `do_mir_borrowck`와 `finalize`를 호출한다.
즉, 소유권 검사는 문법 설명에만 존재하는 규칙이 아니라 컴파일러 처리 흐름의 한 단계다.[2]

이 파일 하나가 모든 안전성을 증명하는 것은 아니다.
실제 제약 계산과 진단은 `region_infer`, `type_check`, `diagnostics` 같은 하위 모듈로 나뉘므로 진입점에서 관심 경로를 따라가는 편이 읽기 쉽다.[2]

## 가상 예시: 참조를 사용하는 작은 도구

개인용 텍스트 처리 프로그램에서 문자열의 참조를 여러 함수에 넘기는 상황을 가정한다.
작성자는 코드를 컴파일하고, 빌림 오류가 나오면 값과 참조를 사용하는 범위를 고친다.
이 흐름을 내부에서 이해하려면 프로그램 본문이 MIR 빌림 검사에 들어가고 검사 결과가 확정되는 경로를 살펴볼 수 있다.[1][2]

이 예시는 실제 컴파일 결과나 특정 오류 메시지를 재현한 기록이 아니다.
학습 관점에서는 에러를 없애는 수정과 프로그램이 원하는 결과를 내는지 확인하는 테스트를 별도로 생각해야 한다.

## 컴파일러를 만드는 과정은 별도 문제다

`INSTALL.md`는 `x.py`가 부트스트랩, 즉 기존 컴파일러를 이용해 새 컴파일러를 만드는 과정을 관리한다고 설명한다.
설정은 `bootstrap.toml`로 전달하며, C 컴파일러와 플랫폼별 빌드 도구 등도 필요하다.
Rust로 작성된 컴파일러를 처음부터 만들기 위해 미리 컴파일된 스냅샷을 내려받는 단계가 있다.[3]

따라서 소스 다운로드만으로 오프라인 빌드 준비가 끝났다고 보면 안 된다.
공식 안내 자체도 일반 사용자의 소스 설치를 권하지 않으며, 지원 플랫폼과 LLVM 준비 조건을 먼저 확인하도록 한다.[3]

Rust는 주로 MIT와 Apache-2.0의 이중 라이선스로 배포되지만 일부는 BSD 계열 조건을 따른다.
이름과 로고의 상표 정책도 코드 이용 조건과 별개이므로 재배포 때 `COPYRIGHT`와 관련 고지를 함께 확인해야 한다.[1]

## 직접 읽어볼 자료

1. README의 Why Rust와 Quick Start: 언어의 목표와 사용자용 진입점을 나누어 읽는다.[1]
2. `compiler/rustc_borrowck/src/lib.rs`: `provide` → `mir_borrowck` → 검사 컨텍스트의 순서를 따라간다.[2]
3. `INSTALL.md`: 앱을 빌드하는 것과 Rust 자체를 빌드하는 것의 준비 차이를 확인한다.[3]

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 자료를 확인했다. 설치·빌드·실행은 하지 않았다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] rust-lang/rust — README.md

<https://github.com/rust-lang/rust/blob/main/README.md>

[2] rust-lang/rust — compiler/rustc_borrowck/src/lib.rs

<https://github.com/rust-lang/rust/blob/main/compiler/rustc_borrowck/src/lib.rs>

[3] rust-lang/rust — INSTALL.md

<https://github.com/rust-lang/rust/blob/main/INSTALL.md>
