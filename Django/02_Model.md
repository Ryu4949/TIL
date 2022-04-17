# Model



## Model

### Model

- 단일한 데이터에 대한 정보
  - 필드와 동작을 포함
- django는 model을 통해 데이터에 접속하고 관리함
- 일반적으로 각각의 model은 하나의 데이터베이스 테이블에 매핑됨



### Database

- 체계화된 데이터의 모임
- 쿼리(Query)를 사용하여 DB를 조작



#### DB의 기본구조

- 스키마(Schema): 자료의 구조, 표현방법, 관계 등을 정의
- 테이블(Table)
  - 열(column): 필드(field), 속성
  - 행(row): 레코드(record), 튜플



### 정리

- `Model`은 데이터를 **구조화**하고 **조작**하기 위한 도구







## ORM



### ORM

- Object-Relational-Mapping
- 데이터베이스와 객체지향프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 기법



### 장단점

#### 장점

- SQL을 잘 알지 못해도 DB 조작이 가능
- 높은 생산성

#### 단점

- ORM 만으로 완전한 구현이 어려운 경우가 있음



### model.py 작성

- 각 모델은 `django.models.Model` 클래스의 서브 클래스로 표현

  ```python
  from django.db import models
  
  class Article(models.Model):
      pass
  ```

- 어떤 타입의 컬럼을 정의할 것인지 정의. 여기서는 `title`과 `content`를 정의한다고 하자

  ```python
  from django.db import models
  
  class Article(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
  ```

- 문자열 필드, 숫자 필드, 날짜 필드 등 다양하며 공식 문서를 활용하여 적절한 필드를 사용하자







## Migrations



### Migrations

- `Model`에 생긴 변화를 반영하는 방법

- 관련 명령어

  - `makemigrations`: `model`을 변경했을 때 새로운 마이그레이션을 만들 때 사
  - `migrate`: 새로 만든 마이그레이션을 DB에 반영
  - `sqlmigrate`: 마이그레이션에 대한 SQL 구문 확인
  - `showmigrations`: 전체 마이그레이션 상태를 확인

- 예를 들어서 위의 `Article` `model`에 생성시간과 수정시간 필드를 포함하면

  ```python
  from django.db import models
  
  class Article(models.Model):
      title = models.CharField(max_length=10)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  ```

  이렇게 수정할 수 있고, 이렇게 `Model`을 수정해주었다면

  ```bash
  $ python manage.py makemigrations
  $ python manage.py migrate
  ```

  명령어를 통해 DB에 Model의 변화를 반영한다.







## Database API



### DB API

- DB를 조작하기 위한 도구
- Django에서는 기본적으로 ORM을 제공
- Class + Manager + QuerySet API로 구성
  `Article.objects.all()`
- Manager
  - Django 모델에 데이터베이스 query 작업이 제공되는 인터페이스
  - 기본적으로 Django 모든 모델 클래스에 `objects`라는 Manager 추가
- QuerySet
  - 데이터베이스로부터 전달받은 객체의 목록
  - 조회, 필터, 정렬 등을 수행할 수 있음



### Django  shell

- shell_plus를 사용

- 이를 위해서는

  ```bash
  $ pip install ipython
  $ pip install django-extensions
  
  # INSTALLED_APPS에 'django-extensions' 등록 후
  
  $ python manage.py shell_plus
  ```

  





## CRUD

- 3월 8일 슬라이드 52p~
- QuerySetAPI
- https://docs.Djangoproject.com/en/3.2/ref/models/querysets/#queryset-api-reference
- 



- 

- redirect에는 url을 쓰고 render에는 `aritlces/index.html`이렇게 쓰는 이유?

- (88p)create 한다음에

  - `return render(request, 'articles/index.html')`: 안됨

  - `reutrn redirect('articles:index')`: 됨

  - ```python
    ...
    article=form.save()
    articles = Article.objects.all()
    return render(request, 'articles/index.html', {'articles': articles})
    ```

    : 됨

  - 결국 `render`와 `redirect`가 뭐가 다른지 잘 모르겠다 

  - `create`할 때 첫번째꺼가 안되는건 그냥 template에서 받을 `form`이 없기 때문 아냐?

  - 결과적으로는 같은거 같은데??

- (96p)detail도 마찬가지

  - `return redirect('articles:detail', article.pk)`: 됨

  - ```python
    article = form.save()
    return render(request, 'articles/detail.html', {'article': article})
    ```

    :됨

    근데 차이가 있다면 위에꺼는 url이 `articles/<pk>/`인데 밑에꺼는 계속 `articles/create`로 남아있음

  - 