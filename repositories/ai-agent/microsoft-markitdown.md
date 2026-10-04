---
title: "microsoft/markitdown"
repository: "microsoft/markitdown"
url: "https://github.com/microsoft/markitdown"
category: "ai-agent"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "ai-agent"
  - "starred-draft"
---

# microsoft/markitdown

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

MarkItDown은 여러 문서 형식을 Markdown 텍스트로 바꾸는 Python 도구다.
문서를 검색하거나 언어 모델에 전달하기 전에 제목, 목록, 표, 링크 같은 구조를 남기는 데 초점을 둔다.
원래 문서의 모든 시각적 배치를 그대로 재현하는 출판용 변환기나 문서 편집 앱은 아니다.[1]

## 문서가 읽히는 형식과 분석되는 형식

사람은 스프레드시트를 열어 탭을 바꾸거나 PDF 페이지를 넘기며 내용을 읽는다.
하지만 텍스트 분석 프로그램은 각 형식을 그대로 이해하지 못할 수 있다.
MarkItDown은 서로 다른 입력을 Markdown이라는 공통 표현으로 바꾸어 후속 처리 단계가 내용을 다루기 쉽게 한다.
Markdown은 일반 텍스트에 제목과 표 같은 최소한의 구조 표시를 덧붙이는 문법이다.[1]

README는 PDF, Word, PowerPoint, Excel, HTML, 이미지와 오디오 등 여러 입력을 소개한다.
지원 목록이 길다는 사실보다 중요한 것은 형식마다 처리 방법과 필요한 의존성이 다르다는 점이다.
의존성은 해당 변환을 수행하는 데 함께 필요한 라이브러리를 뜻한다.
모든 추가 패키지를 선택할 수도 있고 PDF·DOCX·PPTX처럼 필요한 형식만 선택할 수도 있다.[1]

## 변환기와 선택적 외부 처리

Python API의 기본 예제는 `MarkItDown(enable_plugins=False)` 객체를 만들고 `test.xlsx`를 변환한 뒤 `result.markdown`을 읽는 구조다.
CLI는 파일 경로를 받아 Markdown을 출력하거나 지정한 파일에 저장한다.
즉 같은 변환 기능을 Python 프로그램 내부에서 호출할 수도 있고 터미널 작업으로 사용할 수도 있다.[1]

플러그인은 기본적으로 꺼져 있다.
별도 OCR 플러그인이나 이미지 설명을 위한 LLM 클라이언트를 연결하면 처리 범위가 달라지며, README의 OCR 플러그인은 클라이언트가 없을 때 OCR을 건너뛴다고 설명한다.
OCR은 이미지 속 글자를 텍스트로 읽는 기능이다.
따라서 이미지가 포함된 문서를 변환했다는 사실만으로 그림 안의 글자까지 읽혔다고 판단할 수는 없다.[1]

클라우드 분석 서비스도 선택 경로다.
README는 기본 변환기와 Document Intelligence, Content Understanding을 구분하고, 후자의 구조화 필드 추출이나 오디오·영상 분석을 설명한다.
이때 일부 형식의 변환 호출은 과금되는 외부 API 요청이 된다.
파일을 Markdown으로 바꾸는 겉모습은 같아도 로컬 처리와 외부 전송, 비용 조건은 다르다.[1]

## 예시로 따라가는 흐름

공식 Python 예제의 `test.xlsx`를 대표 경로로 살펴보자.
실제 파일을 실행한 결과가 아니라 README와 XLSX 변환 구현을 연결해 읽는 사례다.
변환기는 확장자나 MIME 형식 정보로 XLSX 입력을 받아들일지 판단하고, 필요한 라이브러리가 없으면 의존성 오류를 내도록 되어 있다.
준비가 되면 `pandas.read_excel`에 모든 시트를 읽도록 요청하고 `openpyxl` 엔진을 사용한다.
여기서 시트는 통합 문서 안의 개별 표 탭이다.[1][2]

시트마다 이름을 Markdown 2단계 제목으로 붙이고, 셀 데이터를 HTML 표로 만든 다음 내부 HTML 변환기를 통해 Markdown으로 바꾼다.
여러 시트의 결과를 이어 붙여 하나의 `DocumentConverterResult`로 반환하는 것이 코드에서 확인되는 흐름이다.
사람은 출력된 시트 이름과 표가 원본의 의도와 맞는지, 빈 값이나 헤더가 다르게 해석되지는 않았는지 확인해야 한다.
기본 이미지 변환 훅은 아무 결과도 반환하지 않으므로, 표 옆에 붙은 그림의 설명이나 원래 화면 배치가 자동으로 모두 보존된다고 기대해서도 안 된다.[2]

