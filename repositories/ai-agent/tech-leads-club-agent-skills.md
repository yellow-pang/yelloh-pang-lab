---
title: "tech-leads-club/agent-skills"
repository: "tech-leads-club/agent-skills"
url: "https://github.com/tech-leads-club/agent-skills"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# tech-leads-club/agent-skills

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Agent Skills는 코딩 에이전트에 작업 지침과 참고 자료를 전달하는 Skill 카탈로그이며, 이를 선택해 설치하는 CLI와 필요한 자료를 조회하는 MCP 서버를 함께 제공한다.
자체 언어 모델이나 코드를 대신 실행하는 독립 에이전트는 아니다.
실제 행동은 지침을 읽는 에이전트와 그 에이전트가 가진 도구·권한에 달려 있다.[1]

## 지침을 모으는 일과 배포하는 일

반복해서 코드를 검토하거나 기능을 설계할 때 매번 긴 작업 규칙을 새로 작성하면 기준이 빠지기 쉽다.
Skill은 그런 절차를 `SKILL.md`와 참고 문서·템플릿으로 묶은 단위다.
이 저장소는 개별 절차뿐 아니라 어떤 에이전트의 어느 위치에 배치할지를 고르는 설치 경로를 제공한다.[1]

CLI는 카탈로그를 받은 뒤 선택한 항목만 다운로드하고 로컬 캐시에 보관한다.
설치 대상 에이전트, 복사 또는 심볼릭 링크 방식, 사용자 전체 또는 현재 프로젝트 범위를 고르는 구조다.
심볼릭 링크는 다른 경로의 파일을 가리키는 연결이므로 복사본과 업데이트·접근 경계가 다르다.
설치 범위 선택은 단순한 화면 옵션이 아니라 어떤 작업에서 그 지침을 만날지 정하는 선택이다.[1]

MCP는 에이전트가 외부 도구를 정해진 규약으로 호출하는 연결 방식이다.
이 프로젝트의 MCP 서버는 `search_skills`로 찾고 `read_skill`로 본문을 읽으며 `fetch_skill_files`로 필요한 부속 자료를 가져오는 점진적 공개를 설명한다.
모든 내용을 한꺼번에 대화에 붙이지 않고 현재 필요한 자료부터 읽도록 나눈 구조다.[1]

## 같은 카탈로그 안에서도 다른 개입 수준

대표 항목 `tlc-spec-driven`은 기능 요구 정의, 설계, 작업 분해, 실행을 다룬다.
작은 변경에서는 설계 문서나 별도 작업 목록을 생략할 수 있지만 무엇을 만들지 정하고 확인하는 단계는 남긴다.
EARS는 조건과 기대 동작을 일정한 문장 형태로 쓰는 요구사항 표기법으로 사용된다.
요구 ID와 수용 조건을 테스트에 연결해 구현자가 임의로 성공 기준을 바꾸지 못하도록 지시한다.[2]

이 Skill은 단순 조언을 넘어 `.specs/` 아래 상태·요구·검증 파일을 만들고 Python 검증 스크립트를 호출하도록 한다.
구현자와 검증자를 구분하며 각 작업의 로컬 커밋도 절차에 포함한다.
반면 같은 카탈로그의 `coding-guidelines`는 불명확한 가정을 숨기지 않기, 필요한 범위만 바꾸기, 검증 가능한 목표를 세우기 같은 행동 규칙이 중심이다.
따라서 카탈로그 전체를 하나의 동일한 작업 방식으로 취급하면 안 된다.[2][3]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
개인 공개 예제 앱에 입력값 검증 기능을 추가하려는 요청을 생각해 보자.
카탈로그에서 `tlc-spec-driven`을 선택해 읽으면 바로 파일 수정부터 시작하는 대신 어떤 입력을 거부하고 사용자에게 무엇을 보여 줄지를 먼저 정의하도록 안내한다.
작은 기능은 간단한 명세로 시작하고, 저장 상태나 외부 호출처럼 암묵적 요구가 있으면 질문을 통해 범위를 좁히는 방식이다.[2]

처리 과정에서는 수용 조건에서 테스트를 만들고 작업 단위로 구현과 검증을 묶는다.
규모가 큰 경우 `spec.md`, `tasks.md`와 검증 보고서를 연결하지만, 필요하지 않은 빈 문서를 미리 만들지 말라는 규칙도 있다.
최종 산출물은 코드만이 아니라 어떤 조건을 확인했는지의 근거다.
별도 검증자는 조건과 테스트 결과를 대조하고, 테스트가 실제 오류를 잡는지 격리된 복사본에서 확인하도록 지시받는다.[2]

사람이 확인해야 할 것은 “검증 완료” 문구보다 승인한 요구와 검사 근거가 일치하는지, 실제 에이전트가 지원하는 도구로 그 절차가 수행되었는지다.
특히 이 Skill이 작업별 로컬 커밋을 요구한다는 점은 실행 전에 알아야 한다.
여기서는 저장소의 지침을 조사 대상으로 읽었을 뿐 해당 절차를 이 문서 작업에 적용하거나 커밋을 만들지 않았다.[2]

