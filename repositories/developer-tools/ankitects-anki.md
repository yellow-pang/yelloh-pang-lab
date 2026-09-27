---
title: "ankitects/anki"
repository: "ankitects/anki"
url: "https://github.com/ankitects/anki"
category: "developer-tools"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# ankitects/anki

> https://github.com/ankitects/anki

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Anki는 질문과 답을 담은 학습 카드를 반복해서 복습하고, 기억 정도에 맞춰 다음 복습 시점을 정하는 프로그램이다.[1][2]

## 이 Repository는 무엇인가?

이 저장소는 Anki의 컴퓨터용 버전 소스 코드를 제공하며, README는 이를 간격 반복 학습 프로그램으로 소개한다.[1]

간격 반복은 복습 사이에 시간을 두는 방식이며, Anki는 이미 아는 내용보다 어려운 내용에 더 많은 학습 시간을 배분하도록 돕는다.[1][2]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- 학습 카드를 덱이라는 묶음으로 나누어 전체 자료 중 특정 부분을 선택해 공부할 수 있다.[2]
- 카드에 음성, 이미지, 동영상과 과학 표기법을 넣고 카드 배치와 복습 시점을 조정할 수 있다.[2]
- AnkiWeb 동기화로 기기 간 카드를 맞추고, 추가 기능을 설치해 프로그램의 기능을 확장할 수 있다.[2]

## 어떻게 동작하는가?

```text
학습할 내용을 카드로 만들고 덱에 모은다.
↓
카드를 복습하면서 얼마나 잘 기억했는지 적절한 선택지로 평가한다.
↓
Anki가 평가에 따라 다음 복습 시점을 정해 다시 공부할 순서를 관리한다.
```

공식 웹사이트가 설명하는 기본 복습 흐름이며, 동기화와 추가 기능은 기본 카드 복습과 구분되는 확장 기능이다.[2]

### 확인한 파일 구성

- `README.md`는 컴퓨터용 소스 저장소라는 범위를 밝히고 기여·개발 문서와 베타 배포 안내로 연결한다.[1]
- `pyproject.toml`은 로컬 개발 환경의 의존성을 선언하며 `pylib`, `qt`, `qt/installer/briefcase_plugins`를 uv 작업 공간의 구성원으로 지정한다.[3]

## 어떤 기술을 사용하는가?

- `Python`: 확인한 개발 환경 manifest는 Python 3.12 이상을 요구하고 pytest, mypy, ruff 등의 개발 도구를 선언한다.[3]
- `uv`: 여러 Python 하위 프로젝트의 작업 공간과 의존성 해석 조건을 관리하도록 설정되어 있다.[3]

## 실제로 어디에 사용할 수 있는가?

- 외국어 단어나 개인 학습 개념을 질문·답 카드로 정리하고 기억 정도에 따라 반복 복습하는 데 사용할 수 있다.[2]
- 음성·이미지가 포함된 카드를 만들어 표현 방식을 비교하거나, 카드 배치와 복습 간격을 조정하는 학습 도구로 활용할 수 있다.[2]

## 왜 주목할 만한가?

학습 관점에서는 카드 내용의 작성과 복습 일정 결정을 분리하고, 사용자의 기억 평가를 일정 조정에 연결하는 구조를 살펴볼 수 있다.[2]

## 장점

- 텍스트뿐 아니라 여러 매체를 같은 카드 학습 흐름에서 사용할 수 있다.[2]
- 동기화, 덱 공유, 추가 기능을 통해 학습 자료의 이동과 프로그램 확장 경로를 제공한다.[2]

## 단점 / 주의점

- 이 저장소의 범위는 컴퓨터용 Anki이며, 공식 사이트가 별도로 소개하는 iOS의 AnkiMobile과 Android의 AnkiDroid 전체 소스를 뜻하지 않는다.[1][2]
- `pyproject.toml`의 Python 요구사항은 소스 개발 환경에 대한 선언이며 일반 사용자의 배포판 실행 요구사항과 구별해야 한다.[3]
- 기본 코드는 AGPL-3.0-or-later이며 일부 기여분·포함 라이브러리에는 다른 조건이 적용된다. `docs-site` 문서는 CC BY-SA 4.0으로 구분한다.[4]

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

## 정리

카드와 덱을 입력으로 받아 기억 평가에 따른 복습 일정을 관리하는 개인 학습 프로그램이다.[2] 컴퓨터용 소스의 개발 환경과 모바일 앱·동기화 서비스의 범위는 나누어 이해해야 한다.[1][2][3]

## Sources

[1] ankitects/anki 공식 README

<https://github.com/ankitects/anki/blob/main/README.md>

[2] Anki - powerful, intelligent flashcards

<https://apps.ankiweb.net>

[3] ankitects/anki pyproject.toml

<https://github.com/ankitects/anki/blob/main/pyproject.toml>

[4] ankitects/anki LICENSE

<https://github.com/ankitects/anki/blob/main/LICENSE>
