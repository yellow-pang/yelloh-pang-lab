---
title: "oblien/openship"
repository: "oblien/openship"
url: "https://github.com/oblien/openship"
category: "backend-infra"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# oblien/openship

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Openship은 GitHub 저장소나 로컬 소스에서 앱을 빌드하고 실행한 뒤 도메인과 HTTPS 연결까지 관리하는 자체 호스팅 가능 배포 플랫폼이다.
데스크톱 앱, 웹 화면, CLI, SDK는 같은 배포 기능으로 들어가는 서로 다른 통로이며, 데스크톱 앱 자체가 노트북을 공개 앱 서버로 만드는 것은 아니다.[1]

## 관리 프로그램이 있는 곳과 앱이 도는 곳

이 프로젝트를 이해할 때 먼저 구분할 것은 control plane, 즉 배포를 지시하고 상태를 관리하는 프로그램과 실제 앱 실행 위치다.
데스크톱 방식은 앱이 열려 있을 때 로컬에서 관리 기능을 실행하고, SSH로 연결한 서버나 Openship Cloud에 배포한다.
반면 push-to-deploy처럼 GitHub의 변경 알림을 받거나 여러 사람이 접근하려면 항상 켜진 공개 endpoint가 필요하다고 README는 설명한다.[1]

자체 호스팅도 두 경로다.
Linux와 Docker가 있는 Compose 모드는 Postgres·Redis·API·dashboard·OpenResty edge를 실행하고 같은 머신에서 배포 앱을 호스팅한다.
bare 모드는 내장 데이터베이스를 쓰는 관리 프로세스로 다른 서버나 Cloud에 배포한다.
루트 `docker-compose.yml`과 자체 호스팅용 `docker/docker-compose.yml`도 역할이 다르다고 명시하므로 파일 이름만 보고 아무 구성을 실행하면 안 된다.[1]

## 소스에서 공개 주소까지 이어지는 단계

README의 배포 흐름은 감지, 빌드, 실행, 경로 연결과 TLS 설정으로 이어진다.
감지 단계는 `package.json`, 잠금 파일, 프레임워크 설정과 Compose 파일 등을 읽어 명령과 포트를 결정한다.
`openship.json`으로 자동 판단을 덮어쓸 수 있고, 결정된 설정은 snapshot으로 보관한다.
이는 무엇을 배포했는지 남기는 구조이지 앱 데이터나 모든 외부 의존성까지 동일하게 복원한다는 증명은 아니다.[1]

실행은 컨테이너 또는 관리되는 호스트 프로세스로 진행하고, OpenResty는 들어온 요청을 앱으로 전달하는 reverse proxy 역할을 한다.
TLS는 HTTPS의 암호화 통신을 구성하는 부분이다.
README는 앱이 뜬 뒤 도메인·인증서 처리를 하므로 DNS나 인증서 문제가 별도 조치 필요 상태로 나타날 수 있다고 설명한다.
따라서 배포 완료 표시와 외부 사용자가 정상 HTTPS 주소로 접근할 수 있다는 확인을 분리해야 한다.[1]

## 예시로 따라가는 흐름

공식 첫 배포 가이드는 dashboard의 Library에서 GitHub 연결 권한을 부여하고 앱 저장소를 선택하는 사례를 보여준다.
입력은 저장소와 branch, 배포 대상, 환경 변수다.
사용자는 자동 감지된 프레임워크와 install·build 명령, 주소·이름을 확인한 뒤 Deploy를 누른다.
가이드는 기본값을 강조하지만, 민감한 환경 변수와 실행할 코드의 출처는 사람이 확인해야 하는 부분이다.[2]

그다음 단계별 상태와 실시간 빌드 로그가 표시되고, 완료 시 Ready 상태와 Visit Site 진입점이 제공된다.
빌드가 실패하면 누락된 명령·환경 변수·잘못된 포트 같은 조건을 로그에서 확인하도록 안내한다.
사람은 성공 문구만 읽지 말고 목표 branch가 맞는지, 앱이 예상한 포트에서 응답하는지, 도메인·인증서 처리가 끝났는지 확인해야 한다.
이는 공식 화면 흐름의 요약이며 실제 저장소 연결이나 배포를 수행한 기록이 아니다.[2][1]

같은 동작을 코드로 좁혀 보는 자료가 `native-lifecycle.mjs` 예제다.
이 코드는 `createShip`으로 임시 설치를 만들고 Alice와 Bob의 사용자 범위를 분리한다.
Alice가 `index.html` 파일 내용을 입력으로 배포하고 준비 상태를 기다린 뒤, 같은 프로젝트에 두 번째 버전을 배포해 이력을 확인한다.
`routing: "none"`이므로 이 예제의 결과는 공개 URL이 없는 정적 release다.
SDK로 ready 상태를 얻었다는 사실과 인터넷에 공개된 사이트를 만들었다는 사실이 다름을 보여준다.[3]

예제에는 Bob이 Alice의 프로젝트를 조회·삭제하려 할 때 `NOT_FOUND`가 나오는지, 세션을 지운 뒤 `UNAUTHORIZED`가 나오는지 확인하는 assertion도 있다.
assertion은 예상 조건을 코드로 검사하는 문장이다.
설치를 다시 열어 저장 상태와 비밀값 마스킹을 확인한 다음 프로젝트와 임시 설치를 삭제하므로, 단순히 읽기만 하는 샘플은 아니다.
이 원고는 그 검사 코드를 읽었을 뿐 통과 결과를 직접 확인하지 않았다.[3]

## 배포 권한과 실행 격리는 별개다

