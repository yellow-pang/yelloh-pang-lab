---
title: "logseq/logseq"
repository: "logseq/logseq"
url: "https://github.com/logseq/logseq"
category: "developer-tools"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# logseq/logseq

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Logseq는 메모를 페이지와 작은 블록 단위로 정리하고 참조·속성·할 일을 연결하는 지식 관리 도구다.
이 저장소를 읽을 때는 기존 파일 기반 사용법과 새 DB 그래프의 저장·동기화 방식을 먼저 구분해야 한다.[1][2][3]

## 노트를 저장하는 방식까지 구분해야 하는 이유

README는 지식 관리, 협업, PDF 주석, 할 일 관리, 플러그인과 테마를 소개한다.
여기서 그래프는 여러 메모와 그 관계를 하나로 관리하는 작업 공간으로 이해할 수 있다.
DB 버전은 이 그래프를 다루는 별도 흐름을 도입하며, 새로운 모바일 앱과 RTC도 따로 안내한다.
RTC는 여러 기기에서 변경을 맞추거나 함께 편집하기 위한 실시간 협업 방식이다.[1]

가장 먼저 읽을 경고는 DB 버전이 베타, 새 모바일 앱과 RTC가 알파 상태라는 점이다.
README는 데이터 손실 가능성을 명시하고 자동 백업이나 정기 SQLite DB 백업을 권한다.
중요하지 않은 프로젝트 하나를 별도 테스트 그래프에서 다루라는 안내도 있다.
새로운 기능이 소개된 것과 중요한 기록을 맡길 안정성이 검증된 것은 같은 판단이 아니다.[1]

또 `test/db`와 `master`, 웹 앱과 nightly 데스크톱을 구분해 접근 경로를 제시한다.
이 원고의 고정 커밋 설명을 모든 배포 버전의 기능 설명으로 일반화하지 않는다.
기존 사용자에게는 DB 버전의 변경점을 별도 문서에서 읽도록 안내하고 있으므로, 오래된 사용법을 그대로 적용하기 전에 자신이 사용하는 버전을 확인해야 한다.[1]

## 파일 그래프와 DB 그래프는 같은 저장 모델이 아니다

README에는 Markdown과 Org-mode 지원이라는 일반 소개가 남아 있지만, DB 그래프를 “페이지마다 Markdown 파일을 직접 편집하는 구조”로 설명하면 잘못 전달된다.
Markdown Mirror 설계 문서는 DB 그래프가 그래프 폴더 안에 페이지별 편집 가능한 Markdown 파일을 노출하지 않는다고 명시한다.
따라서 같은 Logseq 이름 아래에서도 파일 기반 자료와 DB 버전 설명의 전제를 구분해야 한다.[1][4]

`deps/db` 설명은 앱과 CLI가 사용하는 데이터 계층을 DataScript와 SQLite로 나눈다.
DataScript는 앱에서 자료를 다루는 쪽, SQLite는 DB 그래프의 저장 쪽에 해당한다.
이 라이브러리가 DB 그래프의 기본 스키마, 즉 데이터 항목과 관계의 구조를 정의하며, 일부 이름공간만 파일 그래프의 파서도 지원한다고 설명한다.[2]

이 차이는 단순한 파일 확장자의 문제가 아니다.
어느 데이터가 원본인지, 외부 편집기를 사용하면 변경이 어디로 반영되는지, 백업이 무엇을 보존하는지를 바꾼다.
DB 그래프의 Markdown 출력이 있다고 해서 파일 그래프와 동일한 양방향 편집 구조라고 추론해서는 안 된다.[4]

## Markdown Mirror는 원본이 아니라 파생된 읽기 자료다

Mirror는 DB 내용을 외부 도구에서 읽을 수 있도록 Markdown으로 내보내는 기능이다.
설계 문서는 데스크톱 설정에서 켜면 그래프 폴더의 `mirror/markdown/pages/`와 `mirror/markdown/journals/`에 파생 파일을 둔다고 설명한다.
원본은 계속 DB이며, 그 폴더의 파일을 고쳐도 그래프로 되돌아오지 않는다는 점을 비목표로 명시한다.[4]

대표 worker 구현에도 출력 경로 계산, 변경된 페이지 찾기, 같은 내용이면 쓰기 생략, 파일 쓰기와 삭제를 처리하는 함수가 있다.
worker는 편집 화면의 주 처리 흐름과 별도로 작업을 맡는 구성요소다.
여기서 코드로 확인한 것은 DB의 변경 정보를 받아 영향을 받은 페이지의 파일 작업을 수행하는 경로이지, Markdown을 수정해 DB로 가져오는 경로가 아니다.[5]

설계 문서는 처음 지원하는 환경을 Electron 데스크톱으로 규정하지만, 현재 대표 구현의 런타임 판정에는 Node와 Electron이 들어 있다.
설계 문서와 구현의 범위를 구분해야 하며, 이것만으로 모든 CLI 명령이나 브라우저·모바일에서 동일하게 제공된다고 판단하지 않는다.
또 Mirror는 완전한 DB 내보내기나 복원 충실도를 보장하는 백업 형식이 아니라고 명시되어 있다.[4][5]

## 예시로 따라가는 흐름

