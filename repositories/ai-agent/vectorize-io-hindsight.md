---
title: "vectorize-io/hindsight"
repository: "vectorize-io/hindsight"
url: "https://github.com/vectorize-io/hindsight"
category: "ai-agent"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# vectorize-io/hindsight

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Hindsight는 대화와 문서에서 정보를 뽑아 저장하고, 나중에 관련 기억을 찾거나 그 기억을 바탕으로 답변하도록 돕는 에이전트 기억 시스템이다.
별도의 모델을 학습시키는 코드라기보다 에이전트가 호출하는 저장·검색·추론 계층으로 이해할 수 있다.[1]

## 대화 기록과 기억의 차이

긴 대화 전체를 다시 읽히는 대신, 무엇이 있었고 누구와 관련되며 언제 일어났는지를 구조화해 다음 요청에 연결하는 것이 기본 흐름이다.
README는 세계에 대한 사실, 에이전트의 경험, 여러 기억에서 종합한 관찰을 구분한다.
이때 ‘학습’이라는 표현을 모델 가중치가 자동으로 갱신된다는 뜻으로 읽으면 안 된다.
문서가 설명하는 핵심은 기억을 추출·조직·검색하고 새 정보를 기존 관찰에 반영하는 과정이다.[1]

사용자는 Python·TypeScript 등의 클라이언트나 REST API로 서버를 호출한다.
저장 공간은 `bank`라는 단위로 나뉜다.
bank는 한 사용자·프로젝트 같은 문맥에 속한 기억, 원문 문서, 추출한 개체와 관계를 묶는 공간이다.
개체는 사람·장소·개념처럼 여러 문장에서 같은 대상을 가리키는 요소다.
bank 문서는 쓰기 때 공간이 자동 생성되지만 존재하지 않는 bank를 읽으면 `404`를 반환한다고 명시한다.[1][2]

## 세 연산은 서로 다른 결과를 낸다

`retain`은 입력을 저장 가능한 기억으로 바꾸는 연산이다.
일반적인 LLM 기반 추출에서는 사실·시간·개체·관계를 찾고 검색용 구조를 만든다.
LLM은 문장을 읽고 생성하는 언어 모델이다.
추출 방식에 따라 동작이 다르며, 원문 조각을 그대로 저장하고 LLM을 호출하지 않는 `chunks` 모드도 별도로 있다.[1][2]

`recall`은 관련 기억을 찾는다.
문서가 설명하는 검색 축은 의미 유사성, 키워드, 개체·시간 등의 관계, 시간 범위다.
여러 결과를 합치고 관련성 순서를 다시 조정한 뒤 응답의 토큰 한도에 맞춘다.
토큰은 모델이 처리하는 텍스트 조각 단위다.
`reflect`는 이렇게 찾은 기억을 바탕으로 질문에 답하거나 더 깊이 해석하는 연산이므로, 검색된 사실 목록과 생성된 설명은 같은 결과물이 아니다.[1]

관찰을 합치는 작업도 기억 저장과 구분한다.
Retain 문서는 새 사실과 기존 관찰을 비교하는 정리가 백그라운드에서 비동기로 수행된다고 설명한다.
따라서 retain이 끝났다는 응답만으로 모든 후속 종합이 완료됐다고 판단하면 안 된다.[3]

## 예시로 따라가는 흐름

공식 Python Quickstart는 로컬 서버를 가리키는 클라이언트를 만들고 `my-bank`에 Alice의 직업을 설명한 한 문장을 넣는다.
같은 bank에 `What does Alice do?`로 recall을 요청하고, `Tell me about Alice`로 reflect를 요청한다.
이 예제에서 입력은 한 사람에 관한 문장, 중간 처리는 기억 추출과 저장, 뒤의 두 요청은 검색과 설명 생성이라는 차이를 보여준다.[4]

