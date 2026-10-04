---
title: "yuliskov/SmartTube"
repository: "yuliskov/SmartTube"
url: "https://github.com/yuliskov/SmartTube"
category: "developer-tools"
created: "2026-09-14"
status: "draft"
star_reason: ""
tags:
  - "developer-tools"
  - "starred-draft"
---

# yuliskov/SmartTube

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

SmartTube는 Android TV와 TV 박스에서 공개 미디어를 탐색하고 재생하는 TV용 클라이언트다.
리모컨 중심 화면, 재생 속도 조정, 버튼 사용자화, SponsorBlock 연동 등을 제공한다.
스마트폰용 범용 동영상 앱이나 모든 스마트 TV 운영체제에서 실행되는 프로그램은 아니다.[1]

## 큰 화면과 리모컨에 맞춘 재생 도구

TV에서는 마우스로 작은 버튼을 고르기 어렵고, 휴대폰 화면의 조작 방식도 그대로 옮기기 어렵다.
SmartTube는 TV에 맞는 탐색과 재생 화면을 제공하며 Google Services가 필수는 아니라고 설명한다.
해상도·프레임률·HDR 지원도 소개하지만 이는 모든 기기와 영상 조합에서 최고 사양 재생을 보장한다는 뜻은 아니다.
README도 음성 검색·캐스팅 성능이 기기에 따라 공식 앱보다 떨어질 수 있다고 한계를 적는다.[1]

기기 이름보다 운영체제를 먼저 확인해야 한다.
README는 Android 기반 TV·박스를 대상으로 하며 Tizen·webOS·Apple TV 같은 비Android 플랫폼은 지원하지 않는다고 안내한다.
특히 새로운 FireTV 계열 중 VegaOS 기기는 호환되지 않는다는 별도 경고가 있다.
같은 제품군 이름이 붙어도 기반 운영체제가 달라지면 설치 가능 여부가 달라지는 사례다.[1]

## 구간 건너뛰기는 영상 분석이 아니라 자료 연동이다

SponsorBlock은 이용자들이 제출한 영상 구간 정보를 바탕으로 후원 구간이나 도입부 등을 건너뛰도록 돕는 외부 서비스다.
README는 SmartTube에서 건너뛸 범주를 선택할 수 있지만 새 구간을 제출할 수는 없다고 설명한다.
리모컨으로 정밀한 구간을 지정하기 어렵다는 이유도 적혀 있다.
알려진 구간을 이용하는 것이므로 앱이 모든 영상을 스스로 이해해서 표시하는 기능으로 보면 안 된다.[1]

대표 구현 `SponsorBlockController.java`에서는 영상이 로드될 때 기능 활성 여부, 영상 정보, 제외 채널 여부를 확인한다.
조건이 맞으면 영상 ID와 활성화한 범주로 구간 목록을 요청한다.
라이브 영상이거나 영상 ID가 없거나 선택 범주가 비어 있으면 요청 흐름을 중단한다.
기능 버튼을 켰다는 사실과 모든 영상에 실제 건너뛰기가 적용되는 상태는 다르다.[2]

받은 목록은 원본 구간과 활성 구간으로 보관된다.
색상 표식 설정이 켜져 있으면 재생 막대에 구간을 표시하고, 동작 설정이 켜져 있으면 재생 위치를 관찰하는 반복 처리를 시작한다.
재생 중인 위치와 구간을 맞춰 후속 동작을 적용하는 구조다.
영상 전환이나 플레이어 해제 때 기존 처리를 정리하는 코드도 있어 이전 영상의 구간 상태를 계속 사용하는 일을 피하도록 구성한다.[2]

## 예시로 따라가는 흐름

이해를 위한 가상 예시이며 직접 실행한 결과가 아니다.
이용 권한이 있는 녹화 영상을 TV에서 보면서 도입부만 건너뛰고 후원 구간은 그대로 보고 싶다고 하자.
입력은 영상 선택과 SponsorBlock의 범주 설정이다.
범주를 정하지 않고 '자동으로 불필요한 장면을 제거한다'고 기대하는 것과 달리, 실제 동작은 사용자가 선택한 종류와 외부에 등록된 자료에 의존한다.[1][2]

영상이 로드되면 컨트롤러는 제외 채널인지와 기능 활성 여부를 확인하고, 해당 영상 ID로 선택 범주의 구간을 조회한다.
구간이 비어 있거나 플레이어가 없으면 관찰 처리를 만들지 않는다.
자료가 있으면 표식 표시와 동작 설정을 각각 적용한다.
이후 현재 재생 위치가 대상 구간과 맞는지 확인하는 경로로 이어진다.
여기서는 어떤 시각에서 어느 시각으로 이동했다는 임의의 재생 결과를 만들지 않는다.[2]

