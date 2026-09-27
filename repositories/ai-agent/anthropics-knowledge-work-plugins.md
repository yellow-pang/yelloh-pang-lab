---
title: "anthropics/knowledge-work-plugins"
repository: "anthropics/knowledge-work-plugins"
url: "https://github.com/anthropics/knowledge-work-plugins"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/knowledge-work-plugins

> https://github.com/anthropics/knowledge-work-plugins

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude가 문서 작성, 자료 검색, 데이터 분석 같은 역할별 작업을 수행하도록 지침과 도구 연결을 묶은 플러그인 모음이다.[1]

## 이 Repository는 무엇인가?

독립적인 AI 모델이나 실행 프로그램이 아니라 Claude Cowork를 중심으로 만들고 Claude Code에서도 사용할 수 있도록 제공하는 확장 자료이다.[1]

Skill은 특정 분야의 지식과 작업 순서를 담은 지침이며, 이 저장소는 이를 명시적 명령과 외부 서비스 연결 설정에 결합한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 데이터 분석, 생산성, 문서 검색 등 역할별 플러그인을 선택해 필요한 작업 지침을 도입할 수 있다.[1]
- 상황에 맞춰 자동으로 참고하는 Skill과 사용자가 직접 호출하는 슬래시 명령을 구분한다.[1]
- MCP라는 AI 도구 연결 규약을 통해 문서 서비스, 데이터 저장소 등 외부 도구를 연결하도록 구성한다.[1]
- 데이터 플러그인의 `validate-data`는 계산·집계·차트와 결론의 근거를 검토하고, 공유 가능 여부와 개선점을 보고하도록 지시한다.[2]

## 어떻게 동작하는가?

```text
사용할 역할의 플러그인과 필요한 도구 연결을 선택한다.
↓
Claude가 관련 Skill을 참고하거나 사용자가 역할별 명령을 호출한다.
↓
선택한 지침과 연결 도구를 바탕으로 질의 작성, 문서 초안, 분석 결과 등의 작업을 수행한다.
```

이 흐름은 README가 설명하는 플러그인 사용 모델이며, 플러그인 자체가 Claude와 별도로 작업을 실행하는 구조로 설명되지는 않는다.[1]

### 확인한 파일 구성

- `README.md`는 역할별 플러그인 목록과 사용 방법, 사용자 정의 방법을 설명한다.[1]
- `data/README.md`는 데이터 분석 플러그인의 사용 흐름을 설명하며, 연결된 데이터베이스가 없어도 CSV·Excel이나 SQL 결과를 입력할 수 있다고 안내한다.[3]
- `data/skills/validate-data/SKILL.md`는 분석 방법·계산·표현의 검증 순서와 보고서 형식을 정의한다.[2]

## 어떤 기술을 사용하는가?

- `Markdown`과 `JSON`: 지침과 설정을 파일로 표현하며 README는 별도 빌드나 자체 인프라가 필요 없는 구성을 설명한다.[1]
- `MCP`: 플러그인이 필요한 외부 도구에 접근하는 연결 방식이다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 학습용 공개 데이터에 대해 SQL 질의를 작성하고 시각화·분석 지침을 살펴보는 데 활용할 수 있다.[1]
- 생산성 플러그인의 작업·일정 관리 지침이나 플러그인 제작 방식을 읽으며 역할별 AI 확장의 구성을 학습할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 자동으로 적용되는 지식과 사용자가 직접 요청하는 행동을 분리한 설계가 관찰 지점이다.[1]

## 장점

- 파일 기반 구성이라 지침을 읽고 연결 설정과 작업 순서를 별도로 수정할 수 있다.[1]
- 역할별 기본 지침을 제공하면서 기존 연결 교체와 새 플러그인 제작을 함께 안내한다.[1]

## 단점 / 주의점

- README는 기본 플러그인을 범용 출발점으로 설명하므로 구체적인 용어와 작업 방식은 사용 목적에 맞춰 조정해야 한다.[1]
- 자체 빌드가 없다는 설명은 Claude 사용 환경이나 연결 대상 서비스가 불필요하다는 뜻이 아니다.[1]
- 데이터 README의 `/validate` 표기와 실제 Skill의 `validate-data` 이름이 달라, 설치 후 제공되는 명령을 확인해야 한다.[3][2]

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

- [anthropics/financial-services](anthropics-financial-services.md)

## 정리

역할별 지침, 명령, 연결 설정을 묶어 Claude의 작업 방식을 확장하는 자료이다.[1] 실제 활용 범위는 선택한 플러그인과 사용자 정의, 연결된 도구에 따라 달라진다.[1]

## Sources

[1] anthropics/knowledge-work-plugins 공식 README

<https://github.com/anthropics/knowledge-work-plugins/blob/main/README.md>

[2] anthropics/knowledge-work-plugins data/skills/validate-data/SKILL.md

<https://github.com/anthropics/knowledge-work-plugins/blob/main/data/skills/validate-data/SKILL.md>

[3] anthropics/knowledge-work-plugins data/README.md

<https://github.com/anthropics/knowledge-work-plugins/blob/main/data/README.md>
