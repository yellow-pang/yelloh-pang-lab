---
title: "JetBrains/go-modern-guidelines"
repository: "JetBrains/go-modern-guidelines"
url: "https://github.com/JetBrains/go-modern-guidelines"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# JetBrains/go-modern-guidelines

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 프로젝트 버전에 맞는 Go 관용구를 알려 주는 참고 도구

go-modern-guidelines는 AI 코딩 Agent가 오래된 작성 패턴 대신 프로젝트의 Go 버전에서 사용할 수 있는 언어 기능과 표준 라이브러리를 선택하도록 돕는 지침 모음이다.
새 Go 컴파일러나 코드를 모두 자동 변환하는 독립 프로그램이 아니다.
README는 `go.mod`에서 버전을 확인하고 그 버전까지의 기능을 사용하도록 하는 흐름을 설명한다.[1]

반복문으로 값을 찾는 코드나 여러 조건문으로 기본값을 고르는 코드는 동작하더라도 더 직접적인 표준 함수로 의도를 표현할 수 있다.
문제는 Agent가 기억하는 예제가 오래되었거나, 새로운 문법을 알아도 익숙한 옛 패턴을 더 자주 선택한다는 점이다.
README는 이 두 가지를 학습 자료의 시차와 빈도 편향으로 설명하고, 명시적인 참고 자료를 주는 방식으로 대응한다.[1]

## 최신 기능보다 버전 경계가 먼저다

여기서 ‘modern’은 가장 새로운 기능을 무조건 쓰라는 뜻이 아니다.
프로젝트가 허용하는 Go 버전이 선택의 상한이다.
예를 들어 공식 기능 설명에서 `slices.Contains`는 Go 1.21 이상, `cmp.Or`는 Go 1.22 이상으로 표시된다.
Agent는 요청받은 코드의 의미와 버전 조건을 함께 만족하는 표현을 골라야 한다.[1][2]

대표 문서인 `FEATURES.md`는 범주, 최소 Go 버전, 현대화 분석기 지원 여부와 이전·이후 예시를 연결한다.
modernize 분석기는 기존 코드의 오래된 패턴을 찾아 새 표현으로 바꾸는 도구를 뜻한다.
이 지침과 방향은 비슷하지만, 문서에 실린 모든 권고가 그 분석기로 자동 변환된다는 의미는 아니다.
실제 표는 분석기 지원이 없는 항목도 분리해 표시한다.[2]

`FEATURES.md`는 내부 지침 데이터에서 생성된 파일이라고 밝힌다.
따라서 단순한 블로그식 팁 목록보다는 버전 조건과 설명, 예시를 함께 유지하는 참고 카탈로그로 읽는 편이 맞다.
Impact 등급은 프로젝트가 제시하는 분류이며, 이 글에서 임의의 코드베이스를 측정한 결과로 인용하지 않는다.[2]

## 예시로 따라가는 흐름

공식 `slices_contains` 예시는 `items`를 반복하며 `x`와 같은 값이 있는지 찾고, 발견하면 `found`를 참으로 바꾼 뒤 멈추는 코드다.
이 반복문의 유일한 결과가 포함 여부라면 `found := slices.Contains(items, x)`로 의도를 직접 표현할 수 있다고 설명한다.
여기서 입력은 값들의 모음과 찾을 값, 처리의 목적은 같은 값의 존재 여부, 결과는 참·거짓이다.
원소의 위치를 원한다면 이 예시가 아니라 `slices.Index` 항목을 봐야 한다.[2]

사람은 짧아진 줄 수보다 의미가 보존되는지 확인해야 한다.
기존 반복문이 검색 도중 별도 기록을 남기거나 다른 조건을 함께 검사했다면 단순 포함 검사와 같지 않을 수 있다.
또한 해당 프로젝트의 Go 버전이 이 표준 함수를 지원하는지 확인해야 한다.
이 문서에서는 공식 변경 전후 예시를 읽었을 뿐 실제 Go 파일을 수정하거나 컴파일하지 않았다.
‘새 함수가 있다’는 사실을 알고 바꾸는 단계와, 프로젝트에서 테스트해 기존 동작이 유지되는지 검증하는 단계는 분리된다.

## 짧은 코드에도 의미상의 함정이 있다

