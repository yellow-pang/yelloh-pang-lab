---
title: "firecrawl/pdf-inspector"
repository: "firecrawl/pdf-inspector"
url: "https://github.com/firecrawl/pdf-inspector"
category: "browser-automation"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "browser-automation"
  - "starred-draft"
---

# firecrawl/pdf-inspector

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

pdf-inspector는 PDF 안에 추출할 수 있는 글자가 있는지 검사하고, 텍스트와 문서 구조를 Markdown으로 옮기는 Rust 라이브러리다.
화면에서 PDF를 넘겨 보는 리더가 아니라, 문서 처리 프로그램이 읽기 방식과 후속 처리를 결정하도록 돕는 검사·추출 도구다.[1]

## 같은 PDF라도 읽는 방법은 다르다

화면에서 비슷하게 보이는 문서도 내부는 다를 수 있다.
문서 편집기에서 내보낸 PDF에는 문자와 위치 정보가 들어 있지만, 종이를 스캔한 PDF에는 페이지 사진만 있을 수 있다.
후자는 이미지 속 글자를 인식하는 OCR이 필요하다.
이미 글자가 있는 문서까지 모두 OCR에 보내면 불필요한 처리 단계가 생긴다.
pdf-inspector가 제시하는 사용 흐름은 먼저 문서 유형을 검사하고, 원래의 텍스트를 읽을 수 있으면 로컬에서 추출하며, 그렇지 않은 페이지에는 다른 처리 경로를 선택하는 것이다.[1]

프로젝트는 Rust API와 명령줄 도구 외에도 Python·Node.js 연결 기능, 브라우저에서 실행하는 WebAssembly 패키지를 제공한다.
같은 이름 아래에서도 기본 텍스트 추출과 선택적 OCR의 실행 조건은 다르다.
기본 Rust·브라우저 빌드는 추출 중심이며, OCR은 별도의 실행 환경을 요구한다.
따라서 “PDF를 읽는다”는 한 표현으로 검사, 추출, 문자 인식이 모두 같은 동작이라고 이해해서는 안 된다.[1]

## 검사 결과가 후속 처리를 나눈다

검사기는 `TextBased`, `Scanned`, `ImageBased`, `Mixed`로 문서 유형을 구분한다.
문자 출력 명령인 `Tj`·`TJ` 등이 PDF 내용에 있는지 살펴보고, 여러 페이지에서 얻은 정보를 바탕으로 분류한다.
`src/detector.rs`에는 유형뿐 아니라 전체 페이지 수, 실제 표본으로 본 페이지 수, 신뢰도, OCR이 필요한 페이지와 그 이유를 담는 결과 구조가 있다.
이는 문서 전체에 이름표 하나만 붙이는 것보다, 다음 단계가 어떤 페이지를 별도로 처리할지 판단할 정보를 주려는 설계다.[1][2]

모든 페이지를 똑같이 검사하는 것은 아니다.
확인한 기본 설정은 최대 여덟 페이지를 고르게 고르는 `Sample(8)`이고, 전체를 확인하는 `Full`이나 지정 페이지를 보는 `Pages`도 있다.
표본 검사는 속도를 위한 선택이므로 표본 밖의 예외 페이지까지 모두 확인했다는 뜻은 아니다.
코드의 기본값에는 그림 표지 다음에 본문이 이어지는 보고서를 첫 비문자 페이지만 보고 판단하지 않으려는 설명도 들어 있다.[2]

추출 단계는 글자만 이어 붙이는 데서 멈추지 않는다.
문자의 좌표와 글꼴 정보로 줄과 다단 편집의 읽기 순서를 다루며, 표는 사각형 그리기 정보와 문자 정렬을 이용한 두 방식으로 찾는다.
이후 제목·목록·표 등을 Markdown 문법으로 바꾼다.
문서를 한 번 읽어 검사와 추출에 공유하는 구조도 README에 제시되어 있다.
다만 글꼴 크기나 정렬을 이용한 판단은 원래 작성자가 지정한 의미를 항상 그대로 복원한다는 보장이 아니다.[1]

