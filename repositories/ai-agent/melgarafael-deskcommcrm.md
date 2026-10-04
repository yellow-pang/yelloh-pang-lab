---
title: "melgarafael/DeskcommCRM"
repository: "melgarafael/DeskcommCRM"
url: "https://github.com/melgarafael/DeskcommCRM"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# melgarafael/DeskcommCRM

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

DeskcommCRM은 WhatsApp 대화, 고객과 상담 단계, AI 에이전트의 응대, 사람에게 넘기는 과정을 함께 관리하는 자체 호스팅 CRM이다.
CRM은 연락처와 고객 관계를 기록하는 시스템이며, 이 프로젝트는 그 기록 옆에 대화와 자동화를 붙인다.
모델 가중치를 제공하는 저장소가 아니라 웹 애플리케이션과 데이터베이스, 외부 채널 연결로 구성된 서비스다.[1]

## 대화 기록만으로는 보이지 않는 진행 상태

메신저에 문의가 쌓이면 마지막 답변만으로 현재 담당자, 다음 행동, 상담이 멈춘 이유를 알기 어렵다.
DeskcommCRM은 Inbox의 대화, Kanban의 단계별 고객 상태, AI 실행과 예산 화면을 하나의 운영 흐름으로 묶는다고 설명한다.
여기서 Kanban은 항목을 진행 단계별 칸에 놓아 상태를 보는 방식이다.
AI가 문장을 생성하는 일과 상담 기록을 갱신하는 일이 같은 제품 안에서 만난다.[1]

README는 대화 중 필요한 지식을 조직별로 검색하는 RAG, 상담 배정, 후속 연락, 사람에게 넘기기 등을 소개한다.
RAG는 관련 자료를 찾아 모델 입력에 함께 넣는 방식이다.
모델이 처음부터 모든 자료를 안다고 가정하는 대신, 해당 조직에 허용된 자료를 꺼내 응대에 사용하도록 구성한다는 뜻이다.
이 설명은 기능 방향이지 응답의 사실성이나 개인정보 분리가 자동으로 완벽하다는 보증은 아니다.[1]

## 이벤트를 기록하고 나중에 처리하는 구조

아키텍처 문서는 웹 화면과 API를 담당하는 Next.js, 인증·저장·실시간 갱신을 맡는 Supabase, WhatsApp 연결, AI 처리 계층을 나누어 설명한다.
여러 조직이 같은 시스템을 사용하는 multi-tenant 구조에서는 `organization_id`로 데이터를 구분한다.
RLS는 데이터베이스가 행마다 접근을 제한하는 장치이며, 관리자 권한으로 이를 우회하는 경로에서는 코드가 조직 조건을 직접 적용해야 한다고 명시한다.[2]

들어온 메시지와 상태 변화는 `event_log`에 기록되고 worker가 후속 동작을 수행한다.
worker는 화면 요청과 별도로 대기 중인 일을 처리하는 프로그램이다.
데이터베이스 트리거가 즉시 외부 HTTP 요청을 보내는 대신 기록과 외부 효과를 분리한다.
README가 cron 설정을 강조하는 이유도 여기에 있다.
정기 실행이 연결되지 않으면 자동화 규칙을 만들어도 큐에 있는 작업이 처리되지 않을 수 있다.[1][2]

사람에게 대화를 넘기는 기능은 구체적 코드에서도 확인된다. `ai-handoff-from-sentiment.handler.ts`는 감정 분석 알림 이벤트를 받아 메시지와 대화의 경계를 확인하고 중앙 handoff 처리기를 호출한다.
Handoff는 자동 응대의 책임을 사람에게 넘기는 절차다.
이 처리기는 식별자가 없거나 메시지가 가리키는 상담 경계가 오래되었으면 건너뛰고, `low_sentiment`라는 사유와 분석 점수 등의 정보를 전달한다.[3]

## 예시로 따라가는 흐름

공개 구현을 바탕으로 한 이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
한 상담 대화에 불만을 나타내는 메시지가 들어오고, 별도 감정 분석 worker가 `ai.sentiment_alert` 이벤트를 생성했다고 하자.
읽어 본 handoff 처리기는 이 이벤트에서 메시지 ID, 대화 ID 힌트, 감정 점수를 꺼낸다.
단순히 점수가 있다는 이유만으로 임의 대화를 사람에게 넘기는 것이 아니라, 해당 조직과 메시지에서 현재 상담 경계를 다시 확인한다.
힌트와 실제 대화가 다르면 오래된 경계로 판단해 처리하지 않는 분기가 있다.[3]

