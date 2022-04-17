# Form/ModelForm



## Form



### Intro

- 기존에는 HTML form, input을 통해서 사용자의 데이터를 입력받음
- 이렇게 입력을 받으면 입력된 데이터의 유효성을 검증하고 필요시 데이터와 검증결과를 함께 다시 표시하게 됨
- 이러한 과정을 **유효성 검증**이라고 하는데 이 과정을 코드로 모두 구현하는 것은 많은 노력이 필요하기 때문에 이러한 많은 작업과 반복되는 코드를 줄일 수 있도록 지원하는 것이 Django의 `Form`
- Django가 지원하는 작업은
  - 렌더링을 위한 데이터 준비 및 재구성
  - 데이터에 대한 HTML forms 생성
  - 클라이언트로부터 받은 데이터 수신 및 처리



### Django 'Form Class'



#### 기능

- field
- field의 배치
- 디스플레이 widget
- label
- 초기값
- 유효하지 않은 field에 관련된 에러 메세지



### Form 선언하기

- Model 선언과 매우 유사하며 같은 필드 타입 사용

- forms 라이브러리에서 파생된 Form 클래스를 상속받음

  ```python
  from django import forms
  
  class ArticleForm(forms.Form):
      title = forms.CharField(max_length=10)
      content = forms.CharField()
  ```

  



### Form 사용하기

```python
from .forms import ArticleForm

def new(request):
    form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

```html
{% extends 'base.html' %}

{% block content %}
  <h1 class="text-center">NEW</h1>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <hr>
  <a href="{% url 'articles:index' %}">[back]</a>
{% endblock %}
```



### Widgets

- 웹 페이지의 HTML input 요소 렌더링/표현
  - button, text, textarea, email 등등등..
- GET/POST 딕셔너리에서 데이터 추출
- 반드시 From fields에 할당
- Form Fields와 혼동하지 말 것
  - Form fields는 input 유효성 검사를 처리

```python
from django import forms

class ArticleForm(forms.Form):
    REGION_A = "sl"
    REGION_B = "dj"
    REGION_C = "gj"
    REGION_CHOICES = [
        (REGION_A, '서울'),
        (REGION_B, '대전'),
        (REGION_C, '광주'),
    ]
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(choices=REGION_CHOICES, widget=forms.Select())
```

- 위의 경우 `content`와 `region`에 각각 `widget`이 적용되었음을 볼 수 있다.







## ModelForm



### Intro

- Django Form을 사용하다보면 Model에서 정의한 필드를 사용자로부터 입력받기 위해서 Form에서 같은 필드를 다시 정의하게 되어 중복된 행위 발생
- 그래서 Django는 Model을 통해 Form Class를 만들 수 있는 ModelForm 제공
- 예를 들어 위에서 `Article`이라는 Model은 title, content, created_at, updated_at 이라는 필드로 구성되어 있고, 뒤에 두개는 작성시, 수정시에 자동으로 데이터가 입력되는 필드이므로 사용자로부터 입력받게되는 필드는 title, content임.
- 이때 사용자가 입력할 Form을 만드는 과정에서 다시 title과 content라는 필드를 정의하게되어 이런 부분이 중복된다는 뜻



### ModelForm 선언하기

- 기존의 Form

  ```python
  from django import forms
  
  class ArticleForm(forms.Form):
      title = forms.CharField(max_length=10)
      content = forms.CharField()
  ```

- ModelForm

  ```python
  from django import forms
  from .models import Article
  
  class ArticleForm(forms.ModelForm):
      
      class Meta:
          model = Article
          fields = '__all__'
          #exclude = ('title', )        
  ```

  구조를 보면, `forms`라이브러리에서 파생된 `ModelForm` 클래스를 상속받고, 정의한 클래스 안에 `Meta` 클래스를 선언한다. 그리고 `Meta` 클래스에는 어떠한 Model을 기반으로 Form을 작성할 것인지에 대한 정보를 작성



### create view 수정해보기

```python
def create(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)	#유효하면 해당 정보를 가지고 detail 페이지로
    return redirect('artcles:new')	#유효하지 않으면 다시 new 페이지로
