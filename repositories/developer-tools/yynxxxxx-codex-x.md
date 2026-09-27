---
title: "yynxxxxx/Codex-X"
repository: "yynxxxxx/Codex-X"
url: "https://github.com/yynxxxxx/Codex-X"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# yynxxxxx/Codex-X

> https://github.com/yynxxxxx/Codex-X

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Codex-X는 Codex 데스크톱 앱과 CLI의 프롬프트, 모델 제공자, 로컬 대화 기록, 확장 설정을 한 화면에서 관리하는 별도 데스크톱 도구이다.[1]

## 이 Repository는 무엇인가?

여러 파일에 흩어진 Codex 설정을 시각적으로 확인하고 바꾸기 위한 관리 화면이며, 자체 언어 모델을 제공하는 제품으로 소개되지는 않는다.[1]

Provider는 모델 API를 제공하는 연결 대상이고, Skill은 재사용 지침, MCP는 Agent와 외부 도구를 연결하는 설정 대상으로 다룬다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Markdown 프롬프트를 가져오거나 편집하고, 기존 지침에 덧붙이는 방식과 교체하는 방식으로 활성화할 수 있다.[1]
- 공식 로그인 프로필과 제삼자 API 설정을 저장·복제·전환하고, 연결 확인과 모델 조회·테스트 기능을 제공한다.[1]
- 세션을 검색하고 프로젝트 경로로 묶으며, Skill·MCP의 활성화 상태와 날짜·모델별 로컬 Token 사용량을 관리한다.[1]

## 어떻게 동작하는가?

```text
기존 Codex 설정과 로그인 정보, 선택한 프롬프트를 읽는다.
↓
화면에서 변경 대상을 선택하고 중요한 쓰기 전에 백업한다.
↓
변경한 지침·Provider·확장 설정을 Codex 설정 디렉터리에 반영한다.
```

단순 설정 미리보기만 제공하는 도구가 아니라 실제 파일을 갱신하며, 기본 대상은 `~/.codex/config.toml`과 `~/.codex/auth.json`이다.[1]

### 확인한 파일 구성

- 루트 `package.json`은 개발·빌드·형 검사 작업을 `apps/desktop`으로 전달하는 작업 공간 진입점이다.[2]
- `apps/desktop/package.json`에는 Tauri 앱 구동, Vite 화면 빌드, TypeScript 검사 명령과 React 의존성이 선언되어 있다.[3]
- `examples/`는 앱이 GitHub에서 동기화하는 프롬프트 템플릿의 공급 경로로 README에 설명되어 있다.[1]

## 어떤 기술을 사용하는가?

- `Tauri 2·Rust`: 데스크톱 앱의 기반과 백엔드를 맡고, `React·TypeScript·Vite`는 화면 개발에 사용한다.[1][3]
- `SQLite·rusqlite`: 앱 자체 로컬 데이터를 저장하며, Codex 설정 편집에는 TOML과 JSON을 사용한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 Codex 환경에서 서로 다른 작성·개발용 프롬프트와 API 연결을 분리해 관리하는 사례를 학습할 수 있다.[1]
- 로컬 세션의 프로젝트별 정리와 Token 사용량 확인을 하나의 관리 화면에서 수행할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 프롬프트 본문만 모으는 저장소가 아니라, 활성화 방식·설정 쓰기·백업·로컬 캐시를 함께 다루는 관리 계층이라는 점이 구체적이다.[1]

## 장점

- 프롬프트·Provider·Skill·MCP 설정을 같은 앱에서 확인하므로 관리 대상별 화면과 파일의 관계를 살펴볼 수 있다.[1]
- 사용자 프롬프트를 별도로 유지하고 동기화한 템플릿을 캐시하는 기능을 제공한다.[1]

## 단점 / 주의점

- 세션 영구 삭제는 복구할 수 없다고 경고하며, 프롬프트 교체와 인증 설정 변경도 기존 Codex 환경에 영향을 준다.[1]
- README는 MIT 라이선스와 합법적·허가된 연구 범위를 명시하고, 일부 템플릿을 제한 해제·역공학 용도로 소개하므로 효과나 안전성을 보장하는 기능으로 해석하지 않아야 한다.[1]
- 1M 문맥 설정은 모델 지원이 필요하며, 미서명·미공증 macOS 배포물에 관한 경고도 있으므로 표시된 옵션과 실제 지원을 구분해야 한다.[1]

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

## 관련 Repository

- [openai/codex](../ai-agent/openai-codex.md)

## 정리

Codex-X는 기존 Codex 환경의 지침과 연결·세션을 시각적으로 관리하고 실제 설정에 반영하는 도구이다.[1] 설정 백업과 삭제·교체의 영향을 확인하며 사용해야 하고, 모델 성능이나 모든 템플릿의 효과를 보증하는 제품은 아니다.[1]

## Sources

[1] yynxxxxx/Codex-X 공식 README

<https://github.com/yynxxxxx/Codex-X/blob/main/README.md>

[2] yynxxxxx/Codex-X package.json

<https://github.com/yynxxxxx/Codex-X/blob/main/package.json>

[3] yynxxxxx/Codex-X apps/desktop/package.json

<https://github.com/yynxxxxx/Codex-X/blob/main/apps/desktop/package.json>