공개 문서를 읽고 독서 메모를 남기는 상황을 이해용으로 가정해 보자.
입력은 페이지 제목, 요약 블록, 참고할 다른 페이지, 아직 작성해야 할 초안 상태다.
블록은 노트를 구성하는 작은 항목이며 들여쓰기로 상위·하위 관계를 표현한다.
DB 그래프의 Markdown Mirror 문법은 `- Parent` 아래에 들여쓴 `- Child`를 두는 예제를 보여준다.[3]

자료를 다시 찾기 위한 관계는 원문의 `[[Project Plan]]` 같은 참조로 표현하고, 담당자 같은 값은 `* owner:: [[Alice]]`처럼 속성으로 나타낸다.
할 일 상태는 `- TODO Write draft`와 `- DONE Publish note`처럼 블록 줄에 담는다.
이 문법을 따르면 외부에서 읽는 결과 파일에서도 본문, 속성, 참조, 할 일 상태를 구분할 수 있다.
이는 문법 문서의 예제를 설명한 것이며 실제 사용자의 메모나 실행 결과는 아니다.[3]

데스크톱에서 Mirror를 켠 상황이라면 설계상 이후 편집은 관련 페이지의 출력 작업으로 이어지고, 기존 전체 내용을 내보내려면 별도 전체 재생성 동작이 있다.
사람은 DB에서 고친 내용이 출력에 반영됐는지, 참조할 페이지와 속성이 맞는지 확인해야 한다.
외부 Markdown 편집기로 Mirror만 고친 뒤 DB에도 저장됐다고 생각하면 안 된다.
설계 문서는 이후 Logseq 편집으로 외부 수정이 덮어써질 수 있고, 중복 페이지 제목의 참조는 모호할 수 있다고 경고한다.[4]

## 백업, 동기화, 공개의 경계

Mirror에 읽기 좋은 파일이 생긴다는 사실은 복구 가능한 백업이 있다는 뜻이 아니다.
설계상 속성 페이지와 일부 내장 페이지는 출력하지 않으며, 자산 파일도 Mirror 폴더로 복사하지 않는다.
원본 DB를 보존하는 백업과 다른 도구에서 읽기 위한 사본은 목적이 다르다.
베타 경고와 함께 읽어야 하는 이유다.[1][4]

프라이버시 중심이라는 README의 목표 역시 “모든 기능에서 데이터가 기기 밖으로 나가지 않는다”는 보증으로 바꾸지 않는다.
README가 소개하는 RTC는 기기 간 동기화와 협업 경로이므로, 실제 사용 시 저장·전송 대상과 공유 범위를 확인해야 한다.
이 원고는 서버 구성, 암호화 구현, 플러그인 동작을 감사하지 않았고 동기화 서비스의 요금도 확인하지 않았다.[1]

코드의 루트 라이선스는 AGPLv3다.
앱을 사용하는 일과 코드를 수정·배포하거나 네트워크로 수정본을 제공하는 일의 조건을 구분해야 한다.
특히 라이선스의 네트워크 상호작용 조항은 수정본 사용자에게 해당 소스를 받을 기회를 제공하도록 규정하므로, 별도 배포를 검토할 때 원문을 확인할 대상이다.[6]

## 직접 읽어볼 자료

1. [루트 README의 DB 버전 안내](https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/README.md)

   기능 목록보다 베타·알파 경고와 백업 안내를 먼저 읽는다.
   배포 경로와 기존 사용자용 변경 문서를 구분하면 오래된 파일 그래프 설명과 새 DB 기능을 섞지 않을 수 있다.
2. [데이터 계층 설명](https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/deps/db/README.md)

   DataScript와 SQLite, DB 그래프와 일부 파일 그래프 지원 범위를 확인한다.
   내부 구조를 깊이 외우기보다 앱이 사용하는 자료 구조와 저장되는 원본을 구분하는 데 초점을 둔다.
3. [Markdown Mirror 설계](https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/docs/adr/0016-markdown-mirror.md)

   Decision 다음에 Non-goals와 Tradeoffs를 읽는다.
   외부 편집이 되돌아오지 않는다는 점과 백업으로 보장하지 않는 범위가 핵심이다.
4. [DB 그래프의 Mirror 문법](https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/docs/logseq-markdown-syntax.md)

   블록, 속성, 참조, 할 일 예제를 차례로 읽는다.
   페이지 ID 표식은 일반 속성이 아니라 DB 페이지와 연결하기 위한 내부 식별자라는 설명도 확인한다.

## 자료 확인 범위

2026-10-04 고정 커밋 `22a29b30dee3b3930cf49bba50454650c31d2a07`의 README, DB 라이브러리 설명, Mirror 설계·문법·대표 worker 구현과 AGPLv3 라이선스를 읽었다.
설치·실행·성능 검증, 그래프 생성·이전·복원, 실제 동기화와 개인정보 보호 검증은 하지 않았다.

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

## Sources

[1] logseq/logseq — README.md

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/README.md>

[2] logseq/logseq — deps/db/README.md

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/deps/db/README.md>

[3] logseq/logseq — docs/logseq-markdown-syntax.md

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/docs/logseq-markdown-syntax.md>

[4] logseq/logseq — docs/adr/0016-markdown-mirror.md

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/docs/adr/0016-markdown-mirror.md>

[5] logseq/logseq — src/main/frontend/worker/markdown_mirror.cljs

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/src/main/frontend/worker/markdown_mirror.cljs>

[6] logseq/logseq — LICENSE.md

<https://github.com/logseq/logseq/blob/22a29b30dee3b3930cf49bba50454650c31d2a07/LICENSE.md>
