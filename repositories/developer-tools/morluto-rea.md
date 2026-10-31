---
title: "morluto/rea"
repository: "morluto/rea"
url: "https://github.com/morluto/rea"
category: "developer-tools"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["developer-tools", "starred-draft"]
---

# morluto/rea

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

REA는 Agent가 바이너리·JavaScript 애플리케이션·웹사이트 등을 조사하도록 분석 도구를 연결하는 MCP 서버와 CLI다.[1]
MCP는 Agent가 외부 도구를 호출하고 결과를 받는 연결 규약이다.
REA 자체가 모든 추론을 끝내는 언어 모델이거나, 어떤 프로그램이든 원본 소스로 완벽 복원하는 도구라는 뜻은 아니다.[1]

## 관찰한 사실과 추론을 함께 전달하기

애플리케이션의 동작을 이해하려면 화면뿐 아니라 모듈 관계, 호출 경로, 실행 중 관찰을 연결해야 한다.
REA는 Agent의 질문에 분석 결과와 근거를 반환하고, Agent가 다음 질문이나 구현·검증으로 이어가도록 구성한다.
네이티브 분석에는 Hopper·Ghidra·IDA 같은 도구를 연결할 수 있고 정적 JavaScript 분석은 별도 네이티브 엔진 없이 가능하다고 설명한다.[1]

루트에는 `src`, `bridge`, `native`, `docs`, `tests`가 함께 있다.
분석 연결부와 애플리케이션 코드, 안내 문서를 분리해서 읽어야 하며, 설치 안내만으로 모든 대상의 준비가 끝났다고 보면 안 된다.[2][1]

## JavaScript 기능 추적은 기존 Evidence에서 출발한다

대표 문서 `docs/javascript-application-workflows.md`는 분석 결과를 JavaScript Application Graph로 보존하는 방식을 설명한다.
그래프는 모듈 같은 항목과 항목 사이 관계를 연결한 자료 구조이며, 원본 산출물의 digest와 그래프 ID에 묶인다.
정적 관찰·수동적 실행 관찰·관계 추론·알 수 없는 사실을 서로 다른 근거 상태로 유지한다.[3]

`inspect_analysis_view`는 이미 완료된 Evidence에서 요약이나 모듈 정보를 보여주는 투영 도구다.
반면 `trace_application_feature`와 관련 비교 작업은 앞서 생성된 분석 Evidence를 받아 관계를 추적한다.
이 후속 작업 자체는 애플리케이션 코드를 실행하거나 프로세스에 붙거나 새 네이티브 분석 엔진을 여는 동작이 아니다.[3]

문서는 의미 관계 그래프가 없을 때 이를 빈 그래프로 취급하지 말고 unavailable로 보고한다고 설명한다.
또 조회 페이지가 끝났다고 해서 불완전하게 분석된 앱 전체에 해당 기능이 없다고 결론 내릴 수는 없다고 명시한다.
“찾지 못했다”와 “존재하지 않는다”를 나누는 것이 이 조사 방식의 중요한 특징이다.[3]

## 가상 시나리오: 공개 Electron 예제의 검색 흐름 읽기

분석 권한이 있는 공개 Electron 예제에서 검색 요청이 어떤 모듈로 전달되는지 알고 싶은 상황을 가정한다.
먼저 JavaScript 애플리케이션 분석으로 Evidence를 만들고 그 결과의 실제 ID를 조회 요청에 넣는다.
관심 모듈의 참조와 소스 범위를 확인한 뒤 기능 추적으로 연결 관계와 미확인 부분을 살펴본다.[1][3]

그 결과를 바탕으로 설명을 작성할 수 있지만 정적 관계가 런타임 실행을 증명하는 것은 아니다.
원본의 동작을 재현한 새 구현은 별도의 테스트로 확인해야 한다.
여기서는 대상 앱 분석이나 새 구현을 실행하지 않았다.[3]

## 설정 변경과 분석 범위를 먼저 승인하기

README의 setup은 Agent 설정에 MCP와 작업 지침을 추가하고 기존 설정을 백업하는 절차다.
일부 네이티브 도구 설치도 승인 후 선택할 수 있으므로 안내를 읽는 것과 setup 실행은 별개다.[1]
Node 버전과 운영체제·대상별 외부 도구 요구사항을 확인해야 하며, 모든 분석이 동일한 준비 조건을 갖지는 않는다.[1]

REA의 MIT 라이선스는 분석 대상 앱의 사용 허가를 대신하지 않는다.
자신이 소유하거나 허가받은 자료를 대상으로 삼고, 배포·재구현할 때 대상의 저작권과 계약을 따로 검토해야 한다.[4]
도구가 로컬 분석을 한다는 설명과 Agent가 결과를 처리하는 경로도 구분해서 설정해야 한다.[1]

## 직접 읽어볼 자료

- [1] How REA works와 대상별 요구사항에서 Agent·REA·외부 분석기의 역할을 나눈다.
- [3] Retained module facts의 `evidence_id`, 페이지 범위와 unknown 관련 설명을 읽는다.
- [3][4] 사실의 출처 보존과 결과 재사용 권한을 별도 확인 항목으로 둔다.

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

[1] morluto/rea — README

<https://github.com/morluto/rea/blob/main/README.md>

[2] morluto/rea — 루트 파일 목록

<https://api.github.com/repos/morluto/rea/contents>

[3] morluto/rea — docs/javascript-application-workflows.md

<https://github.com/morluto/rea/blob/main/docs/javascript-application-workflows.md>

[4] morluto/rea — LICENSE

<https://github.com/morluto/rea/blob/main/LICENSE>
