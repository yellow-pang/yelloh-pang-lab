---
title: "NomaDamas/k-skill"
repository: "NomaDamas/k-skill"
url: "https://github.com/NomaDamas/k-skill"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# NomaDamas/k-skill

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한국 생활 서비스를 에이전트의 작업 절차로 묶기

k-skill은 한국의 생활 정보·공개 데이터·문서 작업을 AI 에이전트가 처리하도록 돕는 스킬 모음이다.
스킬은 모델 자체가 아니라 특정 상황에서 읽을 지침과 보조 파일의 묶음이다.
에이전트가 어떤 입력을 받고 어떤 자료를 조회하며 어디서 멈춰야 하는지 설명한다.
따라서 한국어를 학습시킨 새 언어 모델이나 모든 서비스를 대신 운영하는 단일 앱과는 구분된다.[1]

열차 시간표를 묻는 것과 글자 수를 세는 것은 겉으로는 모두 자연어 요청이지만 필요한 처리는 다르다.
전자는 날짜·구간에 맞는 공식 자료가 필요하고, 후자는 문자열을 정해진 규칙으로 계산해야 한다.
k-skill은 이런 작업마다 절차와 도구를 연결한다.
README의 생활 분야 목록을 기능 보증표로 보기보다 각 작업으로 들어가는 안내판으로 읽는 것이 정확하다.[1]

## 짧은 스킬 입구 뒤에 있는 실제 지침

확인한 `railway-timetable/SKILL.md`에는 이름, 설명, 라이선스 같은 선택 정보와 전체 지침을 가져오는 CLI 안내가 있다.
CLI는 터미널에서 명령을 받아 동작하는 도구다.
이 파일 자체에 모든 구현이 들어 있는 것이 아니라 `instruction.md`와 번들된 스크립트·참고 자료를 연결하는 입구라는 점이 중요하다.
짧은 SKILL 파일만 보고 기능을 모두 확인했다고 할 수 없다.[2]

지침 조립 코드인 `assemble.js`는 스킬의 `skill.json`과 본문을 읽고, 공통 규칙과 해당 작업의 프로필을 합친다.
프로필은 브라우저·조회·로컬 실행처럼 필요한 규칙 묶음을 고르는 표식이다.
실행 환경에 맞는 템플릿 부분을 선택하고 보조 파일 참조를 CLI 경로로 바꾸는 코드도 있다.
같은 모음집을 여러 에이전트에서 사용한다고 해서 환경 차이가 사라지는 것이 아니라, 그 차이를 지침 조립 단계에서 다루는 방식이다.[3]

인증 정보가 필요한 작업과 필요하지 않은 작업도 나뉜다.
공통 설정 문서는 호스트의 비밀정보 저장소나 주입된 환경 변수를 우선하는 절차를 설명하고, 일부 공개 조회는 운영 측의 프록시를 경유해 사용자 개인 키 없이 동작하도록 안내한다.
프록시는 요청을 중간에서 전달하는 서버다.
사용자 키가 필요 없다는 말은 모든 요청이 로컬에서 끝난다거나 외부 의존성이 없다는 뜻이 아니다.[4]

## 작은 계산부터 여러 출처 조사까지

`korean-character-count`는 모델이 눈대중으로 길이를 추정하지 않고 보조 스크립트로 글자·줄·바이트를 계산하도록 한다.
글자는 Unicode의 사용자 인식 단위를 기준으로 세고, UTF-8 바이트와 NEIS용 규칙을 구분한다.
같은 텍스트라도 제출처가 요구하는 단위가 다르면 답도 달라질 수 있으므로, 결과와 함께 계산 기준을 보여주도록 정한 것이 특징이다.[5]

더 복잡한 `government-support-survey`는 여러 공개 공고 출처를 모은 뒤 전체 성공 여부와 출처별 실패를 확인하도록 한다.
목록 제목만으로 자격을 확정하지 않고 공식 상세 문서와 제외 조건을 확인하는 단계도 있다.
이 사례는 여러 API를 부르는 것만으로 조사가 끝나지 않으며, 부분 결과를 완전한 결과라고 부르지 않는 보고 규칙도 스킬의 일부임을 보여준다.[6]

