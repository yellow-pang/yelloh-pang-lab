---
title: "Swordfish90/cool-retro-term"
repository: "Swordfish90/cool-retro-term"
url: "https://github.com/Swordfish90/cool-retro-term"
category: "backend-infra"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# Swordfish90/cool-retro-term

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

cool-retro-term은 오래된 브라운관 화면의 색과 빛 번짐, 곡면 느낌을 재현하는 터미널 에뮬레이터다.
터미널 에뮬레이터는 셸과 프로그램의 문자 입출력을 화면으로 보여 주는 창이며, 셸 자체나 옛 컴퓨터의 운영체제를 흉내 내는 가상 머신과는 다르다.
README는 Linux와 macOS에서 동작하며 Qt6가 필요하다고 설명한다.[1]

## 옛 화면을 닮게 만들되 명령은 실제로 실행하기

화면에 녹색 글씨가 보인다고 과거의 컴퓨터 명령 체계로 바뀌는 것은 아니다.
이 프로젝트가 바꾸려는 대상은 터미널의 시각적 표현이다.
README는 색상, 글꼴, 효과를 문맥 메뉴에서 설정할 수 있고 QML로 옮긴 qtermwidget을 사용한다고 설명한다.
QML은 Qt에서 화면 구성과 상호작용을 기술하는 언어다.
문자 입출력을 담당하는 구성 요소 위에 오래된 디스플레이의 표현을 더하는 구조로 이해할 수 있다.[1]

이 구분은 단순한 용어 문제가 아니다.
화면이 장난감처럼 보이더라도 셸에서 입력한 명령은 선택된 실제 프로그램에 전달된다.
확인한 `PreprocessedTerminal.qml`은 기본 명령 또는 사용자 지정 명령을 정한 뒤 `startShellProgram`을 호출한다.
시작 작업 디렉터리가 전달돼 있으면 그 위치도 세션에 설정한다.
시각 효과를 켰다는 이유로 명령 실행이 안전한 실습 공간에 격리되지는 않는다.[2]

## 문자 화면을 효과의 입력으로 쓰기

표시 흐름은 크게 터미널 원본과 효과 처리로 나뉜다. `PreprocessedTerminal.qml`에는 `QMLTermWidget`과 `QMLTermSession`이 있고, 화면 크기와 글꼴 폭을 이용해 터미널의 가상 크기를 계산한다.
이어 `ShaderEffectSource`가 터미널 화면을 효과의 입력으로 제공한다.
여기서 셰이더는 화면의 픽셀을 계산해 색이나 모양을 바꾸는 작은 그래픽 프로그램이다.[2]

`ShaderTerminal.qml`은 설정에 따라 사용할 셰이더 파일을 선택한다.
정적인 처리 쪽에는 색 분리, 빛 번짐, 곡면과 테두리 광택이 연결되고, 동적인 처리 쪽에는 깜박임, 화면 흔들림, 노이즈와 잔상 관련 값이 전달된다.
단순히 배경색 하나를 바꾸는 테마와 달리 시간과 해상도도 화면 계산에 참여하는 것이다.
어떤 효과가 활성화됐는지에 따라 파일 경로가 달라지는 구현도 확인된다.[3]

글꼴과 해상도 역시 효과와 별개가 아니다. `PreprocessedTerminal.qml`은 저해상도 글꼴 설정에 따라 안티앨리어싱, 부드러운 확대, 굵게·기울임 표현을 조정한다. `ShaderTerminal.qml`에는 가상 해상도와 실제 화면 해상도가 가까워질 때 래스터 효과의 강도를 줄이는 계산이 있다.
보기 좋은 복고풍 화면을 만드는 일에 문자 선명도와 확대 비율의 조건이 함께 들어간다는 의미다.[3][2]

## 예시로 따라가는 흐름

다음은 두 QML 파일의 연결을 풀어 쓴 가상 예시이며 직접 실행한 결과가 아니다.
사용자가 현재 셸의 텍스트 출력을 읽는 도중 글꼴 배율을 바꾸고 화면 곡률 효과를 켠다고 하자.
입력은 새로운 글꼴 설정과 화면 효과 설정이며, 터미널에서 실행 중인 명령의 의미를 바꾸려는 요청은 아니다.
글꼴 변경 신호를 받은 전처리 화면은 터미널 표시를 갱신하고 새 크기에 맞춘 화면 소스를 준비한다.[2]

효과 처리 쪽에서는 설정값에 맞는 셰이더와 곡률 값을 사용해 원본 터미널 화면을 그린다.
화면이 휘어 보이는 상태에서 사용자가 문자를 선택하려면 마우스 좌표도 원본 문자 화면에 맞아야 한다. `correctDistortion` 함수는 여백과 테두리, 곡률을 반영해 위치를 보정하고, 마우스 누름·해제·이동 이벤트를 터미널 위젯으로 전달한다.
화면을 휘게 그리는 일과 클릭 위치를 맞추는 일을 함께 처리하는 예다.[3][2]

