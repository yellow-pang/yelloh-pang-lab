---
title: "NVIDIA/OpenShell"
repository: "NVIDIA/OpenShell"
url: "https://github.com/NVIDIA/OpenShell"
category: "ai-agent"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# NVIDIA/OpenShell

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

OpenShell은 자율 AI 에이전트가 파일과 API를 사용하되, 미리 정한 정책 안에서만 활동하도록 실행 환경을 제공하는 런타임이다.
에이전트나 언어 모델 자체가 아니라 실행 경계와 권한 통제를 담당하며, 기본 sandbox 이미지에는 에이전트가 설치돼 있지 않다.[1]

## “하지 말라”는 지시와 실행 제한의 차이

에이전트에게 문서로 금지 사항을 알려 주는 것과 실제 접근을 막는 것은 다르다.
OpenShell의 설명은 파일 접근, 시스템 호출, 네트워크 연결을 실행 중 통제하고, 정책 변경이 넓히는 접근을 별도로 검사하는 구조에 초점을 둔다.
sandbox는 프로그램이 접근할 자원을 제한한 실행 공간이고, 정책은 어떤 경로와 목적지·동작을 허용할지 적은 규칙이다.[1][2]

이 경계가 에이전트의 모든 판단을 올바르게 만든다는 뜻은 아니다.
허용된 폴더를 잘못 수정하거나, 허용된 외부 서비스에 민감한 내용을 보내는 문제는 여전히 검토해야 한다.
공식 보안 문서도 허용한 endpoint, 즉 접속 목적지마다 정보가 빠져나갈 경로가 생길 수 있다고 경고한다.[3]

## Gateway, supervisor, sandbox가 맡는 일

Architecture 문서에서 gateway는 sandbox의 생성·정책·접속을 관리하는 제어 계층이다.
compute driver는 선택한 Docker·Podman·Kubernetes·VM 환경에 작업 영역과 supervisor를 준비한다.
supervisor는 신뢰되는 쪽에서 정책을 판단하고 자격증명을 붙이며, sandbox 쪽은 에이전트와 같은 경계 안에서 프로세스를 관리하고 요청을 중계한다.[2]

네트워크 요청은 에이전트의 연결 시도에서 시작해 호출 프로그램 식별, 보호된 통신 경로, supervisor의 정책 판단을 거친다.
허용된 경우에만 supervisor가 실제 외부 연결을 열고 데이터를 전달한다는 설계다.
Docker에서는 작업 컨테이너의 네트워크를 끄고 별도 supervisor와 소켓으로 연결하며, Kubernetes에서는 NetworkPolicy로 supervisor 외의 직접 통신을 제한한다.
README도 Kubernetes의 CNI, 즉 컨테이너 네트워크 구성요소가 NetworkPolicy를 실제로 집행해야 한다고 요구한다.[2][1]

자격증명도 같은 경계에서 다룬다.
provider는 서비스와 저장된 인증 정보를 연결하며, 에이전트에는 실제 키 대신 대체값을 주고 승인된 목적지로 가는 요청에서 이를 치환하는 방식이다.
이는 OpenShell을 통해 관리한 자격증명에 대한 설명이지, 사용자가 직접 파일이나 환경 변수에 넣은 모든 비밀까지 자동으로 숨겨 준다는 보장은 아니다.[2][3]

## 예시로 따라가는 흐름

공식 First Network Policy 튜토리얼은 AI 모델 없이 `curl`만 있는 사용자 소유 이미지로 네트워크 정책을 설명한다.
입력은 네트워크 허용 규칙이 없는 sandbox와 GitHub API의 읽기 요청이다.
처음에는 `api.github.com`으로 나가는 연결이 차단되며, 호스트 터미널에서 로그를 읽어 목적지와 호출 프로그램, 거부 이유를 확인하는 순서다.
문서의 오류·응답 예시는 공식 설명이며 이 원고에서 얻은 실행 결과가 아니다.[4]

다음 단계는 모든 인터넷 접속을 여는 것이 아니라 `/usr/bin/curl`이 `api.github.com:443`에 접근하도록 규칙을 추가하는 것이다.
여기에 `protocol: rest`, `access: read-only`, `enforcement: enforce`를 함께 둔다.
`rest`는 HTTP 요청 내용을 검사하고, `read-only`는 GET·HEAD·OPTIONS를 허용하며, `enforce`는 나머지를 차단하는 조건이다.
L7 검사는 연결 주소만 보는 것보다 더 안쪽인 HTTP 메서드와 경로 수준을 보는 것을 뜻한다.[4]

같은 읽기 요청은 통과하고 POST 요청은 연결 자체가 가능해도 요청 단계에서 거부되는 것이 예제의 결과다.
사람은 성공 응답만 볼 것이 아니라 실제 정책 revision 반영과 거부 로그를 확인해야 한다.
공식 예제의 쓰기 차단 시험을 다른 서비스에 옮길 때는 그 대상에서 시험할 권한이 있어야 하며, 차단될 것이라는 기대만으로 임의의 쓰기 요청을 보내면 안 된다.[4]

## 규칙이 있다는 사실과 차단된다는 사실은 다르다

보안 문서의 L7 기본 모드는 `audit`다.
위반을 기록하지만 요청은 전달하므로 `read-only`라는 글자가 보이는 것만으로 쓰기가 막혔다고 판단하면 안 된다.
프로토콜별 요청 규칙 없이 호스트·포트만 허용하면 메서드와 경로를 제한하지 않으며, `tls: skip`은 TLS 내부 검사와 자격증명 치환을 끈다.[3]

