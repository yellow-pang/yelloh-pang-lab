---
title: "gitroomhq/postiz-app"
repository: "gitroomhq/postiz-app"
url: "https://github.com/gitroomhq/postiz-app"
category: "backend-infra"
created: "2026-10-28"
status: "draft"
star_reason: ""
tags: ["backend-infra", "starred-draft"]
---

# gitroomhq/postiz-app

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 한 줄 요약

여러 소셜 채널의 글 작성·예약 발행·분석을 묶은 셀프호스팅 소셜 미디어 관리 앱이다.[1]

## 글을 쓰는 일과 발행하는 일을 분리한다

Postiz는 여러 소셜 채널의 예약 발행, 분석, 협업과 API 자동화를 제공하는 웹 앱이다.
클라우드 서비스와 직접 운영하는 오픈소스 판이 있으며, 이 저장소는 후자의 소스다.
AI 작성 기능과 에이전트 연결도 소개하지만, 기본 역할은 콘텐츠와 채널별 발행 일정을 관리하는 것이다.[1]

같은 소식을 여러 플랫폼에 올리려면 본문·이미지와 시간뿐 아니라 플랫폼별 형식 제한도 맞춰야 한다.
한 화면에서 계획해도 실제 게시 요청은 각 채널의 승인된 계정과 API를 거친다.
README는 직접 운영할 때 플랫폼별 개발자 앱을 만들고 승인을 받아야 할 수 있다고 명시한다.[1]

## 저장 상태와 예약 작업을 연결하는 구현

대표 파일 `posts.service.ts`는 NestJS 서비스로 게시물 저장소, 채널 통합 관리자와 Temporal 서비스를 받는다.
Temporal은 오래 걸리거나 나중에 실행할 작업의 흐름을 관리하는 기반 도구다.
게시물 저장과 외부 채널에 실제로 발행하는 일을 분리하는 구조를 읽을 수 있다.[2]

`createPost`는 본문에 따라 지금 발행할 시간이나 예약 시간을 정해 저장소에 생성·수정을 요청한다.
일반 수정이 아닌 경우 워크플로를 시작하며, 워크플로 시작 경로는 초안 상태인 `DRAFT`이면 실행을 멈춘다.
즉 초안을 저장하는 것과 게시 작업을 예약하는 것은 코드에서도 서로 다른 상태다.[2]

이미 게시된 글을 예약·즉시 발행 방식으로 다시 저장하면 재발행될 수 있어 별도 보호 로직이 있다.
명시적인 재발행 의사가 없으면 수정 방식으로 바꾸도록 오류를 안내한다.
화면에서 비슷하게 보이는 저장 동작이 외부 공개라는 부작용을 낼 수 있음을 드러내는 부분이다.[2]

## 가상 사례: 개인 프로젝트 업데이트 예약

입력은 공개 개인 프로젝트의 업데이트 본문, 이미지, 연결된 두 채널과 희망 게시 시각이라고 가정한다.
먼저 초안으로 저장해 채널별 본문과 미디어 제한을 검토하고, 승인한 내용만 예약 상태로 전환한다.
저장된 게시물 식별자를 기반으로 발행 워크플로가 시작되는 흐름이다.[1][2]

예상 결과는 캘린더의 예약 항목과 이후 채널별 발행 결과다.
이 조사에서는 계정을 연결하거나 게시물을 만들지 않았으므로 실제 발행 성공을 확인한 것은 아니다.
작업 등록 성공과 외부 플랫폼 게시 성공은 다르며, 변경할 때는 수정과 재발행을 구분해야 한다.

## 셀프호스팅이 없애지 않는 비용

README의 기술 구성은 Next.js, NestJS, Prisma·PostgreSQL과 Temporal을 포함한다.
직접 운영 안내는 데이터베이스·Redis·저장소·환경 변수 구성, 업데이트·백업·보안 패치의 책임을 사용자에게 둔다.
자체 서버에 설치했다고 외부 소셜 플랫폼의 권한 승인이나 API 제한까지 없어지지는 않는다.[1]

AI 기능은 제공자 키와 비용이 추가로 필요할 수 있다.
또한 소셜 채널 발행 자체가 외부 서비스와 통신하므로, “자체 환경에 데이터 보관”이라는 안내를 모든 데이터의 외부 전송이 없다는 뜻으로 읽으면 안 된다.[1]

소스 라이선스는 AGPL-3.0이다.
특히 수정한 네트워크 서비스 제공 시 소스 제공 의무와 관계되는 조건을 포함하므로, 배포·서비스 제공 방식에 맞춰 원문을 확인해야 한다.[3]
프로젝트 전체의 보안이나 모든 채널의 지원 상태는 이 대표 서비스 파일만으로 검증할 수 없다.

## 정리

Postiz의 핵심은 글 생성보다 초안·예약·발행을 채널 계정과 작업 실행기로 연결하는 데 있다.
직접 운영의 제어권을 얻는 대신 인프라 유지와 외부 플랫폼 승인, 공개 게시의 책임도 함께 맡게 된다.

## 직접 읽어볼 자료

1. [README의 Cloud 비교](https://github.com/gitroomhq/postiz-app/blob/main/README.md)
   호스팅 방식보다 개발자 앱 승인과 운영 책임의 차이를 읽는다.
2. [게시물 서비스](https://github.com/gitroomhq/postiz-app/blob/main/libraries/nestjs-libraries/src/database/prisma/posts/posts.service.ts)
   초안의 조기 반환과 이미 발행된 글의 재발행 보호 조건을 확인한다.

## 자료 확인 범위

2026-10-28 공식 README·루트 트리와 위 대표 문서·코드를 읽었다.[4]
설치·실행·외부 계정 연결은 하지 않았으며, 예시는 직접 사용 경험이 아닌 가상 흐름이다.

## 나에게 어떤 가치가 있는가?

<!-- 사용자 작성 -->

## 개인 프로젝트 활용 가능성

<!-- 사용자 작성 -->

## ⭐ Star 가치

<!-- 사용자 작성 -->

## 나중에 할 일

<!-- 사용자 작성 -->

## Sources

[1] gitroomhq/postiz-app — README.md

<https://github.com/gitroomhq/postiz-app/blob/main/README.md>

[2] gitroomhq/postiz-app — libraries/nestjs-libraries/src/database/prisma/posts/posts.service.ts

<https://github.com/gitroomhq/postiz-app/blob/main/libraries/nestjs-libraries/src/database/prisma/posts/posts.service.ts>

[3] gitroomhq/postiz-app — LICENSE

<https://github.com/gitroomhq/postiz-app/blob/main/LICENSE>

[4] gitroomhq/postiz-app — 공식 루트 트리

<https://github.com/gitroomhq/postiz-app/tree/main>
