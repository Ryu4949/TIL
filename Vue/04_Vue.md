# Vue_04

## Server & Client

### Server

- 클라이언트에게 '정보', '서비스'를 제공하는 컴퓨터 시스템
- 여기서 정보나 서비스는 Django를 통해 응답한 template, DRF를 통해 응답한 JSON 등이 해당한다고 할 수 있다.

### Client

- 서버에게 서비스를 요청하고, 서비스 요청을 위해 필요한 인자를 서버의 요구에 맞게 제공하며
- 서버로부터 반환되는 응답을 사용자에게 적절한 방식으로 표현하는 기능을 가진 시스템



## CORS

### Same-origin policy(SOP)

- 동일 출처 정책
- 특정 출처(origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용 하는 것을 제한하는 보안 방식
- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임

### Origin(출처)

- 두 URL의 Protocol, Port, Host가 모두 같아야 동일한 출처

### Cross-Origin Resource Sharing(CORS)

- :left_right_arrow: SOP
- 교차 출처 리소스(자원) 공유
- 추가 HTTP header를 사용하여, 특정 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
- 리소스가 자신의 출처(Domain, Protocol, Port)와 다를 때 교차 출처 HTTP 요청을 실행
- 보안 상의 이유로 브라우저는 교차 출처 HTTP 요청을 제한(SOP)
- 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS header를 포함한 응답을 반환해야 함

### 교차 출처 접근 허용하기

- CORS 활용
- CORS는 HTTP의 일부로, 어떤 호스트에서 자신의 컨텐츠를 불러갈 수 있는지 서버에 지정할 수 있는 방법

### Why CORS?

- 브라우저 & 웹 애플리케이션 보호
  - 악의적인 사이트의 데이터를 가져오지 않도록 사전 차단
  - 응답으로 받는 자원에 대한 최소한의 검증
  - 서버는 정상적으로 응답하지만 브라우저에서 차단
- Server의 자원 관리
  - 누가 해당 리소스에 접근할 수 있는지 관리 가능

### How?

- CORS 표준에 의해 추가된 HTTP Header를 통해 통제
- Access-Control-Allow-Origin 응답 헤더

### Access-Control-Allow-Origin 응답 헤더

- 해당 응답이 주어진 출처(origin)로부터 요청 코드와 공유될 수 있는지를 나타냄
- 프레임워크 별로 이를 지원하는 라이브러리가 존재함
  - Django의 경우 `django-cors-headers` 라이브러리를 통해 응답 헤더 및 추가 설정 가능

### `django-cors-headers` 라이브러리

- 응답에 CORS header를 추가해주는 라이브러리
- 다른 출처에서 보내는 Django 애플리케이션에 대한 브라우저 내 요청을 허용함
- Django App이 header 정보에 CORS를 설정한 상태로 응답을 줄 수 있게 도와주며, 이 설정을 통해 브라우저는 다른 origin에서 요청을 보내는 것이 가능
- 설치
  - `pip install django-cors-headers`
  - `INSTALLED_APPS`에 추가
  - `MIDDLEWARE` 추가
    - 순서 중요
  - `CORS_ALLOWED_ORIGINS`에 교차 출처 자원 공유를 허용할 Domain 등록



## Authentication & Authorization

### Authentication

- 자신이라고 주장하는 사용자가 누구인지 확인하는 행위
  - 인증, 입증
- 모든 보안 프로세스의 첫번째 단계이자 가장 기본 요소
- 401 Unauthorized는 표현상 '미승인'이지만 의미상 '비인증'을 의미

### Authorization

- 사용자에게 특정 리소스 또는 기능에 대한 액세스 권한을 부여하는 과정(절차)
  - 권한 부여, 허가
- 보안 환경에서 권한 부여는 항상 인증을 따라야 함
- 403 Forbidden
  - 401과의 차이점은 403의 경우 서버는 클라이언트가 누구인지 알고 있다는 것

### Authentication and authorization work together

- 보통 인증 이후에 권한이 따라오는 경우가 많음
- 하지만 모든 인증을 거쳤더라도 권한이 동일하게 부여되는 것은 아님
  - 로그인을 하더라도 다른 사람의 글을 수정/삭제 할 수 없다는 것을 예로 들 수 있음
- 세션, 토큰, 제 3자를 활용하는 등의 다양한 인증 방식 존재



## DRF Authentication

### 다양한 인증방식

- Session based
- Token based
  - Basic Token
  - JWT
- Oauth
  - google
  - facebook
  - github

### Basic Token Based

- 클라이언트가 로그인을 하면 해당 정보가 서버에 전송
- 서버는 해당 정보를 확인하여 일치하는 경우 token 테이블에 저장
- 클라이언트에 token 값 응답
- 클라이언트는 브라우저에 token 정보 저장
- Request Header에 토큰과 함께 요청
- 서버는 token값을 token 테이블에서 확인 후 로그인
- 응답

### JWT

- JSON Web Token
- JSON 포맷을 활용하여 요소 간 안전하게 정보를 교환하기 위한 표준 포맷
- 암호화 알고리즘에 의한 디지털 서명이 되어있어 그 자체로 검증 가능 

### JWT의 특징

- 데이터베이스에서 유효성 검사가 필요 없음
  - 세션 or 기본 토큰을 기반으로 한 인증과의 핵심 차이점
- 토큰 탈취시 서버 측에서 토큰 무효화가 불가능
- 매우 짧은 유효기간(5min)과 Refresh 토큰을 활용하여 구현
- MSA 구조에서 서버간 인증에 활용
- One Source(JWT) Multi Use 가능

### `dj-rest-auth` & `django-allauth` 라이브러리

```bash
$ pip install django-allauth
$ pip install dj-rest-auth
```

