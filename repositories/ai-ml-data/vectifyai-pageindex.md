---
title: "VectifyAI/PageIndex"
repository: "VectifyAI/PageIndex"
url: "https://github.com/VectifyAI/PageIndex"
category: "ai-ml-data"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# VectifyAI/PageIndex

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

PageIndex는 긴 문서를 목차처럼 계층화한 색인으로 만들고, 언어 모델이 필요한 절과 페이지를 찾아 읽도록 돕는 문서 검색·질의응답 도구다.
RAG는 답변에 필요한 자료를 먼저 찾아 모델에 제공하는 방식이며, 이 프로젝트는 벡터 데이터베이스의 유사도 검색 대신 문서 구조를 따라가는 경로를 제시한다.[1]

## 문서 전체를 매번 넣는 대신 어디를 읽을지 정한다

README는 문서마다 트리 색인을 만드는 단계와, 모델이 그 트리를 탐색하는 단계를 구분한다.
트리는 큰 장 아래 작은 절이 매달리는 목차 형태의 자료 구조다.
색인에서 관련 부분을 고른 뒤 실제 페이지를 읽는 것이 핵심이지, 목차의 제목만 보고 정답을 확정하는 시스템은 아니다.[1][2]

따라서 PageIndex를 새로운 언어 모델이나 완성된 문서 편집기로 이해하면 범위가 어긋난다.
Python SDK는 프로그램에서 호출하는 도구이며, README가 소개하는 PageIndex App과 Cloud는 별도 이용 경로다.
특히 파일 전체를 관리하는 Cloud 전용 기능을 로컬 SDK의 기능과 섞어 읽지 않아야 한다.[1]

## Flash 색인과 검색 도구의 역할

현재 로컬 기본 색인 방식인 Flash는 PDF의 배치 통계를 사용해 기본 구조를 추출한다.
그 구조에 요약을 붙이고 검색에 맞게 다듬는 단계에는 LLM, 즉 텍스트를 이해하고 생성하는 언어 모델을 사용한다.
Flash 문서의 `summary=False, optimize=False` 예제는 모델 없이 원시 트리만 만드는 경로다.
“트리 추출에 모델이 없다”는 설명은 질의응답까지 모델 없이 된다는 뜻이 아니다.[1][3]

색인 결과에는 문서 이름, 절 제목, `node_id`, 시작·종료 페이지, 자식 절 등이 들어간다.
`toc_source`는 구조가 문서 배치에서 검출됐는지, PDF의 내장 책갈피에서 왔는지 등을 표시한다.
읽을 텍스트가 전혀 없으면 `unreadable`과 빈 구조를 반환한다.
계층을 찾지 못해 페이지별 목록으로 대체한 경우에도 제한이 있으며, Flash 문서는 이 평평한 목록이 10페이지를 넘으면 로컬 클라이언트와 CLI가 거부한다고 명시한다.[3]

대표 구현인 `agent_tools.py`는 문서 목록, 처리 상태, 구조, 페이지 본문을 읽는 도구를 분리한다.
긴 문서에서는 먼저 구조를 보고 좁은 페이지 범위를 선택하도록 안내한다.
페이지 요청이 응답 크기 한도를 넘으면 일부를 생략하고 남은 범위를 알려주므로, “호출 성공”과 “요청한 내용을 모두 읽음”도 다르다.[2]

로컬 모드의 채팅 구현에는 OpenAI SDK와 LiteLLM을 통해 선택한 모델 제공자로 연결하는 경로가 있다.
따라서 로컬이라는 말은 색인과 저장 위치를 설명할 뿐, 선택한 모델 API로 문서 내용이 나가지 않는다는 보장은 아니다.[4]

## 예시로 따라가는 흐름

공식 빠른 시작은 `report.pdf`를 등록하고 “2023년 영업이익률은 얼마인가?”라고 묻는 예제다.
이를 문서 탐색 과정으로 풀면, 먼저 `submit_document`가 돌려준 문서 ID로 질문 대상을 고정한다.
Flash가 만든 목차의 절 제목과 페이지 범위를 바탕으로 재무 성과를 다루는 부분을 찾고, 관련 페이지의 본문을 읽어 답변 근거를 확보하는 흐름이다.[1][3][2]

