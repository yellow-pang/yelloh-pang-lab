---
title: "anthropics/financial-services"
repository: "anthropics/financial-services"
url: "https://github.com/anthropics/financial-services"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/financial-services

> https://github.com/anthropics/financial-services

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude for Financial Services는 금융 분석 자료의 초안을 만들고 사람이 검토하도록 설계한 에이전트, 작업 지침, 데이터 연결 모음이다.[1]

## 이 Repository는 무엇인가?

작업 흐름을 맡는 에이전트 플러그인과 분야별 Skill 묶음을 제공하며, Skill은 Claude가 참고할 전문 지식과 단계별 절차이다.[1]

Cowork 플러그인과 Managed Agents API 배포가 같은 지침을 참조하며, Claude Code에서 설치하는 방법도 안내한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Model Builder는 종목 식별자와 가정을 받아 현금흐름을 현재 가치로 환산하는 DCF 등의 계산 모델을 Excel 통합 문서로 작성하도록 지시한다.[2]
- `audit-xls`는 선택 범위·시트·전체 모델에 따라 수식 오류와 재무제표 연결을 점검하고, 먼저 문제를 보고한 뒤 요청 시 수정하도록 한다.[3]
- 분야별 명령과 데이터 연결을 제공하며, 완성된 분석 흐름 대신 필요한 Skill 묶음만 선택할 수도 있다.[1]

## 어떻게 동작하는가?

```text
Model Builder에 대상 종목, 모델 종류와 가정을 제공한다.
↓
자료 수집, 해당 Skill을 통한 모델 작성, 수식 검사와 가정 변화에 따른 민감도 분석을 진행한다.
↓
계산 결과와 입력을 연결한 Excel 모델을 사용자 검토 대상으로 제시한다.
```

위 흐름은 Model Builder의 대표 사례이며, 지침은 작성·검사 뒤 멈추고 민감도 분석 전에 사용자 승인을 받도록 요구한다.[2]

### 확인한 파일 구성

- `plugins/agent-plugins/model-builder/agents/model-builder.md`는 역할, 허용 도구와 사용 Skill을 정의한다.[2]
- `plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md`는 검사 범위와 결과 보고 형식을 정의한다.[3]
- `plugins/vertical-plugins/financial-analysis/.mcp.json`에는 외부 데이터 서비스의 HTTP 연결 설정이 있으나, 수집본에 JSON 구문 문제가 있다.[4]

## 어떤 기술을 사용하는가?

- `Markdown·YAML·JSON`: 에이전트 지침과 배포 구성을 파일로 표현한다.[1]
- `MCP`: Claude와 외부 데이터 도구를 연결하는 방식이다.[1]
- `Office JS·Python/openpyxl`: DCF Skill은 Excel 내부 작업과 독립적인 파일 생성 환경을 구분해 사용하도록 명시한다.[5]

## 실제로 어디에 사용할 수 있는가?

- 공개 재무자료로 모델의 입력 출처와 계산 수식을 연결하는 절차를 학습하는 데 참고할 수 있다.[2][5]
- 개인 학습용 스프레드시트에서 수식 오류와 가정 검토 항목을 정리하는 지침으로 읽을 수 있다.[3]

## 왜 주목할 만한가?

학습 관점에서는 숫자를 만드는 단계뿐 아니라 입력 출처 표시, 수식 검사, 사람의 승인을 작업 경계로 명시한 점이 관찰점이다.[2][5]

## 장점

- 에이전트가 필요한 Skill을 묶고 분야별 원본과 동기화하는 구성을 제공한다.[1]
- 검사 지침이 오류 위치·중요도·수정 제안을 구분하고 임의 수정을 금지한다.[3]

## 단점 / 주의점

- 투자·법률·세무·회계 자문이 아니며, 거래 실행이나 최종 승인 대신 전문가 검토를 위한 초안을 만든다고 명시한다.[1]
- 데이터 제공자의 구독이나 API 키가 필요할 수 있고, 하위 에이전트 위임은 Research Preview로 안내한다.[1]
- 수집한 `.mcp.json`은 `box` 항목 앞 구분자 등 구문이 불완전하며 표준 JSON 파싱도 실패했으므로, 연결 설정을 그대로 사용할 수 있다고 단정할 수 없다.[4]

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

- [anthropics/knowledge-work-plugins](anthropics-knowledge-work-plugins.md)

## 정리

분석 초안 작성과 검토 절차를 재사용하는 참조 에이전트 모음이다.[1] 사람의 승인과 데이터 접근 조건이 전제되며 수집 시점의 연결 설정 오류도 구별해야 한다.[1][4]

## Sources

[1] anthropics/financial-services 공식 README

<https://github.com/anthropics/financial-services/blob/main/README.md>

[2] anthropics/financial-services plugins/agent-plugins/model-builder/agents/model-builder.md

<https://github.com/anthropics/financial-services/blob/main/plugins/agent-plugins/model-builder/agents/model-builder.md>

[3] anthropics/financial-services plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md

<https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md>

[4] anthropics/financial-services plugins/vertical-plugins/financial-analysis/.mcp.json

<https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/.mcp.json>

[5] anthropics/financial-services plugins/vertical-plugins/financial-analysis/skills/dcf-model/SKILL.md

<https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/skills/dcf-model/SKILL.md>
