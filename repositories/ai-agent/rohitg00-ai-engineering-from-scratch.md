---
title: "rohitg00/ai-engineering-from-scratch"
repository: "rohitg00/ai-engineering-from-scratch"
url: "https://github.com/rohitg00/ai-engineering-from-scratch"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# rohitg00/ai-engineering-from-scratch

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

AI의 기초 개념부터 모델과 에이전트 응용까지, 읽을 문서·구현 코드·작은 산출물을 연결한 공개 교육 저장소다.
하나의 에이전트 프레임워크나 모델 서비스가 아니라, 목표별 경로와 수업 단위를 따라가며 작동 원리를 확인하는 커리큘럼이다.[1]

## 결과를 만드는 것과 설명할 수 있는 것

라이브러리 예제를 복사하면 결과는 얻어도 왜 그렇게 동작하는지 설명하지 못할 수 있다.
이 저장소의 README는 수학과 작은 직접 구현을 먼저 다룬 뒤 라이브러리를 사용하도록 구성한다고 설명한다.
학습의 완료를 화면을 읽었다는 사실로 보지 않고, 명령·작업 디렉터리·종료 코드·의미 있는 출력·산출물을 남기도록 요구한다.[1]

모든 사람이 처음부터 같은 길을 걸을 필요는 없다는 구성도 보인다.
초기 환경, 수학 기초, LLM 응용, 에이전트, MCP, Agent Skills 같은 목적에 따라 시작점을 선택한다.
LLM은 대규모 언어 모델이고 MCP는 모델을 쓰는 앱과 외부 도구를 연결하는 규약이다.
이름을 많이 암기하기보다 자신이 만들 대상과 필요한 선행 개념을 연결하는 목차로 읽을 수 있다.[1]

## 수업 폴더가 학습 흐름을 표현한다

README가 설명하는 수업 단위는 `code/`, `docs/en.md`, `outputs/`다.
문서가 문제와 개념을 설명하고, 코드는 실행 가능한 작은 구현을 담고, outputs는 수업에서 만들어 내는 프롬프트·Skill·에이전트·서버 같은 산출물의 자리다.
“Build It”과 “Use It”은 작은 직접 구현과 실용 라이브러리 사용을 나누는 구성이다.[1]

별도의 튜터 Skill은 에이전트에 학습 진행 지침을 제공한다.
배치 퀴즈를 통해 출발점을 정하고 학습 계획 파일을 저장하거나 한 수업씩 개념·수학·코드·질문을 이어 가는 방식으로 소개된다.
튜터 지침을 설치했다고 모든 수업의 실행 환경이 준비되는 것은 아니며, README도 읽기용 Skill과 실행 가능한 실습의 요구 조건을 구분한다.[1]

이 글에서 고른 대표 수업은 Phase 14의 “The Agent Loop”다.
에이전트 루프는 모델이 도구를 선택하고, 실행 결과를 관찰한 뒤 다음 행동 또는 종료를 결정하는 반복 구조다.
수업은 메시지 기록, 도구 등록부, 종료 조건, 반복 예산, 관찰 결과 형식이라는 구성요소를 설명한다.
기능을 많이 덧붙이기 전에 이 반복이 어디서 멈추고 오류를 어떻게 받아들이는지 보게 한다.[2]

## 작은 모형이 드러내는 제어 흐름

실제 `main.py`에는 `ToolRegistry`, `ToyLLM`, `AgentLoop`가 분리되어 있다.
등록부는 이름으로 함수를 찾고 잘못된 도구 이름이나 인자 오류를 문자열로 반환한다. `AgentLoop`는 기록에 요청을 넣고 정해진 최대 차례까지 모델 역할 객체를 호출한다.
종료 응답이면 마지막 결과를 저장하고, 그렇지 않으면 도구를 호출해 관찰값을 기록한다.[3]

중요한 제한은 `ToyLLM`이 실제 언어 모델이 아니라 미리 적힌 순서를 내보내는 결정적 정책이라는 것이다.
여기서 결정적이라는 말은 같은 스크립트에 따라 같은 행동을 선택한다는 뜻이다.
API 키 없이 반복 구조를 읽고 시험하기 좋은 교육 장치지만, 자연어를 이해하거나 잘못된 도구 결과를 스스로 해석하는 지능의 증거는 아니다.[3]

## 예시로 따라가는 흐름

공식 코드의 데모는 기본 가격과 세금을 다루는 질문을 입력으로 받는다.
이 글은 실행하지 않고 소스 흐름을 읽었다.
데모는 먼저 `kv_set`으로 기본 가격을 저장하고, `calculator`로 세금 표현식을 계산하고, 다시 값을 저장한 뒤 합계 표현식과 `kv_get` 조회를 수행하도록 작성되어 있다.
KV는 이름표인 key와 값인 value를 연결하는 작은 저장 구조다.[3]

