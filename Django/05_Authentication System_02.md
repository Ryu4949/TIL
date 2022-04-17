# Authentication System_02

[TOC]



## 회원가입



### UserCreationForm

- 주어진 username과 password로 권한이 없는 새 user를 생성하는 ModelForm
- 필드 3개로 구성
  - username
  - password1
  - password2(확인용)



### 구현

```python
app_name = 'accounts'
urlpatterns = [
    path('signup/', views.signup, name='signup'),
]
```

```html
{% extends 'base.html' %}

{% block content %}
	<form action="{% url 'accounts:signup' %}" method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        <input type="submit">
	</form>
{% endblock %}
```

```python
from django.contrib.auth.forms import AuthenticationForm, UserCreationForm

def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            #user = form.save()
            #auth_login(request, user)
            return redirect('articles:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

- 만약 회원가입 후 동시에 로그인을 진행하려면 `form.save()`를 주석처리된 부분으로 대체하면 된다.





## 회원탈퇴

- DB에서 사용자를 삭제하는 것과 같다.



### 구현

```python
app_name = 'accounts'
urlpatterns = [
    path('delete/', views.delete, name='delete'),
]
```

```html
<form action="{% url 'accounts:delete' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="회원탈퇴"
</form>
```

```python
from django.views.decorators.http import require_POST

@require_POST
def delete(request):
    if request.user.is_authenticated:
        request.user.delete()
    return redirect('articles:index')
```



- 탈퇴하면서 해당 유저의 세션 데이터도 함께 지울 경우

  ```python
  @require_POST
  def delete(request):
      if request.user.is_authenticated:
          request.user.delete()
          auth_logout(request)
      return redirect('articles:index')
  ```

  반드시 탈퇴 후 로그아웃 순으로 처리해야 함





## 회원정보 수정



### UserChangeForm

- 사용자의 정보 및 권한 변경을 위해 admin 인터페이스에서 사용되는 ModelForm



### 구현

```python
app_name = 'accounts'
urlpatterns = [
    path('update/', views.update, name='update'),
]
```

```html
<form action="{% url 'accounts:update' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
</form>
```

```python
from django.contrib.auth.forms import AuthenticationForm, UserCreationForm, UserChangeForm

@require_http_methods(['GET', 'POST'])
def update(request):
    if request.method == 'POST':
        pass
    else:
        form = UserChangeForm(instance=request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/update.html')
```



### 문제점

- 일반 사용자가 접근해서는 안될 정보들(fields)까지 모두 수정이 가능해짐
- 따라서 `UserChangeForm`을 상속받아 `CustomUserChangeForm`이라는 서브클래스를 작성해서 접근 가능한 필드를 조정해야 함



### CustomUserChangeForm 작성

```python
from django.contrib.auth.forms import UserChangeForm
from django.contrib.auth import get_user_model

class CustomUserChangeForm(UserChangeForm):
    
    class Meta:
        model = get_user_model()
        fields = ???
```



#### get_user_model()

- 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환

- Django는 User 클래스를 직접 참조하는 대신 `django.contrib.auth.get_user_model()`을 사용하여 참조해야 한다고 강조

  ```python
  from django.contrib.auth.forms import UserChangeForm
  from django.contrib.auth import get_user_model
  
  class CustomUserChangeForm(UserChangeForm):
      
      class Meta:
          model = get_user_model()
          fields = ('email', 'first_name', 'last_name',)
  ```

  수정할 때 필요한 필드만 선택해서 작성

- view 함수 변경

  ```python
  from django.contrib.auth.forms import AuthenticationForm, UserCreationForm, UserChangeForm
  from .forms import CustomUserChangeForm
  
  @login_required
  @require_http_methods(['GET', 'POST'])
  def update(request):
      if request.method == 'POST':
          form = CustomUserChangeForm(request.POST, instance=request.user)
          if form.is_valid():
              form.save()
              return redirect('articles:index')
      else:
          form = UserChangeForm(instance=request.user)
      context = {
          'form': form,
      }
      return render(request, 'accounts/update.html')
  ```







## 비밀번호 변경



### PasswordChangeForm

- 비밀번호 변경 Form
- 이전 비밀번호를 입력하여 변경할 수 있도록 함
- 회원정보 수정 페이지에 작성된 비밀번호 변경 form 주소 확인
  - 해당 페이지에서 개발자도구에서 확인 가능



### 구현

```python
app_name = 'accounts'
urlpatterns = [
    path('password/', views.change_password, name='change_password'),
]
```

```html
<form action="{% url 'accounts:change_password' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
</form>
```

```python
from django.contrib.auth.forms import (
	AuthenticationFrom,
	UserCreationForm,
	PasswordChangeForm,
)

@login_required
@require_http_method(['GET', 'POST'])
def change_password(request):
    if request.method == 'POST':
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            form.save()
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/change_password.html', context)
```





### 암호 변경 시 세션 무효화 방지

- `update_session_auth_hash(request, user)`

  ```python
  from django.contrib.auth.forms import (
  	AuthenticationFrom,
  	UserCreationForm,
  	PasswordChangeForm,
  )
  from django.contrib.auth import update_session_auth_hash
  
  @login_required
  @require_http_method(['GET', 'POST'])
  def change_password(request):
      if request.method == 'POST':
          form = PasswordChangeForm(request.user, request.POST)
          if form.is_valid():
              form.save()
              update_session_auth_hash(request, form.user)
              return redirect('articles:index')
      else:
          form = PasswordChangeForm(request.user)
      context = {
          'form': form,
      }
      return render(request, 'accounts/change_password.html', context)
  ```

  

---

- 회원탈퇴할 때 세션 데이터도 함께 지울 이유가 있음? 저걸로 뭐할 수 있나?