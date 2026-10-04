---
title: "apache/maka"
repository: "apache/maka"
url: "https://github.com/apache/maka"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# apache/maka

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Apache Maka (Incubating)는 모델 메시지, 도구 호출, 권한 판단, 종료를 기록으로 남기는 에이전트 작업 공간이다.
Desktop·TUI·CLI가 하나의 Runtime Host를 이용하며, 사용자가 클라우드 API나 로컬 모델 등을 연결한다.
자체 공유 모델 계정을 포함한 서비스가 아니라 실행과 이력을 관리하는 harness다.[1]

## 마지막 화면만으로는 실행을 복구할 수 없다

에이전트가 파일을 바꾸던 중 프로그램이 종료되면 화면의 마지막 메시지만으로 실제 변경 여부를 알기 어렵다.
도구 요청은 보냈지만 실행하지 않았을 수도 있고, 파일은 바꿨으나 결과 기록을 남기지 못했을 수도 있다.
Maka의 중심 설계는 이 작업 사실을 append-only RuntimeEvent로 보존하는 것이다.
append-only는 기존 기록을 고쳐 쓰기보다 새 사건을 뒤에 추가하는 방식을 뜻한다.[1][2]

UI와 다음 모델 요청, 복구 상태는 이 기록에서 계산된 projection으로 설명된다.
projection은 원본 기록을 읽어 만든 화면이나 상태표다.
오래된 도구 출력이 다음 프롬프트에서 빠져도 기록 자체에서는 사라지지 않는다는 README 설명은, 모델에게 전달할 맥락의 크기와 감사 가능한 실행 이력을 분리하려는 설계다.[1]

## 하나의 실행 권한과 여러 입구

Desktop, TUI, CLI, 평가 시스템은 같은 실행 권한을 가진 Runtime Host의 클라이언트로 설명된다.
세션 관리와 AgentRun, 모델·도구 실행이 이 경로 아래 놓이고 평가 계층은 실험과 점수를 맡는다.
화면마다 별도의 실행 상태를 독립적으로 판단하는 대신 공통 런타임을 이용하도록 경계를 세운다.[1]

대표 공식 문서인 runtime-resume-architecture는 repair·reconcile·resume을 나눈다.
repair는 중단된 이전 실행을 명시적 종료 상태로 정리하는 일, reconcile은 불확실한 도구 부작용을 외부 증거로 확인하는 일, resume은 안전한 이력을 토대로 새 실행을 만드는 일이다.
이전 프로세스를 되살리거나 같은 도구를 무조건 다시 부르는 것이 복구가 아니라는 설명이다.[2]

RecoveryResolver는 도구 상태를 해석하며, 이어 호스트가 작업 공간·도구 목록·백그라운드 작업의 안전 조건을 확인한다.
증명할 수 없으면 park, 즉 사실을 보존하고 자동 진행을 멈춘다.
모델의 “아마 성공했을 것”이라는 답변을 실행 증거보다 높은 수준으로 인정하지 않는 것이 이 설계의 중요한 경계다.[2]

## 예시로 따라가는 흐름

공식 복구 문서는 `config.json`의 포트를 `3000`에서 `4000`으로 바꾸다가 중단되는 상황으로 시작한다.
입력은 설정 변경 요청이고, 도구가 쓰기를 시작한 시점에 충돌이 발생한다.
재시작 뒤 결과 이벤트가 없다고 해서 도구가 실행되지 않았다고 결론 낼 수 없다.
아직 시작하지 않았거나, 쓰기 전에 멈췄거나, 이미 썼지만 결과를 저장하지 못했거나, 이후 다른 프로세스가 다시 바꿨을 수 있다.[2]

처리 순서는 남은 영속 기록을 열고 이전 실행의 종료 경계를 정리한 뒤 각 도구의 상태를 해석하는 것이다.
그 다음 작업 공간과 도구·백그라운드 상태가 안전 조건에 맞는지 검사하고, 안전하면 새 Run·Invocation·Turn을 만들어 모델을 다시 호출한다.
불명확한 상태에서는 자동 재실행 대신 중단 상태를 유지한다.
이 사례는 문서 속 사고 실험이며 여기서 파일을 변경하거나 충돌 복구를 시험한 결과가 아니다.[2]