## 예시로 따라가는 흐름

공식 Python 예제의 `document.pdf` 입력을 바탕으로, 공개 보고서의 텍스트 본문과 스캔된 부록을 정리하는 상황을 생각할 수 있다.
보고서의 구성은 이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
먼저 문서 유형만 알고 싶다면 `detect_pdf`로 검사하고, 검사부터 Markdown 변환까지 필요하다면 `process_pdf`를 선택한다.
같은 PDF 경로를 받더라도 전자는 추출을 하지 않으며, 문서에 따르면 그 결과의 `markdown`은 `None`이다.[3]

`process_pdf`의 결과에서는 문서 유형, 페이지 수, 신뢰도, Markdown, OCR 필요 페이지 등을 읽을 수 있다.
본문이 추출되었다면 산출물은 검색·검토에 사용할 수 있는 Markdown 문자열이다.
스캔 부록이 남았다고 해서 그 페이지의 글자를 기본 추출기가 읽은 것처럼 채워 넣어서는 안 된다.
선택적 OCR이 필요하면 `process_pdf_with_ocr`라는 별도 진입점을 이용하며, `auto`는 원래 텍스트 추출로 받아들이지 못한 페이지를 OCR로 보낸다.[3]

이때 PDFium은 페이지를 이미지로 만들고, ONNX Runtime과 PP-OCRv6 Small 모델은 글자 인식에 쓰인다.
결과에는 페이지별 Markdown뿐 아니라 원래 텍스트인지 OCR인지 등의 출처 정보와 경고가 포함된다.
사람은 원본과 변환본을 나란히 보며 표의 행·열, 다단 본문의 순서, 깨진 문자, 부록 누락을 확인해야 한다.
분류 신뢰도는 문서 내용의 사실성이나 모든 표의 정확성을 인증하는 값이 아니므로, 숫자 하나로 검토를 대체할 수 없다.[1][3][4]

## OCR을 켠다는 말에 포함된 조건

선택적 OCR은 기본 패키지 안에 모든 모델이 포함된 방식이 아니다.
OCR 경로로 실제 페이지가 넘어가면 별도로 설치된 PDFium·ONNX Runtime이 필요하고, 첫 처리에서는 고정된 모델 파일을 내려받아 체크섬으로 확인한다.
네트워크 접근을 금지하려면 모델을 미리 준비하고 `offline=True`와 모델 디렉터리를 지정하는 식의 구성이 필요하다.
반대로 `auto`에서 OCR 대상 페이지가 없다면 이 외부 라이브러리나 모델 캐시·네트워크를 사용하지 않는다고 문서는 설명한다.[4]

로컬 처리 후의 품질 문제와 실행 환경 실패도 구별된다.
`pages_recommending_hosted`는 OCR 결과가 비어 있거나 신뢰도가 낮고 불완전해 보이는 페이지를 알려주는 정보다.
외부 라이브러리 누락이나 모델 다운로드 실패는 그 결과를 만들기 전의 오류로 반환된다.
호스팅된 문서 서비스로 보낼지 결정하고 오류를 처리하는 일은 연결 프로그램의 책임이며, 추천 필드가 있다는 이유로 문서가 자동 업로드된다고 읽어서는 안 된다.[4]

개인정보 경계도 이 지점에서 정해야 한다.
PDF 원본의 열람·처리 권한과, 추출한 본문을 외부 OCR·AI 서비스에 보내도 되는 권한은 별개다.
로컬 처리 뒤라도 Markdown에는 원문의 민감한 정보가 남을 수 있으므로 공유 대상과 저장 위치를 확인해야 한다.
여기서 외부 서비스 전송은 후속 통합을 선택할 때의 검토 사항이며, 이 라이브러리 자체가 항상 문서를 외부로 보낸다는 주장은 아니다.[3][4]

## 결과를 읽을 때 놓치기 쉬운 차이