`cmp.Or` 설명은 주의점을 잘 보여 준다.
이 함수는 인자 중 첫 번째 0값이 아닌 값을 고르지만 모든 인자는 호출 전에 평가된다.
뒤쪽 후보를 만드는 작업이 비싸거나 부작용이 있으면 조건문과 같지 않다. 0이나 false가 ‘없는 값’이 아니라 유효한 결과인 상황에도 맞지 않는다.
README의 짧은 대체 예시만 읽기보다 기능별 본문을 함께 읽어야 하는 이유다.[2]

비슷하게 포함 여부, 인덱스 찾기, 조건에 맞는 원소 찾기는 서로 다른 요구다.
공식 문서는 직접 동등 비교에는 `slices.Contains`나 `slices.Index`, 필드나 계산된 조건에는 `slices.IndexFunc`를 구분한다.
표현이 현대적이라는 이유만으로 자료형이나 반환 의미의 차이를 없앨 수 없다.
이 프로젝트의 참고 가치는 바로 이런 선택 조건과 예시를 같은 자리에서 찾는 데 있다.[2]

## 지침을 읽는 도구와 적용 대상의 요구사항

README에 따르면 마켓플레이스 통합은 처음 사용할 때 작은 CLI를 `go install`로 준비하며 Go 도구가 PATH에 있어야 한다.
CLI는 로컬 캐시에 설치되고 프로젝트를 수정하지 않는다고 설명한다.
도구 자체의 목표 버전은 Go 1.25 이상이며, 더 오래된 환경에서는 자동 도구체인 전환이 활성화되어 있어야 호환 도구체인을 받을 수 있다.[1]

이 요구사항을 분석 대상 프로젝트의 최소 Go 버전과 혼동하지 않아야 한다.
지침을 출력하는 도구가 쓰는 Go와 수정할 코드가 따라야 하는 Go는 역할이 다르다.
자동 전환에는 네트워크 다운로드가 수반될 수 있으며, 실제 Agent가 생성한 코드는 기존 CI와 테스트로 확인해야 한다.
README는 Agent별 설치와 갱신 경로를 별도로 설명하므로 한 Agent의 명령을 다른 Agent에 그대로 적용하는 것도 피해야 한다.[1]

## 직접 읽어볼 자료

- [README의 목적과 버전 감지 설명](https://github.com/JetBrains/go-modern-guidelines/blob/main/README.md)

  ‘최신 Go’보다 ‘프로젝트의 Go 버전까지’라는 조건을 먼저 읽는다.
  이어 Requirements에서 참고 CLI의 도구체인과 수정 대상의 버전이 다른 문제임을 확인한다.

- [FEATURES의 포함 검사 예시](https://github.com/JetBrains/go-modern-guidelines/blob/main/FEATURES.md)

  `slices_contains`, `slices_index`, `slices_index_func`를 비교한다.
  반환하는 것이 포함 여부인지 위치인지, 직접 비교인지 조건 함수인지에 따라 선택이 달라진다.

- [FEATURES의 기본값 선택 주의점](https://github.com/JetBrains/go-modern-guidelines/blob/main/FEATURES.md)

  `cmp_or`의 전후 코드와 인자 평가 경고를 함께 읽는다.
  간결해진 표현이 원래 조건문의 실행 순서와 유효한 0값을 보존하는지 묻는 것이 중요한 확인 지점이다.

## 정리

go-modern-guidelines는 Go의 기능을 Agent가 프로젝트 버전에 맞춰 선택하도록 돕는다.
변화의 핵심은 줄 수 절감이 아니라 의도를 잘 표현하는 표준 기능이며, 적용 가능성과 의미 보존은 코드별로 검토해야 한다.

## 자료 확인 범위

2026-09-27 기준 기존 원고, README, 루트 구조와 공식 FEATURES의 대표 예시를 읽었다.
CLI 설치·실행, Go 코드 변환, 컴파일과 테스트는 하지 않았다.

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

[1] JetBrains/go-modern-guidelines — README.md

<https://github.com/JetBrains/go-modern-guidelines/blob/main/README.md>

[2] JetBrains/go-modern-guidelines — FEATURES.md

<https://github.com/JetBrains/go-modern-guidelines/blob/main/FEATURES.md>