여기서 사람이 확인할 것은 그럴듯한 숫자가 나왔는지만이 아니다.
보고서의 대상 연도와 지표 이름이 질문과 맞는지, 인용한 페이지에 실제 근거가 있는지, 다른 연도의 비교 수치를 답으로 선택하지 않았는지 확인해야 한다.
도구의 인용 지침도 실제 읽은 내용만 답하고 자료가 없으면 없다고 말하도록 요구한다.
다만 이런 지침은 모델이 항상 정확히 따른다는 증명이 아니다.
이 원고는 예제의 입력과 처리 경로를 설명한 것이며, PDF를 등록하거나 영업이익률 답변을 생성한 실행 기록은 아니다.[2]

## 로컬, Cloud, 비용을 분리해서 읽기

README는 로컬을 텍스트 중심 PDF에 적합한 경로로, Cloud를 문서 파싱·OCR·이미지 이해·저장을 관리하는 경로로 구분한다.
OCR은 스캔 이미지에서 글자를 읽어내는 처리다.
로컬의 페이지 단위 인용과 Cloud의 블록 단위 인용, 폴더·메타데이터 지원에도 차이가 있다.
스캔 문서나 그림이 많은 파일에서 같은 기능을 기대해서는 안 된다.[1]

Cloud로 전환하면 색인과 저장이 서비스로 이동하고 PageIndex API 키가 필요하다.
자기 모델을 사용하는 채팅에는 해당 모델의 인증도 별도로 필요하다.[1]
로컬에서도 요약·검색에 외부 모델을 쓰면 호출 비용과 데이터 전송 조건이 생긴다.[3][4]
MIT 라이선스는 코드의 사용·복제·수정 조건이지 모델 API나 Cloud 요금이 무료라는 뜻이 아니다.[5]

README의 색인 비용, 소요 시간, FinanceBench 정확도 수치는 프로젝트 측이 제시한 특정 조건의 주장이다.
문서 구성, 모델, 질문, 캐시 조건이 달라져도 같은 비용과 성능이 나온다고 일반화하지 않는다.
이 원고에서는 벤치마크 저장소의 실행 조건을 재검증하거나 다른 검색 방식과 비교 실험하지 않았다.[1]

또한 에이전트 도구에는 문서와 관련 데이터를 영구 삭제하는 `remove_document` 계약도 있다.
읽기 도구와 관리 도구가 분리되어 있으므로 통합 시 노출할 도구를 구분해야 한다.
명시적 문서 지정과 삭제 확인을 요구하는 설명이 있더라도, 연결한 에이전트의 권한과 호출 경로는 별도로 검토할 대상이다.[2]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/README.md)

   빠른 시작에서 문서 등록과 질문을 분리해 읽고, Local/Cloud 비교표를 확인한다.
   비용 그래프는 측정 조건과 함께 읽으며 SDK, App, 관리형 서비스의 경계를 구분한다.
2. [Flash 설명](https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/flash/README.md)

   요약·최적화를 끈 예제와 기본 예제를 비교한다.
   결과 필드와 `toc_source`의 실패·대체 상태를 먼저 읽으면 입력 PDF가 적합한지 판단할 질문을 얻을 수 있다.
3. [에이전트 도구 구현](https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/agent_tools.py)

   처리 상태 확인에서 구조, 페이지 본문으로 이어지는 순서를 따라간다.
   인용 지침과 응답 크기 제한, 삭제 도구의 별도 계약도 함께 확인한다.
4. [로컬 채팅 구현](https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/local_chat.py)

   `_openai_model`에서 모델 제공자 연결을 살핀다.
   저장이 로컬인 것과 모델 추론이 로컬인 것이 같은 조건인지 점검하는 출발점이다.

## 자료 확인 범위

2026-10-04 고정 커밋 `6d23caf416858f2ca136840305d1f479a86f6ef7`의 README, Flash 설명, 에이전트 도구와 로컬 채팅의 관련 구현, MIT 라이선스를 읽었다.
설치·실행·성능 검증, 외부 모델 호출, Cloud 업로드 및 요금 확인은 하지 않았다.
문서상의 모델명이나 홍보 수치를 독립적으로 검증한 결과로 취급하지 않는다.

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

[1] VectifyAI/PageIndex — README.md

<https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/README.md>

[2] VectifyAI/PageIndex — pageindex/agent_tools.py

<https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/agent_tools.py>

[3] VectifyAI/PageIndex — pageindex/flash/README.md

<https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/flash/README.md>

[4] VectifyAI/PageIndex — pageindex/local_chat.py

<https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/pageindex/local_chat.py>

[5] VectifyAI/PageIndex — LICENSE

<https://github.com/VectifyAI/PageIndex/blob/6d23caf416858f2ca136840305d1f479a86f6ef7/LICENSE>
