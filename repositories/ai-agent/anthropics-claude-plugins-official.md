---
title: "anthropics/claude-plugins-official"
repository: "anthropics/claude-plugins-official"
url: "https://github.com/anthropics/claude-plugins-official"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/claude-plugins-official

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

claude-plugins-official은 Claude Code용 플러그인을 모아 제공하는 관리형 디렉터리다.
루트 README는 내부에서 개발·유지하는 `plugins/`와 외부 기여자의 `external_plugins/`를 구분한다.
따라서 이름의 official을 모든 항목의 코드와 외부 서비스가 동일한 주체에 의해 만들어지고 보증된다는 뜻으로 읽어서는 안 된다.[1]

## 확장 기능의 입구를 하나로 모으기

코딩 에이전트에 명령, 전문 지침, 외부 도구 연결을 추가하려면 여러 파일을 일관된 구조로 묶어야 한다.
이 저장소는 그 묶음을 마켓플레이스에서 발견하고 설치할 수 있도록 하는 배포 입구다.
플러그인마다 완전히 다른 설치 방식을 사용하기보다 공통 구조와 식별자를 갖게 하는 것이 핵심이다.[1]

플러그인은 그 자체가 새 언어 모델이 아니다.
지침과 도구 설정을 호스트가 읽을 수 있게 포장한 단위이며 구성 요소는 선택적이다.
루트 설명의 필수 파일은 `.claude-plugin/plugin.json` 메타데이터이고, 외부 연결 `.mcp.json`, 명령, agent 정의, Skill 등이 함께 놓일 수 있다.
어떤 항목이 포함되었는지에 따라 권한과 사용 방식이 달라진다.[1]

## 실제 예제에서 보는 명령과 Skill

공식 참조 구현인 `plugins/example-plugin`의 README를 읽으면 Skill을 모델이 문맥에 따라 선택하는 기능과 사용자가 slash command로 부르는 기능 모두에 권장한다고 설명한다.
예전 `commands/*.md`와 새 `skills/<name>/SKILL.md`는 로드 방식은 같고 파일 배치가 다르며, 새 플러그인에는 skills 형식을 권한다.[2]

추가로 조회한 `example-command/SKILL.md`는 name, description, argument-hint, allowed-tools를 선언한다.
본문에서는 `$ARGUMENTS`로 전달된 사용자 인자를 파악하고 허용 도구로 요청을 수행한 뒤 결과를 보고한다.
이는 특정 업무의 완제품 명령이 아니라 플러그인 작성자가 호출 조건과 도구 범위를 어디에 적는지 보여주는 골격이다.[3]

allowed-tools 예에는 Read·Glob·Grep·Bash가 들어 있다.
파일 읽기와 검색뿐 아니라 셸 명령 도구가 포함될 수 있음을 보여준다.
Skill 파일은 읽기 쉬운 텍스트지만 그것을 수행하는 호스트는 실제 도구를 사용할 수 있다.
따라서 Markdown이라는 형식만 보고 실행 영향이 없다고 판단하지 말고, 허용 도구와 본문 지시를 함께 읽어야 한다.[3]

## 예시로 따라가는 흐름

공식 예제의 호출은 `/example-command my-argument`다.
사용자가 인자를 입력하면 호스트는 등록된 Skill을 찾고 `$ARGUMENTS`에 그 값을 전달한다.
앞부분 메타데이터는 도움말에 보이는 설명과 인자 힌트, 미리 허용한 도구를 제공한다.
본문은 인자를 해석하고 요청 동작을 수행한 뒤 결과를 보고하는 순서를 안내한다.[3]

이 예시에서 주의할 점은 `my-argument`만으로 어떤 유용한 분석 결과가 정해지는 것은 아니라는 사실이다.
참조 파일에는 인자에 따른 실제 업무 규칙이나 구체적인 검증 대상이 없다.
그래서 이 파일을 읽고 “리뷰가 완료되었다” 같은 결과를 만들어 낼 수 없다.
산출물은 플러그인 작성자가 이어서 정의할 동작에 달려 있으며 이 조사는 호출 계약만 확인했다.[3]

