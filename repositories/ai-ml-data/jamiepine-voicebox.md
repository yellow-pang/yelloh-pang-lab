---
title: "jamiepine/voicebox"
repository: "jamiepine/voicebox"
url: "https://github.com/jamiepine/voicebox"
category: "ai-ml-data"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "ai-ml-data"
  - "starred-draft"
---

# jamiepine/voicebox

> https://github.com/jamiepine/voicebox

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Voicebox는 로컬 컴퓨터에서 목소리 프로필을 만들고, 글을 음성으로 읽거나 음성을 글로 바꾸는 AI 음성 스튜디오이다.[1]

## 이 Repository는 무엇인가?

텍스트를 말소리로 만드는 TTS와 말소리를 글로 바꾸는 STT를 같은 앱에서 다루며, 음성 복제·받아쓰기·AI 에이전트 음성 출력을 연결한다.[1]

참조 녹음으로 음성 프로필을 만들거나 기본 제공 목소리를 선택하며, 프로필에는 효과와 말투를 위한 설정도 연결할 수 있다.[1]

### 자료 확인 범위

2026-09-27에 공개 README와 아래에 인용한 공식 파일을 확인한 기본 초안이다.
설치·실행 시험이나 성능 검증을 한 문서는 아니다.
개인적인 Star 이유와 평가, 사용 후기는 직접 작성할 수 있도록 비워두었다.

## 주요 기능

- Qwen3-TTS, Chatterbox, Kokoro 등 여러 합성 엔진을 선택하고 생성한 음성에 음높이·잔향·필터 같은 후처리 효과를 적용한다.[1]
- 긴 글을 문장 경계로 나누어 생성한 뒤 이어 붙이고, Stories 편집기로 여러 목소리를 시간축에 배치할 수 있다.[1]
- Whisper 기반 전사와 전역 받아쓰기를 제공하며, 녹음과 전사문을 Captures에 함께 보관한다.[1]
- REST API와 MCP 서버를 제공하여 외부 앱이나 AI 에이전트가 발화·전사·프로필 조회 기능을 호출할 수 있다.[1]

## 어떻게 동작하는가?

```text
사용 권한이 있는 녹음으로 프로필을 만들거나 제공 목소리를 선택한다.
↓
텍스트와 합성 엔진을 지정하여 로컬에서 음성을 만들고 필요한 효과를 적용한다.
↓
생성 음성을 재생하거나 Stories 편집기에 배치하며, API로 외부 도구와 연결할 수도 있다.
```

위 흐름은 음성 출력 쪽을 요약한 것으로, 받아쓰기에서는 반대로 녹음이 전사 과정을 거쳐 텍스트 입력으로 이어진다.[1]

### 확인한 파일 구성

- `app/`은 공유 React 화면, `tauri/`는 데스크톱 앱, `backend/`는 Python FastAPI 서버로 README에 설명되어 있다.[1]
- 루트 `package.json`은 `app`, `tauri`, `web`, `landing`을 작업 공간으로 묶고, 서버·웹·데스크톱의 개발과 빌드 작업을 구분한다.[2]
- `RESPONSIBLE_USE.md`는 음성 사용 권한과 금지되는 사칭·사기 등 책임 있는 사용 기준을 설명한다.[3]

## 어떤 기술을 사용하는가?

- `Tauri`와 `Rust`: 데스크톱 앱과 전역 단축키·붙여넣기 같은 네이티브 연결을 구성한다.[1]
- `FastAPI`와 `FastMCP`: 음성 기능을 HTTP API와 AI 도구 연결 규약인 MCP로 제공한다.[1]
- `MLX`와 `PyTorch`: 운영체제와 하드웨어에 따라 모델 추론, 즉 학습된 모델로 결과를 만드는 처리를 맡는다.[1]

## 실제로 어디에 사용할 수 있는가?

- 자신의 목소리나 명시적으로 허락받은 목소리로 개인 창작물의 대사와 오디오를 구성할 수 있다.[3]
- 개인 AI 도구에 음성 입력과 발화를 연결하거나, 음성 합성과 전사 흐름을 함께 학습할 수 있다.[1]

## 왜 주목할 만한가?

학습 관점에서는 음성 입력과 출력을 분리된 제품으로 다루지 않고, 같은 프로필·로컬 모델·API에 연결하는 구조를 살펴볼 수 있다.[1]

## 장점

- 합성·전사·효과·시간축 편집을 같은 앱에서 연결한다.[1]
- 화면에서 쓰는 기능을 API와 MCP로도 제공하여 별도 개인 도구와 연결할 수 있다.[1]

## 단점 / 주의점

- 음성 소유권을 독립적으로 확인하는 기능은 없으며, 사용자는 음성 복제·가져오기·생성에 필요한 권리를 직접 확인해야 한다.[3]
- Windows·Linux의 자동 붙여넣기는 로드맵 항목이고, Linux 사전 빌드 바이너리는 아직 제공되지 않는다고 README에 명시되어 있다.[1]
- 프로젝트는 MIT License로 안내되지만, 여러 엔진의 언어·표현 기능은 서로 다르며 문서의 지원 목록을 모두 동일한 기능으로 해석해서는 안 된다.[1]

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

- [debpalash/VoiceStudio](../frontend-design/debpalash-voicestudio.md)

## 정리

Voicebox는 로컬 음성 생성과 전사를 편집 화면 및 API로 연결하는 도구이다.[1] 음성 사용 동의와 플랫폼·엔진별 기능 차이를 구분해야 한다.[1][3]

## Sources

[1] jamiepine/voicebox 공식 README

<https://github.com/jamiepine/voicebox/blob/main/README.md>

[2] jamiepine/voicebox package.json

<https://github.com/jamiepine/voicebox/blob/main/package.json>

[3] jamiepine/voicebox RESPONSIBLE_USE.md

<https://github.com/jamiepine/voicebox/blob/main/RESPONSIBLE_USE.md>
