---
title: "microsoft/generative-ai-for-beginners"
repository: "microsoft/generative-ai-for-beginners"
url: "https://github.com/microsoft/generative-ai-for-beginners"
category: "ai-ml-data"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# microsoft/generative-ai-for-beginners

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Generative AI for Beginners는 생성형 AI 응용을 만드는 개념과 코드를 배우는 공개 강의 저장소다.
모델 가중치를 배포하는 프로젝트나 완성된 챗봇 서비스가 아니다.
확인한 README는 21개 주제별 강의를 안내하며 개념을 설명하는 Learn과 코드로 연결하는 Build를 구분한다.
가능한 경우 Python과 TypeScript 예제를 함께 제공하고 각 강의에 추가 학습 경로를 둔다.[1]

## 답변을 얻는 것에서 앱을 만드는 것으로

챗봇에 질문해 그럴듯한 문장을 받는 일과 그 답변을 프로그램 안에서 안전하게 처리하는 일은 다르다.
강의 목록에는 프롬프트 작성뿐 아니라 검색, 이미지 생성, 함수 호출, 사용자 경험, 보안과 응용의 생명주기가 함께 있다.
따라서 좋은 문장을 입력하는 요령만을 다루는 프롬프트 모음으로 읽으면 강의의 중요한 부분을 놓치게 된다.[1]

각 강의는 독립 주제를 다루므로 필요한 부분부터 읽을 수 있다고 안내한다.
다만 처음 읽는 독자라면 모델의 역할과 응답 한계를 이해한 뒤 텍스트 생성과 도구 연결을 살피는 식으로 선행 개념을 확인할 필요가 있다.
“입문자용”이라는 이름은 프로그래밍 지식이 전혀 필요 없다는 뜻이 아니다.
README도 Python 또는 TypeScript의 기초가 도움이 된다고 설명한다.[1]

## 외부 기능을 호출한다는 말의 정확한 의미

대표 자료로 읽은 11강은 함수 호출을 다룬다.
함수는 프로그램이 이름을 붙여 재사용하는 작업 단위이고, 함수 호출은 작업에 필요한 인자를 전달하는 방식이다.
여기서는 모델에게 어떤 함수가 있으며 어떤 형식의 입력이 필요한지 설명한 뒤 그 구조에 맞는 응답을 요청한다.
모델 자체가 개발자의 컴퓨터에서 함수를 실행하는 것은 아니라고 강의가 명확히 구분한다.[2]

처음 등장하는 문제는 자연어 답변의 형식이 흔들린다는 것이다.
강의는 학생 소개 두 개에서 정보를 뽑을 때 성적이 숫자 문자열 또는 단위가 붙은 문자열로 달라질 수 있는 예를 제시한다.
사람이 읽기에는 비슷해도 데이터베이스나 다음 함수에 전달할 때는 다른 값이다.
JSON은 이름과 값을 정해 놓은 구조로 데이터를 표현하는 형식이며, 문장 의미와 데이터 형식이 별도 검토 대상임을 보여주는 예다.[2]

도구 설명에는 함수 이름, 용도와 `parameters`가 들어간다.
응답의 함수 이름을 실제 Python 함수에 연결하는 것은 애플리케이션 코드다.
그 결과를 다시 모델에게 주면 자연어 설명을 만들 수 있다.
이처럼 구조화된 호출 요청, 외부 실행, 최종 답변을 분리하는 것이 한 번의 대화처럼 보이는 앱 내부의 핵심 구조다.[2]

## 예시로 따라가는 흐름

11강의 공식 사례는 초보 학생에게 맞는 Azure 학습 강좌를 찾아달라는 요청이다.
입력 문장과 함께 `search_courses`라는 함수의 설명을 모델에 보낸다.
함수의 인자는 학습자 역할인 `role`, 대상 기술인 `product`, 경험 수준인 `level`이다.
강의의 응답 예시는 요청에서 student·Azure·beginner를 구분해 함수 호출용 값으로 반환한다.
이는 강의에 제시된 예시이지 이 조사에서 API를 호출해 얻은 결과는 아니다.[2]

