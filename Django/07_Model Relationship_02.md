# Model relationship_02

[TOC]



## Intro

- 병원 진료 기록 시스템을 통한 M:N 학습



### 1:N의 한계



#### 1:N 모델 관계 설정

```python
class Doctor(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'
    
class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```



#### 의사 2명, 환자 2명 생성

- 의사 1번 - justin, 2번 - eric
- 환자 1번 - tony - 1번 의사, 2번 - harry - 2번 의사



#### 1:N의 한계

- 1번 환자가 1번 의사의 진료를 마치고 2번 의사에게도 방문하려고 한다면 새로운 예약을 생성해야 함
- 이 때 기존의 예약(1번 의사의 진료)은 유지해야 하는데, 만약 이상태에서 새로 'tony'라는 이름의 예약을 생성하게 되면, 이 'tony'는 1번 환자인 'tony'와는 다르게 되어버림(id가 다르므로)
- 그렇다고 1번 'tony'의 `doctor_id`'에 한번에 두 개 이상의 `doctor_id`를 넣을 수 없음. 즉 하나의 외래 키에 2개 이상의 의사 데이터를 넣을 수 없음



### 중개 모델



#### 중개 모델 작성

```python
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)

    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```



#### 관계 확인

- 각 `reservation.id`에 예약이 형성된 `doctor_id`와 `patient_id`를 담고 있음



#### 예약 형성

```bash
doctor1 = Doctor.objects.get(pk=1)
patinet1 = Patient.objects.get(pk=1)
Reservation.objects.create(doctor=doctor1, patient=patient1)
```



#### 예약 조회

```bash
doctor1.reservation_set.all()
patient1.reservation_set.all()
#------------------------------------
Doctor.reservation_set.all() #불가능
```



#### 환자2번-의사1번 추가 예약 생성

```bash
Reservation.objects.create(doctor=doctor1, patient='harry') #불가능
Reservation.objcts.create(doctor=doctor1, patient=Patient.objects.get(pk=2)) #가능
```





### ManyToManyField

- 다대다 (M:N) 관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스)가 필요



#### ManyToManyField 작성

- 중개모델은 삭제

- Doctor 또는 Patient 어느 쪽에든 작성 가능

  ```python
  class Patient(models.Model):
      # doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
      doctors = models.ManyToManyField(Doctor)
      name = models.TextField()
  
      def __str__(self):
          return f'{self.pk}번 환자 {self.name}'
  
  #OR --------------------------------------------------------------------
  class Doctor(models.Model):
      name = models.TextField()
      patients = models.ManyToManyField(Patient)
  
      def __str__(self):
          return f'{self.pk}번 의사 {self.name}'
  ```



#### 중개테이블 확인

- 1:N과는 다르게 어느 한쪽의 클래스 내에 하나의 필드가 추가된 것이 아니라 별개의 중개 테이블이 생성됨
  - `hospitals_patient_doctors`
  - 1:N에서는 Comment에서 Article을 참조하면 `Comment` 내에 `article_id`라는 필드가 생성되었음



#### 예약생성(참조)

- 1번 의사와 1번 환자 간의 예약을 생성해보자

  ```bash
  patient1.doctors.add(doctor1)
  patient1.doctors.all()
  doctor1.patient_set.all()
  ```

  밑에처럼은 안됨

  ```bash
  patient1 = Patient.objects.get(pk=1)
  doctor1 = Doctor.objects.get(pk=1)
  patient1.doctors = doctor1 #불가능
  #Direct assignment to the forward side of a many-to-many set is prohibited. Use doctors.set() instead.
  ```



#### 예약생성(역참조)

- 의사1이 환자2를 예약 하는 경우

  ```bash
  doctor1.patient_set.add(patient2)
  ```

  

#### 예약삭제(역참조)

- 의사1이 환자1 예약 취소

  ``` bash
  doctor1.patient_set.remove(patient1)	#환자1 삭제
  doctor1.patient_set.all()	#확인
  patient1.doctors.all()	#환자에게 예약된 의사 확인
  ```

  

#### 예약삭제(참조)

- 환자2가 의사1 진료 예약 취소

  ```bash
  patient2.doctors.remove(doctor1)
  patient2.doctors.all()
  doctor1.patient_set.all()
  ```



### related_name

- `ForeignKey`의 `related name`과 동일





### 중개 모델(테이블) in Django

- 지금까지는 `ManyToManyField`를 통해 중개 테이블을 자동으로 생성
- 그렇다면 중개 테이블을 직접 작성하는 경우는?
  - `through` 옵션 사용하여 중개 테이블을 나타내는 Django 모델 지정 가능
  - 일반적인 용도는 중개 테이블에 데이터를 사용해 다대다 관계로 연결하려는 경우





### 요약

- 1:N 관계는 완전한 종속의 관계이지만 M:N 관계는 양방향의 형태로 표현이 가능





## ManyToManyField



### 개념 및 특징

- 다대다(M:N)관계 설정 시 사용하는 모델 필드
- 하나의 필수 위치인자 필요(M:N 관계로 설정할 모델 클래스)
- `add(), remove(), create(), clear()`등의 `RelatedManager` 제공



### Arguments

#### related_name

- 관계 필드를 가지지 않은 모델(target 모델)이, 관계 필드를 가진 모델(source 모델)을 참조(역참조)할 때 사용할 manager의 이름 설정
- ForeignKey에서의 그것과 같다



