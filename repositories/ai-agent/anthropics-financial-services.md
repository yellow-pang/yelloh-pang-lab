---
title: "anthropics/financial-services"
repository: "anthropics/financial-services"
url: "https://github.com/anthropics/financial-services"
category: "ai-agent"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# anthropics/financial-services

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Claude for Financial Services는 금융 분석 초안을 만드는 참조 에이전트, Skill, 데이터 connector를 묶은 저장소다.
모델·메모·조사 노트·대사 자료를 사람이 검토하도록 만들며, 투자 추천이나 거래 실행, 최종 승인을 맡는 시스템으로 소개되지 않는다.
공개 자료를 이용한 재무 모델의 작성·검토 절차를 읽을 때도 이 경계를 먼저 두어야 한다.[1]

## 계산 결과보다 입력의 근거가 먼저다

기업 가치 모델에는 과거 수치와 미래 가정, 계산 수식이 함께 들어간다.
보기 좋은 Excel 파일을 만들더라도 어떤 값이 공시에서 왔고 어떤 값이 가정인지 구분하지 못하면 결과를 검토하기 어렵다.
이 저장소의 Model Builder는 처음부터 모델을 만들 때 입력 자료를 수집하고 수식을 연결하며 검사 후 사용자에게 멈춰 보여주는 절차를 정의한다.[2]

기존 분석 모델을 업데이트하는 일은 다른 역할인 earnings-reviewer로 안내한다.
모든 재무 작업을 하나의 에이전트에게 맡기기보다 작업의 시작 상태와 목표에 따라 역할을 나누는 것이다.
사용자가 “모델을 만들어 달라”고 말할 때에도 새 모델 작성인지 기존 모델 갱신인지 구분해야 적절한 절차를 선택할 수 있다.[2]

## 에이전트, 분야별 Skill, 데이터 연결

에이전트 플러그인은 시스템 지침과 사용할 Skill을 포함한 자급형 묶음으로 설명된다.
분야별 vertical plugin에는 계산·검토 방법, 명시적으로 부르는 명령, 데이터 connector가 있다.
Cowork 플러그인과 Managed Agents 배포는 같은 지침과 Skill을 참조하는 두 실행 경로이며, API 기반 위임은 Research Preview로 안내된다.[1]

Model Builder가 만드는 유형에는 DCF, LBO, 재무제표 통합 모델, 비교 배수 표가 있다.
DCF는 미래 현금흐름을 현재 가치로 환산하는 모델이다.
여기서 중요한 지침은 계산 셀에 결과 숫자를 직접 쓰지 말고 수식을 연결하는 것, 입력마다 출처를 붙이거나 `[ASSUMPTION]`으로 가정을 표시하는 것이다.
모델이 생성한 숫자도 출처 없는 사실로 보이면 안 된다.[2]

검사 Skill인 `audit-xls`는 선택 영역·현재 시트·전체 모델을 나눈다.
모든 범위에서 수식 오류, 잘못된 참조, 합계 범위, 단위 혼합, 수식을 덮어쓴 고정값을 점검하고 전체 모델 범위에서는 대차대조표와 현금흐름 연결까지 본다.
검사 깊이를 “파일을 열었다”는 사실이 아니라 사용자가 정한 범위로 결정한다.[3]

## 예시로 따라가는 흐름

공식 Model Builder의 입력은 종목 식별자, 모델 종류, 가정 집합이다.
DCF를 택하면 과거 자료·예상치·공시를 데이터 연결에서 가져오고, 해당 Skill로 예측 구간과 잔존 가치, 할인율 계산 등을 구성하도록 되어 있다.
이 글은 공식 지침의 흐름을 설명하며 특정 종목의 계산을 실행하거나 가격을 산출한 결과가 아니다.[2]

그 다음 `audit-xls`로 수식 연결과 모델 균형을 검사한다.
예를 들어 전체 모델을 검토할 때 현금흐름표의 기말 현금과 대차대조표의 현금이 맞는지를 보고, 균형이 맞지 않으면 기간별 차이를 수치화하고 어디에서 끊기는지 추적하도록 지시한다.
실제 차이나 오류 셀을 이 조사에서 발견한 것은 아니며, 이것이 공식 검사 계약의 내용이라는 뜻이다.[3]

출력은 계산이 연결된 Excel 모델과 위치·심각도·범주·수정 제안을 가진 검사 결과다.
사람은 입력 출처, 가정 표시, 계산 셀의 수식, 기간·단위 일관성을 먼저 확인해야 한다.
Model Builder는 작성 후와 검사 후에 멈추며 민감도 분석 전에 사용자 승인을 요구한다.
민감도 분석은 가정을 바꿨을 때 결과가 얼마나 달라지는지 보는 작업으로, 기본 모델 확인을 건너뛰는 장식용 표가 아니다.[2][3]

