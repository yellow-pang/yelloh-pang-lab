# Yelloh, Pang!

GitHub에서 발견하고, 궁금해서 찾아보고, 직접 공부하고 써보는 개인 개발 기록 저장소입니다.

## 저장소 목적

관심 있는 오픈소스 Repository를 조사하고 분류해 기록합니다. 이후 기술을 더 공부하거나 직접 실행해본 내용도 이어서 쌓습니다. 작성한 글은 GitHub 학습 기록을 중심으로 Tistory와 개인 개발 블로그에서도 활용합니다.

## 기록하는 내용

- GitHub에서 발견하거나 Star한 Repository 분석
- 기술과 개념을 공부하며 이해한 내용
- Repository 또는 기술을 직접 설치하고 실행한 경험
- 글에서 사용하는 이미지와 정적 자료

## 폴더 구조

```text
.
├── README.md
├── AGENTS.md
├── .gitignore
├── templates/
│   ├── repository.md
│   ├── learning.md
│   └── experiment.md
├── repositories/
│   ├── README.md
│   ├── ai-agent/
│   ├── browser-automation/
│   ├── developer-tools/
│   ├── frontend-design/
│   ├── backend-infra/
│   ├── ai-ml-data/
│   └── system-design/
├── learning/
│   └── README.md
├── experiments/
│   └── README.md
└── assets/
    └── yelloh-pang/
```

`repositories/` 아래 문서는 Repository의 성격에 맞는 카테고리에 저장합니다. Repository 분석과 직접 사용한 경험은 각각 `repositories/`와 `experiments/`에 나누어 기록합니다.

## 기록 방식

새 글은 `templates/`의 템플릿을 복사해 작성합니다. Repository 분석 문서의 파일명은 Repository 이름을 기준으로 kebab-case를 사용하고 날짜는 파일명이 아닌 Front Matter의 `created`에 기록합니다.

예: `chrome-devtools-mcp.md`, `browser-use.md`, `portless.md`

## 기본 작업 흐름

```text
GitHub에서 Repository 발견
→ 관심 있으면 Star
→ Repository 조사
→ Markdown 작성
→ GitHub에 기록
→ Tistory 게시
→ 필요하면 직접 공부 또는 실행
→ learning 또는 experiments에 후속 기록
```
