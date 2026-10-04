---
title: "supabase/supabase"
repository: "supabase/supabase"
url: "https://github.com/supabase/supabase"
category: "backend-infra"
created: "2026-09-27"
status: "draft"
star_reason: ""
tags:
  - "backend-infra"
  - "starred-draft"
---

# supabase/supabase

## ⭐ 내가 이 Repository를 Star한 이유

<!-- 사용자 작성 -->

## 화면 뒤에 필요한 기능을 PostgreSQL 중심으로 묶는다

Supabase는 PostgreSQL 데이터베이스를 중심으로 로그인, 데이터 접근 API, 파일 저장, 실시간 기능 등을 제공하는 개발 플랫폼이다.
백엔드란 앱의 화면 뒤에서 데이터를 보관하고 요청을 처리하는 부분인데, Supabase는 여기에 필요한 여러 공개 소프트웨어를 연결한다.
README는 Firebase와 비슷한 개발 경험을 목표로 삼지만, 기능이 일대일로 대응하는 제품은 아니라고 명시한다.[1]

할 일 목록을 만든다고 생각해 보자.
화면에 입력 칸과 추가 버튼을 놓는 것만으로는 다른 기기에서 목록을 다시 불러올 수 없다.
데이터를 저장할 곳, 누가 로그인했는지 구분할 방법, 다른 사람의 목록을 읽거나 지우지 못하게 하는 규칙이 필요하다.
Supabase의 공식 Todo 예제는 이 문제를 브라우저 화면과 데이터베이스 권한 정책으로 나누어 보여 준다.[2][3]

이 저장소를 단일 데이터베이스 엔진이나 JavaScript 라이브러리 하나로 이해해서는 안 된다.
README는 Postgres, PostgREST, 인증 서버, Storage 등 구성 요소를 설명하고 별도 저장소로 연결한다.
또한 호스팅 서비스 이용, 자체 운영, 로컬 개발을 구분한다.
`supabase/supabase`에는 Studio 관리 화면과 공식 문서·소개 사이트 같은 여러 앱도 함께 들어 있다.[1][4]

## 데이터를 저장하는 것과 접근을 허용하는 것은 다르다

### 테이블을 앱에서 읽고 쓸 수 있게 연결한다

PostgreSQL은 데이터를 테이블에 저장하는 관계형 데이터베이스다.
테이블은 행과 열로 구성된 표에 비유할 수 있고, 할 일 예제에서는 한 행이 할 일 하나에 해당한다.
PostgREST는 PostgreSQL 데이터베이스를 REST API로 노출하는 서버다.
API는 앱이 데이터를 요청하고 응답을 받기 위한 약속이므로, 화면 쪽 코드는 데이터베이스 내부를 직접 조작하는 대신 이 통로를 사용한다.[1][3]

공식 예제의 `TodoList.tsx`는 Supabase 클라이언트를 통해 `todos` 테이블을 선택하고, 조회·추가·수정·삭제를 요청한다.
이 코드에서는 화면에 표시할 목록을 React 상태에 보관한다.
데이터베이스에 저장된 행과 현재 화면에 나타난 목록은 같은 정보를 다루지만, 서로 다른 위치에 있는 데이터라는 점을 구분해야 한다.[5]

### 로그인은 신원 확인, RLS는 행마다 적용하는 출입 규칙이다

인증(authentication)은 요청한 사람이 누구인지 확인하는 과정이고, 권한 부여(authorization)는 그 사람이 무엇을 할 수 있는지 정하는 과정이다.
예제 설명에 따르면 로그인한 사용자에게는 역할과 사용자 식별자가 담긴 JWT가 발급된다.
JWT는 로그인 정보를 전달하는 토큰 형식이다.
여기에 PostgreSQL의 RLS(Row Level Security)를 결합하여 사용자가 접근할 행을 제한한다.[2]

RLS를 풀어 쓰면 ‘행 단위 보안’이다.
Todo 예제의 SQL은 RLS를 켜고, 현재 사용자를 나타내는 `auth.uid()`와 해당 할 일의 `user_id`가 일치하도록 정책을 설정한다.
새 행을 만들 때뿐 아니라 조회·수정·삭제에도 사용자별 정책이 있다.
화면에서 다른 사람의 항목을 숨기는 데 그치지 않고 데이터베이스가 접근 조건을 판단하도록 구성한 것이다.[3]

