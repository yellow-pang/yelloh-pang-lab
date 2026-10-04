---
title: "goauthentik/authentik"
repository: "goauthentik/authentik"
url: "https://github.com/goauthentik/authentik"
category: "backend-infra"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# goauthentik/authentik

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

authentik은 여러 애플리케이션에서 사용자의 신원을 확인할 때 공통으로 이용하는 Identity Provider, 줄여서 IdP다.
SSO, 즉 한 번의 로그인 경험을 여러 서비스에 연결하는 환경을 위한 자체 호스팅 프로젝트이며 SAML, OAuth2/OIDC, LDAP와 RADIUS 등의 규약을 지원한다고 설명한다.[1]

## 로그인 화면을 모으는 것 이상의 문제

여러 개인용 서비스를 운영하면 각 서비스에서 계정과 인증 방식을 따로 관리하게 된다.
인증 제공자는 이들 사이에 공통 신원 확인 지점을 둔다.
여기서 인증은 “누구인가”를 확인하는 일이며, 각 서비스 안에서 어떤 자료를 읽고 바꿀 수 있는지 정하는 권한과는 구분해야 한다.
authentik을 연결했다고 모든 서비스의 세부 권한 정책까지 자동으로 같아지는 것은 아니다.[1]

프로토콜 이름이 여럿인 이유는 애플리케이션이 신원을 주고받는 방식이 다르기 때문이다.
지원 규약이 있다는 사실만으로 임의의 서비스와 바로 연결되는 것은 아니며, 해당 서비스가 요구하는 연동 방식과 설정을 맞춰야 한다.
README의 전체 지원 목록과 실제 로그인 절차를 구성하는 내부 흐름을 나누어 읽으면 이 프로젝트의 역할이 선명해진다.[1][2]

## 로그인은 순서가 있는 단계의 조합이다

대표 자료인 기본 인증 blueprint는 로그인 흐름을 YAML 선언으로 표현한다.
blueprint는 여러 설정 객체와 그 연결 관계를 재현 가능한 형태로 적은 문서다. `authentik_flows.flow` 객체를 만들고, 식별·비밀번호·추가 인증 검증·로그인 단계를 이 흐름에 연결한다.
화면 하나의 동작을 커다란 함수로만 보는 대신 순서와 조건이 있는 구성으로 읽을 수 있다.[2]

식별 단계에는 이메일과 사용자 이름이 포함된다.
이는 먼저 어느 계정의 인증을 진행할지 정하는 일이다.
비밀번호 단계는 내부 인증과 외부 원천의 backend들을 나열하며, 추가 인증 검증은 별도 stage로 존재한다.
stage는 로그인 과정의 한 단계이고 binding은 그 단계를 특정 flow에 붙이는 연결이다.
코드에 보이는 순서는 식별, 비밀번호, 추가 인증 검증, 최종 로그인으로 이어진다.[2]

하지만 모든 사용자가 언제나 같은 화면을 통과하는 것은 아니다.
기본 blueprint에는 단계 실행 여부를 정하는 표현식 정책이 있다.
이미 인증 backend가 붙은 사용자인지 살펴 비밀번호 단계를 판단하고, 비밀번호 없는 WebAuthn 방식이라면 추가 검증 stage를 건너뛰는 조건을 둔다.
“단계가 선언되어 있음”과 “그 단계가 매번 실행됨”은 다른 사실이다.[2]

이 구조를 읽을 때는 인증 수단과 흐름 제어를 분리하는 편이 좋다.
비밀번호를 확인하는 코드 자체가 없어져도 된다는 뜻이 아니라, 어떤 경로로 신원이 이미 확인되었는지에 따라 후속 절차가 달라진다는 뜻이다.
blueprint를 복사하기보다 대상 사용자의 인증 경로와 정책 조건을 검토해야 하는 이유가 여기에 있다.[2]

## 예시로 따라가는 흐름

공식 기본 인증 blueprint를 기준으로 비밀번호를 쓰는 로그인 경로를 문서상에서 따라가 보자.
먼저 식별 stage가 이메일 또는 사용자 이름을 입력받도록 설정된다.
연결 순서가 다음 단계로 넘어가면 비밀번호 stage 실행 여부를 표현식 정책이 결정한다.
확인한 식은 flow plan이 없으면 참을 반환하고, 아직 인증 backend가 붙지 않은 pending user라면 비밀번호 단계가 필요하다는 취지로 작성되어 있다.[2]