두 사례의 차이는 스킬을 읽을 때 볼 지점을 알려준다.
로컬 계산은 입력 보존과 계산 규칙이 중요하고, 외부 조회는 출처·조회 범위·실패·최신성의 해석이 중요하다.
이 모음집을 하나의 성능 수치나 스킬 개수로 설명하기 어려운 이유다.

## 예시로 따라가는 흐름

대표 사례는 `railway-timetable`의 공식 지침에 실린 서울→부산 KTX 계획 시간표 조회다.
문서는 출발역·도착역·날짜·시작 시각·끝 시각·결과 제한을 입력으로 받는 예시를 제시한다.
여기서 날짜는 문서 속 입력 예시이며 이번 조사에서 열차 운행을 조회한 날짜가 아니다.
에이전트가 먼저 구간과 시간 범위를 분명히 해야 한다는 구조를 보여준다.[7]

Python helper의 `search`는 이 입력을 `ktx_backend.search_public_timetable`로 전달하고, 반환 결과에 출처 설명과 주의 문구를 더한다.
문서에 따르면 바탕 자료는 코레일 공식 게시판의 XLSX 통합 계획 시간표다.
출력에는 열차 번호·종류·출발과 도착 시각, 공식 출처와 예약 진입 URL이 포함되도록 정의되어 있다.
이는 웹에서 임의로 찾은 시각을 모델이 조합하는 방식과 구분된다.[7][8]

사람이 마지막으로 확인할 부분은 계획 시간표와 실시간 운행·좌석 상태의 차이다.
helper는 운휴·지연·잔여석 정보가 아니라는 안내를 결과에 붙인다.
시간표에 열차가 등장한다고 좌석을 확보한 것도, 실제로 그날 정상 운행한다고 확인한 것도 아니다.
게시판 장애·해당 날짜에 맞는 시간표 부재·형식 변경·구간 결과 없음도 문서에 실패 유형으로 나와 있다.
실패하면 값을 메워 넣는 것이 아니라 공식 페이지 확인으로 넘겨야 한다.[7][8]

이 흐름에는 회원 로그인, 예약·예약대기·좌석 선점·결제·취소·자동 재조회가 포함되지 않는다.
자연어 요청에 여행 목적이 들어 있다고 에이전트가 예매 권한까지 얻는 것은 아니다.
결과를 읽고 실제 예매 여부를 결정하는 단계와 조회 도구의 역할을 명확히 나누는 사례다.[7]

## 설치 범위와 외부 서비스의 조건

README는 전체 스킬 설치와 특정 스킬 선택 설치를 모두 안내하며, 기본 설치에는 Node.js 18 이상과 npx, KTX helper에는 Python 3.11 이상과 uv가 필요하다고 설명한다.
스킬 지침을 가져오는 작업과 실제 helper를 실행할 환경을 갖추는 일은 별개다.
설치 안내에 나온 명령을 이 문서 조사에서 실행하지 않았다.[1]

공통 규칙은 결제·전송·최종 제출·공개 게시 등에 직전의 명시적 승인을 요구하고, 평문 인증 정보를 채팅이나 명령 인자로 노출하지 말라고 한다.
CAPTCHA·본인인증·전자서명 경계를 우회하지 않는 조건도 있다.
문서에 이런 규칙이 있다는 것과 사용하는 에이전트가 실제로 모든 상황에서 지킨다는 것은 구분해야 한다.[2][4]

기본 라이선스는 MIT이지만 프록시 서버와 모니터링 관련 일부 디렉터리는 AGPL-3.0-only다.
외부 서비스 이름이 등장해도 공식 제휴·승인을 뜻하지 않는다는 설명, 개인적 공개정보 조회로 범위를 제한하고 대량 수집이나 접근 통제 회피를 하지 말라는 조건도 확인된다.
기능별 문서와 데이터 제공처의 조건을 따로 읽어야 한다.[1]

