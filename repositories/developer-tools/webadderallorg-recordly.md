---
title: "webadderallorg/Recordly"
repository: "webadderallorg/Recordly"
url: "https://github.com/webadderallorg/Recordly"
category: "developer-tools"
created: "2026-10-04"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# webadderallorg/Recordly

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

Recordly는 화면 녹화에 확대·축소, 커서 효과, 배경과 웹캠 배치를 더해 설명 영상을 만들고 MP4나 GIF로 내보내는 데스크톱 앱이다.
녹화된 원본을 그대로 보관하는 도구에 그치지 않고, 시청자가 볼 지점을 강조하는 편집 과정을 함께 제공한다.[1]

## 화면을 기록하는 일과 보여줄 장면을 만드는 일

README가 제시하는 주된 대상은 기능 소개, 사용법 안내, 데모 영상이다.
전체 화면이나 특정 창을 선택해 녹화하고, 종료하면 편집기로 넘어간다.
커서 움직임을 바탕으로 확대 구간을 제안하거나 직접 확대 구간을 추가할 수 있으며, 불필요한 부분 자르기와 재생 속도 변경도 지원한다고 설명한다.[1]

여기에 텍스트·이미지·도형 주석, 추가 오디오, 웹캠 영상의 위치와 모양, 화면 테두리와 배경 스타일을 조합한다.
이 기능들은 화면 속 행동을 설명하기 위한 편집 수단이다.
README에서 확인한 범위를 넘어 범용 영상 편집기의 모든 기능이나 자동 개인정보 가림까지 제공한다고 해석하지 않는다.[1]

## 캡처, 편집 상태, 최종 파일의 구성

캡처는 운영체제별 경로를 사용한다.
macOS는 ScreenCaptureKit, 지원되는 Windows는 Windows Graphics Capture와 네이티브 오디오 도우미, Linux는 Electron 캡처 API를 사용한다고 안내한다.
Electron은 데스크톱 앱의 흐름을 조정하고, PixiJS는 영상과 장식 요소를 하나의 장면으로 합성하는 역할을 맡는다.[1]

타임라인은 시간축 위에서 어느 구간에 효과를 적용할지 표시하는 편집 구조다.
확대, 자르기, 속도, 오디오와 주석을 이 시간축에 배치하고, 미리보기와 내보내기는 같은 장면 구성 논리를 사용한다고 설명한다.
원본 영상, 효과의 설정, 최종 MP4를 구분하면 편집 결과를 다시 열거나 전달할 때 무엇이 필요한지 이해하기 쉽다.[1]

`.recordly`는 원본 미디어 경로와 편집 상태를 보관하는 프로젝트 파일이다.
완성된 영상 자체와 동일한 파일은 아니므로, 프로젝트 파일 하나만 옮기면 모든 원본이 함께 이동한다고 가정해서는 안 된다.
원본 위치를 바꾸거나 다른 컴퓨터로 작업을 옮길 때는 연결된 미디어가 유지되는지 확인할 필요가 있다.[1]

## 예시로 따라가는 흐름

이해용 사례로 공개 데모 페이지에서 버튼을 누르고 결과 화면을 보여주는 짧은 안내 영상을 가정할 수 있다.
입력은 녹화할 창, 필요에 따라 선택한 마이크·시스템 오디오, 사용자의 화면 조작이다.
README의 사용 순서에 따르면 녹화를 멈춘 뒤 편집기로 이동해 기다리는 부분을 자르고, 버튼을 누르는 구간에 확대 효과를 두며, 결과 화면에는 설명 주석을 붙일 수 있다.[1]

다음에는 커서 크기와 움직임, 배경 여백, 웹캠 위치를 미리보기에서 조정한다.
중간 작업은 `.recordly`로 저장하고, 전달할 때는 MP4 또는 GIF를 선택한다.
GIF에는 프레임 수·반복·크기 설정이 있으며, MP4는 일반 영상 출력 경로로 소개된다.
여기서 결과물은 편집 프로젝트와 내보낸 영상이라는 서로 다른 산출물이다.[1]

사람은 자동 확대가 정말 중요한 버튼을 보여주는지, 효과 때문에 설명할 글자가 화면 밖으로 나가지 않는지, 불필요한 대기 구간이나 알림이 남지 않았는지 확인해야 한다.
마이크를 선택했다면 의도하지 않은 주변 음성, 화면을 녹화했다면 계정 정보나 비공개 알림도 공개 전 검토 대상이다.
이 사례는 문서의 기능을 연결한 설명이지 이 원고에서 실제로 녹화·편집·내보내기를 수행한 경험은 아니다.

## 권한 확인은 녹화 범위 확인과 별개다

대표 권한 구현에는 macOS의 화면 녹화 상태를 조회하고 시스템 설정을 여는 처리, 접근성 권한의 신뢰 상태를 조회하거나 요청하는 처리가 있다.
화면을 선택하는 앱 UI와 운영체제가 허용하는 접근 권한은 다른 층이므로, 필요한 권한과 캡처 대상을 각각 확인해야 한다.[2]

앱 내부의 `permissionPolicy.ts`도 모든 페이지에 미디어 접근을 허용하는 형태는 아니다.
신뢰된 캡처 창인지, 메인 프레임인지, 요청 문서와 출처가 맞는지를 확인하는 조건이 있다.
이는 읽은 코드의 허용 조건이지 앱 전체에 대한 보안 감사 결과는 아니다.
또 운영체제 권한을 얻었다는 사실은 타인의 화면·음성이나 재배포할 자료에 대한 사용 허락을 대신하지 않는다.[3]

