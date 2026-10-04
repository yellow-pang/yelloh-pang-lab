---
title: "genlayerlabs/genlayer-project-boilerplate"
repository: "genlayerlabs/genlayer-project-boilerplate"
url: "https://github.com/genlayerlabs/genlayer-project-boilerplate"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# genlayerlabs/genlayer-project-boilerplate

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 외부 웹과 LLM을 쓰는 계약의 시작 예제

이 저장소는 GenLayer 사용 사례를 시작할 수 있도록 계약·테스트·프런트엔드·배포 구성을 모은 boilerplate다.
boilerplate는 처음부터 모든 파일을 구성하지 않아도 되도록 제공하는 출발 틀이다.
대표 사례는 축구 경기의 승자를 예측하고 결과에 따라 점수를 얻는 Football Bets이며, README는 웹 접근과 LLM 연동을 포함한 intelligent contract 예제로 설명한다.
GenLayer 네트워크 자체나 완성된 상용 베팅 서비스와는 범위가 다르다.[1]

일반적인 프로그램도 외부 경기 결과를 가져올 수 있지만, 계약 상태를 바꾸는 판단에 웹 문서와 언어 모델을 넣으면 다른 문제가 생긴다.
자료가 아직 없거나 모델이 다른 형식으로 답할 수 있고, 같은 요청에 대한 결과를 어떻게 검증할지도 정해야 한다.
이 예제는 웹 결과 수집, 모델을 통한 구조화, 계약 상태 변경, 테스트 대체 응답을 분리해 그 경계를 보여 준다.[1][2]

## 계약 상태와 비결정적 작업

`football_bets.py`의 `Bet`에는 날짜, 팀, 예상 승자, 결과 URL, 해결 여부와 실제 점수가 저장된다. `FootballBets`는 사용자 주소별 예측 목록과 점수를 별도 `TreeMap`으로 가진다.
주소는 요청 주체를 구별하는 값이며, 같은 경기라도 다른 사용자의 예측은 다른 위치에 놓인다.
생성 시 날짜와 두 팀 이름을 조합해 소문자 ID를 만들고, 같은 사용자의 중복 예측을 거부한다.[2]

경기 확인 함수는 저장한 URL을 텍스트로 렌더링한 뒤, 두 팀 이름과 웹 내용을 포함한 프롬프트를 구성한다.
프롬프트는 점수와 승자를 JSON으로 응답하도록 요청한다.
JSON은 이름 붙은 값들을 기계가 읽기 쉽게 표현하는 자료 형식이다.
이때 외부 웹과 LLM 호출은 `gl.nondet` 경로에 들어간다.
비결정적이라는 표현은 외부 상태나 생성 응답 때문에 단순 계산처럼 항상 같은 값을 기대하기 어렵다는 뜻이다.[2]

응답은 정렬된 JSON 문자열로 바뀌고 `gl.eq_principle.strict_eq` 호출을 거쳐 다시 자료로 읽힌다.
README는 이를 equivalence principle을 통한 검증이라고 설명한다.
다만 호출이 존재한다는 사실만으로 그 합의 알고리즘의 안전성이나 외부 경기 정보의 진실성까지 확인한 것은 아니다.
이 파일에서는 어디에 외부 판단을 넣고 그 결과를 언제 상태 변경에 사용하는지가 관찰 범위다.[1][2]

`resolve_bet`은 이미 해결한 예측을 다시 처리하지 못하게 하고, 결과가 미완료를 나타내면 실패시킨다.
완료된 경우 해결 여부와 실제 승자·점수를 저장하고 예상 승자와 같을 때 사용자 점수를 늘린다.
확인한 코드는 점수 기록 예제이며 금전 지급이나 자금 보관 절차를 구현한 것으로 소개하지 않는다.[2]

## 예시로 따라가는 흐름

공식 `test_resolve_winning_bet`은 계약을 메모리 안에 배포하고 요청자를 Alice로 설정한다.
입력은 날짜 `2024-06-20`, 두 팀 `Spain`과 `Italy`, 예상 승자 문자열 `"1"`이다.
테스트는 실제 웹 경기를 다시 조회하지 않고 웹 응답과 LLM 응답을 모의 값으로 등록한다.
모의 응답은 시험 조건을 통제하기 위해 실제 외부 서비스를 대신하는 값이며, 여기서는 점수 `"1:0"`과 승자 `1`을 사용한다.[3]

그다음 생성된 경기 ID로 `resolve_bet`을 호출한다.
테스트는 해당 예측이 해결 상태이고, 저장된 승자와 점수가 모의 응답과 일치하며, 사용자의 점수가 1이기를 기대한다.
이 숫자는 저장소의 테스트 기대값이지 이번 조사에서 실행해 얻은 결과나 실제 경기 사실 확인이 아니다.
읽는 사람은 웹과 모델을 대체한 부분, 계약 상태를 바꾼 부분, 마지막에 검증한 필드를 각각 구분할 수 있다.[3]

