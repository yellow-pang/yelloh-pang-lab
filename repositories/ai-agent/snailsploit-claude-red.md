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

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude-Red는 보안 주제별 방법론을 `SKILL.md`로 정리한 지침 라이브러리다.
독립적인 취약점 스캐너나 자동 공격 프로그램이 아니라, Claude가 관련 요청을 처리할 때 참고하도록 만든 문서 모음이다.
이 글은 명시적으로 허가된 실습과 방어적 보고서 작성 범위만 다룬다.[1][2]

## 발견 내용을 고칠 수 있는 기록으로 바꾸기

보안 검토에서 이상한 동작을 발견하는 것과 다른 사람이 수정할 수 있게 설명하는 것은 다른 일이다.
어떤 환경에서 무엇을 관찰했는지, 영향이 어디까지인지, 확인하지 못한 부분은 무엇인지 남기지 않으면 강한 표현만 있는 보고서가 되기 쉽다.
Claude-Red는 여러 보안 분야의 지침과 함께 보고서 작성용 Skill도 별도로 제공한다.[1][3]

README의 주제 분류는 웹, 인증, 클라우드, 모바일, 코드 검토, 보고서 같은 서로 다른 표면을 나눈다.
각각을 대화 주제에 맞춰 불러오는 방식으로 소개하므로 모든 내용을 항상 입력에 넣는 자료집과는 사용 의도가 다르다.
다만 지침을 넣었다고 모델의 전문성이나 사실 정확성이 자동으로 보장되지는 않는다.[1]

## Skill 형식과 실제로 읽을 대표 문서

기여 지침은 `Skills/<category>/<skill-folder>/SKILL.md` 구조와 YAML frontmatter를 요구한다.
frontmatter는 문서 맨 앞에 이름과 설명을 넣는 메타데이터 영역이며, 설명은 관련 요청과 지침을 연결하는 단서다.
한 Skill은 한 주제에 집중하고, 출처를 표시하며, 실제 피해 대상이나 자격 증명을 예제에 넣지 말라는 규칙도 있다.[4]

대표 문서 `offensive-reporting`은 공격 실행보다 결과의 전달과 증거 관리에 초점을 둔다.
먼저 개별 발견을 기록하고, 위험도를 정리한 뒤 비전문 독자가 읽을 요약을 마지막에 작성하도록 한다.
발견 사항은 제목·영향 범위·관찰·근거·수정 제안·재검증 항목으로 나눈다.
초보자에게는 전문 약어보다 무엇이 노출되거나 변경될 수 있는지를 설명하도록 하는 부분이 중요하다.[3]

위험도 지표인 CVSS도 결론 그 자체가 아니라 판단 도구로 설명한다.
각 요소를 왜 그렇게 선택했는지 근거를 쓰고, 상황의 영향이 수치와 다르면 별도 설명을 추가하라고 한다.
따라서 보고서에 점수를 적는 일과 실제 위험을 증명하는 일은 다르다.
이 글은 특정 취약점의 점수나 피해 범위를 만들어 제시하지 않는다.[3]

증거 위생은 민감한 값이 보고서에 섞이지 않게 관리하는 규칙이다.
문서는 시각, 대상, 행동과 결과를 기록하고 자격 증명을 가리며, 스크린샷의 다른 탭과 URL에 토큰이 남지 않았는지 확인하도록 한다.
범위·제약·가정을 따로 쓰는 구성은 수행하지 않은 점검을 수행한 것처럼 보이게 하지 않기 위한 장치다.[3]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
허가된 로컬 교육용 애플리케이션의 설정 검토에서 불필요하게 공개되는 테스트 정보를 발견했다고 하자.
입력은 검토 범위와 시간, 해당 설정, 관찰한 응답 및 비밀 값을 제거한 화면이다.
보고서 Skill을 적용할 때 첫 단계는 그 관찰을 하나의 식별 가능한 발견 기록으로 만들고, 추정되는 영향과 실제 확인한 노출을 나누는 것이다.[3][2]

다음에는 비전문 독자를 위한 짧은 요약과 기술 검토자를 위한 상세 근거를 분리한다.
“중요한 정보가 노출된다”는 문장만 쓰기보다 어떤 테스트 정보가 어느 조건에서 보였는지를 적되 실제 자격 증명이나 개인 식별값을 첨부하지 않는다.
검토하지 않은 다른 기능이나 운영 환경까지 같은 문제가 있다고 확대하지 않고, 재현 가능 범위와 가정을 표시한다.
이 예시는 악용 요청이나 침입 절차를 포함하지 않는다.[3]