## 보안 소개와 보안 보장의 차이

보안 문서는 설치 경로 검사, 심볼릭 링크 방어, lockfile, 콘텐츠 해시와 감사 로그를 설명한다.
lockfile은 설치된 항목을 기록하는 파일이고 해시는 내용이 바뀌었는지 비교하는 지문 역할을 한다.
이는 설치 파일의 관리 근거이지, Skill이 제안하는 모든 명령의 적절성을 증명하는 장치는 아니다.[4]

또 Snyk Agent Scan을 배포 전에 사용하고 변경된 내용 중심으로 다시 검사한다고 설명한다.
일반적인 fork PR 흐름은 비밀 값에 접근할 수 없어 검사가 실행되지 않으며 모든 PR의 병합 차단에는 별도 merge queue 설정이 필요하다고 명시한다.
README의 검증·안전 표현을 모든 기여와 모든 사용자 환경에 대한 무조건적인 안전 보장으로 옮기지 않는 이유다.[4]

MCP 서버는 문서상 레지스트리에 쓰지 않는 조회 전용이지만, 조회한 Skill을 실행하는 에이전트까지 읽기 전용이 되는 것은 아니다.
실제 Skill에 파일 작성·브라우저·외부 API 또는 커밋 지시가 있는지 별도로 확인해야 한다.
특정 에이전트로 설치할 수 있다는 사실도 그 에이전트에서 모든 보조 스크립트와 위임 기능이 동일하게 작동한다는 시험 결과는 아니다.[1][2][4]

라이선스도 나뉜다.
CLI 등 소프트웨어 엔진은 MIT, 관리자가 만든 Skill은 별도 표기가 없다면 CC-BY-4.0, 외부 저자의 항목은 원래 라이선스를 따른다고 README가 안내한다.
전체 저장소를 한 라이선스로 단순화하지 말고 가져갈 항목의 저자·표기 조건을 확인해야 한다.[1]

## 직접 읽어볼 자료

- [README의 How It Works와 MCP Server](https://github.com/tech-leads-club/agent-skills/blob/main/README.md)
  설치 방식과 조회 방식을 구분해 읽는다.
  검색한 지침을 읽는 일과 에이전트 설정에 파일을 배치하는 일이 언제 일어나는지 확인할 수 있다.
- tlc-spec-driven의 실행 계약: <https://github.com/tech-leads-club/agent-skills/blob/main/packages/skills-catalog/skills/(development)/tlc-spec-driven/SKILL.md>
  자동 생략되는 단계, 요구사항 검사, 별도 검증자와 로컬 커밋 규칙을 먼저 본다.
  “절차 모음”이 실제로 어떤 파일과 행동을 요구하는지 드러난다.
- [Security Policy](https://github.com/tech-leads-club/agent-skills/blob/main/SECURITY.md)
  설치 보호와 배포 검사 조건을 읽고 fork PR의 예외를 확인한다.
  콘텐츠 변조 탐지와 지침 자체의 정확성을 서로 다른 문제로 이해하는 데 필요한 자료다.

## 정리

Agent Skills는 작업 지침을 재사용하고 선택적으로 배포하는 기반이다.
짧은 행동 원칙과 커밋·검증까지 포함하는 큰 절차가 함께 있으므로 개별 Skill의 실행 범위와 라이선스를 먼저 읽어야 한다.

## 자료 확인 범위

2026-09-27 기준 README, 루트 구성, 보안 정책과 대표 Skill 두 개를 읽었다.
CLI·MCP 서버 설치와 Skill 실행, 보안 스캐너 검증은 하지 않았다.

## 사용자 생각

아래는 실제 판단을 넣기 전까지 비워두는 영역입니다.

- [ ] 지금 보려는 이유가 분명한가?
- [ ] 내 작업 환경에서 바로 적용할 수 있을까?
- [ ] 실험 10~20분으로 검증 가능한 값이 있는가?

## 나중에 할 일

- [ ] README 전체 읽기
- [ ] 설치/실행 예시가 있는지 확인
- [ ] 장단점, 주의점, 대체안 비교
- [ ] 블로그 글 제목/개인 결론 반영

## Sources

[1] tech-leads-club/agent-skills — README.md

<https://github.com/tech-leads-club/agent-skills/blob/main/README.md>

[2] tech-leads-club/agent-skills — packages/skills-catalog/skills/(development)/tlc-spec-driven/SKILL.md

<https://github.com/tech-leads-club/agent-skills/blob/main/packages/skills-catalog/skills/(development)/tlc-spec-driven/SKILL.md>

[3] tech-leads-club/agent-skills — packages/skills-catalog/skills/(development)/coding-guidelines/SKILL.md

<https://github.com/tech-leads-club/agent-skills/blob/main/packages/skills-catalog/skills/(development)/coding-guidelines/SKILL.md>

[4] tech-leads-club/agent-skills — SECURITY.md

<https://github.com/tech-leads-club/agent-skills/blob/main/SECURITY.md>