이 흐름의 결과물은 ‘분석 가능한 텍스트 표현’이다.
스프레드시트의 계산 기능을 Markdown에서 계속 실행하거나 원래 셀 스타일을 편집하는 기능이 아니다.
변환된 문서를 검색이나 요약에 넘기기 전에 표의 경계가 유지되었는지 확인해야 후속 모델이 서로 다른 시트의 값을 하나의 표로 오해하는 일을 줄일 수 있다.[1][2]

## 입력을 받는 권한도 함께 넘어온다

README의 보안 주의는 변환기가 현재 프로세스의 권한으로 파일과 네트워크에 접근한다는 데서 출발한다.
신뢰하지 않는 사용자가 준 경로나 URL을 그대로 전달하면, 변환 내용만의 문제가 아니라 접근 가능한 자원의 문제가 된다.
웹 서비스에 붙이는 경우 파일 경로, URI 종류, 네트워크 목적지의 제한을 호출하는 쪽에서 정해야 한다.[1]

이를 위해 문서는 가능한 한 범위가 좁은 함수를 쓰라고 권한다.
로컬 파일만 필요하면 `convert_local()`, 직접 연 스트림만 처리하려면 `convert_stream()`을 택하는 식이다.
범용 `convert()`가 편리하다는 이유로 모든 입력을 허용하는 것이 기본 보안 설계가 되어서는 안 된다.
이 도구가 서버 운영자의 접근 통제 정책을 대신 만들지는 않는다.[1]

또한 원문이 복잡한 표나 스캔 이미지일수록 ‘변환 완료’와 ‘내용 누락 없음’을 따로 검사해야 한다.
README도 사람용 고충실도 문서 변환에는 가장 적절한 선택이 아닐 수 있다고 밝힌다.
기능별 옵션, 플러그인 활성화, 외부 서비스 경로를 기록해 두어야 결과를 다시 비교할 때 같은 조건인지 판단할 수 있다.[1]

## 직접 읽어볼 자료

- [README의 소개와 Python API](https://github.com/microsoft/markitdown/blob/main/README.md)
  먼저 텍스트 분석을 위한 도구라는 목적과 `result.markdown` 예제를 읽는다.
  출력이 원본의 대체 편집 문서인지, 다음 프로그램에 전달할 텍스트인지 구분하는 질문으로 시작하면 좋다.
- [XLSX 변환기](https://github.com/microsoft/markitdown/blob/main/packages/markitdown/src/markitdown/converters/_xlsx_converter.py)
  `accepts`, `convert`, `_image_to_html`을 순서대로 본다.
  입력 판별, 시트별 표 변환, 기본 그림 처리의 경계가 서로 다른 책임이라는 점을 확인할 수 있다.
- [README의 Security Considerations](https://github.com/microsoft/markitdown/blob/main/README.md)
  API를 선택하기 전에 권한 경고를 다시 읽는다.
  문서를 어디서 받아 어떤 자원까지 접근하게 할지 결정하는 문제는 변환 품질과 별도로 검토해야 한다.

## 정리

MarkItDown은 다양한 문서의 내용을 Markdown으로 통일하는 전처리 도구다.
파일 형식별 변환 경로와 선택적 OCR·클라우드 처리, 그리고 입력 접근 권한을 구별하면 출력의 의미와 한계를 더 정확히 이해할 수 있다.

## 자료 확인 범위

2026-09-27 수집본의 README와 XLSX 변환 구현을 확인했다.
문서 변환, OCR, 외부 분석 API 호출이나 패키지 설치는 실행하지 않았으며, 실제 파일별 변환 품질은 측정하지 않았다.

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

[1] microsoft/markitdown — README.md

<https://github.com/microsoft/markitdown/blob/main/README.md>

[2] microsoft/markitdown — packages/markitdown/src/markitdown/converters/_xlsx_converter.py

<https://github.com/microsoft/markitdown/blob/main/packages/markitdown/src/markitdown/converters/_xlsx_converter.py>
