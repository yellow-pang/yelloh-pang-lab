---
title: "rlaope/oh-my-hermes"
repository: "rlaope/oh-my-hermes"
url: "https://github.com/rlaope/oh-my-hermes"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# rlaope/oh-my-hermes

> https://github.com/rlaope/oh-my-hermes

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

oh-my-hermes는 Hermes Agent를 대체하지 않고 요청에 맞는 Skill·모델·작업 흐름을 선택하며 실행과 검증 상태를 구분하는 확장 패키지이다.[1]

## 이 Repository는 무엇인가?

Hermes를 대화와 실행의 기반으로 유지하면서 그 위에 문제 정리, 계획, 조사, 실행·검증 규칙을 얹는 운영 계층으로 설명한다.[1]

Skill은 특정 작업의 지침이고 Workflow는 이를 적용하는 단계별 절차이며, 이 프로젝트는 둘을 요청 분류와 근거 확인 규칙으로 연결한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 작업 성격별 모델과 추론 노력 설정을 순서 있는 후보 목록으로 관리하고, 제공자가 모델을 거부하면 다음 후보를 적용한다.[1]
- 계획·조사·병렬 작업·반복 검증을 위한 Workflow와 선택적인 Codex·Claude Code 작업 위임 경로를 제공한다.[1]
- 검토를 거친 기억 후보를 별도 파일 저장소에 보관하고 출처·검토 기한·입력 길이 예산에 따라 다음 작업에 회상한다.[1]

## 어떻게 동작하는가?

```text
요청의 목적과 제약, 작업 소유자와 완료 조건을 정한다.
↓
관련 Skill·Workflow와 모델을 선택하고 승인된 범위의 작업을 실행하거나 위임한다.
↓
관찰된 실행·검사 근거로 상태를 보고하고 검토된 교훈만 기억에 반영한다.
```

README는 준비된 계획, 실행 중인 작업, 완료했다고 보고된 작업과 검사로 확인된 작업을 서로 다른 상태로 구분한다.[1]

### 확인한 파일 구성

- `pyproject.toml`은 `omh` 명령의 진입점을 `omh.cli:main`으로 지정하고 여러 하위 모듈을 Python 패키지로 묶는다.[2]
- 같은 manifest는 `src/routing`, `src/workflows`, `src/evidence`를 각각 routing·workflows·evidence 패키지에 연결한다.[2]
- 플러그인 배포 데이터에는 `plugin.yaml`, 설정 YAML, 참조 Markdown과 도구 목록 JSON을 포함하도록 선언한다.[2]
- `src/routing/dispatch_evidence.py`는 후보 Skill을 고른 근거를 명시적 호출·직접 조건·구문 일치·약한 근거 등으로 분류한다. 점수 자체를 바꾸는 모듈과는 역할을 나눈다.[3]

## 어떤 기술을 사용하는가?

- `Python`과 `setuptools`: Python 3.11 이상을 요구하는 CLI 패키지이며 빌드 백엔드로 setuptools를 선언한다.[2]
- `Hermes Agent`: 자연어 요청과 기본 실행 루프를 제공하는 호스트이며 OMH는 그 위에서 Skill과 작업 흐름을 조정한다.[1]

## 실제로 어디에 사용할 수 있는가?

- 개인 공개 프로젝트의 조사·계획·수정·검증을 분리하고 각 단계의 완료 근거를 기록하는 방식에 활용할 수 있다.[1]
- 서로 다른 파일을 담당하는 병렬 작업과 검토된 프로젝트 기억을 설계하는 Agent 확장 사례로 학습할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 실행자가 완료를 주장한 상태와 실제 검사 통과를 구분하고, 검증 기록의 재사용에도 환경·명령·수정본 일치를 요구하는 설계가 관찰 지점이다.[1]

## 장점

- 모델 선택과 코드 변경 담당자 선택을 별개의 결정으로 다룬다.[1]
- 기억 후보의 승인·거절·보류를 기록하며 Hermes 자체 기억과 별도 저장소를 구분한다.[1]

## 단점 / 주의점

- MIT 라이선스의 패키지이며 설치 후 설정 과정이 필요하므로 파일만 내려받는 것과 호스트 등록·모델 준비를 구별해야 한다.[1][2]
- 모델 추천 목록은 실제 제공자 접근이나 실행의 증거가 아니며, README의 제품 전체 비교 항목은 측정 실행이 아직 공개되지 않았다고 명시한다.[1]

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

- [NousResearch/hermes-agent](hermes-agent.md)

## 정리

Hermes 위에서 작업 분류·흐름·위임·검증과 기억을 조정하는 확장 패키지이다.[1] 문서의 기능 설명과 모델 추천을 실제 실행 성공이나 일반적인 성능 향상으로 동일시해서는 안 된다.[1]

## Sources

[1] rlaope/oh-my-hermes 공식 README

<https://github.com/rlaope/oh-my-hermes/blob/main/README.md>

[2] rlaope/oh-my-hermes pyproject.toml

<https://github.com/rlaope/oh-my-hermes/blob/main/pyproject.toml>

[3] rlaope/oh-my-hermes src/routing/dispatch_evidence.py

<https://github.com/rlaope/oh-my-hermes/blob/main/src/routing/dispatch_evidence.py>