사람이 확인할 결과는 글자가 읽기 편한지, 선택과 스크롤이 의도한 위치에 작동하는지다.
코드에는 복사·붙여넣기를 활성 터미널에 전달하는 경로도 있다.
이런 동작을 읽었다고 실제 화면에서 특정 글꼴이나 고해상도 모니터의 조합을 검증한 것은 아니다.
이 예시에서 확인한 것은 입력 이벤트, 터미널 화면, 그래픽 효과가 연결되는 구조다.[2]

## 표현 효과와 실용성의 경계

README의 “reasonably lightweight”는 프로젝트의 설계 의도를 설명하는 표현이다.
이 글은 실행 중 자원 사용량이나 다른 터미널과의 속도를 측정하지 않았으므로 가볍다는 성능 판단으로 바꾸지 않는다.
효과 처리 소스가 화면 해상도와 여러 설정값을 사용한다는 점은 확인했지만, 특정 컴퓨터의 부하나 프레임 속도는 별도의 실행 확인이 필요하다.[1][3]

깜박임·노이즈·곡률은 옛 화면의 인상을 만드는 요소이지 텍스트 정확성을 높여 주는 기능은 아니다.
코드가 색상과 효과를 조절할 수 있게 하는 만큼 읽기 편한 조합은 표시 환경에 맞춰 판단해야 한다.
특히 문자 내용과 화면 장식을 구별하면, 프로그램의 오류 메시지가 바뀐 것인지 단지 시각적 표현이 달라진 것인지 혼동하지 않을 수 있다.[1][3]

배포본과 소스 빌드도 구분된다.
README는 Linux의 AppImage, macOS의 dmg, 배포판 패키지를 안내하고 소스 빌드는 별도 위키로 연결한다.
여기서는 연결된 빌드 절차를 수행하지 않았다.
확인한 두 QML 파일은 GPL v3 또는 그 이후 버전 조건을 명시하지만, 포함된 모든 구성 요소의 라이선스를 이 두 파일만으로 일괄 판정하지는 않는다.[1][3][2]

## 직접 읽어볼 자료

- [공식 README](https://github.com/Swordfish90/cool-retro-term/blob/master/README.md):

  Description에서 무엇을 흉내 내는지와 지원 플랫폼을 먼저 읽는다.
  화면 예시의 이름을 운영체제 지원 목록으로 오해하지 않고, 색·글꼴·효과를 바꾸는 터미널이라는 정체를 확인하는 출발점이다.[1]
- [PreprocessedTerminal.qml](https://github.com/Swordfish90/cool-retro-term/blob/master/app/qml/PreprocessedTerminal.qml):

  `startSession`, `ShaderEffectSource`, `correctDistortion`을 순서대로 찾는다.
  실제 셸 세션이 시작되고 그 화면을 효과에 넘기며, 마우스 입력은 다시 원래 터미널 좌표로 돌아오는 연결을 볼 수 있다.[2]
- [ShaderTerminal.qml](https://github.com/Swordfish90/cool-retro-term/blob/master/app/qml/ShaderTerminal.qml):

  정적·동적 셰이더 경로 선택과 설정값을 비교한다.
  배경색 변화와 시간에 따라 변하는 깜박임이 같은 처리인지, 해상도가 왜 효과 강도에 영향을 주는지 질문하며 읽으면 된다.[3]

## 정리

cool-retro-term의 중심은 실제 터미널 입출력 위에 복고풍 화면 효과를 입히는 데 있다.
셸 명령의 기능을 교체하지 않으며, 시각적 왜곡에 맞춘 입력 좌표 처리까지 포함한다.
외형의 재현과 명령 실행의 책임을 분리해서 이해해야 한다.[1][3][2]

## 자료 확인 범위

2026-09-27에 수집한 README와 전처리·셰이더 QML 구현을 읽었다.
앱 설치, 셸 실행, 화면 캡처 또는 자원 사용 측정은 하지 않았으며 배포판별 동작을 시험하지 않았다.

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

[1] Swordfish90/cool-retro-term — README.md

<https://github.com/Swordfish90/cool-retro-term/blob/master/README.md>

[2] Swordfish90/cool-retro-term — app/qml/PreprocessedTerminal.qml

<https://github.com/Swordfish90/cool-retro-term/blob/master/app/qml/PreprocessedTerminal.qml>

[3] Swordfish90/cool-retro-term — app/qml/ShaderTerminal.qml

<https://github.com/Swordfish90/cool-retro-term/blob/master/app/qml/ShaderTerminal.qml>