Python API마다 페이지 번호의 시작점이 다르다.
문서상 `process_pdf`의 `pages`와 `PdfResult.pages_needing_ocr`는 1부터 세지만, `extract_pages_markdown`의 선택 목록과 페이지 객체는 0부터 센다.
함수 사이에 목록을 넘길 때 번호 체계를 그대로 섞으면 다른 페이지를 처리할 수 있으므로, 같은 이름의 페이지 필드라도 반환 형식을 확인해야 한다.[3]

운영체제 지원도 기본 추출과 OCR을 따로 봐야 한다.
OCR 실행 안내는 Linux x64에서 전체 경로를 CI로 시험한다고 설명하며, macOS·Windows 외부 실행 환경 경로는 동등한 검증이 추가되기 전까지 preview로 취급한다.
Intel macOS에는 Python wheel이 있어도 해당 ONNX Runtime 배포본 조건이 다르다.
README의 속도·정확도 표 역시 저자가 특정 데이터와 조건으로 제시한 결과이며, 이 글에서 다른 문서나 환경에 대해 재현한 값은 아니다.[1][4]

## 직접 읽어볼 자료

- [공식 README](https://github.com/firecrawl/pdf-inspector/blob/main/README.md)

  Architecture와 How classification works를 먼저 읽으면 검사, 텍스트 추출, 표 처리, Markdown 변환이 어떻게 이어지는지 볼 수 있다.
  Benchmark는 OCR을 끈 비교라는 조건까지 함께 읽어, 스캔 문서 성능으로 잘못 확대하지 않는 것이 중요하다.

- [검사기 구현 `src/detector.rs`](https://github.com/firecrawl/pdf-inspector/blob/main/src/detector.rs)

  처음에는 긴 구현 전체보다 `PdfTypeResult`, `ScanStrategy`, `DetectionConfig`의 기본값을 보면 된다.
  “분류했다”가 어떤 결과 필드를 뜻하는지, 일부 페이지를 본 결과와 전체 검사 결과의 차이가 무엇인지 확인할 수 있다.

- [Python API 문서](https://github.com/firecrawl/pdf-inspector/blob/main/docs/python.md)

  Usage에서 검사 전용과 전체 처리 예제를 비교한 뒤 Types에서 `PdfResult`와 OCR 결과의 차이를 읽는다.
  Markdown이 없는 경우와 페이지 번호가 0·1 중 어디서 시작하는지까지 확인하면 함수를 잘못 연결할 가능성을 줄일 수 있다.

- [OCR 실행 환경 안내](https://github.com/firecrawl/pdf-inspector/blob/main/docs/ocr-runtime.md)

  모델 캐시와 오프라인 모드, Hosted fallback boundary를 이어서 읽는다.
  모델 다운로드, 로컬 추출 실패, 외부 서비스로의 전달이 서로 다른 단계라는 점을 확인할 수 있다.

## 정리

pdf-inspector의 핵심은 문서를 보여주는 것이 아니라, PDF를 어떤 방식으로 읽어야 할지 판단하고 추출 결과를 구조화하는 데 있다.
기본 추출, 선택적 OCR, 외부 서비스 연계를 구분하고 페이지별 결과를 원문과 대조해야 그 역할을 정확히 사용할 수 있다.[1][3][4]

## 자료 확인 범위

2026-09-27 기준으로 수집된 README, 검사기 결과 구조와 기본 설정, Python API, OCR 실행 환경 문서를 확인했다.
설치나 문서 변환을 실행하지 않았으며, OCR 품질·처리 속도·운영체제별 호환성을 직접 검증하지 않았다.

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

[1] firecrawl/pdf-inspector — README.md

<https://github.com/firecrawl/pdf-inspector/blob/main/README.md>

[2] firecrawl/pdf-inspector — src/detector.rs

<https://github.com/firecrawl/pdf-inspector/blob/main/src/detector.rs>

[3] firecrawl/pdf-inspector — docs/python.md

<https://github.com/firecrawl/pdf-inspector/blob/main/docs/python.md>

[4] firecrawl/pdf-inspector — docs/ocr-runtime.md

<https://github.com/firecrawl/pdf-inspector/blob/main/docs/ocr-runtime.md>
