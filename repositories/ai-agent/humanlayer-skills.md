---
title: "humanlayer/skills"
repository: "humanlayer/skills"
url: "https://github.com/humanlayer/skills"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# humanlayer/skills

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 코딩 대화를 구체적인 설명과 작업으로 바꾸는 Skill 모음

humanlayer/skills는 Claude Code에서 개별적으로 추가해 사용하는 Skill 모음이다.
Skill은 모델 자체가 아니라 특정 요청을 받았을 때 무엇을 읽고 어떤 형태로 결과를 낼지 알려 주는 지침이다.
README에는 시각적 설명, 시각적 PR 개요, CLAUDE.md 정리, React 속성 타입 축소, 반복 Agent 작업과 제어 루프 설계가 소개되어 있다.
하나의 자동 실행 프로그램이나 모든 기능이 함께 돌아가는 단일 Agent로 볼 필요는 없다.[1]

코드 설명을 길게 읽어도 함수 호출 순서와 파일 책임이 머릿속에 남지 않을 때가 있다.
반대로 간단한 질문에 거대한 아키텍처 그림을 만들면 중요한 한 가지를 찾기 어려워진다.
이 모음의 `show-me`는 현재 대화에서 필요한 최소한의 시각적 표현을 선택하도록 지시한다.
목표는 보기 좋은 그림을 많이 만드는 것이 아니라 사용자가 묻는 구조와 변화를 드러내는 것이다.[2]

## 질문에 맞춰 표현 수단을 고른다

`show-me/SKILL.md`는 논리나 알고리즘에는 의사코드, 실행 순서에는 호출 트리, UI 구조에는 컴포넌트 트리, 파일 책임에는 얕은 파일 트리를 제시한다.
의사코드는 실행 문법보다 판단 순서를 보여 주는 글이고, 트리는 상위와 하위의 관계를 들여쓰기로 표시하는 방식이다.
데이터 이동과 상호작용을 표현할 때는 텍스트로 다이어그램을 만드는 Mermaid를 사용할 수 있다.[2]

이미 존재하는 코드에서 무엇이 바뀌는지가 핵심이라면 diff를 사용한다.
diff는 추가·삭제되는 줄을 나란히 보지 않아도 구별할 수 있는 변경 표현이다.
문서는 컴포넌트, 파일 배치, 호출 순서, 상태 처리의 변화에 각각 다른 diff 예시를 보여 준다.
반면 대부분이 새 내용이거나 생략하면 소유 관계가 사라지는 경우에는 전체 블록을 보여 주도록 한다.
축약 자체보다 이해에 필요한 맥락을 보존하는 규칙이다.[2]

복잡한 화면 비교나 레이아웃에는 하나의 목적에 집중한 HTML 파일을 작성해 열도록 안내한다.
이때 실제 라벨과 데이터를 쓰고 기존 제품의 색·글꼴·간격을 맞추며 데스크톱과 모바일을 고려하도록 적혀 있다.
따라서 이 Skill을 실행하면 채팅 출력만 생기는 경우도 있지만 로컬 파일 생성과 열기가 포함될 수도 있다.
메타데이터에는 `disable-model-invocation: true`가 명시되어 있다.[2]

## 예시로 따라가는 흐름

공식 Skill의 저장 처리 예시는 `on(save)`에서 내용이 바뀌지 않았으면 캐시 결과를 반환하고, 바뀌었다면 새 내용을 쓰고 새 결과를 돌려주는 의사코드다.
이 설명의 입력은 ‘저장 요청이 어떻게 처리되는가’라는 현재 대화의 주제이고, 출력은 조건과 결과만 남긴 작은 구조다.
함수나 파일을 전부 나열하지 않아도 같은 내용을 다시 쓸 필요가 없는 분기가 눈에 들어온다.[2]

