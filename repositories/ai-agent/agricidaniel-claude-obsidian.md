---
title: "AgriciDaniel/claude-obsidian"
repository: "AgriciDaniel/claude-obsidian"
url: "https://github.com/AgriciDaniel/claude-obsidian"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# AgriciDaniel/claude-obsidian

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

claude-obsidian은 원본 자료를 보존하면서 출처가 연결된 Obsidian 노트를 만들고, 그 노트를 다시 검색·질의하는 로컬 우선 지식 관리 시스템이다.
Claude Code 플러그인과 호환 Agent Skills 호스트에서 사용하는 절차, Python 기반 관리 도구가 함께 들어 있다.
Obsidian 자체나 자동 대화 녹음기를 대체하는 프로젝트는 아니다.[1]

## 요약을 저장한 뒤에도 남는 문제

웹 문서 하나를 요약해 저장하는 일과 지식 기반을 유지하는 일은 다르다.
다음에 비슷한 질문을 했을 때 원문을 찾을 수 있어야 하고, 서로 반대되는 주장을 섞지 않아야 하며, 기존 노트와 새 노트의 관계도 보여야 한다.
README는 원본 보존, 주장에 대한 근거 연결, 지식 간 연결, 재사용이라는 반복 흐름으로 이 문제를 다룬다.[1]

보관함인 vault는 일반 Markdown·JSON·원본 파일이 있는 디렉터리로 남는다.
제품 소스나 플러그인 캐시가 사용자 보관함으로 자동 선택되지 않도록 분리하는 것도 중요한 특징이다.
도구를 교체한 뒤에도 파일을 읽을 수 있다는 점과, 어떤 디렉터리를 바꾸는지 명확해야 한다는 요구가 같은 설계 안에 있다.[1]

## 자료와 주장을 따로 기록하는 구조

`wiki-ingest`는 제공된 자료를 연결된 페이지로 바꾸는 절차이고, `wiki-query`는 보관함의 근거를 이용해 읽기 전용으로 답하는 절차다. `save`는 특정 답변이나 통찰을 명시적으로 저장할 때 사용한다.
따라서 대화 내용을 저장하는 것과 외부 자료를 조사해 지식 페이지를 만드는 것은 다른 작업으로 취급된다.[1]

출처 ledger와 claim ledger는 각각 자료의 신원과 그 자료가 뒷받침하는 주장을 관리하는 장부다.
ingest Skill은 원본의 SHA-256을 계산해 동일 입력을 확인하고, 자료 유형에 맞춰 주장·개념·충돌·열린 질문을 추출하도록 한다.
SHA-256은 파일 내용이 같은지 비교하는 지문으로 이해하면 된다.
새 문장을 많이 만드는 것보다 기존 근거와 연결되는 정보가 생겼는지가 페이지 생성의 기준이다.[2]

병렬 작업자에게는 초안과 근거만 반환하도록 제한한다.
최종 파일, 주소 할당, manifest, ledger를 각 작업자가 따로 바꾸지 않고 하나의 조정자가 합친다.
여러 노트를 함께 고치는 논리적 작업을 하나의 복구 가능한 transaction으로 묶는 방식이다.
transaction은 관련 변경을 한 덩어리로 검사하고 적용하는 단위이며 단순히 여러 파일을 순서대로 덮어쓰는 것과 구분된다.[1][2]

## 예시로 따라가는 흐름

공식 시작 흐름은 별도 vault를 준비하고 `inbox/`에 자료를 넣은 다음 `wiki-ingest`를 호출하는 것이다.
예를 들어 공개 연구 문서를 처리한다고 이해해 보자.
이 문서 선택은 이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
에이전트는 먼저 처리할 파일과 분량, 기존 페이지를 읽을 범위를 정한다.
다른 경로에 있는 파일을 그대로 영구 출처로 간주하지 않고, 선택한 vault 안에서 원본을 보존하는 capture 절차를 거치도록 되어 있다.[1][2]

그 다음에는 해시와 기존 장부를 비교해 같은 자료인지 확인하고, 연구 자료라면 주장·방법·한계를 구분한다.
기존에 같은 개념을 설명하는 페이지가 있으면 그 주소를 재사용하며, 요약을 하나 더 만드는 것만으로 의미가 없으면 출처 기록만 남기거나 변경하지 않을 수 있다.
새 원본은 create 방식으로 보존하고 이미 저장된 원본을 덮어쓰지 않는다.[2]