같은 파일에는 잘못 예측했을 때 점수를 받지 않는 경우, 무승부, 이미 해결한 예측, 아직 끝나지 않은 경기, 두 사용자의 독립 처리도 있다.
정상 사례 하나만 보는 대신 실패와 중복 처리를 같이 읽으면 계약이 어떤 조건에서 상태를 바꾸지 않아야 하는지 알 수 있다.
사람이 확인할 부분은 mock이 실제 응답의 형식과 실패 양상을 충분히 대표하는지, 모델이 반환한 값이 예상 범위를 벗어났을 때 처리하는지다.
direct 테스트가 통과하더라도 실제 웹·모델·합의까지 함께 검증한 것은 아니다.[1][3]

## 테스트 가능한 예제와 운영 가능한 서비스의 거리

README는 정적 분석, direct 테스트, Studio 대상 integration 테스트를 나눈다.
정적 분석은 실행 없이 금지된 호출·저장 타입·데코레이터 같은 규칙을 살피고, direct 테스트는 웹과 LLM을 대체해 메모리에서 빠르게 확인한다.
integration 테스트는 Studio에 계약을 배포해 합의 동작을 포함하는 범위로 설명된다.
세 검사는 서로 다른 질문에 답하므로 하나의 성공으로 나머지를 생략할 근거가 되지 않는다.[1]

소스에는 특히 중요한 예제용 완화가 있다.
생성 시 경기가 이미 끝났는지 확인하는 코드가 과거 경기를 테스트할 수 있도록 주석 처리되어 있다.
따라서 README의 축구 예측 게임이라는 설명만 보고 경기 종료 후 예측 등록이 완전히 막힌다고 쓰면 틀린다.
이 저장소가 출발 틀임을 보여 주는 실제 경계이며, 점수의 공정성이나 남용 방지를 보장하는 완성품으로 읽지 않아야 한다.[2]

프런트엔드는 배포된 계약 주소를 환경 설정으로 받으며, README는 Python 3.12 이상과 GenLayer CLI, 배포·통합 테스트용 Studio를 요구한다.
화면을 실행하는 것과 계약이 올바른 네트워크에 배포되어 연결되는 것은 별개의 준비다. 'production-ready'라는 프런트엔드 소개 문구가 계약의 모든 운영·보안·법적 조건까지 충족한다는 의미는 아니다.
외부 모델과 네트워크의 비용·가용성 역시 이번 코드 열람으로 측정하지 않았다.[1]

## 직접 읽어볼 자료

- [README의 프로젝트 구조와 Testing Strategy](https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/README.md)
  contracts, direct 테스트, integration 테스트, frontend의 역할을 먼저 구별한다.
  각 검사에서 실제 외부 서비스를 부르는지 대체하는지 표시하면 서로 다른 검증 범위를 이해할 수 있다.
- [football_bets.py](https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/contracts/football_bets.py)
  생성, 결과 조회, 상태 변경 순서로 읽고 `gl.nondet`와 `strict_eq`의 경계를 찾는다.
  특히 과거 경기 시험을 위해 주석 처리된 조건이 최종 서비스의 공정성에 어떤 추가 검토를 요구하는지 확인한다.
- [결과 판정 direct 테스트](https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/tests/direct/test_resolve_bet.py)
  mock 등록과 assert를 나누고 정상·실패·다중 사용자 사례를 비교한다.
  이 테스트가 입증하려는 계약 내부 조건과 실제 웹·LLM에 남아 있는 불확실성을 따로 기록할 수 있다.

## 정리

이 boilerplate는 외부 웹·LLM 판단을 계약 상태와 연결하고, 그 경계를 모의 응답으로 시험하는 예제다.
실행 가능한 시작 틀과 금전·공정성·실환경 합의가 검증된 서비스는 같은 것이 아니다.[1][2][3]

## 자료 확인 범위

2026-09-27 기준 기존 초안, README와 루트 구성, 축구 계약과 결과 판정 direct 테스트를 읽었다.
의존성을 설치하거나 테스트·배포·프런트엔드를 실행하지 않았고 실제 경기 조회와 모델 호출도 하지 않았다.

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

[1] genlayerlabs/genlayer-project-boilerplate — README.md

<https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/README.md>

[2] genlayerlabs/genlayer-project-boilerplate — contracts/football_bets.py

<https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/contracts/football_bets.py>

[3] genlayerlabs/genlayer-project-boilerplate — tests/direct/test_resolve_bet.py

<https://github.com/genlayerlabs/genlayer-project-boilerplate/blob/main/tests/direct/test_resolve_bet.py>