같은 주제를 변경 전후 비교로 설명해야 한다면 문서의 diff 방식으로 기존 저장 앞에 ‘변경 없음’ 검사를 추가하고 쓰기 뒤에는 캐시 무효화를 표시할 수 있다.
이 과정에서 확인할 부분은 도식의 예쁨이 아니라 캐시 반환과 실제 쓰기의 순서, 무효화 책임이 어디에 있는지다.
여기서는 Skill에 수록된 설명 형태를 읽었으며 실제 프로젝트를 수정하지 않았다.
이 예시가 모든 저장 API의 올바른 구현이라는 뜻도 아니다.
실제 코드의 동시성이나 오류 처리까지 묻는다면 그 경계를 포함한 더 큰 그림이 필요하고, 설명용 의사코드를 그대로 제품 코드라고 취급해서는 안 된다.

## 반복 실행 Skill은 별도의 운영 장치다

추가로 읽은 `design-control-loop`는 작은 설명용 Skill과 성격이 다르다.
원하는 코드 상태를 목표점으로 두고 현재 상태를 측정하는 sensor, 다음 변경을 결정하는 controller, 코드를 고치고 PR을 여는 actuator로 나눈다.
외부 변경과 의존성 변화는 목표에서 멀어지게 만드는 교란으로 설명한다.
한 번에 모두 고치는 대신 측정과 작은 수정을 반복하는 방식이다.[3]

이 Skill은 먼저 저장소의 CI와 검증 명령, 패키지 구조를 읽은 뒤 사용자와 설계 인터뷰를 하도록 요구한다.
구성 요소를 로컬에서 독립적으로 실행할 수 있게 만든 다음 예약 워크플로로 연결하고, 지속적인 피드백을 기억 파일에 남긴다.
측정할 지표가 실제 개선을 나타내는지와 자동 변경의 위험을 사용자가 설계해야 하므로 설치만으로 안전한 유지보수가 완성되는 것은 아니다.[3]

README의 `visual-pr`는 PR 생성·갱신을, `improve-claude-md`는 지침 파일 재작성을 안내한다.
이름이 모두 Skill이라도 부작용은 다르다.
읽기용 설명을 원하는 상황과 파일 변경·외부 PR 작업을 허용한 상황을 구분해야 한다.
특히 반복 실행 기능은 스케줄과 권한, 검증 실패 시 처리 범위를 먼저 확인할 필요가 있다.[1][3]

## 직접 읽어볼 자료

- [README의 Available Skills](https://github.com/humanlayer/skills/blob/main/README.md)

  필요한 작업 하나를 먼저 고른다.
  설명 도구인지 코드·문서 수정 도구인지, 예약 작업을 구축하는 도구인지 나누어 읽으면 전체를 동일한 권한으로 설치하고 호출하는 실수를 줄일 수 있다.

- [show-me 지침](https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md)

  저장 처리 예시와 여러 diff 형태를 비교한다.
  어느 경우에 부분 변경만 보여 주고 어느 경우에 전체 문맥을 보여 주는지, HTML 파일 생성이 언제 등장하는지 확인한다.

- [design-control-loop 지침](https://github.com/humanlayer/skills/blob/main/plugins/design-control-loop/skills/design-control-loop/SKILL.md)

  Mental model과 Outputs, 초기 조사 단계를 읽는다.
  측정·판단·수정이 독립적인지와 사용자와 합의한 설계를 먼저 문서화하는 이유를 살필 수 있다.

## 정리

humanlayer/skills는 설명을 시각화하는 작은 작업부터 반복 코드 개선 흐름을 설계하는 작업까지 제공한다.
대표적인 공통점은 결과 형태와 작업 절차를 명시한다는 것이며, 실제 변경 범위는 선택한 Skill마다 따로 확인해야 한다.

## 자료 확인 범위

2026-09-27 기준 기존 초안과 README, 플러그인 구성, show-me와 design-control-loop 지침을 읽었다.
Skill 설치, HTML 열기, 코드 변경과 PR·예약 워크플로 생성은 실행하지 않았다.

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

[1] humanlayer/skills — README.md

<https://github.com/humanlayer/skills/blob/main/README.md>

[2] humanlayer/skills — plugins/show-me/skills/show-me/SKILL.md

<https://github.com/humanlayer/skills/blob/main/plugins/show-me/skills/show-me/SKILL.md>

[3] humanlayer/skills — plugins/design-control-loop/skills/design-control-loop/SKILL.md

<https://github.com/humanlayer/skills/blob/main/plugins/design-control-loop/skills/design-control-loop/SKILL.md>
