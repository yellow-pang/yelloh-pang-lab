---
title: "kepano/obsidian-skills"
repository: "kepano/obsidian-skills"
url: "https://github.com/kepano/obsidian-skills"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# kepano/obsidian-skills

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## Obsidian 파일 형식과 조작법을 알려주는 Skill 모음

`obsidian-skills`는 AI Agent가 Obsidian의 노트 형식과 명령행 기능을 다룰 때 참고하는 지침 모음이다.
Obsidian 앱 자체나 새로운 동기화 서버가 아니라, 기존 Agent에게 파일 규칙과 작업 순서를 알려주는 자료다.
README는 Agent Skills 표준을 따른다고 설명하며 Markdown, Bases, JSON Canvas, Obsidian CLI, 웹 본문 추출과 템플릿 렌더링을 서로 다른 Skill로 구분한다.[1]

일반 Markdown을 쓸 줄 안다고 Obsidian의 모든 파일을 올바르게 만들 수 있는 것은 아니다.
노트 사이를 잇는 위키 링크, 노트 속성으로 만드는 표, 캔버스의 노드와 연결선은 표현 방식이 다르다.
이 저장소는 그 차이를 Agent가 추측하지 않도록 문법과 예제를 제공한다.
또한 직접 파일을 생성하는 작업과 실행 중인 앱에 명령을 보내는 작업을 분리하므로, 같은 '노트 정리' 요청도 어떤 경로로 수행할지 먼저 정해야 한다.[1][2][3]

## 노트 내용, 보는 방식, 앱 조작을 나눈다

README의 `obsidian-markdown`은 링크·임베드·콜아웃·속성 같은 노트 문법을, `json-canvas`는 노드와 선으로 연결한 공간형 파일을 대상으로 한다. `obsidian-bases`는 노트들을 조건에 맞게 모아 표나 카드로 보는 `.base` 파일을 다룬다.
여기서 Base는 노트 원문을 다른 데이터베이스로 복제하는 프로그램이 아니라, 어떤 파일을 어떤 열과 계산값으로 보여줄지 정의하는 YAML 파일이다.[1][2]

실제로 읽은 Bases Skill의 순서는 파일 생성, 표시 범위 결정, 계산식 추가, 보기 구성, 문법 검증, Obsidian에서 확인이다. `filters`는 어떤 노트를 포함할지, `formulas`는 속성에서 어떤 값을 계산할지, `views`는 결과를 어떻게 보여줄지 정한다.
모든 보기에 공통인 필터와 특정 보기에만 적용할 필터가 구분되므로, 한 파일에서 '진행 중'과 '완료' 목록을 따로 만들 수 있다.[2]

속성도 노트의 Front Matter에서 가져오는 값, 파일 이름·수정 시각 같은 파일 정보, 계산식으로 만든 값의 세 종류다.
이 구분은 오류를 찾는 기준이 된다. `formula.days_until_due`를 표시하려면 실제 `formulas`에 그 정의가 있어야 하고, 마감일이 없는 노트에는 빈 값 처리가 필요하다.
문법이 맞아도 참조하는 속성이 존재하지 않으면 의도한 표가 되지 않는다.[2]

CLI Skill은 실행 중인 Obsidian에 명령을 전달하는 별도의 경로다.
파일 이름을 위키 링크처럼 해석하는 `file`과 보관함 루트부터 정확한 경로를 주는 `path`가 구분되며, 보관함을 지정하지 않으면 최근 포커스를 받은 보관함을 대상으로 한다.
보관함은 Obsidian 노트가 들어 있는 폴더 단위다.
읽기 명령뿐 아니라 생성·추가·속성 변경과 플러그인 재시작도 있으므로, Skill을 읽었다는 사실과 실제 파일을 수정할 권한은 별개다.[3]

## 예시로 따라가는 흐름

Bases Skill의 공식 `Task Tracker Base` 예제는 `task` 태그가 붙은 Markdown 파일을 입력 집합으로 삼는다.
노트마다 `status`, `priority`, `due` 같은 속성이 있고, Base는 이 속성을 참조해 남은 날짜와 우선순위 표시를 계산한다.
이 글은 공식 예제를 해설한 것이며 직접 실행한 결과가 아니다.
중요한 입력은 자연어로 '할 일 표를 만들어줘'라고 요청하는 문장만이 아니라 실제 노트에 같은 이름과 자료형으로 저장된 속성이다.[2]

예제의 `Active Tasks` 보기는 완료 상태가 아닌 노트만 모으고 파일 이름, 상태, 우선순위, 마감일과 남은 날짜를 표시한다. `Completed` 보기는 완료된 노트와 완료일을 보여준다.
같은 원본 노트를 두 방식으로 나누어 보는 것이므로, 표를 만들기 위해 노트 본문을 다시 작성할 필요는 없다.
마감일이 없는 항목은 조건식을 통해 빈 값으로 처리하며, 날짜를 뺀 결과에서 `.days`를 읽는 이유는 날짜 차이가 일반 숫자가 아닌 기간 객체이기 때문이다.[2]