```



#### is_valid() method

- 유효성 검사 실행하고 데이터의 유효 여부를 boolean으로 반환
- 데이터가 특정 조건에 부합하는지를 확인함
  - 이메일 형식이라든지, 글자수라든지



#### save() method

- Form에 바인딩 된 데이터(`form  = ArticleForm(request.POST)`)에서 데이터베이스 객체를 만들고 저장

- ModelForm의 하위클래스는 기존 모델 인스턴스를 키워드 인자  `instance`로 받아들일 수 있음

  - 위의 예에서는 `ArticleForm`이 ModelForm의 하위 클래스
  - 이것이 제공되면 `save()`는 해당 인스턴스를 수정하게 되고, 제공되지 않으면 새로운 인스턴스를 생성하는 것

- 생성

  ```python
  def create(request):
      form = ArticleForm(request.POST)
      if form.is_valid():
          article = form.save()
          return redirect('articles:detail', article.pk)
      return redirect('artcles:new')
  ```

- 수정

  ```python
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      form = ArticleForm(request.POST, instance=article)
      form.save()
      return redirect('articles:detail', article.pk)
  ```

  

#### create view 함수 구조 변경

- 이제는 `new.html`이 필요 없어지고, 하나의 `create.html`로, 그리고 이에따라 `url path`에서 `new` 삭제

  ```python
  #1단계
  def create(request):
      if request.method == 'POST':	#만약 POST방식으로 들어오는 경ㅇ
          pass
      else:	#POST가 아닌 방식으로 들어오는 경우
          pass
      
  #2단계
  def create(request):
      if request.method == 'POST':
          form = ArticleForm(request.POST)
          if form.is_valid():
              article = form.save()
              return redirect('articles:detail', )
  ```

- create.html

  ```html
  <form action="{% url 'articles:create' %}" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
  </form>
  ```

  이제는 위에 쓴 `action`값을 생략해도 정상 동작한다. 현재의 페이지로 다시 전송할 것이기 때문에



### DELETE

- DELETE 로직 작성

  ```python
  def delete(request, pk):
      article = Article.objects.get(pk=pk)
      if request.method == 'POST':
          article.delete()
          return redirect('articles:index')
      return redirect('articles:detail', article.pk)
  ```

- detail.html

  ```html
  <from action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="DELETE">
  </from>
  ```

  

### UPDATE

- 로직

  ```python
  def update(request, pk):
      article = Article.objects.get(pk=pk)
      if request.method == 'POST':
          form = ArticleForm(request.POST, instance=article)
          if form.is_valid():
              form.save()
              return redirect('articles:detail', article.pk)
      else:
          form = ArticleForm(instance=article)
      context = {
          'form': form,
          'article': article,
      }
      return render(request, 'articles/update.html', context)
  ```

- update.html

  ```html
  {% extends 'base.html' %}
  
  {% block content %}
    <form action="" method="POST">
      {% csrf_token %}
      {{ form.as_p }}
      <input type="submit">
    </form>
  {% endblock %}
  ```

  



### Form과 ModelForm의 비교

#### Form

- 어떤 Model에 저장해야 하는지 알 수 없으므로 유효성 검사 이후 `cleaned_data` 딕셔너리를 생성

  - `cleaned_data`는 `ModelForm`에는 존재 X

- `cleaned_data` 딕셔너리에서 데이터를 가져 온 후에 `.save()`를 호출

  ```python
  def create(request):
      if request.method == 'POST':
          form = ArticleForm(request.POST)
          if form.is_valid():
              title = form.cleaned_data.get('title')
              content = form.cleaned_data.get('content')
              article = Article.objects.create(title=title, content=content)
              return redirect('article:detail', article.pk)
      else:
          #생략
          pass
  ```

- Model에 연관되지 않은 데이터를 받을 때 사용





### Widgets 활용하기

- 첫번째 방법:  Meta 클래스 내에 지정

- 두번째 방법(권장)

  ```python
  class ArticleForm(forms.ModelForm):
      titles = forms.CharField(
          label='제목'
          widget=forms.TextInput(
              attrs={
                  'class': 'my-title',
                  'placeholder': 'Enter the title',
              }
          ),
      )
      
      
      class Meta:
          model = Article
          fields = '__all__'
  ```

  



## Rendering fields manually



### 수동으로 Form 작성하기

- Rendering fields manually
- Looping over the form's fields (for 사용)



#### Rendering fields manually

- `{{ form }}` 대신 하나하나 직접 rendering 해주는 방식

  ```html
  <div>
      {{ form.title.errors }}
      {{ form.title.label_tag }}
      {{ form.title }}
  </div>
  ```



#### Looping over the form's fields

- `for` 활용

  ```html
  {% for field in form %}
    {{ field.errors }}
    {{ field.label_tag }}
    {{ field }}
  {% endfor %}
  ```





### Bootstrap과 함께 사용하기



#### Bootstrap Form class

- Bootstrap Form의 class 를 widget에 작성하는 방법
- 핵심 class 는 `form-control`



#### Django Bootstrap Library

- form class에 bootstrap을 적용시켜주는 라이브러리

  ```bash
  $ pip install django-bootstrap-v5
  ```

- `settings.py` > `INSTALLED_APPS`에 `bootstrap5` 등록

- 새로 install 한 후에는 반드시 패키지 목록 업데이트

  ```bash
  $ pip freeze > requirements.txt
  ```

- 설치했으면 `base.html`에 적용할 수 있다. 기존의 bootstrap CDN 대신 아래와 같이 적용한다.

  ```html
  {% load bootstrap5 %}
  
  <head>
      ...
      {% bootstrap_css %}
  </head>
  <body>
      <div class="container">
          {% block content %}
          {% endblock %}
      </div>
      {% bootstrap_javascript %}
  </body>
  ```

- 이제 각 템플릿에는 아래와 같이 적용한다.

  ```html
  {% extends 'base.html' %}
  {% load bootstrap5 %}
    <form action="" method="POST">
      {% csrf_token %}
      {% bootstrap_form form layout='horizontal' %}
      {% buttons submit="Submit" reset="Cancel" %}{% endbuttons %}
    </form>
  {% block content %}
  {% endblock %}
  ```

  



## 개인적인 Q

---

- ModelForm을 써서 new-create가 따로 있을 필요가 없고 create 하나만으로 된다는 식으로 써져있는데
- ModelForm 말고 그냥 Form 쓸 때도 사실 create 하나만 있으면 되는거 아닌가???

---

- 유효한 경우 저장하게 되는데, 그 저장된 정보를 바로 활용하기 위해 따로 `article`이라는 변수에 생성된 인스턴스를 저장해줌 

  ```python
  article = form.save()
  return redirect('articles:detail', article.pk)
  #-------------------------------------------------
  
  return redirect('articles:detail', form.save().pk)
  #이거 가능?
  ```

---

- DELETE 처음꺼 함수에서 POST 아닌 경우에 그냥 render 하면 안됨? pk랑 같이 주고

---

- Widgets 활용할 때 어차피 저렇게 다 지정해줄거면 굳이 ModelForm을 쓸 이유가 있을까?
  - 위에 써놓은 질문 중에 ModelForm 말고 그냥 Form 써도 함수나 템플릿 하나로 가능하지 않냐는 거랑 연계될거 같긴함

---

- base.html에 `{% load bootstrap5 %}` 이거 있는데 각 템플릿마다 또 load 해줘야됨?