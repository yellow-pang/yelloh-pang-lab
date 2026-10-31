---
title: "TencentCloud/Octop"
repository: "TencentCloud/Octop"
url: "https://github.com/TencentCloud/Octop"
category: "ai-agent"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# TencentCloud/Octop

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Octop은 여러 사용자와 전문 역할의 에이전트를 한 설치에서 운영하는 자체 호스팅 AI 비서 플랫폼이다.
모델 자체나 지침 파일 모음이 아니라 웹 대시보드·메신저·예약 작업·에이전트 실행을 연결하는 애플리케이션이다.[1]

## 여러 입구에서 같은 에이전트를 사용하기

개별 채팅 화면만 있으면 메신저 요청, 예약 작업, 문서와 작업 공간을 각각 관리해야 한다.
README는 웹·IM·cron 요청을 프로세스 안의 `HarnessProcessor`로 모으고 제어용 데이터베이스에서 상태를 복원하는 구조를 설명한다.
제어용 DB는 기본 SQLite, 선택 PostgreSQL이며 에이전트 파일 저장소와는 별개다.[1]

기술 구성은 Python·FastAPI 서버와 React·TypeScript 대시보드를 중심으로 한다.
실행·메시지 연결·기억·브라우저 기능에는 Octop Harness, Gateway, Memory, Browser라는 별도 구성 요소가 연결된다.
자체 호스팅한다고 외부 모델이나 OAuth·MCP 연결까지 자동으로 오프라인이 되는 것은 아니다.[1]

## 전문가 팀의 대화와 작업 공간

`docs/expert-teams.md`는 팀을 독립적인 전문가들을 조정하는 진행자 형태로 설명한다.
진행자도 `kind=team`인 에이전트이며, 구성원 목록은 진행자의 작업 공간 `.octop/manifest.json`에 기록한다.
구성원은 자기 작업 공간을 유지하고 여러 팀에 참여할 수 있다.[2]

사용자 메시지는 진행자에게 먼저 도착하고, 진행자는 전문 작업을 구성원에게 비동기로 나눈다.
같은 구성원의 작업은 순차 처리하지만 서로 다른 구성원은 병렬로 처리하며, 끝나면 플랫폼이 진행자를 다시 깨워 정리를 맡긴다.
하나의 대화 화면에 보여도 구성원들의 체크포인트를 같은 기록 하나에 섞지는 않는다.[2]

이 분리는 협업 화면과 실제 상태 저장을 구별하게 한다.
공유받은 전문가에게 일을 맡기면 산출물이 상대 전문가의 작업 공간과 대화에 생기며, 상대 설정을 편집할 권한까지 얻는 것은 아니다.[2]

## 인증 코드로 확인하는 서버 경계

`src/octop/api/middleware/jwt_auth.py`는 명시적 예외를 제외한 `/api/` 요청에 JWT 인증을 적용한다.
JWT는 서버가 확인할 수 있는 서명된 접근 토큰이며, 검증된 사용자는 요청 상태에 보관해 뒤 단계에서 재사용한다.
인증 오류는 JSON 응답으로 반환하고, 조건에 맞으면 새 토큰을 응답 헤더에 싣는다.[3]

이 코드가 존재한다는 사실과 모든 경로·배포 설정이 안전하다는 검증은 다르다.
예외 경로와 세부 권한은 다른 함수와 라우터에도 걸쳐 있으므로 외부 공개 전에 별도로 살펴야 한다.[3]

## 가상 예시: 공개 자료를 읽는 두 전문가

개인 학습용으로 자료 요약 전문가와 내용 검토 전문가를 만들어 팀에 넣는 상황을 가정한다.
사용자가 공개 자료 비교를 요청하면 진행자가 각각의 과제를 정리하고, 구성원들이 자기 작업 공간에서 처리한 내용을 팀 화면으로 돌려준다.
진행자는 도착한 결과를 보고 추가 작업이나 마무리를 결정한다.[2]

서로 다른 에이전트가 답했다고 자동으로 상호 검증이 완료되는 것은 아니다.
자료 출처와 실제 산출물, 검토가 수정에 반영되었는지는 사람이 따로 확인해야 하며 여기서는 실행하지 않았다.

## 중단·재시작·외부 연결의 주의점

AgentTeams는 README에서 Beta로 표시된다.
팀 문서는 진행자를 중지해도 이미 대기 중이거나 실행 중인 구성원 작업은 취소되지 않으며, 프로세스 재시작 시 진행 중 inbox 작업의 지속성은 비목표라고 설명한다.
따라서 UI의 중지와 전체 작업 중단, DB 상태 복원과 진행 중 작업 복구를 같은 것으로 보면 안 된다.[1][2]

셸·브라우저·원격 데스크톱은 실제 파일과 화면에 영향을 줄 수 있고, 외부 커넥터는 계정 권한을 요구할 수 있다.
먼저 에이전트별 도구 권한과 서비스 노출 범위를 제한해 읽어야 한다.
저장소 코드는 MIT 라이선스를 제공하며 외부 모델·서비스 조건은 따로 확인해야 한다.[1][4]

## 직접 읽어볼 자료

1. README: 단일 프로세스, 제어용 DB, 별도 실행 구성 요소의 역할을 구분한다.[1]
2. `docs/expert-teams.md`: 진행자·구성원·작업 공간과 중지·재시작의 비목표를 읽는다.[2]
3. `src/octop/api/middleware/jwt_auth.py`: 인증 예외 분기, 오류 응답, 토큰 갱신을 따라간다.[3]

## 자료 확인 범위

2026-10-28 공식 README, 루트 트리와 아래 대표 자료를 확인했다. 설치·빌드·실행은 하지 않았다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] TencentCloud/Octop — README.md

<https://github.com/TencentCloud/Octop/blob/main/README.md>

[2] TencentCloud/Octop — docs/expert-teams.md

<https://github.com/TencentCloud/Octop/blob/main/docs/expert-teams.md>

[3] TencentCloud/Octop — src/octop/api/middleware/jwt_auth.py

<https://github.com/TencentCloud/Octop/blob/main/src/octop/api/middleware/jwt_auth.py>

[4] TencentCloud/Octop — LICENSE

<https://github.com/TencentCloud/Octop/blob/main/LICENSE>
