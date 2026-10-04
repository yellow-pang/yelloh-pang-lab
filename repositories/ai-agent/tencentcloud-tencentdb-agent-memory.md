---
title: "TencentCloud/TencentDB-Agent-Memory"
repository: "TencentCloud/TencentDB-Agent-Memory"
url: "https://github.com/TencentCloud/TencentDB-Agent-Memory"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# TencentCloud/TencentDB-Agent-Memory

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

TencentDB Agent Memory는 대화·문서·코드에서 얻은 정보를 다른 에이전트 세션에서도 다시 사용할 수 있게 저장하고 배분하는 메모리 시스템이다.
모델의 가중치를 학습시키거나 에이전트 작업 루프 자체를 실행하는 도구는 아니다.
README는 무엇을 남기고, 누가 사용할 수 있으며, 다음 질문에 필요한 만큼 어떻게 찾을지를 핵심 문제로 설명한다.[1]

## 대화 기록에서 재사용 자산으로

새 세션을 열 때마다 같은 배경을 반복하거나 같은 공개 문서를 처음부터 다시 읽는 일이 생긴다.
이 프로젝트는 원문 기록만 쌓는 대신 성격별로 가공한다.
Chat Memory는 사실·선호·결정의 기억, Skill은 다시 수행할 절차, Wiki는 연결된 지식 문서, CodeGraph는 코드의 파일·기호·호출 관계를 나타내는 자료다.[1]

같은 내용을 저장해도 형태에 따라 다음 작업에서 쓰는 방식이 달라진다.
“어떤 결정을 했는가”는 대화 기억을 찾고, “어떤 순서로 검토하는가”는 Skill을 읽으며, “이 함수를 바꾸면 어디에 영향이 있는가”는 CodeGraph를 조회하는 식이다.
단, 이런 자산이 존재한다는 것이 그 내용과 추론 결과가 항상 정확하다는 뜻은 아니다.[1]

Memory Hub는 이 자산의 소유자·버전·상태·공유 범위와 에이전트 연결을 관리하는 화면이다.
새 Chat Memory와 Skill은 기본적으로 비공개이며 공유는 명시적 행동이라고 README가 설명한다.
ACL은 어떤 사용자·역할·에이전트가 접근할 수 있는지 정한 목록이다.
자산을 연결하는 일과 팀 전체에 공개하는 일을 구분하는 설계다.[1]

## 저장소와 요청 경로를 나누어 보기

루트에는 MemoryCore, MemoryKnowledge, MemoryPanel과 MemoryProxy가 구분되어 있다.
추가로 확인한 MemoryProxy 문서는 에이전트와 모델 사이에서 요청을 전달하며 세션 초기화, 메모리 삽입과 대화 기록 반환을 담당한다고 설명한다.
기억 데이터의 읽기·쓰기는 MemoryCore Gateway를 통하고 Proxy 자체가 메모리 자산의 영구 저장소는 아니라고 명시한다.[2]

Proxy가 “투명하다”는 말은 클라이언트가 사용하는 OpenAI·Anthropic 요청 규약을 유지한다는 뜻이다.
요청 내용이 전혀 바뀌지 않는다는 의미는 아니다.
실제로 관련 Skill·Knowledge·기억을 시스템 프롬프트에 넣고 대화가 끝나면 기록을 다시 보낸다.
프로토콜 호환과 입력에 기억을 추가하는 동작을 별도로 이해해야 한다.[2]

대화는 L0 원문에서 사실 단위·상황 단위·장기 맥락으로 가공된다.
다만 루트 README는 L2를 Scenario, L3를 Core/Persona로 설명하고 Proxy 문서는 L2를 Agent Profile, L3를 Team/Global memory라고 부르므로 층 이름을 완전히 같은 의미로 단정하지 않는다.
공통적으로 확인되는 방향은 상위 요약을 초기 맥락에 사용하고 자세한 기록은 필요할 때 조회한다는 것이다.[1][2]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
개인 공개 학습 프로젝트에서 이전 대화로 정리한 코드 읽기 순서와 공개 문서 Wiki를 새 에이전트에게 전달하려 한다고 하자.
우선 가져온 문서가 처리되어 사용 가능 상태가 되었는지 확인하고, 해당 에이전트에 어떤 자산을 연결할지 결정한다.
모든 개인 대화를 팀 전체에 공개하는 대신 필요한 자산만 공유하는 설정이 입력 조건이다.[1]

그다음 모델 요청이 Proxy로 들어오면 문서상의 처리 흐름은 사용자 인증, 팀·에이전트·작업 선택, 관련 맥락 삽입, 한도 확인과 모델 전달로 이어진다.
요약 기억은 프롬프트에 들어가지만 자세한 L0/L1 기록과 지식 자료는 도구로 요청할 수 있다.
응답 뒤에는 대화 조각이 Skill 보관 및 L0 기억용으로 MemoryCore에 전달되어 백그라운드 가공의 재료가 된다.[2]