다음 단계에서 앱은 응답 항목 가운데 `function_call`을 골라 등록한 함수 표와 연결하고 인자 문자열을 JSON으로 해석한다.
실제 `search_courses` 함수가 Microsoft Learn Catalog API에 요청해 강좌 제목과 URL을 추린다.
앱은 원래 호출 항목과 해당 `call_id`에 대응하는 결과를 대화에 추가해 후속 설명을 만들 수 있게 한다.
사람이 확인할 부분은 모델이 제안한 이름이 허용한 함수인지, 인자가 실제 검색 조건에 맞는지, 반환된 링크가 질문의 수준과 기술에 맞는지다.
검색 결과와 최종 문장을 대조해야 하며 모델이 함수 요청을 만들었다는 사실만으로 조회 완료나 추천의 적합성을 보장할 수 없다.[2]

## 강의 예제와 운영 코드의 거리

11강의 코드는 전체 연결을 이해시키기 위한 예제다. `search_courses`의 함수 설명에서 요구하는 필드와 Python 함수의 실제 인자를 대조하면 형식 계약을 확인해야 할 이유도 보인다.
예제 도구 정의는 `role`만 필수 목록에 넣지만 실제 Python 함수는 세 인자를 받는다.
다른 질문에서 값이 생략된다면 앱이 어떻게 처리할지 추가 설계가 필요하다.
이 관찰은 예제를 실행해 실패를 재현했다는 주장이 아니라 문서에 적힌 정의의 비교다.[2]

응답 형식을 정하는 것만으로 외부 API 장애, 권한 검사와 잘못된 입력 문제가 사라지지는 않는다.
강의의 함수 이름 사전처럼 실행 가능한 작업을 한정하고, 입력의 허용 범위와 오류 처리도 별도로 설계해야 한다.
특히 전송·수정 같은 부작용이 있는 기능은 조회 예제를 그대로 바꾸기보다 승인과 검증 단계를 추가할 필요가 있다.

README는 Azure OpenAI, OpenAI API, Microsoft Foundry Models와 로컬 모델 경로를 안내한다.
모든 예제가 어느 제공자에서든 같은 코드로 동작한다는 뜻은 아니며 인증·모델 이름·서비스 가용성과 비용을 구분해야 한다.
확인한 README에는 GitHub Models 종료 안내와 대체 경로도 있으므로 오래된 강의 화면의 서비스명을 그대로 따라가지 말고 현재 준비 안내와 비교해야 한다.
로컬 경로가 있다고 모든 강의가 오프라인 실행을 보장하는 것도 아니다.[1]

## 직접 읽어볼 자료

- [README의 Learn·Build 구분과 강의 목록](https://github.com/microsoft/generative-ai-for-beginners/blob/main/README.md)
  학습하려는 것이 개념인지 코드 연결인지 먼저 정한다.
  보안·사용자 경험·생명주기 항목도 함께 훑어 텍스트 생성 예제가 앱 전체를 대신하지 않는다는 점을 확인한다.
- [11강 함수 호출 문서](https://github.com/microsoft/generative-ai-for-beginners/blob/main/11-integrating-with-function-calling/README.md)
  함수 설명과 실제 함수 구현을 나란히 읽는다.
  모델이 만든 인자가 외부 요청으로 바뀌는 줄, 결과가 호출 ID와 함께 다시 전달되는 줄을 찾아 역할 경계를 표시한다.
- [README의 What You Need](https://github.com/microsoft/generative-ai-for-beginners/blob/main/README.md)
  서비스별 예제 표기와 사전 지식, 로컬 모델 안내를 확인한다.
  무료로 문서를 읽는 것과 API 호출·실습 환경에 필요한 계정 및 자원을 구분한다.

## 정리

이 저장소는 생성형 AI를 응용 코드로 연결하는 입문 강의다.
대표적인 함수 호출 예제는 모델의 제안과 프로그램의 실제 실행을 분리하며 예제를 운영 코드로 옮길 때 필요한 검증 경계도 보여준다.

## 자료 확인 범위

2026-09-27 루트 README와 11강의 함수 호출 개념·공식 코드 예제를 읽었다.
계정 설정, 패키지 설치, 외부 API 호출과 강의 실습은 실행하지 않았다.

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

[1] microsoft/generative-ai-for-beginners — README.md

<https://github.com/microsoft/generative-ai-for-beginners/blob/main/README.md>

[2] microsoft/generative-ai-for-beginners — 11-integrating-with-function-calling/README.md

<https://github.com/microsoft/generative-ai-for-beginners/blob/main/11-integrating-with-function-calling/README.md>