사람은 예제를 복제하기 전에 자신의 명령이 정말 셸 권한을 필요로 하는지, 실패 시 무엇을 보고할지, 입력을 어떤 범위로 제한할지 결정해야 한다.
example-plugin README의 MCP 설정은 외부 도구 연결 형식도 보여주지만 예시 주소를 실제 서비스로 간주하면 안 된다.
공통 포맷의 이해와 운영 가능한 연결의 검증은 별개의 단계다.
여기서는 명령 실행이나 서버 연결을 하지 않았다.[2][3]

## 설치 이름은 사용자와의 약속이다

루트 README는 marketplace 항목의 `name`을 변경 불가능한 slug로 취급한다.
slug는 설치를 식별하는 안정된 짧은 이름이다.
화면 표시만 바꾸려면 `displayName`을 쓰고, 불가피하게 이름을 바꾸면 `renames` 맵으로 기존 설치가 이동할 수 있게 한다.
파일 내용뿐 아니라 이미 설치한 사용자의 참조도 인터페이스라는 뜻이다.[1]

또한 원본 저장소에 plugin.json 없이 Skill만 있을 때는 `strict: false`와 명시적인 skills 배열로 묶을 수 있다고 설명한다.
각 경로는 원본 하위 경로를 기준으로 하고 필요한 일부 Skill만 노출할 수 있다.
즉 “마켓플레이스 플러그인”이라는 이름이 항상 원본 저장소 전체를 동일하게 배포한다는 의미는 아니다.[1]

## 신뢰와 라이선스는 개별 항목에서

README는 설치·업데이트·사용 전에 플러그인을 신뢰할 수 있는지 확인하라고 강조한다.
포함된 MCP 서버, 파일, 소프트웨어가 의도대로 작동하거나 앞으로 변하지 않는다고 보증할 수 없다고 직접 밝힌다.
외부 항목은 품질·보안 기준으로 승인받지만 이 경고는 여전히 적용된다.[1]

라이선스도 연결된 각 플러그인의 LICENSE를 확인하도록 안내한다.
디렉터리의 관리 주체와 개별 구성 요소의 사용 허락을 하나로 취급하면 안 된다.
특히 도구 연결을 포함한 플러그인은 해당 서비스의 인증·접근 권한을 따로 요구할 수 있으며, 실제로 어떤 파일을 읽고 어느 주소로 요청을 보내는지 개별 자료에서 확인해야 한다.[1][2]

## 직접 읽어볼 자료

1. [루트 README의 Structure와 신뢰 경고](https://github.com/anthropics/claude-plugins-official/blob/main/README.md)
   내부·외부 플러그인 구분을 먼저 보고, official이라는 이름이 어떤 보증을 뜻하지 않는지 읽는다.
   설치 명령보다 신뢰 경계를 앞에 두는 이유를 확인할 수 있다.
2. [example-plugin README](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/example-plugin/README.md)
   metadata·Skill·legacy command·MCP가 서로 어떤 역할인지 구조도를 따라간다.
   예제의 외부 주소를 운영 서비스가 아니라 포맷 설명으로 읽는 것도 중요하다.
3. [example-command Skill](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/example-plugin/skills/example-command/SKILL.md)
   frontmatter의 allowed-tools와 인자 전달을 본 뒤 본문의 수행 순서를 읽는다.
   예제에 없는 업무 규칙과 검증 기준이 무엇인지 질문하면 실제 플러그인을 작성할 때 보충해야 할 계약이 드러난다.

## 정리

이 저장소는 Claude Code 확장을 일관된 형식으로 발견·설치하도록 돕는다.
예제는 확장 구조를 설명할 뿐 개별 기능의 정확성과 안전성을 보장하지 않으며, 실제 소스·권한·라이선스는 항목마다 검토해야 한다.

## 자료 확인 범위

2026-09-27 루트 README, 참조 플러그인 README, 사용자 호출 Skill을 읽었다.
어떤 플러그인도 설치하거나 실행하지 않았고 외부 MCP 연결을 시험하지 않았다.

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

[1] anthropics/claude-plugins-official — README.md

<https://github.com/anthropics/claude-plugins-official/blob/main/README.md>

[2] anthropics/claude-plugins-official — plugins/example-plugin/README.md

<https://github.com/anthropics/claude-plugins-official/blob/main/plugins/example-plugin/README.md>

[3] anthropics/claude-plugins-official — plugins/example-plugin/skills/example-command/SKILL.md

<https://github.com/anthropics/claude-plugins-official/blob/main/plugins/example-plugin/skills/example-command/SKILL.md>
