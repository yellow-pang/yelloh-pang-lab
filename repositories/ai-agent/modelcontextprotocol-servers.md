---
title: "modelcontextprotocol/servers"
repository: "modelcontextprotocol/servers"
url: "https://github.com/modelcontextprotocol/servers"
category: "ai-agent"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# modelcontextprotocol/servers

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

modelcontextprotocol/servers는 AI 애플리케이션이 외부 도구와 데이터에 접근하는 MCP(Model Context Protocol)의 참고 서버 모음이다.
MCP 서버 개발과 SDK 사용법을 설명하는 교육용 구현이며, README는 production-ready 솔루션이 아니라고 명시한다.[1]

## 모델, 클라이언트, 서버는 서로 다른 역할이다

AI에게 “이 폴더에서 문서를 찾아 수정해 달라”고 말해도 모델 자체가 파일에 접근하는 것은 아니다.
이 저장소의 흐름에서는 MCP 클라이언트가 서버와 연결되고, 서버가 공개한 도구를 호출해 데이터를 받거나 작업을 수행한다.
루트 README도 서버를 혼자 실행하는 것보다 MCP 클라이언트 설정에 연결해야 한다고 설명한다.
이 저장소는 모델이나 완성된 채팅 앱이 아니라 그 연결을 구현하는 참고 코드다.[1]

범위도 모든 MCP 서버의 목록과 다르다.
README는 서버 탐색에는 MCP Registry를 안내하고, 이 저장소에는 steering group이 관리하는 소수의 참고 서버를 둔다고 밝힌다.
Filesystem, Fetch, Git, Memory, Everything 등이 서로 다른 역할을 보여준다.
GitHub·PostgreSQL 같은 이전 참고 서버는 Archived 목록으로 이동했다고 명시하므로, 오래된 예제 이름만 보고 현재 관리되는 구성으로 분류하면 안 된다.[1]

## Filesystem을 통해 읽는 실제 구성

대표 사례인 Filesystem은 Node.js에서 파일 읽기·쓰기, 목록 조회, 이동, 검색을 MCP 도구로 제공한다.
`index.ts`는 도구 이름과 입력 형식, 설명, 호출 함수를 등록하고, `lib.ts`는 경로 검사와 파일 작업을 구현한다.
실행부는 `StdioServerTransport`를 사용한다.
stdio는 프로세스의 표준 입력·출력으로 메시지를 주고받는 방식이므로 이 구현을 그대로 공개 HTTP 서버의 인증 구조라고 설명할 수는 없다.[2][3][4]

핵심 입력은 접근을 허용할 폴더다.
명령행 인자로 지정하거나, 연결된 클라이언트가 Roots를 제공할 수 있다.
Roots는 클라이언트가 서버에 알려 주는 작업의 기준 디렉터리 목록이다.
유효한 Roots를 받으면 구현은 기존 허용 목록을 그 목록으로 교체한다.
처음 명령행에 좁은 경로를 줬더라도 클라이언트가 나중에 보낸 범위가 어떻게 반영되는지 확인해야 한다.[2][3]

`validatePath()`는 요청 경로를 정리한 뒤 허용 디렉터리 안인지 검사하고, 심볼릭 링크가 가리키는 실제 경로도 확인한다.
심볼릭 링크는 다른 파일이나 폴더를 가리키는 연결이므로 겉으로는 허용 폴더 안의 이름이어도 목적지는 밖일 수 있다.
이런 검사는 실제로 코드에 있지만, 코드 검토만으로 모든 경합 조건이나 운영 환경에서의 격리 안전성을 입증한 것은 아니다.[4]

## 예시로 따라가는 흐름

공식 도구 계약을 설명하기 위해 사용자가 별도로 허용한 문서 폴더 안에 수정 대상 파일이 있다고 가정하자.
먼저 클라이언트가 Filesystem 서버를 시작하고 초기화 과정에서 허용 폴더를 설정한다.
`list_allowed_directories`로 현재 범위를 확인한 다음, `read_text_file`에 대상 경로를 보내 내용을 받는다.
구현은 파일을 읽기 전에 경로 검사를 수행하므로, 모델이 요청한 문자열이 곧바로 파일 접근 허가가 되는 것은 아니다.[2][3]

이어서 특정 문장을 바꾸려면 `edit_file`에 경로와 `oldText`·`newText`, `dryRun: true`를 전달하는 흐름이다.
서버는 편집 결과를 diff, 즉 바뀔 부분의 차이 목록으로 돌려주고 파일에 쓰지는 않는다.
사람이 대상 파일과 변경 범위를 확인한 뒤 실제 적용을 별도로 승인하는 것이 읽기와 쓰기를 구분하는 방법이다.
`dryRun`의 기본값은 `false`라서 생략하면 미리보기가 아니라 실제 변경으로 이어질 수 있다.[2][3][4]

