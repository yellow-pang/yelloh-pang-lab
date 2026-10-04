---
title: "cloudflare/computer"
repository: "cloudflare/computer"
url: "https://github.com/cloudflare/computer"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# cloudflare/computer

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Cloudflare Computer는 Durable Object 안에 지속 파일 상태를 두고 여러 실행 환경에서 접근하게 하는 가상 파일 시스템 프로젝트다.
이름과 달리 화면을 클릭하는 데스크톱 자동화 앱이 아니다.
공식 README는 SQLite를 상태의 기준으로 삼고, `workspace.runtime`이라는 실행 표면에 컨테이너·셸·JavaScript 백엔드를 연결하는 구조를 설명한다.[1]

## 실행은 짧아도 파일은 남아야 한다

에이전트가 파일을 작성한 뒤 다른 단계에서 읽거나 변환하려면 실행 환경이 바뀌어도 작업 상태가 이어져야 한다.
계산을 하는 프로세스마다 파일 복사본이 따로 있으면 어느 쪽이 최신인지, 변경을 언제 동기화할지 문제가 생긴다.
Computer는 Workspace의 파일 상태를 Durable Object의 저장소에 두고 실행 방식만 교체 가능한 구조로 제시한다.[1]

Durable Object는 여기서 이름으로 접근하는 상태 보관 단위이고, SQLite가 그 안의 기준 데이터를 맡는다.
Workspace는 파일 조작과 실행을 묶어 보여주는 작업 공간이다.
백엔드를 붙이지 않고 파일 시스템만 사용할 수도 있으므로, 파일 저장과 코드 실행은 반드시 한 기능으로 결합된 것이 아니다.[1]

## 같은 입구, 서로 다른 실행 능력

컨테이너 백엔드는 `computerd`라는 데몬이 FUSE 마운트를 제공한다.
FUSE는 저장소를 일반 파일 시스템처럼 보이게 하는 연결 방식이다.
이 경로는 실제 Linux 프로그램과 네트워크를 사용할 수 있고, SQLite 상태를 컨테이너에 투영한 뒤 변경을 RPC로 동기화한다고 설명된다.
RPC는 다른 실행 주체의 기능을 원격 호출하는 통신 방식이다.[1]

Isolate shell은 Dynamic Worker에서 `just-bash`를 실행하고, Isolate JavaScript는 새 Dynamic Worker에서 ECMAScript 모듈을 평가한다.
후자는 구조화된 입력·결과, 상대 import, Workspace 기반 `node:fs/promises` 같은 표면을 제공한다.
같은 `runtime.exec`를 사용해도 `source`가 셸 명령인지 JavaScript 모듈인지는 백엔드에 따라 다르다.
일반 Linux 프로그램이 필요한 작업과 JavaScript 처리를 같은 실행 능력으로 보면 안 된다.[1]

## JavaScript 예제에서 상태가 오가는 방식

공식 `worker-javascript` 예제는 HTTP 요청을 Worker가 받고, 이름에 대응하는 Durable Object의 Workspace를 구한다.
코드에는 `withWorkspace`에 저장소, `WorkerJavaScriptBackend`, R2 마운트를 넘기는 구성이 보인다.
파일 경로는 `/workspace` 아래로 제한하고 `..` 구성요소를 거부하는 별도 검사도 있다.[2][3]

파일 쓰기는 PUT, 읽기는 GET, 실행은 POST 경로로 나뉜다.
실행 요청은 비어 있지 않은 `source` 문자열을 요구하고 `input`, 작업 디렉터리, 환경 변수, 표준 입력을 런타임 호출로 넘긴다. `handle.result()`를 기다린 결과를 JSON 응답으로 보내므로 HTTP 응답을 만들기 전에 실행 결과를 받는 구조다.[3]

예제 문서는 매 실행마다 새 Dynamic Worker를 만들지만 파일 작업은 bridge를 통해 호스트 Durable Object의 저장소로 되돌아간다고 설명한다.
이 경로는 기준 저장소가 하나이므로 push·pull 동기화가 필요 없는 `sync: "none"`이다.
새 Worker를 쓴다는 것이 새 빈 작업 디렉터리만 가진다는 뜻은 아닌 셈이다.[2]

## 예시로 따라가는 흐름