사람이 확인할 부분은 YAML 구문 검사에서 끝나지 않는다.
기존 노트의 상태 값이 예제의 `done`과 실제로 같은지, 필요한 속성이 빠진 노트는 어떻게 보이는지, 두 보기에 예상한 항목이 들어가는지 앱에서 확인해야 한다.
CLI로 상태를 바꾸는 작업까지 연결한다면 먼저 정확한 보관함과 파일 경로를 확인해야 한다.
보기 정의가 맞다는 사실은 다른 보관함에 같은 이름의 노트를 수정해도 된다는 뜻이 아니다.[2][3]

## 파일 생성과 앱 검증 사이의 경계

Bases Skill은 따옴표 중첩, 정의되지 않은 계산식, 빈 속성을 흔한 문제로 설명한다.
특히 날짜 차이에는 바로 반올림을 적용하지 않고 숫자 필드를 꺼내야 한다.
지도 보기는 위도·경도뿐 아니라 별도 Maps 커뮤니티 플러그인을 요구한다.
모든 보기 유형이 같은 준비만으로 렌더링된다고 생각하면 안 된다.[2]

CLI는 Obsidian이 열려 있어야 하며, 지원 명령은 실제 `obsidian help`로 확인하도록 안내한다.
이 저장소의 지침 파일을 Agent에게 복사하는 것만으로 앱이나 CLI가 설치되는 것은 아니다.
플러그인 개발용 JavaScript 평가와 재로드 명령까지 포함하므로, 공개 예제라고 해도 개인 보관함 전체에 무심코 실행하지 않고 허용된 파일과 변경 내용을 먼저 정해야 한다.[3]

README는 클라이언트별 배치 경로도 다르게 설명한다.
표준 Skill이라는 사실은 내용을 재사용할 수 있다는 의미이지 모든 Agent가 같은 폴더 구조에서 자동 발견한다는 보장이 아니다.
파일 형식 안내, 클라이언트의 Skill 검색 규칙, Obsidian 앱의 실행 조건을 각각 확인해야 연결이 완성된다.[1]

## 직접 읽어볼 자료

- [README의 Skills와 설치 구분](https://github.com/kepano/obsidian-skills/blob/main/README.md):

  필요한 결과가 노트, Base, Canvas, 앱 명령 중 무엇인지 먼저 고른다.
  같은 저장소 안에서도 입력 파일과 필요한 실행 환경이 달라지는 이유를 찾는 출발점이다.
- [Bases Skill](https://github.com/kepano/obsidian-skills/blob/main/skills/obsidian-bases/SKILL.md):

  Workflow를 읽은 뒤 Task Tracker Base에서 전역 필터와 보기별 필터를 대조한다.
  Troubleshooting까지 연결해 빈 속성이나 정의되지 않은 계산식이 어떤 문제를 만드는지 살펴볼 수 있다.
- [CLI Skill](https://github.com/kepano/obsidian-skills/blob/main/skills/obsidian-cli/SKILL.md):

  File targeting과 Vault targeting을 먼저 확인한다.
  읽기·생성·속성 변경 예제를 구분하고, 기본 대상 선택이 실제로 바꾸려는 파일과 맞는지 질문하며 읽는 것이 안전하다.

## 정리

`obsidian-skills`는 노트를 대신 저장하는 서비스가 아니라 Agent가 Obsidian의 열린 형식과 앱 인터페이스를 정확히 다루도록 돕는 설명서다.
Base 사례에서는 범위, 계산, 표시를 구분하는 방식이 핵심이고, CLI에서는 대상 보관함과 변경 권한을 명시하는 것이 핵심이다.

## 자료 확인 범위

2026-09-27 README와 루트 구조, Bases 및 CLI의 `SKILL.md`를 읽었다.
Obsidian 실행, Skill 설치, 개인 보관함 열람이나 파일 생성은 하지 않았다.
Base 예제의 실제 렌더링도 확인한 범위에 포함되지 않는다.

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

[1] kepano/obsidian-skills — README.md

<https://github.com/kepano/obsidian-skills/blob/main/README.md>

[2] kepano/obsidian-skills — skills/obsidian-bases/SKILL.md

<https://github.com/kepano/obsidian-skills/blob/main/skills/obsidian-bases/SKILL.md>

[3] kepano/obsidian-skills — skills/obsidian-cli/SKILL.md

<https://github.com/kepano/obsidian-skills/blob/main/skills/obsidian-cli/SKILL.md>