권한 문서는 조직·역할·개별 grant로 접근을 판단한다고 설명한다.
특히 일반 `member`는 읽기만 하는 역할이 아니라 과금·감사 로그 등의 예외를 제외한 자원에 읽기·쓰기·삭제 권한을 가진다.
표준 역할의 세밀한 위임은 베타 단계라는 주의가 있으며, 좁은 권한에는 `restricted` 역할이나 scoped token을 사용하도록 한다.
scoped token은 발급자가 owner여도 토큰에 명시한 범위로 제한한다는 계약이다.[4]

실행 환경의 격리는 또 다른 층이다.
Docker 방식은 컨테이너·프로젝트별 네트워크·이름 있는 볼륨을 분리하지만, 지정한 호스트 경로를 붙이는 bind mount는 그대로 유지한다.
Docker 없는 서버 실행은 파일시스템·네트워크·사용자를 호스트와 공유하고 CPU·메모리 상한도 적용하지 않는다고 문서가 경고한다.
자체 호스팅 프로젝트는 기본 자원 한도가 없으므로 제한을 설정했는지도 따로 확인해야 한다.[5]

관리 계층 자체는 강한 권한을 요구할 수 있다.
README의 자체 호스팅 API 컨테이너는 호스트 Docker socket을 마운트해 앱을 빌드·실행하고, 이를 호스트 권한을 가진 경로라고 경고한다.
SDK 예제 역시 `policy: { allowHostExecution: true }`를 명시한다.
앱 사용자 간의 조직 권한 검사가 있다는 사실만으로 신뢰할 수 없는 코드를 호스트에서 안전하게 실행할 수 있다고 결론 내리면 안 된다.[1][3]

## 비용과 혼합 라이선스

README는 자체 호스팅을 무료·과금 없음으로 설명한다.
이는 Openship 자체의 과금 안내이며 서버·도메인·저장·전송 같은 운영 자원이 무상이라는 뜻은 아니다.
Cloud 문서에는 구매한 자원 조건에 따른 할당이 나오지만, 이 조사에서는 실제 요금표나 비용 상한을 확인하지 않았다.[1][5]

라이선스는 Apache-2.0 하나로 끝나지 않는다.
Openship이 작성한 코드는 Apache-2.0이고, 포함된 iRedMail engine에는 GPL v3 본문과 파일별 고지가 있다.
공식 라이선스 목록은 메일 기능을 사용하지 않아도 API 이미지, CLI, desktop 등의 패키징 경로에 엔진 소스가 포함된다고 명시한다.
프로세스가 분리돼 있다는 이유로 배포물 안의 라이선스 고지가 없어지는 것은 아니다.[6]

더 중요한 미해결 사항은 Zero Email로 식별되는 email client/server의 별도 라이선스·출처 고지 검토다.
`docs/licensing.md`는 해당 하위 패키지에 별도 라이선스 파일이나 필드가 없고 upstream revision과 필요한 고지 검토가 남았다고 적는다.
루트 라이선스를 임의로 적용하지 말고, 고정한 실제 배포물에 무엇이 들어 있는지 확인해야 한다.[6]

README의 “production-ready core”는 프로젝트 측 상태 설명이다.
같은 문서에 향후 항목으로 남은 multi-node cluster와 load-balancing UI 등이 있고, 권한 문서에도 베타 제약이 있다.
따라서 전체 기능의 완성도·운영 안정성·격리 수준을 이 문구만으로 보증하지 않는다.[1][4]

## 직접 읽어볼 자료

1. [README](https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/README.md)와 [첫 배포 가이드](https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/apps/web/content/docs/getting-started/first-deployment.mdx)

   제어 계층을 어디에 둘지 먼저 정리한 뒤 GitHub 입력이 공개 주소로 이어지는 단계를 읽는다.
   desktop, bare, Compose를 앱 실행 방식과 혼동하지 않는 것이 출발점이다.
2. [Native SDK 생명주기 예제](https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/packages/openship/examples/native-lifecycle.mjs)

   파일 입력, 두 번째 배포, 사용자 범위, 재시작과 삭제 순서를 따라간다.
   공개 라우팅을 끈 조건과 호스트 실행을 허용한 조건을 함께 읽어 예제 결과의 의미를 한정한다.
3. [권한 안내](https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/apps/web/content/docs/security/permissions.mdx)와 [라이선스·패키징 목록](https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/docs/licensing.md)

   member의 삭제 권한, scoped token의 제한을 확인한다.
   이어 설치 방식마다 포함되는 iRedMail과 아직 정리되지 않은 Zero Email 고지를 읽어 기능 사용 여부와 배포물 책임을 구분한다.

## 자료 확인 범위

2026-10-04, 고정 커밋 `3d33f5d31e775575626d7855d61f50e926999bce`의 README, 첫 배포·권한·격리 문서, SDK 예제와 라이선스 목록을 읽었다.
설치, SDK 실행, 계정 연결, 서버·도메인 설정, 배포·삭제, 성능·보안 검증은 하지 않았다.

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

[1] oblien/openship — README.md

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/README.md>

[2] oblien/openship — apps/web/content/docs/getting-started/first-deployment.mdx

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/apps/web/content/docs/getting-started/first-deployment.mdx>

[3] oblien/openship — packages/openship/examples/native-lifecycle.mjs

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/packages/openship/examples/native-lifecycle.mjs>

[4] oblien/openship — apps/web/content/docs/security/permissions.mdx

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/apps/web/content/docs/security/permissions.mdx>

[5] oblien/openship — apps/web/content/docs/security/isolation.mdx

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/apps/web/content/docs/security/isolation.mdx>

[6] oblien/openship — docs/licensing.md

<https://github.com/oblien/openship/blob/3d33f5d31e775575626d7855d61f50e926999bce/docs/licensing.md>
