# Model Relationship_01



## Foreign Key



### 개념

- 외래 키
- 관계형 데이터베이스에서, 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
- 참조하는 테이블의 외래 키는 참조되는 테이블의 행 1개에 대응
- 참조하는 테이블에서 속성(필드)에 해당, 이는 참조되는 테이블의 기본 키(Primary Key)를 가리킴



### 특징

- 참조 무결성: 키를 사용하여 부모 테이블의 유일한 값을 참조
- 외래 키의 값이 반드시 부모 테이블의 기본키(Primary Key)일 필요는 없지만 유일한 값이어야 함



### ForeignKey field

- N:1 관계
- 2개의 위치 인자 필요
  - 참조하는 model class
  - on_delete 옵션
- migrate 작업시 필드 이름에 _id를 추가하여 데이터베이스를 만듦



### comment 모델 정의하기

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.content
```



#### on_delete

- 외래 키가 참조하는 객체가 사라졌을 때 외래 키를 가진 객체를 어떻게 처리할지 정의
- Database Integrity(데이터 무결성)을 위해서 매우 중요한 설정
- CASCADE, PROTECT, SET_NULL, SET_DEFAULT, SET(), DO_NOTHING, RESTRICT



### Migration

- 동일한 방식으로 migrate를 진행하고, `articles_comment` 테이블의 외래 키 컬럼을 확인해 보면 필드 이름에 `_id`가 추가된 것을 확인할 수 있다.
- 위의 경우 `article_id`가 있을 것
- `comment.article = article` : 가능
- `comment.article_id = article.pk`: 가능
- 위의 두 경우 모두 `comment.article`을 입력하면 `<Article: ~>`가 나올 것
- 단 `comment.articke_pk`는 불가능





### 1:N 관계 related manager



#### 역참조

- Article(1) 에서 Comment(N)을 참조하는 것을 역참조
- 이때 `article.comment`와 같이 사용할 수는 없음. 게시글에 댓글이 몇개나 있는지 Django ORM이 보장할 수 없기 때문. 1개일 수도, 여러 개일수도, 아예 없을 수도 있으므로
- 그래서 `article.comment_set`으로 사용해야 하고, `Comment`를 정의 할 때 `article` 필드에 `related_name` 속성을 주면 `article.{related_name}`으로 사용 가능



#### 참조

- Comment(N) -> Article(1)
- 댓글의 경우 존재한다면 반드시 참조하는 Article이 있으므로, `comment.article`과 같이 접근 가능





## Comment Create



### CommentForm 작성

```python
from .models import Article, Comment

class CommentForm(models.ModelForm):
    
    class Meta:
        model = Comment
        exclude = ('article', ) #ForeignKeyField를 작성자가 직접 입력하게 되는 상황 방지
```



### 댓글 작성 로직

```python
app_name = 'articles'
urlpatterns = [
    path('<int:pk>/comments/', views.comments_create, name='comments_create'),
]
```

```html
<form action="{% url 'articles:comments_create' %}" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit">
</form>
```

```python
@require_POST
def comments_create(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm(request.POST)
    if comment_form.is_valid():
        comment = coment_form.save(commit=False)
        comment.article = article
        comment.save()
    return redirect('articles:detail', article.pk)
```



#### save() method

- save(commit=False)
  - 아직 데이터베이스에 저장되지 않은 인스턴스를 반환
  - 저장하기 전에 객체에 대한 사용자 지정 처리를 수행할 때 유용하게 사용





## Comment READ

```python
from .models import Article, Comment

def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)
```



## Comment DELETE

```python
app_name = 'articles'
urlpatterns = [
    path(
    '<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
]
```

```python
@require_POST
def comments_delete(request, article_pk, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    comment.delete()
    return redirect('articles:detail', article_pk)
```



### 인증된 사용자만 댓글 작성 및 삭제

- 각 함수에 `if request.user.is_authenticated` 추가하면 된다





## Comment 추가사항

47p~



---

- 외래키가 참조하는 대상이 꼭 PK일 필요는 없다고 했는데, 다른 필드를 참조하려면 어떻게 함?

- commit=False가 필요한 이유를 잘 모르겠음 comment_form 이거 그대로 사용하면 되는거 아님?
  - comment_form.article =article 하고 난 다음에 comment_form.save() 이거 해도 될거같은데
