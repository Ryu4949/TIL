# Authentication System

[TOC]



## The Django Authentication System



### Intro

- Django의 인증시스템은 `django.contrib.auth`에서 제공
- 필수 구성은 `settings.py`에 이미 포함되어 있고 `INSTALLED_APPS`에 나열된 `django.contrib.auth`와 `django.contrib.contenttypes`로 구성
- Django의 인증시스템은 **인증(Authentication)**과 **권한(Authorization)** 부여를 함께 처리하며, 이러한 기능들이 결합되어 인증 시스템이라고 함



### accounts 앱 생성하기

- `$ python manage.py startapp accounts`
- app 등록 및 url 설정





## 쿠키와 세션



### HTTP의 특징



#### 비연결지향(connectionless)

- 서버는 요청에 대한 응답을 보낸 후에는 연결을 끊음



#### 무상태(stateless)

- 연결을 끊으면 클라이언트와 서버 간의 통신이 끝나며 상태 정보가 유지되지 않음
- 클라이언트와 서버가 주고 받는 메세지들은 서로 완전히 독립적



#### 정리

- 클라이언트와 서버의 지속적인 관계를 유지하기 위해 쿠키와 세션이 존재



### 쿠키

#### 개념

- 서버가 사용자 측에 전송하는 데이터 조각
- 소프트웨어가 아니므로 실행시키거나 악성코드를 설치할 수는 없지만, 쿠키를 훔침으로써 사용자의 계정 접근 권한을 획득할 수도 있다는 점에서 주의가 필요
- HTTP 쿠키는 상태가 있는 세션을 만들어 줌
- 쿠키는 두 요청이 동일한 브라우저에서 들어왔는지 아닌지를 판단할 때 주로 사용



#### 요청과 응답

- 먼저 웹 브라우저에서 서버에 페이지를 요청하면, 서버에서는 웹페이지와 함께 쿠키를 보냄
- 추후에 브라우저에서 같은 서버에 다른 페이지를 요청할 때 쿠키를 함께 보내게 되고, 서버에서는 이때 받은 쿠키를 통해 요청이 동일한 브라우저에서 왔는지 확인



#### 쿠키의 사용 목적

- 세션 관리(Session management): 로그인, 아이디 자동완성, 공지하루 안보기, 장바구니 등등
- 개인화(Personalization): 사용자 선호, 테마 등 설정
- 트래킹(Tracking): 사용자 행동을 기록 및 분석



#### 쿠키의 수명(lifetime)

- 두가지 방법으로 정의 가능



##### Session cookies

- 현재 세션이 종료되면 삭제
- 현재 세션이 종료되는 시기는 브라우저가 정의

##### Persistent cookies(or Permanent cookies)

- Expires 속성 or Max-Age 속성에 지정된 기간이 지나면 삭제



### 세션

#### 개념

- 사이트와 특정 브라우저 사이의 상태(state)를 유지 시키는 것
- 클라이언트가 서버에 접속하면 서버는 특정 `session id`를 발급하고
- 클라이언트는 발급받은 `session id`를 쿠키에 저장



#### Session in Django

- 미들웨어를 통해 구현
- 기본 방식은 database-backed sessions
- Django는 특정 session id를 포함하는 쿠키를 사용해서 각각의 브라우저와 사이트가 연결된 세션을 알아냄
- 세션 정보는 Django DB의 `django_session` 테이블에 저장





## 로그인



### 로그인

- 로그인은 session을 create하는 로직과 같음
- 이를 위해 인증에 관한 built-in forms 제공



### Authentication Form

- 사용자 로그인을 위한 form
- `request`를 첫번째 인자로 취함



### login 함수

- `login(request, user, backend=None)`

- view 함수에서 사용됨

- HttpRequest 객체와 User 객체 필요

- Django의 session framework를 사용하여 세션에 user의 ID를 저장

  ```python
  from django.urls import path
  from . import views
  
  app_name = 'accounts'
  urlpatterns = [
      path('login/', views.login, name='login'),
  ]
  ```

  ```html
  {% extends 'base.html' %}
  
  {% block content %}
    <form action="{% url 'accounts:login' %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
  {% endblock %}
  ```

  ```python
  from django.shorcuts import render, redirect
  from django.contrib.auth import login as auth_login
  from django.contrib.auth.forms import AuthenticationForm
  from django.views.decorators.http import require_http_methods
  
  @require_http_methods(['GET', 'POST'])
  def login(request):
      if request.method == 'POST':
          form = AuthenticationForm(request, request.POST)
          if form.is_valid():
              auth_login(request, form.get_user())
              return redirect('articles:index')
      else:
          form = AuthenticationForm()
      context = {
          'form': form,
      }
      return render(request, 'accounts/login.html', context)
  ```

  

#### get_user()

- AuthenticationForm의 인스턴스 매서드
- 인스턴스 생성시에는 `None`으로 할당되고, 유효성 검사를 통과하면 로그인 한 사용자 객체로 할당됨





## Authentication data in templates



### user