경계 확인을 통과하면 연락처와 연결된 최근 고객 항목을 조직 조건으로 조회하고, 중앙 처리기에 대화와 조직, `low_sentiment` 사유를 넘긴다.
관련 고객 항목이 없더라도 handoff 자체를 거기에 종속시키지 않는다는 주석이 있다.
최종 처리기가 실제 전환을 하지 않았으면 `skipped`와 이유를 반환하고, 전환을 수행한 경우 `ok`를 반환한다.
사람은 이벤트가 발생한 사실, 실제 전환 여부, 어느 대화와 조직에 적용됐는지를 구별해서 봐야 한다.
이 파일만으로 감정 점수의 정확성이나 상담원의 응답 품질까지 검증할 수는 없다.[3]

## 자체 호스팅과 자동 응대의 책임

자체 호스팅은 앱을 자신의 서버에서 운영한다는 배치 방식이다.
README에는 서버, 도메인, 데이터베이스 자격 증명, AI 공급자 키와 WhatsApp 연결이 전제 조건으로 나온다.
따라서 소스가 공개되어 있다는 점과 서버·외부 API 비용이 들지 않는다는 주장은 다르다.
저장 데이터와 인증 정보, 백업, 업데이트 역시 운영자가 챙겨야 하는 범위다.[1]

공식 문서 사이의 차이도 있다.
README의 기술 표는 rate limit을 sliding window로 표현하지만 아키텍처 문서는 고정 창 카운터이며 현재 적용 지점도 제한적이라고 설명한다.
AI 공급자 구성에 관한 표기 역시 두 문서가 완전히 같지는 않다.
이 글은 이를 하나의 확정된 배포 설정으로 합치지 않는다.
설계 설명을 근거로 모든 공개 경로가 같은 보안 통제를 받는다고 가정하면 안 된다.[1][2]

아키텍처는 중복 실행 방지를 위한 idempotency가 모든 생성 경로에 적용된 것은 아니고, 효과 발생과 영수증 기록 사이에 프로세스가 종료되면 재실행으로 효과가 반복될 가능성도 적는다.
자동화의 존재보다 실패·재시도·권한 경계를 읽어야 하는 이유다.
개인정보 관련 설계와 감사 기록이 있다는 설명도 개별 설치의 법적 준수 판정을 대신하지 않는다.[2]

## 직접 읽어볼 자료

- [README](https://github.com/melgarafael/DeskcommCRM/blob/main/README.md)
  화면 소개와 Webhooks·자동화 부분을 먼저 읽는다.
  메시지, 고객 단계, 이벤트와 사람이 검토하는 규칙이 제품 안에서 어떻게 연결되는지 큰 흐름을 잡을 수 있다.
- [ARCHITECTURE.md](https://github.com/melgarafael/DeskcommCRM/blob/main/ARCHITECTURE.md)
  조직 분리와 요청 흐름을 따라 읽고 service role이 RLS를 우회하는 지점을 확인한다.
  자동화가 존재한다는 설명과 보안 통제가 적용되는 실제 범위 사이의 차이를 찾는 자료다.
- [감정 알림의 handoff 처리기](https://github.com/melgarafael/DeskcommCRM/blob/main/workers/ai-handoff-from-sentiment.handler.ts)
  `missing_ids`, `service_boundary_stale`, `triggerHandoff` 순서로 본다.
  정상 처리보다 건너뛰는 이유부터 확인하면 잘못된 대화로 인계되는 것을 막기 위한 조건이 드러난다.

## 정리

DeskcommCRM은 대화형 AI를 고객 기록·담당자·이벤트 처리와 연결하는 CRM이다.
문서와 코드에서 조직 경계 및 인계 조건을 볼 수 있지만, 호스팅·외부 연결·백업과 실제 보안 검증까지 대신해 주는 완결된 보증은 아니다.

## 자료 확인 범위

2026-09-27 자료 기준으로 README, 아키텍처와 감정 알림 handoff 구현을 읽었다.
서버 설치, 데이터베이스 구성, WhatsApp 연결과 메시지 발송은 실행하지 않았다.

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

[1] melgarafael/DeskcommCRM — README.md

<https://github.com/melgarafael/DeskcommCRM/blob/main/README.md>

[2] melgarafael/DeskcommCRM — ARCHITECTURE.md

<https://github.com/melgarafael/DeskcommCRM/blob/main/ARCHITECTURE.md>

[3] melgarafael/DeskcommCRM — workers/ai-handoff-from-sentiment.handler.ts

<https://github.com/melgarafael/DeskcommCRM/blob/main/workers/ai-handoff-from-sentiment.handler.ts>