#### through

- 중개 테이블을 직접 작성하는 경우에 중개 테이블을 나타내는 Django 모델을 지정할 때 사용가능한 옵션
- 일반적으로 중개 테이블에 추가 데이터를 사용하는 다대다 관계와 연결하려는 경우에 주로 사용



#### symmetrical

- ManyToManyField가 동일한 모델(`on self`)을 가리키는 정의에서**만** 사용
- `symmetrical=True`가 기본값이며, 이 때 Django는 `person_set` 매니저를 추가하지 않음
  - `False`이면 추가함
  - `friends = models.ManyToManyField('self', symmetrical=False)`
- source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델의 인스턴스도 source 모델의 인스턴스를 자동으로 참조
  - 즉 이렇게 자동으로 참조하는 것을 원하지 않는다면 `False` 값을 부여하면 됨



### Related Manager

- add, remove, create, clear, set 등이 있음

#### add()

- 지정된 객체를 관련 객체 집합에 추가
- 이미 존재하는 관계라면 복제되지 않음
  - 즉 `doctor1`의 `patient_set`에 `patient1`이 들어가있으면, 다시한번 `add`하더라도 `patient1`이 두개 들어있지 않음
  - 그렇다고 오류가 발생하지도 않음
- 모델 인스턴스와 필드값(PK)를 인자로 허용



#### remove()

- 관련 객체 집합에서 지정된 모델 객체를 제거
- 사용하면 내부적으로 `QuerySet.delete()`를 사용하여 관계가 삭제
- 모델 인스턴스, 필드값을 인자로 허용



### through

#### 예시

```python
class Doctor(models.Model):
    name = models.TextField()
    
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'

class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')	###
    name = modles.TextField()
    
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'

class Reservation(models.Model):	###
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

- 데이터 생성과정은 지금까지 한 것과 같은 방법으로도 가능하고 `through_defaults`를 사용해서도 가능하다

#### through_defaults

```bash
patient2.doctors.add(doctor1, through_defaults={'symptom':'flu'})
```

through_defaults를 사용해 add(), create(), 또는 set()을 사용해서 관계를 생성





### 데이터베이스에서의 표현

- 테이블 이름은 다대다 필드의 이름과 이를 포함하는 모델의 테이블 이름을 조합하여 생성



### 중개 테이블의 필드 생성 규칙



#### 1. source model 및 target model이 다른 경우

- id
- `<containing_model>_id`
- `<other_model>_id`



#### 2. ManyToManyField가 동일한 모델을 가리키는 경우

- id
- `from_<model>_id`
- `to_<model>_id`





## Like

### 구현

```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    ...
```

```python
urlpatterns = [
    ...
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]
```

```python
@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)
        
        if article.like_users.filter(pk=request.user.pk).exists():
            article.like_users.remove(request.user)
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

```html
<!--index.html-->
...
<form action="{% url 'articles:likes' article.pk %}" method="POST">
    {% csrf_token %}
    {% if user in article.like_users.all %}
    	<input type="submit" value="좋아요 취소">
    {% else %}
    	<input type="submit" value="좋아요">
    {% endif %}
</form>
```





## Profile Page



### Profile Page 작성

```python
urlpatterns = [
    path('<username>/', views.profile, name='profile'),
]
```

```python
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth import get_user_model

def profile(request, username):
    person = get_object_or_404(get_user_model(), username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```





## Follow



### 구현

```python
class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

```python
urlpatterns = [
    ...,
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]
```

```python
@require_POST
def follow(request, user_pk):
    if request.user.is_authenticated:
        person = get_object_or_404(get_user_model(), pk=user_pk)
        if person != request.user:
            if person.followers.filter(pk=request.user.pk).exists():
                person.followers.remove(request.user)
            else:
                person.followers.add(request.user)
        return redirect('accounts:profile', person.username)
    return redirect('accounts:login')
```





---

## Q.

- `ReverseManyToOneDescriptor` 이거 뭐임?

- 그러면 M:N도 결국 한쪽에서 하나를 잡고, 그러니까 여기서는 한명의 patient를 잡고 예약잡은 의사와의 관계를 보면 1:N이 되겠네

- 여기서는 왜 save()가 필요 없지?

- ManyToManyField에는 `on_delete`속성 필요없나?

- `remove`처럼 하나하나 빼는거 말고 한번에 비우는 방법은? `clear()` 
  - `doctor1.patient_set.clear()`와 같이 사용이 가능

- target model과 souce model

- 그러면 한 의사가 같은 환자와 여러번 예약을 잡는 경우(날짜를 달리하든 시간을 달리하든) 이건 어케 표현?

- 모델을 계속 변경하다 보니 에러가 뜸

  ```bash
  ValueError: Cannot alter field hospitals.Patient.doctors into hospitals.Patient.doctors - they are not compatible types (you cannot alter to or from M2M fields, or add or remove through= on M2M fields)
  ```

  이미 생성된 필드를 ManyToManyField로 변경하는 경우, makemigrations를 치면 내부에서 변경된 걸로 치니까 AlterField로 생성이 된다..??

  alter가 불가능하다고 하면 걍 DB 지워주고 다시 생성하면 됨

  별로 좋은 방법은 아닐 것임

- 참고) Model 에서 NOT NULL 속성을 주려면 `null=False` 속성을 주면된다

  - True가 기본값인 듯하다

- blank라는 속성도 있던데 null과의 차이는?