---
title: "cloudflare/security-audit-skill"
repository: "cloudflare/security-audit-skill"
url: "https://github.com/cloudflare/security-audit-skill"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# cloudflare/security-audit-skill

> https://github.com/cloudflare/security-audit-skill

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

security-audit-skill은 코딩 에이전트에 소스 기반 보안 조사, 독립 검증, 기록과 보고 절차를 제공하는 지침 모음이다.[1]

## 이 Repository는 무엇인가?

독립 실행형 보안 스캐너가 아니라 기존 에이전트가 읽는 Skill이며, 조정 역할과 조사·검증 역할을 나누어 한 저장소를 검토한다.[1][2]

신뢰 경계는 서로 다른 권한을 구분하는 선을 뜻하며, 이 Skill은 단순한 권장사항 누락보다 실제 경계 위반과 그 결과를 근거로 판단하도록 요구한다.[2]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 구조와 입력 경로를 조사한 뒤 점검 범위를 ledger라는 기록표로 관리하고, 별도 비평 역할이 누락된 범위를 확인한다.[1]
- 발견 담당자와 다른 검증자가 후보를 반증하려고 검토하며, 최종 기록도 독립적으로 다시 확인한다.[1]
- 결과를 `confirmed`, `needs_validation`, `rejected`로 구분하고 JSON 검증 후 보고서를 생성한다.[1]
- 일반 보안 질문에는 guidance 모드를 사용하고, 명시적인 전체 감사나 보고서 요청에만 전체 절차와 파일 생성을 적용한다.[2]

## 어떻게 동작하는가?

```text
검토 권한이 있는 소스와 요청 범위·사용 모드를 정한다.
↓
전체 감사에서는 구조 파악, 범위별 조사, 후보 검증과 최종 기록 검증을 진행한다.
↓
검증된 JSON 기록과 점검 범위에서 보고서 및 추가 검증 필요 목록을 만든다.
```

위 흐름은 방어적 검토 절차의 요약이며, 자료를 읽는 것만으로 전체 감사를 실행하거나 파일을 생성할 권한이 생기는 것은 아니다.[2]

### 확인한 파일 구성

- `skills/security-audit/SKILL.md`는 모드 선택, 역할 구분, 실행 격리, 출력 소유권과 전체 절차를 정의한다.[2]
- `skills/security-audit/validate-findings.cjs`는 `findings.json`을 보고서 스키마와 대조하고, 스키마 검사 외에 결과별 추가 조건도 확인하도록 작성되어 있다.[3]
- README는 구조 조사, 후보 조사, 검증·보고 지침과 분야별 보조 문서를 역할별로 나누어 소개한다.[1]

## 어떤 기술을 사용하는가?

- `Markdown Skill`: 에이전트가 따르는 역할·판정 기준·작업 순서를 문서로 전달한다.[2]
- `Node.js`: 외부 의존성 없는 findings·coverage 기록 검증기를 실행하는 데 필요한 환경이다.[1]
- `JSON Schema`: 결과 기록의 필드와 형태를 검사하는 기준이며, findings 검증기가 필요한 키워드를 직접 처리한다.[3]

## 실제로 어디에 사용할 수 있는가?

- 본인이 소유하거나 명시적으로 검토를 허가받은 코드에서 소스 근거와 확정·미확정 판단을 구분하는 방어적 학습에 활용할 수 있다.[2]
- 개인 공개 프로젝트의 보안 검토 기록을 구조화하고, 이전 결과와 현재 소스의 차이를 다시 검증하는 절차를 학습할 수 있다.[1][2]

## 왜 주목할 만한가?

학습 관점에서는 취약점 후보를 많이 나열하는 것보다 점검 범위와 미해결 사실, 독립 검증의 이력을 관리하는 설계가 관찰 대상이다.[1][2]

## 장점

- 미확정 항목에는 정확히 남은 질문을 적고 심각도를 부여하지 않아, 의심과 확정 결과를 구분한다.[1]
- 공유 출력은 조정 에이전트만 작성하고 조사자별 임시 공간을 분리하도록 규정한다.[2]

## 단점 / 주의점

- 도구 사용과 병렬 하위 에이전트를 지원하는 모델·실행 환경, Node.js가 필요하며 프로젝트 라이선스는 MIT이다.[1]
- 대상 코드 실행에는 외부망 차단·허용 목록 환경 변수·자원 제한·쓰기 경로 제한을 강제하는 OS 격리가 필요하고, 조건이 없으면 실행하지 않도록 한다.[2]
- 실서비스·다른 사용자의 데이터·실제 인증 정보를 조사 대상으로 삼지 않으며, 더미 데이터와 최소한의 비밀 없는 결과만 취급하도록 요구한다.[2]

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

## 정리

이 저장소는 보안 검토의 역할 분리와 근거 중심 판정·보고를 기존 에이전트에 제공한다.[1][2] 한 번의 실행이 전체 안전성을 증명하지 않으며, 격리 조건과 남은 점검 범위를 명시해야 한다.[2]

## Sources

[1] cloudflare/security-audit-skill 공식 README

<https://github.com/cloudflare/security-audit-skill/blob/main/README.md>

[2] cloudflare/security-audit-skill skills/security-audit/SKILL.md

<https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/SKILL.md>

[3] cloudflare/security-audit-skill skills/security-audit/validate-findings.cjs

<https://github.com/cloudflare/security-audit-skill/blob/main/skills/security-audit/validate-findings.cjs>