시청자는 필요한 설명이 함께 건너뛰어지지 않았는지, 기대한 구간이 등록되어 있는지 확인해야 한다.
README는 이용자 제출 기반이라 누락이 있을 수 있고 외부 서버가 중단되거나 과부하일 수도 있다고 경고한다.
구간이 건너뛰어지지 않았다는 현상 하나만으로 앱 전체의 고장이라고 단정할 수 없는 이유다.
스마트폰 캐스팅 역시 같은 Wi-Fi에 있다고 자동 발견되는 방식이 아니라 TV 코드로 연결하고 TV의 앱을 먼저 열어야 한다는 별도 조건이 있다.[1]

## 배포물과 개인정보 문서를 구분해 읽기

조회한 README 맨 앞에는 개발 환경이 악성 소프트웨어에 감염되어 일부 빌드가 영향을 받았을 수 있다는 공지가 있다.
작성자는 환경 재구성과 빌드 검사, 키 관련 조치를 설명하고 내장 보안 기능을 유지하라고 안내한다.
이 공지는 실제 배포물 선택에 중요한 조건이며, 여기서 특정 APK가 안전하다고 독립 검증한 것은 아니다.
공지의 조치 설명을 모든 과거·현재 파일에 대한 안전 보증으로 바꾸어 읽어서는 안 된다.[1]

README에는 F-Droid 배지와 관련 공지가 있으면서 설치 절에는 앱 스토어에 공식 배포하지 않는다는 문장도 남아 있다.
문서 내 안내가 완전히 정합적이지 않은 부분이다.
따라서 배포 경로를 단정적으로 하나 추천하기보다 공식 공지와 선택한 배포물의 식별·검증 정보를 확인해야 한다.
보호 기능을 끄는 방식으로 설치 문제를 해결하도록 안내하지 않는다.[1]

`PRIVACY.md`는 명시적으로 F-Droid flavor, 즉 특정 빌드 변형에만 적용된다.
해당 빌드는 개발자 서버·분석 코드·자체 업데이트 기능을 사용하지 않는다고 설명하고, 다른 Stable·Beta 변형에는 다른 기능이 있을 수 있다고 구별한다.
SponsorBlock·DeArrow·RYD에 영상 ID가 전달되는 외부 요청도 적는다.
이를 읽고 SmartTube의 모든 빌드가 동일한 통신 정책을 갖는다고 일반화할 수 없다.[3]

## 직접 읽어볼 자료

- [공식 README](https://github.com/yuliskov/SmartTube/blob/master/README.md)
  기능 소개보다 상단 보안 공지와 Device support를 먼저 확인한다.
  SponsorBlock과 Casting 절에서는 자동으로 되는 부분과 범주 선택·코드 연결 등 사용자가 준비할 조건을 구분한다.
- [SponsorBlock 컨트롤러](https://github.com/yuliskov/SmartTube/blob/master/common/src/main/java/com/liskovsoft/smartyoutubetv2/common/app/models/playback/controllers/SponsorBlockController.java)
  `onVideoLoaded`, `updateSponsorSegmentsAndWatch`, `startSponsorWatcher` 순서로 읽으면 활성 조건·외부 자료 조회·재생 위치 관찰의 구분이 보인다.
  라이브 영상과 빈 구간 목록에서 빠져나오는 분기를 확인한다.
- [F-Droid 빌드 개인정보 정책](https://github.com/yuliskov/SmartTube/blob/master/PRIVACY.md)
  문서 첫 부분의 적용 범위를 먼저 읽고 외부 서비스에 보내는 자료를 살핀다.
  빌드 이름을 생략한 채 개인정보 특성을 설명하면 왜 부정확해지는지 확인할 수 있다.

## 정리

SmartTube는 Android 기반 TV에서 탐색과 재생을 조절하는 클라이언트다.
SponsorBlock은 외부 구간 자료와 사용자 설정을 연결하는 기능이며 완전한 자동 판별 장치가 아니다.
기기 운영체제, 배포물 보안 공지, 빌드별 개인정보 범위를 기능 소개와 함께 읽어야 한다.

## 자료 확인 범위

2026-09-27 기준 README의 공지·주요 기능·지원 조건, F-Droid 개인정보 정책과 SponsorBlock 대표 구현을 읽었다.
앱 설치·로그인·영상 재생은 하지 않았으며 APK의 악성 여부나 기기별 품질을 검증하지 않았다.

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

[1] yuliskov/SmartTube — README.md

<https://github.com/yuliskov/SmartTube/blob/master/README.md>

[2] yuliskov/SmartTube — common/src/main/java/com/liskovsoft/smartyoutubetv2/common/app/models/playback/controllers/SponsorBlockController.java

<https://github.com/yuliskov/SmartTube/blob/master/common/src/main/java/com/liskovsoft/smartyoutubetv2/common/app/models/playback/controllers/SponsorBlockController.java>

[3] yuliskov/SmartTube — PRIVACY.md

<https://github.com/yuliskov/SmartTube/blob/master/PRIVACY.md>
