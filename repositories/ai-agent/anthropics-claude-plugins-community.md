---
title: "anthropics/claude-plugins-community"
repository: "anthropics/claude-plugins-community"
url: "https://github.com/anthropics/claude-plugins-community"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/claude-plugins-community

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

이 저장소는 Claude Cowork와 Claude Code용 커뮤니티 플러그인 마켓플레이스의 읽기 전용 미러다.
모든 플러그인을 실행하는 새 에이전트나 자유롭게 코드를 합치는 일반 기여 저장소가 아니라, 심사를 거친 배포 목록과 관련 자료를 공개하는 창구로 설명되어 있다.[1]

## 플러그인을 찾는 곳과 만드는 곳은 다르다

플러그인을 찾았을 때 바로 설치 명령부터 따라가기 전에 확인할 질문이 있다.
목록을 누가 관리하고, 실제 기능은 어디에 정의되며, 개선 요청은 어느 경로로 보내는가?
루트 README는 `.claude-plugin/marketplace.json`이 설치 가능한 목록이고 내부 검토 경로에서 매일 동기화된다고 설명한다.
따라서 이 저장소의 내용은 배포 상태를 보여주는 미러이며 직접 열린 pull request는 자동으로 닫힌다.[1]

등록된 항목은 제출 절차를 거쳐 자동 보안 검사와 배포 승인을 받았다고 README에 적혀 있다.
이는 목록의 관리 방식을 설명하는 근거이지 특정 입력에서 결과가 정확하다거나 모든 향후 변경이 무해하다는 실험 결과는 아니다.
사용자는 목록의 승인 여부와 개별 플러그인의 지침·출력·권한을 서로 다른 질문으로 읽어야 한다.[1]

## 작은 기능도 별도 계약을 가진다

실제 대표 항목으로 `eli5`를 열어 보았다.
이름은 “다섯 살에게 설명하듯”이라는 표현을 줄인 것이며, 초보자가 모르는 주제를 큰 그림과 적은 글로 설명하는 HTML artifact를 만들도록 되어 있다.
여기서 artifact는 대화 문장만이 아니라 사용자가 열어볼 수 있는 생성 결과물이다.
이 항목의 README는 `/eli5 how does DNS work`를 사용 예로 제시한다.[2]

기능의 핵심은 `eli5/skills/eli5/SKILL.md`라는 짧은 파일에 있다.
앞부분의 name과 description은 이름과 호출 조건을 담고, 본문은 주제에 대해 그림 중심 HTML 설명을 작성하라고 지시한다.
사용자 주제는 `$ARGUMENTS` 자리에 전달된다.
긴 실행 코드가 없더라도 호스트 에이전트가 지침을 읽고 생성 작업을 수행하면 플러그인의 기능이 되는 사례다.[3]

그러므로 이 저장소를 이해할 때 “설치했다”는 사실을 “설명 내용을 검증했다”는 사실과 구분해야 한다.
eli5의 지침은 표현 방식과 난이도를 지정하지만, 과학적 근거를 찾는 별도 검색 절차나 인용 형식을 정의하지 않는다.
짧은 Skill이 어떤 일을 요구하는지뿐 아니라 무엇을 요구하지 않는지 읽는 것이 실제 기능 범위를 파악하는 방법이다.[3]

## 예시로 따라가는 흐름

공식 eli5 README의 입력은 DNS가 어떻게 작동하는지 묻는 `/eli5 how does DNS work`다.
이 예시에서 사용자가 제공하는 것은 완성된 HTML이나 조사 자료가 아니라 설명할 주제다.
호스트는 eli5 지침을 선택하고 그 주제를 `$ARGUMENTS`로 본문에 연결한다.
그런 다음 아무 배경지식 없는 독자를 대상으로 그림을 크게, 글을 적게 구성한 HTML 결과를 만들도록 안내받는다.[2][3]

출력은 DNS 서버에 실제 요청을 보내고 측정한 진단 보고서가 아니라 개념을 설명하는 시각 자료다.
사람은 생성된 그림의 비유가 실제 동작과 어디서 달라지는지, 짧아진 문장이 핵심 조건을 지우지는 않았는지, HTML이 의도한 호스트에서 읽히는지 확인해야 한다.
이 조사에서는 artifact를 생성하거나 화면에서 열지 않았으므로 실제 그림의 품질·접근성·정확성을 평가하지 않는다.[2][3]