실제 적용 뒤에는 파일을 다시 읽어 의도한 구절만 바뀌었는지 확인해야 한다.
구현의 `write_file`은 기존 내용을 통째로 덮어쓸 수 있고, `move_file`은 원래 위치에서 파일을 옮긴다.
“파일을 다룰 수 있다”는 기능 설명만으로는 이런 부작용이 드러나지 않는다.
이 사례는 도구 계약과 소스를 연결한 설명이며 실제 파일 수정이나 MCP 호출을 수행한 기록이 아니다.[2][3]

## 허용 폴더가 곧 읽기 전용은 아니다

Filesystem README는 `readOnlyHint`, `destructiveHint` 같은 도구 주석을 설명한다.
이들은 클라이언트가 읽기·쓰기·파괴 가능성을 구분하도록 돕는 힌트이고, 쓰기 도구 자체를 제거하거나 사용자 승인을 구현하는 코드와는 다르다.
Docker 예제의 `ro` 마운트처럼 파일시스템 수준에서 읽기 전용으로 제한하는 조치와 구분해서 읽어야 한다.[2][3]

Roots 갱신에도 문서와 구현의 경계를 확인할 부분이 있다.
README는 클라이언트 Roots가 허용 목록을 완전히 교체한다고 설명하지만, 실제 함수는 유효한 디렉터리가 하나 이상 있을 때만 목록을 교체한다.
빈 목록이나 유효하지 않은 목록이면 메시지를 남기고 이전 값을 유지한다.
따라서 빈 Roots를 보내면 이미 허용한 접근이 즉시 철회된다고 가정하면 안 된다.[2][3]

모든 Filesystem 도구의 `openWorldHint: false`는 이 서버의 로컬 파일 작업에 관한 표시다.
읽은 파일 내용이 MCP 클라이언트로 반환되는 구조까지 숨기는 것은 아니다.
클라이언트가 그 내용을 모델 제공자에게 전달하는지, 어떤 승인 절차를 거치는지, 로그를 어떻게 보관하는지는 별도로 확인해야 한다.
이 저장소만으로 시스템 전체의 외부 전송 금지나 개인정보 보호를 보장하지 않는다.[2][3]

## 라이선스 전환과 운영 비용

고정 커밋의 루트 LICENSE는 MIT에서 Apache-2.0으로 전환 중이라고 설명한다.
새 코드·명세 기여와 재라이선스 동의를 받은 기여는 Apache-2.0, 동의하지 않은 기존 기여는 MIT이며, 명세를 제외한 문서 기여는 CC-BY-4.0으로 구분한다.
반면 Filesystem README에는 MIT라는 설명이 남아 있다.
단일 배지만 보고 전체를 MIT 또는 Apache-2.0으로 단정하지 말고 실제 사용하는 파일과 배포물의 고지를 확인해야 한다.[5][2]

코드 라이선스와 MCP 클라이언트·모델 API·연결 서비스 이용료도 별개다.
이번 자료에서 그 서비스들의 요금은 확인하지 않았고, 로컬 서버라는 이유만으로 전체 구성이 무료라고 주장하지 않는다.
또 교육용 서버를 운영에 연결한다면 경로 제한 외에도 계정 권한, 비밀 파일 배제, 쓰기 승인과 변경 복구 절차를 따로 설계해야 한다는 것이 README의 보안 검토 전제다.[1]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/README.md)

   production 용도가 아니라는 경고와 Reference·Archived 구분부터 읽는다.
   클라이언트 설정 예시가 서버 프로세스 실행과 AI 애플리케이션의 연결을 어떻게 나누는지 확인한다.
2. [Filesystem 안내](https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/README.md)와 [도구 등록 구현](https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/index.ts)

   Roots의 설명을 실제 초기화·갱신 함수와 대조한다.
   이어 `edit_file`의 기본값과 stdio 전송을 읽으면 허용 범위, 미리보기, 원격 서버 인증을 혼동하지 않을 수 있다.
3. [파일 작업 구현](https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/lib.ts)과 [루트 LICENSE](https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/LICENSE)

   경로 검사 뒤에 실제 읽기·쓰기가 연결되는 부분을 따라간다.
   라이선스는 하위 README의 짧은 문구보다 전환 조건과 파일별 고지를 함께 확인한다.

## 자료 확인 범위

2026-10-04, 고정 커밋 `5abed86c5317b833dd59907492d56c65981642aa`의 루트 README·LICENSE와 Filesystem README·도구 등록·파일 작업 구현을 읽었다.
설치, 서버 실행, 클라이언트 연결, 파일 변경, 보안·성능 검증은 하지 않았다.
다른 참고 서버의 내부 동작까지 이 Filesystem 사례와 같다고 일반화하지 않는다.

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

[1] modelcontextprotocol/servers — README.md

<https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/README.md>

[2] modelcontextprotocol/servers — src/filesystem/README.md

<https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/README.md>

[3] modelcontextprotocol/servers — src/filesystem/index.ts

<https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/index.ts>

[4] modelcontextprotocol/servers — src/filesystem/lib.ts

<https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/src/filesystem/lib.ts>

[5] modelcontextprotocol/servers — LICENSE

<https://github.com/modelcontextprotocol/servers/blob/5abed86c5317b833dd59907492d56c65981642aa/LICENSE>