사람이 확인할 부분은 “이전 실행이 더 이상 running이 아니다”와 “파일의 쓰기가 확정되었다”를 구분하는 일이다.
작업 공간의 신원이 같아도 모든 파일 내용이 이전과 같다는 뜻은 아니다.
또한 복구 문서 자체가 구현된 단계와 향후 설계를 구분하므로, 이 전체 이상적 흐름을 현재 모든 도구에서 자동 완수한다고 읽으면 안 된다.[2]

## 복구 설계의 완료 범위를 과장하지 않기

조회한 문서는 Phase 0~2와 Phase 3A의 복구 사실 기록·해석 기반은 구현되었다고 설명하면서, 후속 Phase 3의 production reconciler 연결과 Phase 4의 Git checkpoint·격리 복원은 향후 작업으로 남긴다.
기록 모델이 있다는 사실이 임의의 외부 부작용을 자동으로 되돌린다는 뜻은 아니다.
이 글은 문서가 구분한 상태를 그대로 유지한다.[2]

README는 중단 턴의 재개가 기본적으로 꺼져 있고 `MAKA_RUNTIME_SAFE_BOUNDARY_RESUME=1`로 켜는 기능이며 다시 모델을 호출해 토큰을 사용한다고 안내한다.
복구를 단순 화면 복원으로 생각하면 비용과 새 실행이라는 사실을 놓칠 수 있다.
과거 JSONL 대화와 이전 credential 저장소를 가져오지 않는 조건도 있어 업그레이드 뒤 빈 대화와 인증 재입력이 나타날 수 있다고 설명한다.[1]

로컬 저장이 암호화 저장을 뜻하지 않는 점도 중요하다.
README는 API 키가 운영체제 계정만 읽을 수 있는 로컬 평문 `credential-vault.json`에 있다고 명시한다.
파일 쓰기와 셸 도구는 sandbox 경계를 거쳐야 한다고 설명하지만 이것이 계정 접근·백업 노출 위험을 없애지는 않는다.
공식 Apache release는 아직 없고 개발 빌드는 승인된 공식 릴리스가 아니라는 배포 상태도 함께 명시되어 있다.[1]

## 직접 읽어볼 자료

1. [README의 What Maka is와 Architecture](https://github.com/apache/maka/blob/main/README.md)
   메시지·도구·승인·종료가 한 사건 기록으로 이어지는지 살펴본다.
   UI와 모델 맥락이 원본 기록 자체가 아니라 그 기록의 투영이라는 설명을 먼저 이해하면 복구 문서를 읽기 쉬워진다.
2. [Resume Is Not Retry](https://github.com/apache/maka/blob/main/docs/architecture/runtime-resume-architecture.md)
   포트 변경 도중 충돌하는 사례에서 시작해 Repair, Resume, Reconcile 표를 읽는다.
   이어 단계별 구현 상태를 확인해 현재 가능한 판단과 미래의 자동 복원을 분리한다.
3. [README의 Get Maka와 Local data and recovery](https://github.com/apache/maka/blob/main/README.md)
   공식 릴리스 여부, 평문 자격증명, 재개 기본값, 이전 기록 미이관 조건을 확인한다.
   소스가 공개되어 있다는 사실과 안정 배포·데이터 이전이 완료되었다는 사실은 다르다는 경계를 읽을 수 있다.

## 정리

Maka는 에이전트 작업의 원본을 화면이 아닌 사건 기록에 두고 여러 클라이언트가 이를 공유한다.
복구의 핵심은 재시도보다 사실 판정이며, 불명확한 부작용과 아직 구현되지 않은 복원 단계는 사람의 확인 대상으로 남는다.

## 자료 확인 범위

2026-09-27의 README와 공식 복구 설계 문서의 도입·상태·안전 경계를 읽었다.
애플리케이션 설치, 모델 호출, 충돌 복구나 성능 평가를 직접 실행하지 않았다.

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

[1] apache/maka — README.md

<https://github.com/apache/maka/blob/main/README.md>

[2] apache/maka — docs/architecture/runtime-resume-architecture.md

<https://github.com/apache/maka/blob/main/docs/architecture/runtime-resume-architecture.md>