### 실시간 기능은 별도로 연결하는 기능이다

README의 Realtime 설명은 데이터베이스 변경을 받아 허가된 클라이언트에 WebSocket으로 전달하는 구조다.
WebSocket은 브라우저와 서버 사이에 연결을 유지하며 메시지를 주고받는 방식이다.
파일 저장을 맡는 Storage와 사용자 로그인을 맡는 인증 API도 별도 구성 요소다.
‘Supabase를 사용한다’고 해서 모든 앱이 이 기능들을 전부 쓰는 것은 아니다.[1]

실제로 이번에 읽은 `TodoList.tsx`는 처음에 목록을 조회하고, 추가나 삭제가 성공하면 화면 상태를 직접 바꾼다.
이 파일에는 변경 사항을 구독하는 Realtime 연결이 없다.
따라서 이 예제를 여러 사용자의 화면이 자동 동기화되는 사례로 설명하면 코드에서 확인한 범위를 넘는다.[5]

## 예시로 따라가는 흐름

공식 Next.js Todo 예제에서 입력 칸의 안내 문구인 `make coffee`를 따라가 보자.
다음은 예제 코드와 SQL에 근거한 설명이며, 앱을 직접 실행해 얻은 결과가 아니다.
예제는 로그인 세션에서 사용자 정보를 받고, 데이터베이스에는 `todos` 테이블과 사용자별 권한 정책이 준비된 상태를 전제로 한다.[2][5][3]

먼저 사용자가 할 일을 입력하고 Add 버튼을 누르면 화면의 제출 처리가 `addTodo`를 호출한다.
이 함수는 앞뒤 공백을 제거하고 빈 문자열인지 검사한다.
이후 `task`에 입력한 내용, `user_id`에 로그인한 사용자의 ID를 넣어 새 행을 요청한다.
삽입 뒤에는 저장한 행을 다시 반환받도록 `select().single()`을 연결해 둔다.[5]

데이터베이스 쪽에서는 요청한 사용자가 그 행의 소유자인지 삽입 정책으로 확인한다.
또한 테이블 정의에는 할 일 텍스트가 3자를 넘어야 한다는 조건이 있다.
프런트엔드, 즉 화면 쪽 코드는 빈 입력만 거르므로 한두 글자 입력은 화면 검사를 통과해도 데이터베이스의 길이 조건을 만족하지 못한다.
이처럼 입력 규칙과 권한 규칙을 서버 쪽에서도 적용해야 하는 이유를 작은 예제에서 볼 수 있다.[5][3]

저장이 성공하면 반환된 행을 현재 목록에 붙이고 입력 칸을 비우도록 작성되어 있다.
오류가 오면 메시지를 화면에 표시한다.
나중에 체크박스를 바꾸면 해당 행의 `is_complete` 값을 갱신하고, 반환된 값을 받아 체크 상태를 바꾼다.
삭제 버튼은 행의 ID를 지정해 삭제 요청을 보내고 성공 뒤 화면 목록에서 제외한다.[5]

특히 처음 목록을 가져오는 코드는 `select('*')`를 사용하며 화면 쪽에서 `user_id` 조건을 덧붙이지 않는다.
그렇다고 모든 사용자의 할 일이 보여도 된다는 뜻은 아니다.
SQL의 조회 정책이 현재 사용자와 행의 소유자를 대조하도록 되어 있기 때문이다.
이 지점이 API 호출과 RLS를 함께 읽어야 하는 이유다.[5][3]

사람이 확인할 부분도 여기서 나온다.
서로 다른 두 계정으로 상대방의 행을 조회·수정할 수 없는지, 짧은 입력이 거절될 때 안내가 적절한지, 저장 실패와 화면 표시가 어긋나지 않는지 확인할 필요가 있다.
이는 예제에서 도출한 검증 항목이며, 이번 조사에서 통과 여부를 시험한 것은 아니다.

## 편리한 API가 보안 설계를 대신하지는 않는다

브라우저에서 API를 호출하기 쉬워질수록 어떤 키를 공개하는지와 어떤 정책을 적용하는지가 중요해진다.
예제 README는 클라이언트용 `anon` 키와 보안 정책을 우회할 수 있는 `secret` 키를 구분하며, 후자는 서버에만 두고 브라우저에 넣지 말라고 경고한다.
이 설명은 예제의 키 명칭을 따른 것이다.
현재 프로젝트 설정에 표시되는 키 종류와 권한을 확인하지 않고 이름만 보고 복사해서는 안 된다.[2]