예제의 smoke test는 `/workspace/hello.txt`에 `hello`를 쓰고 다시 읽는 요청으로 시작한다.
이어 실행 요청의 JavaScript는 `node:fs/promises`의 `readFile`을 가져와 같은 경로의 파일을 읽고 공백을 정리해 반환한다.
입력 파일은 HTTP 파일 경로로 저장되지만 실행 코드는 일반 파일 읽기 형태로 접근하므로, 저장과 계산이 서로 다른 입구를 통해 같은 Workspace를 사용하는 과정을 보여준다.[2][3]

중간에는 Worker가 요청의 이름으로 Durable Object를 선택하고, 모듈 실행이 host bridge를 통해 그 상태에 접근한다.
반환값은 `value`에, 콘솔 출력과 오류 출력은 `stdout`·`stderr`에 담기는 것으로 문서에 설명되어 있다.
사람은 파일 내용 반환과 실행 로그를 구분하고 `status`, `exitCode`도 함께 확인해야 한다.
파일 읽기 결과가 맞는지와 런타임이 성공 상태로 끝났는지는 관련되지만 같은 필드는 아니다.
이 글은 공식 예제를 따라 읽은 것이며 Worker를 배포하거나 HTTP 요청·모듈 실행을 수행한 결과가 아니다.[2]

## 예제의 격리와 서비스 운영은 별개다

해당 JavaScript 예제는 `globalOutbound: null`로 모듈이 공용 인터넷에 직접 접근하지 못한다고 설명한다.
환경 변수도 요청에 넘긴 값만 채우고 호스트 환경은 노출하지 않는다고 명시한다.
이는 이 예제의 조건이며, 컨테이너 경로나 다른 네트워크 정책까지 동일하다고 일반화하면 안 된다.[1][2]

확인한 HTTP 예제 코드는 파일과 실행 입구를 보여주는 데 초점을 맞춘다.
이 코드만 읽어 인증·사용자별 권한·비용 제한이 포함된 완성형 공개 서비스를 얻었다고 결론 내릴 수 없다.
이름으로 Workspace를 고르는 구조와 접근을 허용하는 정책은 따로 설계해야 할 문제다.[3]

무엇보다 루트 README는 PREVIEW ONLY를 강조하고 현재 운영용으로 적합하지 않다고 명시한다.
API가 불안정하고 설계가 바뀔 수 있으며, `docs/`의 사양은 오늘의 구현 설명이 아니라 향후 의도를 담는다고 경고한다.
기능 존재를 확인하려면 미래 설계 문구보다 공개 사용 표면과 현재 예제를 우선해서 읽어야 한다.[1]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/cloudflare/computer/blob/main/README.md)
   세 백엔드가 어떤 실행 능력을 주는지 비교하고 preview 경고를 먼저 확인한다.
   파일 시스템만 사용하는 경우와 실행기를 붙이는 경우를 나누면 프로젝트 이름의 범위를 오해하지 않는다.
2. [worker-javascript 예제 설명](https://github.com/cloudflare/computer/blob/main/examples/worker-javascript/README.md)
   Architecture와 smoke test를 이어 읽으며 HTTP 요청, Dynamic Worker, host bridge, SQLite 상태가 어떤 순서로 연결되는지 추적한다.
   로그와 구조화 반환값도 따로 확인한다.
3. [예제의 실제 요청 처리 코드](https://github.com/cloudflare/computer/blob/main/examples/worker-javascript/src/index.ts)
   `resolveMountPath`, `handleFile`, `handleExec`를 읽으면 경로 검사와 입력 검증, 결과 대기 지점을 찾을 수 있다.
   예제가 제공하는 표면과 운영 서비스에 추가해야 할 정책을 구분하는 자료다.

## 정리

Cloudflare Computer는 지속 파일 상태와 교체 가능한 실행 환경을 연결한다.
같은 Workspace를 여러 계산 방식에서 다룬다는 점이 중심이며, preview 상태와 백엔드별 능력·격리 조건을 분명히 나누어 읽어야 한다.

## 자료 확인 범위

2026-09-27 공식 README, worker-javascript 예제 README와 `src/index.ts` 전체를 확인했다.
의존성 설치, Worker 배포, 컨테이너 실행, HTTP 호출이나 성능 측정은 수행하지 않았다.

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

[1] cloudflare/computer — README.md

<https://github.com/cloudflare/computer/blob/main/README.md>

[2] cloudflare/computer — examples/worker-javascript/README.md

<https://github.com/cloudflare/computer/blob/main/examples/worker-javascript/README.md>

[3] cloudflare/computer — examples/worker-javascript/src/index.ts

<https://github.com/cloudflare/computer/blob/main/examples/worker-javascript/src/index.ts>