이 사례를 목록 전체로 일반화해서도 안 된다.
eli5가 간단한 주제 설명 Skill이라고 해서 다른 플러그인도 파일이나 외부 연결을 사용하지 않는다는 뜻은 아니다.
마켓플레이스에서 항목을 찾은 뒤 개별 README와 기능 지침으로 들어가는 경로가 필요한 이유다.
목록의 소개는 발견의 출발점이며 설치 전에 읽어야 할 기능 계약을 대신하지 않는다.[1][3]

## 배포 경로와 사용 환경의 경계

Claude Code에서는 먼저 커뮤니티 마켓플레이스를 추가하고 `<plugin-name>@claude-community` 형태로 개별 항목을 설치하도록 안내한다.
Cowork에서는 별도 플러그인 페이지에서 설치한다.
같은 커뮤니티 목록을 사용하더라도 호스트의 인터페이스가 다르므로 한 환경의 명령을 다른 환경에 그대로 적용한다고 가정할 수 없다.[1]

기여 경로 역시 일반적인 코드 저장소와 다르다.
새 플러그인은 README의 제출 양식으로 보내야 하며 이 미러를 직접 수정하는 pull request 방식은 지원하지 않는다.
이 글에서 제출 주소를 소개하는 것은 기능 설명을 위한 것이며 플러그인 제출이나 계정 접근을 수행했다는 뜻은 아니다.
이미 등록된 항목의 제공자·설명·소스를 확인하는 일도 각각 별도 단계다.[1]

작은 설명 플러그인이라도 사용하는 모델과 호스트의 한계는 남는다.
eli5 지침에는 특정 브라우저, 외부 이미지 제공자, 검사 도구의 필수 조건이 상세히 들어 있지 않다.
따라서 단일 파일만으로 완성 HTML의 모든 동작을 보장한다고 말할 수 없다.
사용 목적이 교육이라면 간단한 표현과 사실 보존을 함께 검토하고, 부족한 정보는 추가 자료로 확인하는 편이 타당하다.[3]

## 직접 읽어볼 자료

1. [루트 README의 What this repo is](https://github.com/anthropics/claude-plugins-community/blob/main/README.md)
   이곳이 읽기 전용 미러이며 어떤 승인 경로로 목록이 갱신되는지 먼저 확인한다.
   이어 제출 절차를 읽으면 설치 목록 열람과 플러그인 개발 기여를 다른 활동으로 구분할 수 있다.
2. [eli5의 README](https://github.com/anthropics/claude-plugins-community/blob/main/eli5/README.md)
   DNS 질문 예시와 HTML artifact라는 출력 형식을 연결해서 읽는다.
   사용자가 입력하는 것은 무엇이고 결과물로 기대하는 것은 무엇인지 가장 짧게 파악할 수 있는 자료다.
3. [eli5의 SKILL.md](https://github.com/anthropics/claude-plugins-community/blob/main/eli5/skills/eli5/SKILL.md)
   description의 호출 조건과 `$ARGUMENTS` 전달을 살펴본다.
   실제 조사·검증 절차가 들어 있는지, 아니면 표현 형식을 지정하는 지침인지 확인하면 기능의 범위를 과장하지 않고 설명할 수 있다.

## 정리

claude-plugins-community는 커뮤니티 확장을 발견하고 배포하는 목록의 미러다.
대표 eli5는 주제를 시각 설명으로 바꾸는 작은 Skill이며, 목록의 승인과 생성 내용의 검증은 서로 다른 책임으로 남는다.

## 자료 확인 범위

2026-09-27 루트 README와 eli5 README·Skill을 실제로 읽었다.
마켓플레이스 등록, 플러그인 설치, HTML 생성 및 화면 시험은 실행하지 않았다.

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

[1] anthropics/claude-plugins-community — README.md

<https://github.com/anthropics/claude-plugins-community/blob/main/README.md>

[2] anthropics/claude-plugins-community — eli5/README.md

<https://github.com/anthropics/claude-plugins-community/blob/main/eli5/README.md>

[3] anthropics/claude-plugins-community — eli5/skills/eli5/SKILL.md

<https://github.com/anthropics/claude-plugins-community/blob/main/eli5/skills/eli5/SKILL.md>