## 직접 읽어볼 자료

1. [README](https://github.com/NomaDamas/k-skill/blob/main/README.md)
   분야별 표에서 작업을 고른 뒤 조회 전용인지 외부 변경을 포함하는지 구분한다.
   철도 안내와 라이선스 예외를 읽으면 모음집 전체에 하나의 권한·설치 조건을 적용할 수 없는 이유를 이해할 수 있다.
2. [철도 시간표 지침](https://github.com/NomaDamas/k-skill/blob/main/railway-timetable/instruction.md)과 [실제 helper](https://github.com/NomaDamas/k-skill/blob/main/railway-timetable/scripts/railway_timetable.py)
   문서의 입력·출력 항목이 `search` 함수의 인자와 결과에 어떻게 이어지는지 대조한다.
   특히 계획 시간표 경고와 예약 금지 경계를 확인하며, 문서 예시를 실제 조회 결과로 읽지 않도록 한다.
3. [공통 설정 가이드](https://github.com/NomaDamas/k-skill/blob/main/docs/setup.md)
   사용자 키, 호스트 비밀 저장소, 프록시 운영자의 키를 구분한다.
   인증 정보가 필요 없다는 문구가 무엇을 생략해 주는지와 여전히 어떤 외부 통신이 필요한지를 질문하며 읽는다.
4. [지침 조립 코드](https://github.com/NomaDamas/k-skill/blob/main/packages/k-skill-cli/src/assemble.js)
   `loadSkill`과 `assemble`을 이어 보며 짧은 SKILL 입구가 실제 작업 규칙으로 확장되는 과정을 확인한다.
   지원 에이전트 이름보다 실행 환경별로 달라지는 지침과 helper 경로에 주목한다.

## 정리

k-skill은 한국의 서비스·데이터 작업을 에이전트가 읽을 지침과 실행 도구로 연결하는 모음이다.
정확한 이해를 위해서는 작업별 데이터 출처, 로컬·프록시 경로, 결과의 한계와 승인 경계를 함께 읽어야 한다.

## 자료 확인 범위

2026-09-27 기준 README와 루트 구성, 스킬 입구·설정 가이드·지침 조립 코드, 철도 지침과 helper, 글자 수 및 공개 공고 조사 지침을 확인했다.
설치하거나 실행하지 않았으며 API 조회, 인증 정보 입력, 예약·결제·제출은 수행하지 않았다.
철도 backend 전체와 모음집의 모든 기능을 실행 검증한 문서는 아니다.

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

[1] NomaDamas/k-skill — README.md

<https://github.com/NomaDamas/k-skill/blob/main/README.md>

[2] NomaDamas/k-skill — railway-timetable/SKILL.md

<https://github.com/NomaDamas/k-skill/blob/main/railway-timetable/SKILL.md>

[3] NomaDamas/k-skill — packages/k-skill-cli/src/assemble.js

<https://github.com/NomaDamas/k-skill/blob/main/packages/k-skill-cli/src/assemble.js>

[4] NomaDamas/k-skill — docs/setup.md

<https://github.com/NomaDamas/k-skill/blob/main/docs/setup.md>

[5] NomaDamas/k-skill — korean-character-count/instruction.md

<https://github.com/NomaDamas/k-skill/blob/main/korean-character-count/instruction.md>

[6] NomaDamas/k-skill — government-support-survey/instruction.md

<https://github.com/NomaDamas/k-skill/blob/main/government-support-survey/instruction.md>

[7] NomaDamas/k-skill — railway-timetable/instruction.md

<https://github.com/NomaDamas/k-skill/blob/main/railway-timetable/instruction.md>

[8] NomaDamas/k-skill — railway-timetable/scripts/railway_timetable.py

<https://github.com/NomaDamas/k-skill/blob/main/railway-timetable/scripts/railway_timetable.py>