사람이 확인할 결과는 새 답변의 품질뿐 아니라 어떤 자산이 연결되었는지, 선택한 사용자·에이전트의 권한으로 필요한 자료만 조회했는지, 대화 반환이 의도한 범위에서 켜져 있는지다.
잘못 추출된 결정이 기억에 남으면 다음 답변도 그 오류를 재사용할 수 있으므로 원문과 버전을 대조해야 한다.
위 흐름은 문서가 제시한 연결 방식을 설명할 뿐 실제 기억 정확도와 접근 격리를 시험한 결과가 아니다.[1][2]

## “로컬 실행”보다 넓은 데이터 경로

Proxy에는 모델로 향하는 전달 경로와 MemoryCore로 향하는 저장 경로가 모두 있다.
선택 구성으로 관찰·사용량 보고 채널도 설명된다.
따라서 자체 서버에서 실행한다는 사실만으로 대화가 외부로 전송되지 않는다고 판단하면 안 된다.
실제 모델 주소와 기록 반환·추적 설정을 함께 살펴야 한다.[2]

인증 설정에도 중요한 차이가 있다.
Proxy 문서는 코드 기본값의 `auth.enabled`가 false이고 예제 설정은 true라고 설명한다.
인증을 끄면 익명 사용자로 요청이 통과할 수 있으므로 비루프백 주소에서 듣거나 다중 노드로 배포할 때는 켜 두라고 안내한다.
메모리 자산의 권한 모델이 있다는 사실과 실제 요청 인증이 활성화되었다는 사실은 별도 확인 사항이다.[2]

Proxy의 세션·삽입 상태 저장에는 여러 backend가 있고 초기화 실패 시 다른 저장 방식으로 자동 저하될 수 있다고 문서화되어 있다. `/health`의 `storage.effective`가 실제 사용 중인 저장 방식을 확인하는 기준이다.
이 상태 캐시와 MemoryCore의 장기 메모리 데이터를 혼동해서는 안 된다.[2]

현재 README는 Team Memory Beta를 명시한다.
Wiki와 CodeGraph는 비동기로 만들어져 `ready` 상태까지 시간이 필요하고, CodeGraph는 공개 HTTPS 저장소를 우선하며 비공개·SSH 지원과 완전 자동 라우팅은 계속 개선 중이라고 설명한다.
발표된 메모리 벤치마크도 특정 조건의 저자 결과이며 이 글에서는 재현하거나 일반 성능으로 확장하지 않는다.[1]

## 직접 읽어볼 자료

- [README의 Memory Assets와 Technical Implementation](https://github.com/TencentCloud/TencentDB-Agent-Memory/blob/feat/server_team/README.md)
  Chat Memory·Skill·Wiki·CodeGraph의 역할을 구분한다.
  같은 정보를 저장하는 네 방식이 아니라 서로 다른 조회 목적을 가진 자산이라는 점을 확인한다.
- [MemoryProxy의 Request pipeline](https://github.com/TencentCloud/TencentDB-Agent-Memory/blob/feat/server_team/MemoryProxy/README.md)
  모델 호출 전후에 무엇이 추가되는지 단계별로 읽는다.
  프롬프트 삽입, 기록 반환, Core 저장의 경계를 따라가면 데이터 흐름을 이해할 수 있다.
- [MemoryProxy의 FAQ와 Choosing a storage backend](https://github.com/TencentCloud/TencentDB-Agent-Memory/blob/feat/server_team/MemoryProxy/README.md)
  인증 기본값과 예제 설정의 차이, 요청 키와 상위 모델 키의 구분을 확인한다.
  상태 저장의 실제 backend를 health에서 확인해야 하는 이유도 함께 읽는다.

## 정리

이 시스템은 저장된 경험을 자산으로 정리하고 권한과 에이전트 연결에 따라 재사용한다.
기억의 내용·공유 범위·인증 상태와 외부 전송 경로를 따로 확인해야 재사용이 곧 무분별한 정보 확산이 되는 것을 피할 수 있다.

## 자료 확인 범위

2026-09-27 기준 `feat/server_team` 브랜치의 README, 루트 구조와 MemoryProxy 설명의 처리·권한·저장 구간을 확인했다.
배포, 에이전트 연동, 자료 가져오기와 API 호출은 실행하지 않았다.

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

[1] TencentCloud/TencentDB-Agent-Memory — README.md

<https://github.com/TencentCloud/TencentDB-Agent-Memory/blob/feat/server_team/README.md>

[2] TencentCloud/TencentDB-Agent-Memory — MemoryProxy/README.md

<https://github.com/TencentCloud/TencentDB-Agent-Memory/blob/feat/server_team/MemoryProxy/README.md>
