---
title: "SnailSploit/Claude-Red"
repository: "SnailSploit/Claude-Red"
url: "https://github.com/SnailSploit/Claude-Red"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# SnailSploit/Claude-Red

> https://github.com/SnailSploit/Claude-Red

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude-Red는 허가된 보안 연구와 훈련을 위해 Claude가 참고할 분야별 지침을 `SKILL.md`로 모아 둔 라이브러리이다.[1][2]

## 이 Repository는 무엇인가?

Skill은 AI가 특정 작업을 할 때 읽는 지침 문서이며, 이 저장소는 독립적인 보안 스캐너보다 Claude에 방법론을 제공하는 문서 모음에 해당한다.[1]

웹·인증·클라우드·무선·코드 검토·보고서 작성 등으로 지식을 나누고, 대화 주제에 맞는 지침을 선택적으로 불러오는 사용 방식을 설명한다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 분야별 목록과 개별 Skill을 제공하므로 특정 보안 주제를 중심으로 자료를 찾아 읽을 수 있다.[1]
- 보고서 작성 Skill은 비전공자용 요약, 기술적 발견 사항, 위험도 근거, 개선 조치와 재검증 기록을 나누어 설명한다.[3]
- 보고서의 증거 관리에는 자격증명 가리기, 시간 기록, 산출물 관리와 범위·한계·가정의 명시가 포함된다.[3]

## 어떻게 동작하는가?

```text
명시적으로 허가된 실습 범위와 학습 주제 정의.
↓
해당 주제의 Skill 문서를 Claude의 지침으로 제공.
↓
방법론에 따른 검토 내용과 근거 중심의 보고서 정리.
```

이는 문서 기반 사용 흐름이며, README는 Claude Skills 환경뿐 아니라 Claude.ai의 지침에 내용을 직접 넣는 경로도 안내한다.[1]

### 확인한 파일 구성

- `Skills/utility/offensive-reporting/SKILL.md`는 YAML 메타데이터와 보고서 구성·증거 관리 지침을 담은 실제 Skill이다.[3]
- `CONTRIBUTING.md`는 `Skills/<category>/<skill-folder>/SKILL.md` 형식, 이름 규칙과 검토 기준을 정의한다.[4]
- `install.sh`는 전체 또는 선택한 범주의 문서를 대상 디렉터리에 복사하는 스크립트이며, 별도 공격 엔진을 구동하는 설치기는 아니다.[5]

## 어떤 기술을 사용하는가?

- `Markdown`·`SKILL.md`: Claude가 읽을 전문 지식과 작업 지침을 담는 기본 형식이다.[1]
- `YAML frontmatter`: 문서 앞부분의 이름·설명 메타데이터로, 기여 안내는 이를 대화 주제와 Skill을 연결하는 필수 형식으로 설명한다.[4]
- `Bash`: 설치 스크립트가 범주 선택과 파일 복사를 처리하는 데 사용한다.[5]

## 실제로 어디에 사용할 수 있는가?

- CTF처럼 문제 풀이용으로 허가된 훈련 환경에서 보안 주제의 분류와 방법론을 공부할 수 있다.[1][2]
- 개인 학습용 모의 보고서를 작성하며 위험도와 증거, 개선 조치와 재검증 항목을 구분하는 연습에 참고할 수 있다.[3]

## 왜 주목할 만한가?

학습 관점에서는 기술 목록뿐 아니라 비전공자에게 설명하는 요약과 검증 가능한 증거 정리를 별도 Skill로 다룬다는 점이 관찰점이다.[3]

## 장점

- 주제별 문서 선택과 전체 범주 선택을 모두 제시하여 필요한 지침만 살펴볼 수 있다.[1][5]
- 기여 규칙은 출처 표시, 실제 피해 대상·비밀정보 배제와 검토를 요구한다.[4]

## 단점 / 주의점

- 보안 정책은 명시적인 서면 허가와 정해진 범위가 있는 테스트를 전제로 하며, 무단 접근 용도로 사용해서는 안 된다고 명시한다.[2]
- MIT 라이선스이지만 기술 사용 허가를 대신하지 않으며, 보고서 작성 시 자격증명과 개인 데이터의 노출을 피하도록 안내한다.[1][2][3]
- 기여 안내는 YAML 메타데이터를 필수로 요구하지만 확인한 `offensive-bug-identification` 문서는 일반 Markdown 제목으로 시작하므로 모든 Skill의 형식이 같다고 볼 수 없다.[4][6]

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

- [zhaoxuya520/reverse-skill](zhaoxuya520-reverse-skill.md)

## 정리

Claude-Red는 허가된 보안 학습·검토를 돕는 지침 모음이며 독립적인 실행 도구와 구분해야 한다.[1][2] 적용 범위와 민감한 증거 처리를 명확히 하고 문서별 메타데이터 차이를 확인할 필요가 있다.[2][3][6]

## Sources

[1] SnailSploit/Claude-Red 공식 README

<https://github.com/SnailSploit/Claude-Red/blob/main/README.md>

[2] SnailSploit/Claude-Red SECURITY.md

<https://github.com/SnailSploit/Claude-Red/blob/main/SECURITY.md>

[3] Claude-Red Skills/utility/offensive-reporting/SKILL.md

<https://github.com/SnailSploit/Claude-Red/blob/main/Skills/utility/offensive-reporting/SKILL.md>

[4] SnailSploit/Claude-Red CONTRIBUTING.md

<https://github.com/SnailSploit/Claude-Red/blob/main/CONTRIBUTING.md>

[5] SnailSploit/Claude-Red install.sh

<https://github.com/SnailSploit/Claude-Red/blob/main/install.sh>

[6] Claude-Red Skills/fuzzing/offensive-bug-identification/SKILL.md

<https://github.com/SnailSploit/Claude-Red/blob/main/Skills/fuzzing/offensive-bug-identification/SKILL.md>