예제를 읽을 때는 응답을 상상해 확정하지 않는 편이 맞다.
파일은 recall과 reflect를 호출하지만 반환 내용을 출력하거나 기대 문장과 비교하는 검사를 넣지 않는다.
따라서 ‘Alice를 정확하게 기억했다’는 실제 결과까지 이 소스만으로 입증되지는 않는다.
사용자가 확인할 것은 저장한 bank와 조회한 bank가 같은지, 직업 정보가 관련 기억에 들어갔는지, reflect가 원문에 없는 사실을 덧붙이지 않았는지다.
이 원고는 서버를 실행하거나 예제 응답을 만들어 제시하지 않는다.[4]

전체 파일에는 웹 문서의 발췌에서 빠진 cleanup도 있다.
끝에서 HTTP DELETE로 `my-bank`를 지우고 성공 문구를 출력한다.
더구나 클라이언트는 `http://localhost:8888`을 직접 사용하지만 삭제 주소는 `HINDSIGHT_API_URL` 환경 변수에 따라 달라질 수 있다.
따라서 파일 전체를 실행할 때는 예제 전용 bank인지, 저장과 삭제가 같은 서버를 가리키는지 확인해야 한다.
맨 끝의 성공 문구 자체도 반환 내용의 정확성이나 삭제 성공을 검증한 결과는 아니다.[4]

## 기억에서 제외해도 원문이 사라지는 것은 아니다

`retain_mission`은 무엇에 주목할지 적는 추출 지침이다.
예를 들어 어떤 종류의 정보만 기억하도록 범위를 좁히면 한 문서에서 기억이 하나도 생성되지 않을 수 있다.
공식 Retain 설명은 이때 retain이 성공으로 끝나고 원문 문서는 여전히 저장된다고 명시한다.
recall과 reflect는 기억을 검색하므로 기억이 없는 원문은 그 두 연산으로 찾을 수 없다.[3]

이 구분은 보관 정책에서 중요하다.
검색되지 않는다는 사실을 삭제됐다는 뜻으로 받아들이면 안 된다.
추출 지침을 넓혀 원문을 재처리할 수 있다는 안내도 원문 저장이 별개임을 보여준다.
사용 전에는 bank뿐 아니라 원문, 백업, 모델 공급자 측 기록까지 어떤 기간에 누가 보관·삭제하는지 확인해야 한다.
이번 자료 확인만으로 배포 형태별 자동 만료 기간이나 공급자의 데이터 보관 약속은 확정하지 않았다.[3]

## 로컬 저장과 외부 전송의 경계

README의 Docker 예제는 데이터 볼륨을 사용한다.
설치 문서는 Full 이미지에서 임베딩과 재정렬을 내부 실행하지만 LLM은 이미지 외부에 있다고 구분한다.
임베딩은 문장을 검색용 숫자 표현으로 바꾸는 과정이다.
이미지 밖의 LLM은 로컬 별도 서버일 수도, 원격 서비스일 수도 있으므로 ‘Docker를 내 컴퓨터에서 실행한다’는 사실만으로 모든 입력이 로컬에 머문다고 말할 수 없다.
Slim 형태는 임베딩·재정렬도 외부 공급자 구성을 전제로 한다.[1][5]

자동 연동에도 전송 범위의 차이가 있다.
README의 LLM Wrapper는 모델 호출 전에 기억을 검색하고 호출 뒤 대화를 저장하며, 별도 주소를 주지 않으면 Hindsight Cloud를 기본으로 한다고 설명한다.
반면 SDK 직접 호출은 저장·조회 시점을 애플리케이션이 정한다.
대화 전체를 자동 저장할지 선택적으로 입력할지부터 정하고 실제 서버 주소와 모델 endpoint를 함께 확인해야 한다.[1]

