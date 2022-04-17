# Django



## Django란?

> Python 언어 기반의 Web Framework



### Web Framework

- 재사용할 수 있는 수많은 코드를 통합하여 개발자가 새로운 애플리케이션을 위한 표준 코드를 다시 작성하지 않을 수 있도록 도와줌
- 데이터베이스 연동
- 템플릿 형태의 표준
- 세션 관리
- 코드 재사용 등 지원



### MTV Pattern

> Model, Template, View

#### Model

- 데이터 구조 정의
- 데이터베이스의 기록을 관리(추가, 수정, 삭제)



#### Template

- 파일의 구조, 레이아웃
- 실제 내용을 보여주는 역할



#### View

- HTTP 요청 수신 및 응답을 반환
- Model을 통해 필요한 데이터에 접근

- template에게 응답의 서식 설정을 맡김



## Django Intro



### 시작하기

- 가상환경 생성 및 활성화

  ```bash
  $ python -m venv venv
  $ source venv/Scripts/activate
  $ pip install django==3.2.12
  #or
  $ pip install -r requirements.txt
  ```

- 프로젝트 생성

  ```bash
  $ django-admin startproject first .
  ```

  마지막에 한칸 띄우고 . 을 찍어야 현재 위치에서 project가 생성되고, 찍지 않으면 폴더가 하나 더 생김

- 서버 활성화하기

  ```bash
  $ python manage.py runserver
  ```



### 앱 생성 및 등록

- 앱 생성하기

  ```bash
  $ python manage.py startapp {app_name}
  ```

- 앱 등록하기

  - project의 `settings.py`
  - `INSTALLED_APPS`에 app 추가

- 반드시 앱을 생성하고 등록



### 요청과 응답



#### URLs

- HTTP 요청(request)을 알맞은 `view`로 전달

  ```python
  from django.contrib import admin
  from django.urls import path
  from articles import views
  
  urlpatterns = [
      path('admin/', admin.site.urls),
      path('index/', views.index),
  ]
  ```

  `~index/`로 요청이 들어오면 `views.py`의 `index`함수로 전달함. app이 많아지게 되면 하나의 `urls.py`에서 관리하기 복잡해지기 때문에 app별로 관리하게 된다. 이때 app마다 `urls.py`를 만들어주고, 프로젝트 디렉토리의 `urls.py`에서는 각 `app`의 `urls.py`로 전달하는 역할을 한다.



#### View

- HTTP 요청을 수신하여 응답을 반환하는 함수 작성

  ```python
  from django.shortcuts import render
  
  def index(request):
      return render(request, 'index.html')
  ```

  `urls.py`를 통해 `views.py`의 `index`함수가 요청(`request`)을 전달받고 위 함수에 따르면 그 요청을 받아 `index.html` 페이지를 렌더링함



#### Templates

- 실제 내용을 보여주는 파일
- 일반적으로 `app`내에 `templates`디렉토리를 만들고 다시 그 안에 `app`의 이름과 같은 디렉토리를 만들어 그 안에서 `templates`파일을 생성한다.
  `articles/templates/articles/index.html`





## Template



### Django Template

- 데이터 표현을 제어하는 도구
- 표현에 관련된 로직
- DTL 사용



### Django Template Language(DTL)

- 조건, 반복, 변수, 치환, 필터 등 다양한 기능 제공
- 표현을 위한 것이지 프로그래밍적 로직은 아님
- Python과 유사해보이지만 Python 코드로 실행되는 것은 아님



### DTL Syntax



#### Variable

- `render()`를 통해 `views.py`에서 변수를 정의하여 `template`으로 넘겨서 사용 가능
- `{{ variable }}`과 같은 형태로 `template`에서 사용
- 변수명 다음에는 dot(.)을 사용하여 변수의 속성에 접근 가능
- `views.py`의 함수에서 `render()`의 세번째 인자로 `{'key': value}`와 같이 딕셔너리의 형태로 넘겨주고, 여기서 `key`에 해당하는 문자열이 사용 가능한 변수명이 됨



#### Filters

- 표시할 변수를 수정하거나 포맷을 변경하는 등의 기능을 수행
- 공식문서를 참고하여 사용하자



#### Tags

- 반복, 논리 등 변수보다 복잡한 일들을 수행
- `if`, `for` 등 약 24개의 `tags`를 제공함



#### Comments

- 주석
- `{# ... #}`으로 한줄 주석
- `{% comment %}`, `{% endcomment %}`로 여러줄 주석



### 코드 작성순서

> `urls.py > views.py > templates`



### Template inheritance

- 템플릿 상속

- 각 template을 작성하는데 동일한 코드가 계속 사용되므로, 중복된 부분을 하나로 만든 스켈레톤 코드를 상속하여 사용함

- 보통 `base.html`이라는 이름의 파일로 만들고 그 안에 하위 템플릿에서 재지정할 수 있는 블록을 정의함

- `base.html`은 프로젝트 디렉토리와 각 app 폴더와 동일한 레벨에 `templates` 폴더를 만들고 그 안에 `base.html`을 생성하여 사용

- 그 후 `settings.py`의 `TEMPLATES > 'DIRS'`에 `[BASE_DIR / 'templates']`를 지정

- 하위 템플릿은 아래와 같은 형식으로 상속받아 사용하게 됨

  ```html
  {% extends 'base.html' %}
  
  {% block content %}
  ...
  {% endblock %}
  ```

- 또한 `include` 태그도 있는데, 템플릿 내에 다른 템플릿을 포함시킬 때 사용

  ```html
  {% include '__nav.html' %}
  ```

  위처럼 사용한다

  

## HTML Form

### "form" element

- 사용자 정보를 입력하는 여러방식을 제공하고, 입력받은 데이터를 서버로 전송하는 역할
- 웹사이트에서 게시글을 작성하거나 회원가입 하는 페이지를 생각하면 좋다.

- 핵심 속성(attribute)
  - `action`: 입력 데이터가 전송될 URL
  - `method`: 데이터 전달 방식



### "input" element

- 사용자로부터 데이터를 입력받기 위해 사용
- type 속성에 따라 동작 방식이 달라짐
- 핵심속성
  - `name`



### label

- `input`요소와의 연결



### for

- for 속성의 값과 일치하는 id를 가진 문서의 첫번째 요소를 제어



### HTTP

- `request methods`를 정의
- `GET, POST, PUT, DELETE ... ` 등 다양함



### GET

- 정보를 조회할 때 사용
- 데이터를 전송할 때 `Query String Parameters`를 통해 전송하므로 URL에 전송하는 데이터가 노출됨



---

- 3월 2일자 진행중
- 