RLS 역시 문서에 예제가 있다는 것과 자신의 테이블에 올바르게 설정했다는 것이 다르다.
이 Todo 정책은 각자 자기 할 일만 다루는 모델이다.
공유 목록이나 공동 편집처럼 여러 사람이 같은 행을 써야 한다면 소유자 한 명을 비교하는 규칙만으로 요구사항을 설명할 수 없다.
앱이 허용해야 하는 행동을 먼저 정하고 이에 맞는 정책을 설계해야 한다.[3]

플랫폼을 사용하는 것과 플랫폼의 소스를 개발하는 것도 구분해야 한다.
`DEVELOPERS.md`의 Node.js·pnpm·make 등의 준비와 Studio용 Docker 실행은 이 저장소의 개발 환경을 위한 안내다.
호스팅된 Supabase에 연결하는 작은 앱을 만들기 위해 반드시 저장소 전체를 빌드해야 한다는 뜻은 아니다.[1][4]

호스팅 이용과 자체 운영 중 무엇을 선택하든, 이 원고만으로 비용·운영 안정성·백업 체계를 판단할 수는 없다.
확인한 자료는 기능의 연결과 예제의 데이터 접근 방식에 관한 것이며, 요금제와 운영 조건은 별도 확인 대상이다.

## 직접 읽어볼 자료

1. [공식 README](https://github.com/supabase/supabase/blob/master/README.md)

   How it works에서 Postgres·PostgREST·인증·Realtime의 역할을 나누어 읽는다.
   기능 이름보다 어떤 요청을 어느 구성 요소가 처리하는지에 초점을 맞춘다.[1]

2. [Next.js Todo 예제 안내](https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/README.md)

   Postgres Row level security 설명과 키 주의사항을 함께 읽는다.
   로그인한 사용자 정보가 데이터 접근 정책으로 이어지는 연결을 확인할 수 있다.[2]

3. [TodoList.tsx](https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/components/TodoList.tsx)

   `addTodo`, `fetchTodos`, `toggle` 순으로 요청과 화면 갱신을 따라간다.
   데이터 반환을 기다리는 곳과 오류를 처리하는 곳도 함께 본다.[5]

4. [테이블과 RLS를 정의하는 SQL](https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/supabase/migrations/20230712094349_init.sql)

   `user_id`, 글자 수 조건, `auth.uid()`를 비교한다.
   화면 코드에 보이지 않는 접근 제한이 어디에 있는지 보여 주는 짧은 파일이다.[3]

5. [저장소 개발 안내](https://github.com/supabase/supabase/blob/master/DEVELOPERS.md)

   앱별 디렉터리 표와 Studio의 Docker 요구사항을 읽는다.
   Supabase를 이용하는 앱 개발과 Supabase 자체 개발 환경을 구분하는 데 도움이 된다.[4]

## 정리

Supabase는 데이터베이스 주변에 반복해서 필요한 백엔드 기능을 연결해 제공한다.
Todo 예제에서 중요한 연결은 ‘버튼 → API 요청 → 데이터베이스 정책 → 반환된 행 → 화면 갱신’이다.
이 구조를 이해하면 API를 적게 작성하는 편리함과 데이터 접근 규칙을 정확히 설계해야 하는 책임을 함께 볼 수 있다.[1][5][3]

## 자료 확인 범위

2026-09-27 기준 공식 README, 루트 구성, 개발 안내, Next.js Todo 예제 README·화면 코드·초기 SQL을 읽었다.
앱 설치·실행, Supabase 프로젝트 생성, 계정별 RLS 시험, 실시간 동기화나 성능 검증은 하지 않았다.

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

[1] supabase/supabase — README.md

<https://github.com/supabase/supabase/blob/master/README.md>

[2] supabase/supabase — examples/todo-list/nextjs-todo-list/README.md

<https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/README.md>

[3] supabase/supabase — examples/todo-list/nextjs-todo-list/supabase/migrations/20230712094349_init.sql

<https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/supabase/migrations/20230712094349_init.sql>

[4] supabase/supabase — DEVELOPERS.md

<https://github.com/supabase/supabase/blob/master/DEVELOPERS.md>

[5] supabase/supabase — examples/todo-list/nextjs-todo-list/components/TodoList.tsx

<https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/components/TodoList.tsx>