bank가 분리된다는 설명은 사용자 인증을 대신하지 않는다.
공식 Extensions는 API 키를 검사하는 TenantExtension과 데이터베이스 schema 선택을 별도 책임으로 설명한다.
내장 단일 키 예시는 인증된 요청 모두에 `public` schema를 쓴다.
설치 문서의 Control Plane UI 접근 키 기본값도 없음으로 표시되어 있으므로, 테스트 실행 명령을 외부 공개용 인증 구성으로 간주해서는 안 된다.[6][5]

## 필터와 비용에 관한 주의점

Memory Defense는 정규식 패턴으로 비밀키나 일부 개인정보 형식을 찾아 지우거나 저장을 막는 선택 기능이다.
기본적으로 꺼져 있고 bank별 정책이 필요하다.
정책을 나중에 켜도 이미 저장한 기억은 다시 검사하지 않는다.
정해진 형식 검사라는 범위 때문에 모든 민감 정보가 자동으로 제거되는 보호 장치로 해석해서는 안 된다.[7]

MIT 공개 코드, 자체 서버 운영비, 모델 호출 비용, 관리형 Cloud 요금은 별개다.
README는 Cloud를 사용량 기반 청구로 설명하며, 시작 크레딧이 모든 사용의 무료 보장을 뜻하지 않는다.
성능과 정확도에 대한 README의 홍보·벤치마크 설명은 이 원고에서 독립 재현하지 않았다.
기억을 저장한다는 기능과 잘못된 사실을 만들지 않는다는 보장은 구분해야 한다.[1]

## 직접 읽어볼 자료

1. [README의 핵심 개념과 Wrapper](https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/README.md)

   retain·recall·reflect가 반환하는 것의 차이를 먼저 읽는다.
   이어 자동 저장 연동의 기본 서버와 직접 SDK 호출의 제어 범위를 비교한다.

2. [실제 Python Quickstart](https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/examples/api/quickstart.py)

   짧은 API 호출뿐 아니라 파일 마지막 삭제 코드까지 읽는다.
   저장·삭제 서버 주소와 bank 이름, 실제로 검증하는 항목이 무엇인지 확인한다.

3. [Retain 설명](https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/retain.md)

   추출 지침 때문에 기억이 생기지 않아도 원문은 저장되는 조건을 확인한다.
   성공 응답, 검색 가능성, 원문 보관을 서로 다른 상태로 읽는 자료다.

4. [Memory Defense](https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/memory-defense/index.md)

   기본 비활성 상태와 미래 retain에만 적용되는 범위를 확인한다.
   민감 정보가 이미 저장된 뒤 켜는 설정으로 과거 자료까지 정리할 수는 없다.

## 자료 확인 범위

2026-10-04 기준 커밋 `f7dd3f4fd7420f7beec60c32c965e5e5cf7be066`의 README, Python Quickstart 전체, Memory Banks, Retain, 설치·확장 문서의 관련 부분과 Memory Defense를 확인했다.
설치, 서버 실행, 외부 모델 호출, 데이터 저장·삭제 및 성능 검증은 하지 않았다.
모델별 응답 정확도, 모든 인증 경로, Cloud의 별도 보관 계약은 검증하지 않았다.

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

[1] vectorize-io/hindsight — README.md

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/README.md>

[2] vectorize-io/hindsight — hindsight-docs/docs/developer/api/memory-banks.mdx

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/api/memory-banks.mdx>

[3] vectorize-io/hindsight — hindsight-docs/docs/developer/retain.md

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/retain.md>

[4] vectorize-io/hindsight — hindsight-docs/examples/api/quickstart.py

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/examples/api/quickstart.py>

[5] vectorize-io/hindsight — hindsight-docs/docs/developer/installation.md

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/installation.md>

[6] vectorize-io/hindsight — hindsight-docs/docs/developer/extensions.md

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/extensions.md>

[7] vectorize-io/hindsight — hindsight-docs/docs/developer/memory-defense/index.md

<https://github.com/vectorize-io/hindsight/blob/f7dd3f4fd7420f7beec60c32c965e5e5cf7be066/hindsight-docs/docs/developer/memory-defense/index.md>
