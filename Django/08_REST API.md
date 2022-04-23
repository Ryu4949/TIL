# REST API



[TOC]

## HTTP

### HTTP

- HyperText Transfer Protocol
- 웹 상에서 컨텐츠를 전송하기 위한 약속
- 웹에서 이루어지는 모든 데이터 교환의 기초가 되며 요청과 응답으로 이루어짐
  - 요청(request): 클라이언트에 의해 전송되는 메시지
  - 응답(response): 서버에서 응답으로 전송되는 메시지
- 기본특성은 비연결성(connectionless), 무상태(stateless)
  - 그래서 쿠키와 세션이 존재



### HTTP 메시지

#### 요청

- Method
- Path
- Version of the protocol
- Headers



#### 응답

- Version of the protocol
- Status Code
- Status Message
- Headers



### HTTP request methods

- 자원에 대한 행위(수행하고자 하는 동작)를 정의
- GET, POST, PUT, DELETE ...



### HTTP response status codes

#### 1. Informational reponses(1xx)

- 정보성 상태 코드라고 하며 정보를 전해주는 용도로 사용
- 대표적으로 100번의 경우 클라이언트가 보낸 요청에 이상이 없으니 다음 단계를 진행해도 괜찮음을 의미
- HTTP/1.1에 도입된 것으로, 복잡함을 감수할만큼 가치가 있는지에 대해 논란이 있다고 한다.



#### 2. Successful reponses (2xx)

- 200번대의 메시지는 요청이 성공적으로 전달되었음을 의미
- 대표적으로 200번 메시지는 요청이 성공했음을 나타내는 성공 응답 상태 코드



#### 3. Redirection messages(3xx)

- 클라이언트의 요청에 대해 적절한 다른 위치를 제공하거나, 대안의 응답을 제공



#### 4. Client error reponses (4xx)

- 클라이언트의 잘못된 요청에 대한 코드
- 400: 클라이언트가 올바르지 못한 요청을 보냄을 의미
- 401: 인증되지 않은 클라이언트
- 403: 클라이언트가 접근할 권한이 없음. 즉 클라이언트의 요청이 서버에 의해 거부되었음을 의미
- 404: 요청한 URL을 찾을 수 없음
- 405: 요청한 URL이 해당 Method를 지원하지 않음
- 이외에도 많은 상태코드가 있다.



#### 5. Server error responses (5xx)

- 클라이언트의 요청에는 문제가 없으나 서버의 문제로 인해 응답할 수 없음을 의미





### 웹에서의 리소스 식별

- HTTP 요청의 대상을 리소스(resource, 자원)라고 함
  - 문서, 사진 기타 등등
- 각 리소스는 URI로 식별됨



### URL, URN

#### URL

- Unifor Resource Locator, 통합 자원 위치
- 네트워크 상에 자원이 어디 있는지 알려주기 위한 약속
- 웹 주소, 링크라고도 함
- 과거의 URL은 실제 위치를 나타냈으나 현재의 URL은 추상적인 의미론적 구성
  - 예를 들어 네이버의 메인페이지를 과거에는 `www.naver.com/index.html` 과 같이 사용했으나, 현재는 `www.naver.com`으로만 입력해도 메인페이지를 볼 수 있다. 물론 지금도 `www.naver.com/index.html`과 같이 입력해도 볼 수 있기는 하다.



#### URN

- Uniform Resource Name, 통합 자원 이름
- 자원의 위치에 영향을 받지 않는 유일한 이름 역할
- 영속적, 유일. 위치에 독립적인 이름을 제공하기 위해 존재
- 예시
  - ISBN(국제표준도서번호)



### URI

#### 개념

- Uniform Resource Identifier, 통합 자원 식별자
- 인터넷의 자원을 식별하는 유일한 주소(정보의 자원을 표현)
- 인터넷에서 자원을 식별하거나 이름을 지정하는데 사용되는 간단한 문자열
- URL, URN의 상위개념
- URI는 크게 URL과 URN으로 구분할 수 있지만, URN을 사용하는 비중이 매우 적기 때문에 URL은 일반적으로 URI와 같은 의미로 사용하기도 함



#### 구조

##### Scheme(protocol)

- 브라우저가 사용해야 하는 프로토콜
- http(s), data, file, ftp, mailto



##### Host(Domain Name)

- 요청을 받는 웹 서버의 이름
- IP 주소를 사용해도 되지만 실사용시 불편하므로 자주 사용되지 않는다.
  - cmd를 열고 `nslookup {도메인}`을 입력하면 해당 웹 서버의 IP주소를 확인할 수 있다.