플랫폼 차이도 편집 결과에 영향을 준다.
README는 macOS 14.0 이상, 네이티브 Windows 캡처에는 Windows 10 Build 19041 이상을 안내한다.
Linux에서는 현재 실제 커서를 숨기지 못하므로 스타일 커서까지 켜면 커서가 둘로 보일 수 있다.
Linux 시스템 오디오는 일반적으로 PipeWire가 필요하다고 설명하며, 각 환경에서 같은 녹화 품질이 나온다고 보장하지 않는다.[1]

## 파일 내보내기와 공유 링크의 경계

고정 커밋의 Cloud Sharing 문서는 내보내기 메뉴에서 공유 링크를 만들 때 임시 MP4를 렌더링하고 업로드한 뒤 메타데이터를 게시하는 흐름을 설명한다.
중요한 제약은 모든 빌드가 현재 로컬 개발용 `http://localhost:8787/api/upload`를 사용하며, 운영 서비스 연동은 아직 예정이라고 명시한 점이다.
따라서 “공유 링크 메뉴가 있다”를 “설치만 하면 운영 중인 클라우드에 바로 게시할 수 있다”로 바꾸어 설명할 수 없다.[4]

게시에는 Recordly 접근 토큰이 필요하다.
공유 서비스 설명에는 공개 링크를 가진 사람이 로그인 없이 시청하고 표시 이름으로 댓글을 남길 수 있다고 적혀 있다.
이 흐름은 로컬 파일 저장과 달리 영상·제목·설명 등을 서비스로 보내는 행위다.
공유 범위와 인증, 서비스 운영 조건을 별도로 확인해야 하며, 호스팅 비용과 운영 서비스의 현재 요금은 이 원고에서 검증하지 않았다.[4]

## 배포와 라이선스에서 확인할 점

루트 라이선스는 AGPLv3를 싣고 있다.
수정본·실행 파일 배포와 수정한 프로그램의 네트워크 제공에는 해당 소스 제공 조건을 검토해야 한다.
또 파일 맨 위의 프로젝트 자체 요약에는 Recordly 이름·브랜딩 사용 금지와 사용자 UI·저장소의 출처 표시 요구가 있다.
이 요약은 “전체 소스 공개”를 넓게 표현하므로 표준 AGPL 본문의 범위를 대체하는 법률 해석으로 받아들이지 말고, 재배포 전에 원문과 적용 범위를 함께 확인해야 한다.[5]

일부 구성요소의 라이선스도 구분된다.
Third-party notices는 공유 서비스에 포함된 Voom 유래 코드의 MIT 고지, Solar Icons의 CC BY 4.0, DM Sans의 SIL Open Font License를 기록한다.
공유 서비스 일부가 MIT라는 설명을 Recordly 전체가 MIT라는 뜻으로 옮겨서는 안 된다.[6]

## 직접 읽어볼 자료

1. [루트 README](https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/README.md)

   녹화 → 편집 → 내보내기 순서와 `.recordly`의 역할을 먼저 읽는다.
   기능 목록 뒤의 커서·시스템 오디오 제한까지 확인해야 플랫폼별 기대 범위를 정할 수 있다.
2. [권한 처리 구현](https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/electron/ipc/register/permissions.ts)

   화면 녹화와 접근성 상태를 따로 다루는 부분을 확인한다.
   설정 창을 열거나 권한을 요청하는 코드가 실제 사용자의 승인까지 보장하지 않는다는 점을 구분한다.
3. [Cloud Sharing 설명](https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/docs/cloud-sharing.md)

   공유 링크의 업로드 과정뿐 아니라 로컬 개발 주소 고정이라는 제약을 읽는다.
   게시자 인증과 공개 링크 시청자의 접근 조건도 서로 나누어 확인한다.
4. [라이선스 원문](https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/LICENSE.md)

   프로젝트 요약과 AGPL 본문, 기존 MIT 고지를 구분해서 읽는다.
   별도의 Third-party notices까지 확인하고 수정·재배포 조건을 판단해야 한다.

## 자료 확인 범위

2026-10-04 고정 커밋 `4fda917aff39729eb22e72bbb881c6b7123d4033`의 README, 권한 처리·정책 구현, 공유 문서, 라이선스와 제3자 고지를 읽었다.
설치·실행·성능 검증, 녹화 권한 변경, 업로드, 운영 서비스 접속 및 배포물 검증은 하지 않았다.

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

## Sources

[1] webadderallorg/Recordly — README.md

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/README.md>

[2] webadderallorg/Recordly — electron/ipc/register/permissions.ts

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/electron/ipc/register/permissions.ts>

[3] webadderallorg/Recordly — electron/permissionPolicy.ts

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/electron/permissionPolicy.ts>

[4] webadderallorg/Recordly — docs/cloud-sharing.md

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/docs/cloud-sharing.md>

[5] webadderallorg/Recordly — LICENSE.md

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/LICENSE.md>

[6] webadderallorg/Recordly — THIRD_PARTY_NOTICES.md

<https://github.com/webadderallorg/Recordly/blob/4fda917aff39729eb22e72bbb881c6b7123d4033/THIRD_PARTY_NOTICES.md>