마지막으로 수정 제안과 재검증 계획을 연결한다.
설정을 고친 뒤 같은 허가 범위에서 정보가 더 이상 불필요하게 보이지 않는지 확인할 항목을 남기고, 수정 전 관찰과 수정 후 증거를 구별한다.
사람이 검토할 것은 문장이 설득력 있는가뿐 아니라 증거가 실제로 존재하는가, 가림 처리가 충분한가, 상태가 “수정됨”으로 바뀔 근거가 있는가다.
보고서 템플릿을 채운 것만으로 문제 해결을 선언하지 않는다.[3]

## 허가와 문서 호환성의 경계

SECURITY.md는 문서화된 범위와 규칙이 있는 점검, 명시적인 서면 허가, CTF나 보안 훈련, 책임 있는 공개를 전제로 한다.
자신이 소유하지 않거나 테스트 허가를 받지 않은 시스템에 대한 무단 접근을 위한 자료가 아니라고 명시한다.
공개 라이선스는 문서를 사용할 조건이지 실제 대상에 접근할 권한이 아니다.[2]

기존 초안의 형식상 주의점도 추가 확인했다.
기여 지침은 YAML frontmatter가 필요하다고 하지만, 수집한 `offensive-bug-identification/SKILL.md`는 일반 제목과 Metadata 절로 시작한다.
모든 파일이 똑같은 자동 발견 규칙을 충족한다고 볼 수 없는 차이다.
이 글에서는 그 문서의 머리 형식만 확인했고 세부 기법은 사용하거나 재현하지 않았다.[5][4]

README의 자동 로딩 설명과 실제 호스트에서의 인식 여부도 나눠야 한다.
선택한 파일의 메타데이터, 설치 위치와 호스트 동작은 별도로 확인해야 한다.
또한 보고서의 예시 수치, 이름, 요청은 설명용 자료이므로 실제 조사 결과에 그대로 복사해 사실처럼 사용할 수 없다.[1][3]

## 직접 읽어볼 자료

- [README의 Overview와 Utility 항목](https://github.com/SnailSploit/Claude-Red/blob/main/README.md)
  실행 엔진이 아니라 주제별 지침 모음이라는 정체를 먼저 확인한다.
  여러 분야를 모두 읽기보다 보고서처럼 방어적 검토와 연결되는 대표 문서를 선택할 수 있다.
- [offensive-reporting Skill](https://github.com/SnailSploit/Claude-Red/blob/main/Skills/utility/offensive-reporting/SKILL.md)
  Quick Workflow, Evidence Discipline, Scope·Limitations·Assumptions를 이어서 읽는다.
  사실·해석·제약·수정 상태를 어떻게 분리하는지 관찰할 자료다.
- [Security Policy](https://github.com/SnailSploit/Claude-Red/blob/main/SECURITY.md)
  사용할 수 있는 허가 범위와 공개 절차를 먼저 확인한다.
  라이브러리 자체의 문제 신고와 다른 제품에서 발견한 문제 신고의 책임 구분도 설명한다.
- [기여 형식](https://github.com/SnailSploit/Claude-Red/blob/main/CONTRIBUTING.md)과 [형식 비교 대상](https://github.com/SnailSploit/Claude-Red/blob/main/Skills/fuzzing/offensive-bug-identification/SKILL.md)
  앞부분의 메타데이터만 비교해도 문서 표준과 모든 파일의 실제 상태가 같지 않을 수 있음을 확인할 수 있다.

## 정리

Claude-Red를 안전하게 읽는 출발점은 허가된 범위와 문서 기반 지침이라는 정체다.
보고서 Skill은 근거와 개선·재검증을 연결하는 자료로 볼 수 있지만 실제 사실 확인과 민감 정보 관리는 사람의 검토가 남는다.

## 자료 확인 범위

2026-09-27 수집된 README, 루트 구성, 보고서 Skill과 보안 정책, 기여 안내 및 비교 문서의 메타데이터를 읽었다.
설치, 스캐닝, 공격 기법과 보고서 자동 생성을 실행하지 않았다.

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

## Sources

[1] SnailSploit/Claude-Red — README.md

<https://github.com/SnailSploit/Claude-Red/blob/main/README.md>

[2] SnailSploit/Claude-Red — SECURITY.md

<https://github.com/SnailSploit/Claude-Red/blob/main/SECURITY.md>

[3] SnailSploit/Claude-Red — Skills/utility/offensive-reporting/SKILL.md

<https://github.com/SnailSploit/Claude-Red/blob/main/Skills/utility/offensive-reporting/SKILL.md>

[4] SnailSploit/Claude-Red — CONTRIBUTING.md

<https://github.com/SnailSploit/Claude-Red/blob/main/CONTRIBUTING.md>

[5] SnailSploit/Claude-Red — Skills/fuzzing/offensive-bug-identification/SKILL.md

<https://github.com/SnailSploit/Claude-Red/blob/main/Skills/fuzzing/offensive-bug-identification/SKILL.md>