파일 제한에는 Linux의 Landlock을 사용한다.
문서는 필수 baseline에 ABI 3 이상을 요구하고 적용 불가 시 시작을 거부한다고 설명한다.
다만 추가 파일 정책의 기본 `best_effort`는 적용할 수 없는 경로를 건너뛸 수 있고, `hard_requirement`는 이를 시작 실패로 다룬다.
“커널에서 제한한다”는 설명과 배포 환경에서 추가 정책이 온전히 적용됐다는 확인은 구분해야 한다.[3]

공식 문서 사이의 배치 설명에도 차이가 있다.
Architecture는 분리된 supervisor가 정책을 판단하는 구조를 설명하지만, Security Best Practices에는 gateway 수준의 CONNECT proxy 및 veth 경로 설명이 함께 남아 있다.
따라서 위 구성 설명은 Architecture를 기준으로 읽은 것이며, 특정 설치본의 정확한 패킷 경로까지 확인한 결과는 아니다.[2][3]

## 형식 검증이 보장하는 범위

policy prover는 정책을 수학적 조건으로 표현해 경계 정책보다 더 넓은 권한을 주는지 검사한다.
정책 후보가 최대 허용 범위를 넘는지 보는 boundary check와, 에이전트의 네트워크 규칙 제안에 위험한 변화가 있는지 보는 proposal risk check는 다른 질문이다.
하나를 통과해도 다른 검사까지 통과했다는 뜻은 아니다.[5]

특히 boundary check의 보장은 모델에 표현된 기능에 한정된다.
문서는 검사 대상이 파일 접근·프로세스 식별·Landlock 설정·네트워크 연결·REST 요청이라고 밝히고, GraphQL·MCP 등 지원하지 않는 규칙은 `unsupported`로 돌려준다고 설명한다.
`within_boundary`만 통과이며, `inconclusive`나 `unsupported`는 안전 판정이 아니다.
통과하더라도 정책이 해당 작업에 안전한지, 실제 sandbox가 이를 집행하는지까지 증명하지 않는다.[5]

## 지원 조건, 비용과 외부 전송

README는 Linux, Apple Silicon macOS, 실험적인 WSL 2 경로와 Docker·Podman 또는 호스트 가상화 요구를 안내한다.
코드는 Apache-2.0이고 무보증으로 제공되며, 외부에서 가져온 자료에는 별도 약관과 라이선스가 적용된다.
자체 실행 코드의 라이선스와 서버 자원·외부 모델 API 비용은 별개다.
특정 무료 모델을 쓰는 시작 예제가 있다고 전체 운영 비용이 무료인 것은 아니며, 이번에는 요금표를 확인하지 않았다.[1][6]

README는 운영 범주·횟수에 한정된 익명 telemetry 수집을 안내하고, gateway의 `OPENSHELL_TELEMETRY_ENABLED=false`로 끌 수 있다고 설명한다.
또 보안 문서는 애플리케이션 출력의 비밀을 sandbox가 자동으로 지우지 않는다고 경고한다.
정책과 별개로 로그 공유와 외부 전송 설정을 확인해야 한다.[1][3]

## 직접 읽어볼 자료

1. [Architecture](https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/about/architecture.mdx)

   gateway와 supervisor를 같은 서버 역할로 합쳐 생각하지 말고 신뢰 경계 양쪽의 책임을 구분한다.
   선택한 런타임이 직접 외부 통신을 막는 방식까지 이어 읽는다.
2. [첫 네트워크 정책 튜토리얼](https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/tutorials/first-network-policy.mdx)과 [보안 설정 안내](https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/security/best-practices.mdx)

   기본 차단, 좁은 읽기 허용, 쓰기 거부의 순서를 따라간다.
   이어 audit와 enforce, TLS 검사 생략, 추가 파일 정책의 적용 실패 조건을 대조한다.
3. [Policy Prover](https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/how-it-works/policies/prover.mdx)

   통과 예제뿐 아니라 unsupported와 inconclusive의 의미를 읽는다.
   정책 모델 비교가 실제 실행 환경 전체의 안전성을 증명하는 것은 아니라는 마지막 경계를 확인한다.

## 자료 확인 범위

2026-10-04, 고정 커밋 `71c3cd957abef062eb7f37010056717cd49f2ed3`의 README, Architecture, 보안 안내, 네트워크 정책 튜토리얼, prover 설명과 LICENSE를 읽었다.
설치, gateway·sandbox 실행, 외부 API 호출, 정책 적용, 보안·성능 검증은 하지 않았다.

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

[1] NVIDIA/OpenShell — README.md

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/README.md>

[2] NVIDIA/OpenShell — docs/about/architecture.mdx

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/about/architecture.mdx>

[3] NVIDIA/OpenShell — docs/security/best-practices.mdx

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/security/best-practices.mdx>

[4] NVIDIA/OpenShell — docs/tutorials/first-network-policy.mdx

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/tutorials/first-network-policy.mdx>

[5] NVIDIA/OpenShell — docs/how-it-works/policies/prover.mdx

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/docs/how-it-works/policies/prover.mdx>

[6] NVIDIA/OpenShell — LICENSE

<https://github.com/NVIDIA/OpenShell/blob/71c3cd957abef062eb7f37010056717cd49f2ed3/LICENSE>
