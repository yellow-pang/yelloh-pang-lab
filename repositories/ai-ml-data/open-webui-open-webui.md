---
title: "open-webui/open-webui"
repository: "open-webui/open-webui"
url: "https://github.com/open-webui/open-webui"
category: "ai-ml-data"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: []
---

# open-webui/open-webui

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Open WebUI는 로컬 또는 외부 모델을 연결해 대화와 자료 활용을 제공하는 자체 호스팅 AI 플랫폼이다.
언어 모델의 가중치나 추론 엔진 자체가 아니라, Ollama와 OpenAI 호환 API 등을 사용자가 다루는 화면과 서버를 제공한다.[1]

## 모델을 하나의 사용 환경으로 묶기

모델 서버를 준비해도 대화 이력, 파일, 사용자 권한, 도구 연결은 별도로 필요하다.
Open WebUI는 이 주변 기능을 한 환경에 모으고 모델마다 지침·도구·지식을 붙이는 구성을 소개한다.
여기서 자체 호스팅은 관리 서버를 직접 운영한다는 뜻이며, 연결한 모델까지 반드시 로컬이라는 뜻은 아니다.[1]

README는 문서 검색을 답변에 활용하는 RAG도 안내한다.
RAG는 질문과 관련된 자료를 찾아 모델 입력에 보태는 방식으로, 모델 재학습과는 다르다.
검색·음성·이미지 기능은 연결하는 제공자와 엔진에 따라 별도의 준비가 필요하다.[1]

## Ollama 대화 요청이 지나가는 코드

`backend/open_webui/routers/ollama.py`의 `generate_chat_completion`은 Ollama로 대화 요청을 전달한다.
검증된 사용자를 의존성으로 받고 Ollama 기능이 활성화되어 있는지 확인한 뒤 입력을 구조화한다.
저장된 모델 설정이 있으면 기반 모델 ID와 생성 매개변수, 시스템 지침을 요청에 반영한다.[2]

이후 `check_model_access`로 모델 접근을 검사하고, 모델에 맞는 Ollama URL을 선택해 `/api/chat`으로 전달한다.
응답을 스트리밍으로 받을지도 요청의 설정을 따른다.
스트리밍은 전체 답변이 완성될 때까지 기다리는 대신 생기는 부분부터 전달하는 방식이다.[2]

코드는 긴 모델 호출 동안 데이터베이스 연결을 계속 붙잡지 않도록 짧은 세션을 사용하는 이유도 설명한다.
이 경로는 화면 뒤에 사용자·모델 설정·접근 제어·상위 모델 서버가 따로 존재한다는 것을 보여준다.
단, 이 파일만으로 모든 배포 환경의 권한 설정이 안전하다고 검증한 것은 아니다.[2]

## 가상 예시: 공개 독서 자료와 로컬 모델

공개 독서 자료를 넣고 내용에 대해 질문하는 개인용 환경을 가정한다.
Ollama 모델을 연결하고 자료를 준비한 다음, 관련 자료를 대화에 포함해 답변을 받는 흐름을 만들 수 있다.[1]

결과를 읽을 때는 모델 응답이 나왔는지와 문서의 정확한 내용을 근거로 답했는지를 따로 확인해야 한다.
모델이 바뀌거나 검색 자료가 빠지면 답이 달라질 수 있으므로 화면이 정상 작동하는 것만으로 검색 품질까지 판단하지 않는다.
이 예시는 설치하거나 문서를 업로드해 얻은 결과가 아니다.

## 저장소와 외부 연결을 함께 관리해야 한다

Docker 안내는 `/app/backend/data`의 영속 볼륨을 유지해야 데이터 손실을 막을 수 있다고 강조한다.
오프라인 운영 기능이 있더라도 외부 모델·웹 검색·연결 도구를 선택하면 그 서비스와 통신하는 구성이 된다.
Docker 예제의 포트 공개 범위와 데이터 백업을 확인한 뒤 운영 범위를 정해야 한다.[1]

현재 LICENSE에는 Open WebUI 이름·로고 등 브랜딩을 보존하는 추가 조건이 있다.
예외로는 연속된 30일 동안 직접 접근하는 최종 사용자가 50명 이하인 배포, 사전 서면 허가, 해당 변경을 허용한 기업 라이선스가 명시된다.
그러므로 코드를 공개했다는 이유로 모든 배포에서 자유롭게 이름을 바꿀 수 있다고 설명하면 안 된다.[3]

과거 기여분에는 이전 라이선스가 유지되며 `LICENSE_HISTORY`가 적용 경계를 커밋으로 기록한다.
현재 저장소 전체를 과거 MIT 조건 하나로 소개하지 않는 것이 중요하다.[4]

## 직접 읽어볼 자료

1. README: 모델 연결, RAG, 영속 볼륨의 역할을 나누어 읽는다.[1]
2. `backend/open_webui/routers/ollama.py`: 사용자 확인 → 모델 설정 → 접근 검사 → 요청 전달을 따라간다.[2]
3. `LICENSE`와 `LICENSE_HISTORY`: 현재 브랜딩 조건과 과거 기여분의 적용 범위를 함께 확인한다.[3][4]

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

[1] open-webui/open-webui — README.md

<https://github.com/open-webui/open-webui/blob/main/README.md>

[2] open-webui/open-webui — backend/open_webui/routers/ollama.py

<https://github.com/open-webui/open-webui/blob/main/backend/open_webui/routers/ollama.py>

[3] open-webui/open-webui — LICENSE

<https://github.com/open-webui/open-webui/blob/main/LICENSE>

[4] open-webui/open-webui — LICENSE_HISTORY

<https://github.com/open-webui/open-webui/blob/main/LICENSE_HISTORY>