그 다음에는 추가 인증 검증 stage와 최종 로그인 stage가 각각 별도의 binding으로 연결된다.
추가 검증 쪽 표현식은 인증 방식이 `auth_webauthn_pwl`인지 확인하는 예외 조건을 포함한다.
그러므로 문서를 읽는 사람은 “MFA라는 이름이 있으니 무조건 모든 로그인에서 추가 화면이 나온다”고 결론 내리지 말고, 실행 조건과 실제 등록된 인증 수단을 함께 살펴야 한다.[2]

예상 결과는 설정된 절차에 따라 로그인 상태에 도달하는 흐름이지, 이 글에서 관측한 로그인 성공 화면이 아니다.
직접 실행하지 않았고 애플리케이션 연동도 수행하지 않았다.
실제 확인에서는 정상 경로뿐 아니라 잘못된 입력, 외부 인증을 이미 마친 경우, 비밀번호 없는 경로에서 어떤 stage가 실행되는지를 구분해야 한다.
이는 기본 파일을 이해하는 점검 항목이며 새로운 인증 우회 절차를 제안하는 것이 아니다.[2]

## 배포 규모와 보안 경계

README는 작은 시험 구성에 Docker Compose, 더 큰 구성에 Kubernetes와 Helm을 안내한다.
별도 클라우드 배포 경로도 있지만 배포 방법을 선택했다는 사실이 계정 복구, 접근 제한과 서비스 연결 정책을 대신해 주지는 않는다.
인증 제공자가 여러 서비스의 공통 지점이 되면 그 설정 오류의 영향 범위도 함께 생각해야 한다.[1]

이번에 읽은 기본 flow는 제품 전체의 지원 규약 구현이나 모든 정책의 보안성을 검증하는 자료가 아니다.
특히 기본 설정에서 보이는 인증 단계의 이름을 근거로 특정 환경의 보안 요건 충족을 단정해서는 안 된다.
실제 사용자는 배포 버전과 자신이 적용한 blueprint·정책의 차이를 확인해야 한다.[1][2]

라이선스도 코드, 문서와 enterprise 영역을 구분해서 안내한다.
README에는 MIT, 문서의 CC BY-SA 4.0, enterprise 라이선스 링크가 함께 있다.
전체 저장소를 하나의 허용 조건으로 뭉뚱그리거나 공개 기능과 별도 제공 기능을 동일하게 취급하지 않는 것이 중요하다.[1]

## 직접 읽어볼 자료

- [공식 README의 정체와 설치 안내](https://github.com/goauthentik/authentik/blob/main/README.md)
  IdP가 서비스 자체가 아니라 서비스 사이의 신원 확인 지점이라는 설명에서 시작한다.
  지원 규약을 확인한 다음 시험 규모에 맞는 배포 경로를 골라 읽되, 배포 성공과 인증 정책 완성을 구분한다.
- [기본 인증 flow blueprint](https://github.com/goauthentik/authentik/blob/main/blueprints/default/flow-default-authentication-flow.yaml)
  stage 이름만 읽지 말고 `flowstagebinding`의 순서와 `expressionpolicy`를 연결한다.
  식별된 사용자의 상태에 따라 비밀번호·추가 인증 검증 단계가 어떻게 달라질 수 있는지 확인할 수 있다.
- [README의 Security와 License](https://github.com/goauthentik/authentik/blob/main/README.md)
  보안 문제 안내 위치와 코드·문서·enterprise 영역의 라이선스 구분을 확인한다.
  필요한 기능의 제공 범위와 변경·배포 조건을 기술적인 로그인 구성과 별도로 점검하는 마지막 단계다.

## 정리

authentik은 여러 서비스의 인증을 연결하는 IdP이며, 기본 로그인도 순서와 조건을 가진 stage들의 조합으로 표현한다.
지원 프로토콜, 단계 실행 정책, 각 애플리케이션의 권한을 따로 구분해야 실제 역할을 과장하지 않고 이해할 수 있다.[1][2]

## 자료 확인 범위

2026-09-27 기준 README와 루트 구조, 기본 인증 blueprint를 확인했다.
서버 배포, 계정 생성과 로그인 시험은 실행하지 않았으며 인증 보안 감사를 수행한 문서는 아니다.

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

[1] goauthentik/authentik — README.md

<https://github.com/goauthentik/authentik/blob/main/README.md>

[2] goauthentik/authentik — blueprints/default/flow-default-authentication-flow.yaml

<https://github.com/goauthentik/authentik/blob/main/blueprints/default/flow-default-authentication-flow.yaml>