각 단계에서 루프는 생각 설명, 도구 이름과 인자, 반환된 관찰값을 기록한다.
종료 응답을 만나면 마지막 문장을 반환하고 `pretty_trace`가 기록을 출력하도록 되어 있다.
따라서 학습자가 볼 대상은 합계만이 아니라 저장·계산·조회가 어떤 순서로 연결되는가다.
숫자 결과는 소스에 이미 작성되어 있으므로 여기서 실제 계산 성공의 출력으로 재현하지 않는다.[3]

이 사례에서 특히 확인할 것은 데모 스크립트가 계산 결과를 동적으로 다음 인자로 전달하지 않고 중간 값과 마지막 답도 고정해 두었다는 점이다.
값을 바꾸면 스크립트 전체의 일관성을 다시 점검해야 한다.
오류 관찰을 기록하는 구현은 있지만 `ToyLLM`이 그 오류를 이해해 스스로 계획을 고치는 것은 아니다.
문서의 잘못된 인자 실험이나 종료 경로 추가 과제는 바로 이 작은 모형의 한계를 드러내며 다음 구현 질문으로 연결된다.[2][3]

## 학습용 구조를 운영 보증으로 읽지 않기

대표 수업에는 실제 모델 제공자로 바꾸는 확장 과제가 있지만, 교체만으로 인증·비용·재시도·권한·관측 가능성이 모두 갖춰지는 것은 아니다.
이 문서는 교육 코드의 반복 구조를 설명한 범위이며 프로덕션 시스템의 안전성을 판정하지 않는다.
특히 도구가 외부 파일이나 서비스를 바꾸기 시작하면 단순 계산 예제보다 훨씬 분명한 권한 경계가 필요하다.[2][3]

README는 영어 문서를 기준본으로 두며 번역 수업 페이지의 기계 번역 경로를 설명한다.
용어가 불명확하면 기준 수업과 실제 코드를 함께 확인하는 것이 타당하다.
또한 수업 명령은 원칙적으로 저장소 루트 기준이라는 README 안내와, 개별 수업의 짧은 상대 경로 예시를 구분해서 읽어야 한다.[1][2]

전체 교육 범위가 넓다는 사실은 모든 수업을 여기서 검증했다는 뜻이 아니다.
수업 구조 검사나 smoke check 명령도 README에 소개되지만 이번 조사에서는 실행하지 않았다.
문서가 제공하는 학습 계획과 실제 학습 성과 역시 별개다.[1]

## 직접 읽어볼 자료

- [README의 Start here와 수업 사용법](https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/README.md)
  먼저 목표 경로와 필요한 선행 지식을 선택한다.
  “Keep evidence”가 어떤 기록을 요구하는지 읽으면 학습 완료를 단순한 코드 복사와 구분할 수 있다.
- [The Agent Loop 수업](https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/phases/14-agent-engineering/01-the-agent-loop/docs/en.md)
  구성요소 설명과 Exercises를 연결해서 읽는다.
  종료 조건과 오류 관찰이 빠졌을 때 어떤 오해가 생기는지 질문하는 자료다.
- [수업의 main.py](https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/phases/14-agent-engineering/01-the-agent-loop/code/main.py)
  `ToyLLM.respond`, `AgentLoop.run`, 데모 스크립트를 차례로 본다.
  실제 모델 판단이 아니라 고정 정책이라는 점과 중간 값 전달 방식까지 확인할 수 있다.

## 정리

이 저장소는 작은 구현을 설명하고 증거를 남기며 다음 응용으로 넘어가는 교육 자료다.
대표 에이전트 수업도 복잡한 제품의 축소 모형으로 읽어야 하며, 데모의 정해진 행동과 실제 모델의 판단 능력을 구별해야 한다.

## 자료 확인 범위

2026-09-27 기준 README의 경로·구조 안내, 루트 구성과 Agent Loop 수업 및 전체 예제 코드를 읽었다.
과정 설치나 수업 실행은 하지 않았고 전체 수업의 정확성과 성과를 검증하지 않았다.

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

[1] rohitg00/ai-engineering-from-scratch — README.md

<https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/README.md>

[2] rohitg00/ai-engineering-from-scratch — phases/14-agent-engineering/01-the-agent-loop/docs/en.md

<https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/phases/14-agent-engineering/01-the-agent-loop/docs/en.md>

[3] rohitg00/ai-engineering-from-scratch — phases/14-agent-engineering/01-the-agent-loop/code/main.py

<https://github.com/rohitg00/ai-engineering-from-scratch/blob/main/phases/14-agent-engineering/01-the-agent-loop/code/main.py>