- 템플릿에서 `user` 변수를 사용하면 현재 로그인되어있는 유저 정보 출력
- 로그인되어 있지 않다면 `AnonymousUser` 출력







## 로그 아웃



### 로그아웃

- 로그아웃은 session을 Delete하는 로직



### logout 함수

- `HttpRequest` 객체를 인자로 받고 반환값이 없음
- 사용자가 로그인하지 않은 경우 오류 X
- 현재 요청에 대한 session data를 DB에서 완전히 삭제하고, 클라이언트의 쿠키에서도 session id가 삭제
- 이는 다른 사람이 동일한 웹 브라우저를 사용하여 로그인하고 이전 사용자의 세션 데이터에 액세스하는 것을 방지하기 위함



### 구현

```python
path('logout/', views.logout, name='logout')
```

```html
<!--base.html-->
<body>
    <div class="container">
        <h3>Hello, {{ user }}</h3>
        <a href="{% url 'accounts:login' %}">Login</a>
        <form action="{% url 'accounts:logout' %}" method="POST">
            {% csrf_token %}
            <input type="submit" value="Logout">
        </form>
        {% block content %}
        {% endblock content %}
    </div>
</body>
```

```python
from django.views.decorators.http import require_http_methods, require_POST
from django.contrib.auth import logout as auth_logout

@require_POST
def logout(request):
    auth_logout(request)
    return redirect('articles:index')
```





## 로그인 사용자에 대한 접근 제한

> `is_authenticated attribute`, `login_required`



### is_authenticated 속성

- 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성

- AnonymousUser에 대해서는 항상 False

- 사용자가 인증되었는지 여부를 확인

- 템플릿에 사용해서 로그인과 비로그인 상태에서 출력되는 링크를 다르게 설정 가능

  ```html
  {% if request.user.is_authenticated %}
  <!--로그인 상태-->
  {% else %}
  <!--비로그인 상태-->
  {% endif %}
  ```

  ```python
  @require_http_methods(['GET', 'POST'])
  def login(request):
      if request.user.is_authenticated:	#인증된 사용자(로그인된 상태)라면 로그인 로직 수행 불가능 하도록
          return redirect('articles:index')
  ```

  ```python
  @require_POST
  def logout(request):
      if request.user.is_authenticated:
          auth_logout(request)
      return redirect('articles:index')
  ```





### login_required decorator

- 사용자가 로그인되어 있지 않으면 `settings.LOGIN_URL`에 설정된 문자열 기반 절대 경로로 redirect

- `LOGIN_URL`의 기본값은 `'accounts/login/'`

- 로그인 되어있으면 정상적으로 view 함수 실행

- 로그인되어있지 않으면 `accounts/login/`으로 redirect하고, 이때 인증 성공시 redirect 되어야 하는 경로는 `next`라는 쿼리 문자열 매개 변수에 저장됨.
  `/accounts/login/?next=/articles/create/`

- 사용 예시

  ```python
  from django.contrib.auth.decorators import login_required
  
  @login_required
  def my_view(request):
      pass
  ```



#### "next" query string parameter

- 로그인이 정상적으로 진행되면 기존에 요청한 주소로 redirect하기 위해 주소를 keep 하는 것

- 별도로 처리해주지 않으면 view에 설정한 경로로 redirect

  ```python
  @require_http_method(['GET', 'POST'])
  def login(request):
      if request.user.is_authenticated:
          return redirect('articles:index')
      
      if request.method == 'POST':
          form = AuthenticationForm(request, request.POST)
          if form.is_valid():
              auth_login(request, form.get_user())
              return redirect(request.GET.get('next') or 'articles:index')
      else:
          pass
  ```

  

#### 두 데코레이터로 인해 발생하는 구조적 문제와 해결

- `@login_required`와 `@require_POST`를 동시에 쓸 경우 에러 발생

- 예를 들어 두 데코레이터를 가진 게시글 삭제 기능을 생각해보자

  ```python
  @login_required
  @require_POST
  def delete(request, pk):
      article = get_object_or_404(Article, pk=pk)
      article.delete()
      return redirect('articles:index')
  ```

  - 비로그인 상태에서 삭제를 시도하면 `@login_required`로 인해 로그인 페이지로 이동
  - 로그인을 하면 `next` 매개변수를 따라 해당 함수로 다시 `redirect`되는데, 이건 `GET` 방식이라 `require_POST`로 인해 405 에러 발생
  - 이 해결을 위해서는

  ```python
  @require_POST
  def delete(request, pk):
      if request.user.is_authenticated:
          article = get_object_or_404(Article, pk=pk)
          article.delete()
      return redirect('articles:index')
  ```

  결국 `@login_required`는 `GET` 방식의 request를 처리할 수 있는 view 함수에서만 사용해야함









---

- 그러면 `is_authenticated`는 로그인/비로그인만 구분하는거? 로그인 상태여도 인증/비인증 이런건 없고?

- 이거 좀 잘.. return redirect(request.GET.get('next') or 'articles:index')