검사 Skill 역시 먼저 보고하고 요청받았을 때 수정하라고 명시한다.
따라서 오류 후보가 발견되었다는 사실이 자동 수정이나 최종 승인 권한을 주지는 않는다.
이 절차의 끝은 투자 결론이 아니라 검토 가능한 모델 초안과 명시된 문제 목록이다.[1][3]

## 문서화된 흐름과 실제 연결 상태

connector는 MCP를 통해 외부 데이터 도구를 연결한다.
README는 제공자에 따라 구독이나 API 키가 필요하다고 명시하므로 저장소를 받았다고 데이터 사용권이 생기지 않는다.
모델 호출과 데이터 접근의 비용, 입력의 최신성, 라이선스 조건을 따로 확인해야 한다.
재무제표 단위나 회계 기준의 차이도 자동 생성 수식만으로 해결되지 않는다.[1][3]

기존 초안의 중요한 제약도 다시 확인했다.
수집한 `financial-analysis/.mcp.json`을 표준 JSON 파서로 읽으면 `box` 항목 앞 구분자 위치에서 실패했다.
파일에는 외부 서비스 URL들이 보이지만 파싱 가능한 연결 설정이라는 뜻은 아니다.
이 조사에서는 원문을 고치거나 실제 접속하지 않았으며, 그대로 설치해 연결된다고 설명하지 않는다.[4]

또한 audit-xls는 VBA 매크로에 의해 계산되는 부분을 수식만으로 검사할 수 없을 때 이를 기록하라고 안내한다.
수식 검사 통과와 재무 해석의 타당성, 외부 데이터 정확성, 규제 적합성은 별도 검토 대상이다.
README의 비자문·사람 승인 경고는 서두의 면책 문구만이 아니라 작업의 실제 종료 위치로 읽어야 한다.[1][3]

## 직접 읽어볼 자료

1. [README의 How It Fits Together](https://github.com/anthropics/financial-services/blob/main/README.md)
   에이전트가 완결된 흐름을 맡고 Skill이 분야 지침, connector가 자료 접근을 맡는 구분을 확인한다.
   이어 비자문 경고와 Managed Agents의 preview 조건을 읽으면 자동화 범위를 정리할 수 있다.
2. [Model Builder 역할 파일](https://github.com/anthropics/financial-services/blob/main/plugins/agent-plugins/model-builder/agents/model-builder.md)
   입력·작성·검사·민감도·검토의 순서를 읽고 Guardrails에서 멈추는 지점을 확인한다.
   “모든 출력은 수식”과 “모든 입력은 출처”라는 규칙이 셀 검토로 어떻게 연결되는지 질문할 수 있다.
3. [audit-xls Skill](https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md)
   범위 선택부터 읽은 뒤 전체 모델에서만 추가되는 검사와 결과 표를 비교한다.
   보고와 수정 승인을 분리하는 마지막 지시까지 확인해야 이 Skill이 맡는 책임이 드러난다.
4. [수집 시점의 MCP 설정](https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/.mcp.json)
   연결 주소의 존재와 JSON 구문 유효성을 구분해 살펴본다.
   이 파일의 파싱 실패는 현재 원고의 실제 제한사항이며 외부 서비스 자체의 장애를 뜻하지는 않는다.

## 정리

financial-services는 금융 분석의 초안 작성과 검사 절차를 재사용하는 참조 묶음이다.
입력 출처·수식 연결·사람 승인이라는 기준이 중심이며 데이터 사용권과 수집본 설정 오류를 별도로 해결해야 실제 실행을 논할 수 있다.

## 자료 확인 범위

2026-09-27 README, Model Builder, audit-xls, MCP 설정 원문을 확인하고 설정의 JSON 파싱 실패를 재확인했다.
에이전트 설치·배포, 데이터 조회, Excel 계산은 실행하지 않았다.

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

## 관련 Repository

- [anthropics/knowledge-work-plugins](anthropics-knowledge-work-plugins.md)

## Sources

[1] anthropics/financial-services — README.md

<https://github.com/anthropics/financial-services/blob/main/README.md>

[2] anthropics/financial-services — plugins/agent-plugins/model-builder/agents/model-builder.md

<https://github.com/anthropics/financial-services/blob/main/plugins/agent-plugins/model-builder/agents/model-builder.md>

[3] anthropics/financial-services — plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md

<https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/skills/audit-xls/SKILL.md>

[4] anthropics/financial-services — plugins/vertical-plugins/financial-analysis/.mcp.json

<https://github.com/anthropics/financial-services/blob/main/plugins/vertical-plugins/financial-analysis/.mcp.json>