##### Port

- 웹 서버 상의 리소스에 접근하는 데 사용되는 기술적인 '문(gate)'
- HTTP 프로토콜의 표준 포트는 HTTP 80, HTTPS 443



##### Path

- 웹 서버 상의 리소스 경로
- 과거에는 실제 파일이 위치한 물리적 위치를 나타냈지만 현재는 추상화 형태의 구조로 표현함



##### Query(Identifier)

- Query String Parameter
- 웹 서버에 제공되는 추가적인 매개변수
- `&`로 구분되는 key-value 목록



##### Fragment

- Anchor
- 자원 내에서 북마크의 한 종류
- 브라우저에게 해당 문서의 특정 부분을 보여주기 위한 방법
- 브라우저에게 알려주는 요소이기 때문에 fragment identifier(부분 식별자)라고 하며, '#' 뒤의 부분은 요청이 서버에 보내지지 않음



##### 예시

- `https://www.example.com:80/path/to/myfile.html/?key=balue#quick-start/`





## RESTful API



### API

- Application Programming Interface
- 프로그래밍 언어가 제공하는 기능을 수행할 수 있게 만든 인터페이스
  - CLI는 명령줄, GUI는 그래픽, API는 프로그래밍을 통해 특정 기능 수행
- 예시
  - 카카오톡에서 메시지를 보낸다고 해보자[^1]
  - 카톡에는 메시지 전송 말고도 채팅방 생성, 친구 추가 등등 수많은 기능이 존재하는데, 이 모든 기능에 대해, 또 각 기능의 모든 요청에 대해 카톡 서버가 응답하는 것은 매우 비효율적
  - 그래서 각각의 요청들을 담당하는 서버에게 요청이 잘 전달 및 처리될 수 있도록 교통정리를 해주는 **체계**가 API
  - 즉 어떠한 방식으로 정보를 요청해야 하는지, 그러한 요청을 보냈을 때 어떠한 형식으로 무슨 데이터를 전달받을 수 있는지에 대해 정리한 일종의 규격이라고 볼 수 있음
- Web API
  - 웹 애플리케이션 개발에서 다른 서비스에 요청을 보내고 응답을 받기 위해 정의된 명세
  - 현재는 모든 것을 직접 개발하는 것보다는 여러 Open API를 활용하는 추세
- 응답 데이터 타입
  - HTML, XML, JSON

- 대표적인 API 서비스 목록
  - Youtube API, Naver Papago API, Kakao Map API



### REST

- REpresentational Stat Transfer
- API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론
- 네트워크 구조(Network Architecture) 원리의 모음
  - 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법
- REST 원리를 따르는 시스템을 RESTful 이라고 지칭함
- 자원을 정의하는 방법에 대한 고민

- 자원과 주소의 지정 방법
  - 자원: URI
  - 행위: HTTP Method
  - 표현: 자원과 행위를 통해 궁극적으로 표현되는 결과물. JSON으로 표현된 데이터를 제공
    - JSON(JavaScript Object Notation)
    - 특징: 사람이 읽거나 쓰기 쉽고, 기계가 파싱하고 만들어내기 쉬움
    - key-value 형태
- 핵심 규칙
  - 정보는 URI로 표현
  - 자원에 대한 행위는 HTTP Method로 표현
- 지켜야만 동작하는 것은 아니지만, 



### RESTful API

- REST 원리에 따라 설계한 API

- 프로그래밍을 통해 클라이언트의 요청에 JSON을 응답하는 서버를 구성







## Response



### 실습

- 생략



### Django REST Framework(DRF)

- Web API 구축을 위한 강력한 Toolkit 을 제공하는 라이브러리
- DRF의 Serializer는 Django의 Form 및 ModelForm 클래스와 매우 유사하게 구성되고 작동







## Single Model

### DRF with Single Model

- 단일 모델의 data를 직렬화(serialization)하여 JSON으로 변환하는 방법
- 단일 모델을두고 CRUD 로직을 수행가능하도록 설계
- DRF built-in form 과 Postman 활용
  - Postman: API 구축/사용하기 위해 여러 도구를 제공하는 API 플랫폼



### 실습

- 생략
- 













## 1:N Relation







---

- [^1]: 1 https://enjoyinjoanne.tistory.com/56 ↩

- api 이해할때까지 알아보기

- 