출력은 원본과 연결된 페이지 초안, 출처·주장 기록, 연결 및 인덱스 변경을 포함하는 적용 묶음이다.
사람은 문장이 자연스러운지만 볼 것이 아니라 어떤 원본이 어떤 주장을 뒷받침하는지, 반대 근거가 숨겨지지 않았는지, 대상 vault가 맞는지 살펴야 한다.
승인 계획의 해시를 이용한 적용은 검토한 변경과 실제 변경이 어긋나지 않도록 하는 장치다.
이 과정에서 없는 근거는 확신 있는 설명으로 메우지 않고 unsupported 상태로 남기도록 요구한다.[1][2]

## 로컬 보관과 외부 전송은 별도 결정

로컬 우선이라는 말이 모든 모델 호출을 오프라인으로 만든다는 뜻은 아니다.
ingest 지침은 URL 조회 전에 대상 도메인과 요청 예산에 동의를 받도록 하고, vault 내용이나 비밀·관련 없는 대화를 전송하지 말라고 명시한다.
PDF·이미지·음성의 추출도 호스트에 해당 기능이 있어야 하며, 도구가 없으면 읽은 것처럼 가장하지 않고 지원하지 않는 추출을 표시해야 한다.[2]

운영체제 경계도 있다.
README는 native Windows에서 읽기와 dry-run은 가능하지만 쓰기는 WSL을 요구하며, 승인 해시는 실제 적용할 환경에서 검토해야 한다고 설명한다.
보관함이 일반 파일이라는 장점은 백업이 불필요하다는 뜻이 아니다.
이 프로젝트도 백업·소스 관리의 대체물이 아니라고 선을 긋는다.[1]

## 직접 읽어볼 자료

1. [README의 From source to living knowledge](https://github.com/AgriciDaniel/claude-obsidian/blob/main/README.md)
   캡처와 질의가 한 번으로 끝나는 작업이 아니라 재사용의 순환으로 연결되는지 읽는다.
   특히 제품 저장소와 사용자 vault 그림을 통해 어느 파일이 도구이고 어느 파일이 보존할 지식인지 먼저 구분한다.
2. [wiki-ingest의 처리 계약](https://github.com/AgriciDaniel/claude-obsidian/blob/main/skills/wiki-ingest/SKILL.md)
   Agree on scope and egress에서 입력과 네트워크 범위를 정하는 방식을 보고, Analyze before drafting의 페이지 생성 기준으로 이동한다.
   기존 자료를 다시 표현하는 것만으로 새 페이지를 만들지 않는 조건이 핵심 읽기 지점이다.
3. [README의 Trust와 Requirements](https://github.com/AgriciDaniel/claude-obsidian/blob/main/README.md)
   승인·transaction·복구가 어떻게 이어지는지 확인한 뒤 운영체제 조건을 읽는다.
   읽기 가능한 환경과 실제 쓰기 가능한 환경을 나누면 설치 설명을 기능 보장으로 오해하지 않을 수 있다.

## 정리

claude-obsidian은 노트를 많이 생성하는 기능보다 원본, 주장, 연결, 변경 승인을 함께 관리하는 체계다.
질의의 신뢰성은 이 기록의 질과 사용할 추출 도구에 달려 있으며, 파일 소유권과 외부 전송 동의는 별도로 확인해야 한다.

## 자료 확인 범위

2026-09-27 기준 공식 README의 지식 순환·신뢰 경계·요구사항과 `wiki-ingest/SKILL.md`를 읽었다.
실제 vault를 만들거나 파일을 수집·수정하는 동작은 실행하지 않았다.

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

[1] AgriciDaniel/claude-obsidian — README.md

<https://github.com/AgriciDaniel/claude-obsidian/blob/main/README.md>

[2] AgriciDaniel/claude-obsidian — skills/wiki-ingest/SKILL.md

<https://github.com/AgriciDaniel/claude-obsidian/blob/main/skills/wiki-ingest/SKILL.md>
