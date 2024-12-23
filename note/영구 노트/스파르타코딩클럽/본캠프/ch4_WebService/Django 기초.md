#본캠프 [[ch4_WebService]]

## 1. Django
### 프레임워크 (Framework)
- **소프트웨어 개발을 위한 구조적인 틀**
	- 개발자들이 프로그램을 만들 때 자주 사용하는 여러 도구를 모아놓은 것
	- 특정한 작업이나 목적을 수행하기 위한 코드 뭉치

- 프레임워크를 사용하는 이유
	- 높은 생산성
	- 빠르고 안정적이며, 높은 품질의 소프트웨어 개발 가능
	- 부가적인 부분은 프레임워크에 맡기고, 핵심 로직에만 집중 가능


### Django
- **python 기반의 웹 프레임워크**
- 빠른 웹 개발을 위해 웹 개발에 필요한 모든 것(보안, 관리자기능, Auth 등)이 준비되어있음
- 레퍼런스가 풍부하고, 현재 여러 유명 애플리케이션에서 사용할 정도로 검증되어있음


---
## 3. Django Project 알아보기
### 프로젝트 시작 과정
1. 가상환경 생성 및 활성화
2. Django 설치: `pip install django==4.2 (LTS version)`
3. `requirements.txt` 생성
4. 프로젝트 생성


### 프로젝트 생성하기
1. 프로젝트 생성
```shell
django-admin startproject <프로젝트 이름> <생성 디렉토리>
```
	- 여기서 생성 디렉토리를 생략하면 현재 위치에 프로젝트 이름의 폴더가 만들어지며 생성됨

```shell
django-admin startproject <프로젝트 이름> .
```
	- [.]은 현재 폴더를 의미하며 현재 폴더를 프로젝트 폴더로 사용해서 생성됨

2. 해당 폴더 안쪽으로 이동
```shell
cd <프로젝트 이름>
```

3. django 개발 서버 실행
```shell
python manage.py runserver
```

- 실행 결과 ![[first_django_server.png]]

- 프로젝트 내 생성된 파일들
	- **`settings.py`** : 프로젝트의 설정을 관리하는 곳
	- **`urls.py`** : 어떤 요청을 처리할지 결정하는 곳
	- `__init__.py` : 하나의 폴더를 하나의 파이썬 패키지로 인식하도록 하는 파일
		→ 3버전 이상으로 가면 없어도 되지만, 3버전 이하에서의; 호환성을 위한 규칙
	- `wsgi.py` : 웹 서버 관련 설정 파일
	- `manage.py` : Django 프로젝트 유틸리티 (조종기)


---
## 4. Django App 알아보기
### Django App
- **Django App == 내가 생각하는 하나의 기능 덩어리**
- 하나의 프로젝트는 여러 개의 앱으로 구성될 수 있음
	- 프로젝트: 어플리케이션의 집합체
	- 앱: 각각의 기능 단위 모듈


### App 사용하기
1. App 생성하기
	- `manage.py`를 이용해 프로젝트를 생성한 후 앱 생성
	- 앱 생성 코드: `python manage.py startapp <앱 이름>`

2. App 등록하기
	- `settings.py`의 `INSTALLED_APPS` 부분에 생성한 앱 이름을 넣어 등록


### App 살펴보기
- 앱 내 생성된 파일들
	- `admin.py` - 관리자용 페이지 관련 설정
	- `apps.py` - 앱 관련 정보 설정
	- **`models.py`** - DB관련 데이터 정의 파일
	- `tests.py` - 테스트 관련 파일
	- **`views.py`** - 요청을 처리하고 처리한 결과를 반환하는 파일


---
## 7. Django의 설계 철학 - MTV Pattern
### 소프트웨어의 디자인 패턴
- 디자인 패턴: **자주 사용되는 소프트웨어의 구조를 일반화 해둔 것**
- 디자인 패턴의 필요성
	- 공통적으로 발생하는 문제에 대해 재사용 가능한 해결 방법을 제시 가능
	- 특정 구조에 대한 빠른 설계가 가능
	- 즉, 프로그래머가 시스템을 디자인할 때 발생하는 공통된 문제를 해결하면서 진행할 수 있는 형식화된 관행


### MVC vs MTV
|  **MVC**   | **MTV**  |                **역할**                 |
| :--------: | :------: | :-----------------------------------: |
|   Model    |  Model   | 데이터 구조 정의, DB 기록 관리 등 데이터와 관련된 로직을 처리 |
|    View    | Template |        UI, 레이아웃 등 화면상의 로직을 처리         |
| Controller |   View   |       클라이언트 요청에 대해 처리를 분기하는 역할        |
- 역할에 따라 분리하는 이유
	- 관심사 분리
	- 각 부분을 독립적으로 개발할 수 있어서 생산성이 증가하고 유지 보수가 쉬워짐
	- 다수의 멤버가 동시에 개발하기도 용이함

- 순서도
![[MTV.webp]]
1) HTTP 요청을 urls.py에서 수신
2) urls.py에서 HTTP 요청에 맞는 View로 이동
3) 해당 View에서 필요한 데이터를 Model과 송수신
4) 송수신한 데이터를 화면에 보여주기 위해 Template과 통신
5) 완성된 작업물을 HTTP 응답으로 내보냄


---
## 9. Django Template System
### DTL (Django Template Language)
- Django Template에서 사용하는 문법
- Django 개발자를 위해 Python과 비슷한 사용 방법을 가짐
- Python과 비슷하다고 해서 Python이 동작하는 것이 아님에 유의


### 변수 (Variable)
- **변수의 기본 형태: `{{ variable }}`**
- view의 context로 넘긴 데이터를 접근할 수 있음
- **`.`** 을 사용하여 변수의 속성 값에 접근 가능
- `render()`의 세번째 인자인 context에 **`dict()`** 형태로 데이터가 넘겨짐
- 넘겨진 데이터에서 **`key`** 값이 template에서 사용 가능한 변수가 됨


### 필터 (Filter)
- **필터의 기본 형태: `{{ variable|filter }}`**
- 변수에 어떠한 작업을 추가적으로 더해 수정하고 싶을 때 사용
- 약 60개의 built-in filter가 제공되며 일부 필터는 인자를 받기도 함


### 태그 (Tag)
- **태그의 기본 형태: `{% tag %}`**
- 반복문 또는 논리, 조건문을 수행하여 제어 흐름을 만들거나 특수한 기능을 수행
- 일부는 시작 태그와 종료 태그가 있음
```django
{% if ~ %}
{% endif %}
```


### 주석 (Comment)
```django
{# 한 줄 주석 #}

{% comment %}
	여러줄
	주석
{% endcomment %}
```


### 템플릿 상속 (Template Inheritance)
- 코드의 재사용성을 높여 코드 중복 문제를 해결
- 상위 템플릿에 공통이 될 부분을 정의하고 하위 템플릿에서 달라질 부분을 block으로 만드는 skeleton 형태

- 상위 템플릿
	- **`{% block block_name %}{% endblock block_name %}`**
	- 상위 템플릿에서 하위 템플릿마다 달라질 부분을 정의

- 하위 템플릿
	- **`{% extends 'template_name' %}`** 
	- 하위 템플릿에서 상위 템플릿을 상속한다는 의미
	- 하위 템플릿의 최상단에 위 명령어가 위치해야함
	- 다중 상속을 지원하지 않음


---
## 11. 다중 앱과 URL
### Variable Routing
- URL 일부를 변수로 지정하여, 해당 부분에 들어온 값을 view로 넘겨줄 수 있음
- view에서 변수를 받아서 그 부분에 맞게 처리
- 하나의 URL에 여러 페이지를 연결하는 것이 가능
```python
# users/urls.py
path("profile/<str:username>/", views.profile)

# users/views.py
def profile(request, username):
	context = {"username": username}
	return render(request, "profile.html", context)
```
```html
# users/templates/profile.html
<h1>{{ username }} Profile Page</h1>
<p> username : {{ username }} </p>
```


### Multiple Apps
- 하나의 프로젝트는 여러 개의 앱으로 구성됨
- 각각의 기능별로 나누어서 App으로 분리하는 것이 좋은 구조
```bash
.
|-- articles  // `articles/` 요청을 처리하는 app
|   ⁝
|   |-- templates
|   |   |-- data-catch.html
|   |   |-- data-throw.html
|   |   |-- hello.html
|   |   `-- index.html
|   |-- urls.py
|   |   `-- views.py
|-- db.sqlite3
|-- manage.py
|-- my_first_pjt
|   ⁝
|   |-- settings.py
|   `-- urls.py
|-- templates
|   `-- base.html
`-- users  // `users/` 요청을 처리하는 app
    ⁝
    |-- templates
    |   |-- profile.html
    |   `-- users.html
    |-- urls.py
    `-- views.py
```


### Naming URL Patterns
- 어떠한 URL을 작성할 때 직접 경로를 넣지 않고 각각의 URL에 '이름'을 붙여주는 것

```python
# /articles/urls.py
urlpatterns = [
    path("hello/", views.hello, name="hello"),
    # data
    path("data-throw/", views.data_throw, name="data-throw"),
    path("data-catch/", views.data_catch, name="data-catch"),
]
```
```html
<!-- /articles/templates/index.html -->
<!-- href에 경로가 아닌 path의 name을 대신 적어서 경로를 표현 -->
{% block content %}
    <h1>INDEX</h1>
    <nav>
        <ul>
            <li><a href="{% url 'hello' %}">Hello</a></li>
            <li><a href="{% url 'data-throw' %}">Data Throw</a></li>
            <li><a href="{% url 'users' %}">Users</a></li>
        </ul>
    </nav>

    <p>My first Django project is working!</p>
{% endblock content %}
```


---
## 12. Django Model
### 용어 정리
#### Model
- 저장할 데이터에 대한 필드와 동작들을 포함한 데이터베이스 구조
- Django는 Model을 이용해서 데이터를 조작
- 일반적으로 하나의 Model은 하나의 데이터베이스 테이블을 의미


#### 데이터베이스(Database)
- 잘 정리된 데이터가 모여있는 것


#### 쿼리(Query)
- 데이터베이스를 조작할 수 있는 언어


#### 스키마(Schema)
- 데이터베이스의 구조, 관계 등을 정의한 것


#### 테이블(Table)
- 열(Column) - 속성 / 필드(Field)
- 행(Row) - 데이터 / 레코드(Record) / 튜플(Tuple)
- 조직화된 데이터 요소들의 집합


#### 데이터베이스 기본 구조
- 테이블(Table)
- 기본키, PK(Primary Key)
- 열(Column)
- 행(Row)


### 모델 작성해보기
- `modles.Model`을 상속받아서 사용하고자 하는 데이터 스키마를 정의
- 각각의 필드는 테이블의 컬럼
- 필드의 타입에 따라 사용하며, 각 필드별로 필요한 옵션들이 존재
> 참고: [Model Field reference](https://docs.djangoproject.com/en/4.2/ref/models/fields/#field-types)
```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=50)  # Article table의 title column
    content = models.TextField()  # content column
    created_at = models.DateTimeField(auto_now_add=True)  # created_at column
    updated_at = models.DateTimeField(auto_now=True)  # updated_at column
```


### 마이그레이션(Migration)
- 현재는 Model 코드는 작성했지만 데이터베이스에 반영이 안된 상태
- **Django는 마이그레이션을 만들고 이 단위로 데이터베이스에 변경사항을 반영**


### 마이그레이션 관련 명령어
- **model에 생긴 변경사항을 마이그레이션으로 만드는 과정**
```shell
python manage.py makemigrations
```

- **마이그레이션을 실제로 데이터에 반영해서 동기화하는 과정**
```shell
python manage.py migrate
```

- 마이그레이션 목록과 적용여부를 보여주는 명령어
```shell
python manage.py showmigration
```

- 해당 마이그레이션이 어떤 sql문을 작성했는지 보여주는 명령어
```shell
python manage.py sqlmigrate <app_name> <migration_no>
```


---
## 13. Django ORM
### ORM (Object-Relational-Mapping)
- 파이썬으로 데이터베이스를 조작할 수 있게 해준다는 것
- SQL을 잘 알지 못해도 DB를 조작 할 수 있고, 잘 알더라도 기존의 복잡한 쿼리 문 없이 객체 지향적인 접근이 가능


### Database API
- Django ORM으로 Database API를 사용해서 DB를 조작
- **Manager**
	- 모델 클래스를 생성하면 Django는 자동적으로 CRUD 할 수 있는 Database API를 제공
	- API와 함께 모델 클래스를 이용하여 DB의 쿼리 작업을 도와주는 Manager도 제공
	- Manager의 기본 이름은 **`objects`** 
	- Manager를 이용해서 Django ORM의 Queryset API를 사용
		- Queryset: ORM을 사용해서 DB로부터 전달받은 객체
		- 기본 형태: **`<Model Class>.<Manager>.<Queryset API>`** 


### CRUD with Shell
- Django shell: Django가 제공하는 여러가지 기능을 명령어로 입력해서 실행할 수 있는 shell 환경
- **CRUD**: 대부분의 소프트웨어가 하는 동작인 Create, Read, Update, Delete의 앞 글자

- 생성(Create)
```python
# 하나의 Article 생성 방법 (1)
article = Article()
article.title = 'first_title'
article.content = 'first_content'

# 여기에서 전체 Article을 조회해보면
Article.objects.all() # 비어있다

# save()하기전에는 저장되지 않음
article.save()

# 다시 전체 Article을 조회해보면 하나의 아티클이 있음
Article.objects.all()


# 속성 하나씩 접근하기
# 제목 
article.title

# 내용
article.content

# 생성일시
article.create_at

# pk(id)
article.id

---
# 하나의 Article 생성 방법 (2)
article = Article(title='second_title', content='second_content')
article.save()

---
# 하나의 Article 생성 방법 (3)
# save()가 필요하지 않음
Article.objects.create(title='third title', content='마지막 방법')
```

- 조회(Read)
```python
# 하나의 Article만 조회하기 (get)
# 조회하는 대상이 하나도 없거나, 2개 이상이면 에러 발생
Article.objects.get(id=1)

---
# 전체 Article 조회
Article.objects.all()

---
# 조건으로 조회하기
# 조건에 사용되는 매개변수를 `lookup`이라고 함
# lookup과 일치되는 객체를 모두 반환
Article.objects.filter(title='second')

# 다양한 lookup을 제공
Article.objects.filter(id__gt=2) # 2보다 큰 id
Article.objects.filter(id__in=[1,2,3]) # 1,2,3에 속하는 id
Article.objects.filter(content__contains='my') # content에 'my'가 포함된
...
```

- 수정(Update)
```python
'''
1. 수정할 객체를 조회
2. 수정할 내용을 입력
3. 수정한 것을 DB에 반영
'''
article = Article.objects.get(id=1)
article.title = 'updated title'
article.save()
```

- 삭제(Delete)
```python
# 삭제할 객체를 조회한 후 삭제
article = Article.objects.get(id=2)
article.delete()
```


---
## 14. Django MTV 사용하기 (CR)
### GET & POST
- GET
	- 원하는 리소스를 가져올 때 사용
	- 생성할 때도 사용 가능하지만, 묵시적으로 리소스 조회용으로 사용하기로 약속
	- DB에 변화를 주지 않는 요청 (Read에 해당)

- POST
	- 서버로 데이터를 전송할 때 사용
	- 특정 리소스를 생성 혹은 수정하기 위해 사용
	- DB에 변화를 주는 요청 (Create, Update, Delete에 해당)


### 사이트 간 요청 위조 CSRF(Cross Site Request Forgery)
- CSRF
	- 유저가 실수로 해커가 작성한 로직을 따라 특정 웹페이지에 수정/삭제 등의 요청을 보내게 되는 것
	- 즉, 유저가 정말로 **의도한 요청인지 검증이 필요**

- CSRF Token
	- 유저가 서버에 요청을 보낼 때 함께 제공되는 특별한 토큰 값
	- 사용자의 세션과 연결되어 요청이 전송될 때 함께 제출됨
	- 서버는 요청을 받을 때 이 토큰을 검증하여 요청이 유효한지 확인하는 방식으로 CSRF 방지
	- 일반적으로 GET을 제외한 데이터를 변경하는 Method에 적용
	- Django에서는 쉽게 CSRF Token 방식을 구현할 수 있게 template tag로 제공
```django
{% block content %}
<h1>New Article</h1>

<form action="{% url 'create' %}" method="POST">
	<!-- csrf token 구현 -->
	{% csrf_token %}
	
	<label for="title">제목</label>
	<input type="text" name="title" id="title"><br><br>

	<label for="content">내용</label>
	<textarea name="content" id="content" cols="30" rows="10"></textarea>

	<button type="submit">저장</button>
</form>

{% endblock content  %}
```


### POST 방식 vs GET 방식
- HEADER
	- HTTP 전송에 필요한 모든 부가정보를 담고있는 부분
	- 메시지 크기, 압축, 인증, 요청 클라이언트(웹 브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등등 을 포함

- BODY
	- 실제 전송할 데이터가 담겨있는 부분
	- 데이터가 없다면 비어있게 됨

- GET은 데이터를 URL에 담아보내고, POST는 BODY에 담아 보냄
- 따라서 GET은 데이터 전송에 한계가 있으나, POST는 그렇지 않음
- POST 방식으로 데이터를 전송할 때는 CSRF Token이 필요


---
## 15. Django MTV 사용하기 (RUD)
### PRG 패턴
- POST - Redirect - GET 패턴
	1) POST요청을 서버에서 처리
	2) 서버에서는 다른 주소로 Redirect하도록 응답
	3) 브라우저는 GET방식으로 서버를 호출하여 사용자의 요청이 반영된 것을 보여줌

- PRG패턴을 사용하면 반복적인 POST호출을 막을 수 있고 사용자의 입장에서도 처리가 끝나고 처음 단계로 돌아간다는 느낌을 줌


---
## 16. Django Form
### Django Form Class
- 유저가 입력하는 데이터는 반드시 **유효성 검사가 필요**하지만 이 과정에서 중복되는 코드가 발생
- Django에서는 반복 작업을 줄일 수 있는 Form을 제공하지만 꼭 그것을 사용할 필요는 없음
	- 직접 구현한 Form+View 로직을 사용해도 괜찮음


#### Form 선언하기
- Model 선언과 비슷하게 내가 이 Form에서 입력받고자 하는 **데이터에 대한 명세를 작성**
```python
# articles/models.py
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=50)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.title
```
```python 
# articles/forms.py
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=50)
    content = forms.CharField(widget=forms.Textarea)
```

#### Form 적용하기
- 새 글을 작성할 때 적용
```python
# articles/views.py
from.forms import ArticleForm
⁝
def new(request):
	form = ArticleForm()
	context = {
		"form": form,
	}
	return render(request, "new.html", context)
⁝
```
```html
<!-- articles/templates/new.html -->
<h1>New Articles</h1>

<form action="{% url 'create' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}  <!-- form 적용 -->
	<button type="submit">저장</button>
</form>
```

- Form rendering options
	- label과 input을 렌더링하는 것에 대한 옵션
		- as_div: \<div> 태그로 감싸져서 렌더링
		- as_p: \<p> 태그로 감싸져서 렌더링
	- 공식 문서: [Form Rendering Options](https://docs.djangoproject.com/en/4.2/topics/forms/#form-rendering-options)

- Form Widget
	- 웹 페이지에서 Form Input 요소가 어떻게 렌더링 되어서 보여질지 정의
	- Form Fields에 할당해서 사용
	- 공식 문서: [Form Widget]([https://docs.djangoproject.com/en/4.2/topics/forms/#widgets](https://docs.djangoproject.com/en/4.2/ref/forms/widgets/#module-django.forms.widgets))


### Django Model Form
- Django Form이 Model과 상당히 유사 -> 누가 자동으로 만들어줄 수 없나?
- ModelForm Class: Django가 알아서 Model을 참조해 Model Field를 보고 Form을 제작

#### Model을 통해서 Form Class를 만드는 방식
- ModelForm이 사용할 데이터를 Meta 클래스에 명시
- fields 항목에 내가 form으로 만들고 싶은 항목들을 지정 가능
```python
# articles/forms.py
from django import forms
from articles.models import Article

class ArticleForm(forms.ModelForm):
    # '어떤 모델을 가져와서 Form을 만들어야 하는가?'를 Meta class에 정의 
    class Meta:
        model = Article  # 가져올 model 
        fields = "__all__"  # model에서 가져올 속성들 
        # exclude = ["created_at", "updated_at"]  # fields 중에서 제외할 것들 

```
```python
# articles/views.py
⁝
def create(request):
  form = ArticleForm(request.POST) # form에 request.POST에 있는 데이터 채워
  if form.is_valid(): # form 형식에 맞으면
      article = form.save() # 저장하고 해당 객체 반환 
      return redirect("article_detail", article.id)
  return redirect("new")
⁝
```

#### 유사한 함수들을 병합하기
1. new - create 수정하기
	- `views.py`의 `new`함수와 `create`가 상당히 유사하게 생김
		- `new`함수: request.method == GET 이면 새로운 빈 Form을 만들어 보여줌
		- `create` 함수: request.method == POST이면 값이 채워진 Form을 새롭게 만들어 저장
		- request 요청에 따라 Form을 만드는 것이 유사함
	- `new`함수를 `create`에 병합
		- `views.py`에서 `new`함수 지우기
		- `urls.py`에서 `new`로 가는 path 지우기
		- `new.html`의 이름을 `create.html`로 바꾸기

```python
# new함수를 create함수에 병합 
def create(request):
    # 기존 create 함수 부분 
    if request.method == "POST":
        form = ArticleForm(request.POST)  # 데이터가 바인딩된(값이 채워진) Form 
        if form.is_valid():  # Form이 유효하다면 데이터를 저장하고 다른 곳으로 redirect 
            article = form.save()
            return redirect("article_detail", article.pk)
    # 기존 new 함수 부분 
    else:
        form = ArticleForm()

    context = {"form": form}
    return render(request, "create.html", context)
```
```html
<!-- articles.html에서도 new url을 create url로 변경 -->
<h1>Articles</h1>

<a href="{% url 'create' %}"><button>Create New Article</button></a>

{% for article in articles %}
	<!-- article_detail에 필요한 인자인 article.pk를 같이 넘겨줌 -->
	<a href="{% url 'article_detail' article.pk %}">
		<p>[ {{ article.pk }} ] {{ article.title }}</p>
	</a>
	<br/>
{% endfor %}
```
```html
<!-- new.html을 create.html로 변경 -->
<h1>New Articles</h1>

<form action="{% url 'create' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}  <!-- form을 p 태그로 감싸겠다는 의미 -->
	<button type="submit">저장</button>
</form>

<br/>
<a href="{% url 'articles' %}">목록 보기</a>
```

2. edit - update 수정하기
	- `views.py`의 `edit`함수와 `update`가 상당히 유사하게 생김
		- `edit`함수:  GET 이면 article값으로 채워진 Form을 만들어 내용 수정
		- `update` 함수: POST이면 article값으로 채워진 Form을 만들어 수정된 내용 저장 
		- request 요청에 따라 article값으로 채워진 Form을 만들어 내용을 수정하는 것이 유사
	- `edit`함수를 `update`에 병합
		- `views.py`에서 `edit`함수 지우기
		- `urls.py`에서 `edit`로 가는 path 지우기
		- `edit.html`의 이름을 `update.html`로 바꾸기
```python
# edit함수를 update함수에 병합 
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == "POST":
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            article = form.save()
            return redirect("article_detail", article.pk)
    else:
        form = ArticleForm(instance=article)  # article의 값을 가지는 객체로 채워진 Form 
    context = {
        "form": form,
        "article": article,
    }
    return render(request, "update.html", context)
```
```html
<!-- article_detail.html에서도 edit url을 update url로 변경 -->
<h1>Article Detail</h1>
⁝
<a href="{% url 'articles' %}">목록보기</a><br/>
<a href="{% url 'update' article.pk %}"><button>수정</button></a>
```
```html
<!-- edit.html을 update.html로 변경 -->
<h1>Update Article</h1>

<form action="{% url 'update' article.pk %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">수정</button>
</form>

<br/>
<a href="{% url 'article_detail' article.pk %}">이전으로</a>
```


---
## 17. URL Namespace
### URL Namespace
- Django는 서로 다른 앱에서 동일한 URL Name을 사용하는 경우, 고유하게 구분할 수 있도록 namespace를 지원
- url에 이름 공간을 만들어주고 나면, `namespace:url_name`형태로 사용
- 처음부터 `namespace`를 지정하고 프로젝트를 진행하는 것을 권장
- `namespace`없이 경로를 명시했던 경우라면 앞에 `namespace`를 추가
	- `{% url 'url_name' %} -> {% url 'namespace:url_name' %}`
	- `redirect('url_name') -> redirect('namespace:url_name')`

```python
# articles/urls.py
from django.urls import path
from . import views

app_name = "articles"  # add namespace

urlpatterns = [
	...
    path("hello/", views.hello, name="hello"),
    ...
]
```


### Templates 구조
- 서로 다른 앱에서 같은 이름의 html 파일이 있는 경우, Django는 `settings.py`의 `INSTALLED_APPS`의 순서에 따라 가장 먼저 검색된 html 파일을 보여줌
- 매번 그 순서를 신경쓰며 작업할 수 없으므로 Templates Namespace를 사용
- 처음부터 `<app_name>/templates/<app_name>`으로 Templates 구조를 만들고, Templates을 사용할 때는 `<app_name>/<template.html>`으로 사용
	- `render(request, 'articles/index.html')`


---
## 18. Django Auth
### Auth
- 인증(Authentication)과 권한(Authorization)을 합쳐서 Auth라고 대개 인증시스템이라고 명명
	- 인증(Authentication) : 내가 누구인지를 입증하는 것
	- 권한(Authorization) : 수행할 수 있는 자격 여부


### 쿠키(Cookie)와 세션(Session)
#### 만약 쿠키와 세션이 없다면?
- 이전의 요청을 기억하지 못하기 때문에, 요청을 보낼 때마다 매번 로그인이 필요
- client와 server가 서로를 기억하기 위해 필요함

#### 쿠키
- 서버 → 웹 브라우저에 전달하는 작은 데이터 조각
    - 유저가 웹을 방문하게 되면 서버로부터 쿠키를 전달받음
- Key-Value 형태로 쿠키 데이터가 유저의 로컬에 저장
- 이후 동일한 서버에 보내는 모든 요청에 쿠키가 함께 전달
- 그냥 문자열이기 때문에 수정하는 것이 간편함

#### 세션
- 쿠키는 수정이 간편해서 보안에 취약 → 특정 쿠키가 이전에 해당 서버에서 발행한건지 확인하는데 사용하는 데이터
- 서버와 클라이언트 간의 **상태(State)** 를 기억하기 위한 것

- 세션과 쿠키가 쓰이는 방법
	1) 클라이언트가 서버에 접속
	2) 서버가 특정 session id를 발급하고 기억
	3) session id 전달받아 쿠키에 저장
	4) 이후 클라이언트는 해당 쿠키를 이용해서 요청
	5) 서버에서는 쿠키에서 session id를 꺼내서 검증
	6) 검증에 성공했다면 알맞은 로직을 처리

→ 쿠키에 민감한 정보를 저장할 필요 없이 session id만 저장하고 서버에서 검증하는 방식으로 사용
→ 로그인은 이러한 절차로 구현됨

- 쿠키의 수명
    - 세션쿠키 (Session Cookie)
        - 현재의 세션이 종료되면(브라우저가 닫히면) 삭제됨
    - 지속쿠키 (Persistent Cookie)
        - 디스크에 저장되며 브라우저를 닫거나 컴퓨터를 재시작해도 남아있음
        - Max-Age를 지정하여 해당 기간이 지나면 삭제가 가능

- Django의 Session과 Auth
	- `settings.py`의 MIDDLEWARE에서 알아서 처리해줌


### Django의 Authentication System
#### 로그인 구현하기
- 로그인 == Session을 Create하는 로직
- Django는 이 과정을 전부 내부적으로 처리할 수 있는 기능을 제공하기 때문에 직접 session에 대한 로직을 작성할 필요는 없음

- Authentication Form
	- Django의 Built-in Form
	- 로그인을 위한 기본적인 form을 제공

- login()
	- 개발자가 직접 구현하지 않아도 login()함수 하나만 사용하면 로그인 구현 가능
	- 사용자 로그인 처리를 해주고 내부적으로 session을 사용해서 user 정보를 저장

#### 로그아웃 구현하기
- 로그아웃 == 서버의 세션 데이터를 지우는 것

- logout()
	- login()과 마찬가지로 logout()을 사용해 간단하게 기능 구현이 가능
	- 현재 request에서 가져온 session을 사용하여 DB에서 삭제
	- 클라이언트 쿠키에서도 삭제


#### HTTP 요청을 처리하는 다양한 방법
- View Decorators
	- `require_http_methods()`: 특정한 method 요청에 대해서만 허용
	- `require_POST()`: POST 요청만 허용 (`require_http_methods()`의 짧은 버전)

- 접근 제한하기
	- `is_authenticated`: 주로 `request.user`가 Auth가 있는 지 확인
	- `@login_required`
		- 로그인이 안된 상태로 접근하면 `settings.LOGIN_URL`에 설정된 경로로 이동
		- 기본 값: `/accounts/login`
		- 로그인이 되어 있다면 로직을 실행
	- 로그인 성공 시 이전 페이지로 자동으로 이동
		- 쿼리스크링에 `next`로 저장

```python
# accounts/views.py
from django.shortcuts import render, redirect
from django.contrib.auth import login as auth_login
from django.contrib.auth import logout as auth_logout
from django.contrib.auth.forms import AuthenticationForm
from django.views.decorators.http import require_POST, require_http_methods

@require_http_methods(["GET", "POST"])  # GET과 POST가 들어올 때만 login 실행 
def login(request):
    if request.method == "POST":
        form = AuthenticationForm(data=request.POST)
        if form.is_valid():
            auth_login(request, form.get_user())  # 실제 로그인 처리를 진행하는 부분 
            
            # 로그인 성공하면 가려던 곳(next)으로 가고, 아니면 index로 이동 
            next_path = request.GET.get("next") or "index"
            return redirect(next_path)
    else:
        form = AuthenticationForm()
    context = {"form": form}
    return render(request, "accounts/login.html", context)

@require_POST  # POST 요청이 들어올 때만 logout 함수 실행 
def logout(request):
    if request.user.is_authenticated:
        auth_logout(request)  # 실제 로그아웃 처리를 진행하는 부분 
    return redirect("articles:index")
```


---
## 19. 회원기능 구현하기
### 회원가입
- Django의 기본 회원가입 ModelForm: `UserCreationForm`
	- `username`과 `password`로 새로운 user를 생성하는 ModelForm
	- `username`, `password1`, `password2`를 가짐

#### 구현 결과
```python
# accounts/urls.py
urlpatterns = [
    ...
    path("signup/", views.signup, name="signup"),
    ...
]
```
```python
# accounts/views.py
from django.contrib.auth.forms import UserCreationForm

@require_http_methods(["GET", "POST"])
def signup(request):
    if request.method == "POST":
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()  # 새로운 유저 회원가입 정보 저장 
            auth_login(request, user)  # 회원가입과 동시에 새 회원 정보로 로그인 
            return redirect("articles:index")
    else:
        form = UserCreationForm()
    context = {"form": form}
    return render(request, "accounts/signup.html", context)
```
```html
<!-- accounts/signup.html -->
<h1>Sign up</h1>

<form action="{% url 'accounts:signup' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">Sign up</button>
</form>
```
```html
<!-- base.html -->
...
 {% else %}
	<a href="{% url 'accounts:login' %}"><button>Login</button></a>
	<a href="{% url 'accounts:signup' %}"><button>Sign Up</button></a>
{% endif %}
...
```


### 회원탈퇴
- DB에서 User를 지우는 로직
- 회원 탈퇴 → 세션 삭제 순서에 유의

#### 구현 결과
```python
# accounts/urls.py
urlpatterns = [
    ...
    path("delete/", views.delete, name="delete"),
    ...
]
```
```python
# accounts/views.py
@require_POST
def delete(request):
    if request.user.is_authenticated:
        request.user.delete()  # 회원 탈퇴 
        auth_logout(request)  # 세션 지우기 
    return redirect("articles:index")
```
```html
<!-- base.html -->
{% if request.user.is_authenticated %}
	...
	<form action="{% url 'accounts:delete' %}" method="POST">
		{% csrf_token %}
		<input type="submit" value="Drop out"></input>
	</form>
{% else %}
```


### 정보 수정
- Django의 기본 정보 수정 ModelForm: `UserChangeForm`
- `UserChangeForm`을 사용하면 user가 수정하면 안될 정보까지 보임
	- 상속을 통해 `UserChangeForm`의 필요한 기능만 Form 커스텀
	- 커스텀한 Form의 이름 설정: `CustomUserChangeForm`

#### CustomUserChangeForm 만들기
- get_user_model()
	- 현재 프로젝트에서 활성화 되어있는 유저모델을 반환
	- 직접 User 모델을 import 할 수 있지만 `get_user_model` 을 사용하기를 권장
	- Django는 다중 User 모델을 지원하므로 확장에 용이
	- 프로젝트의 유연성과 확장성을 높여줌

- Class Meta의 fields에 들어갈 수 있는 값들: [fields](https://docs.djangoproject.com/en/4.2/ref/contrib/auth/#user-model)

#### 구현 결과
```python
# accounts/urls.py
urlpatterns = [
    ...
    path("update/", views.update, name="update"),
    ...
]
```
```python
# accounts/forms.py
from django.contrib.auth.forms import UserChangeForm
from django.contrib.auth import get_user_model

class CustomUserChangeForm(UserChangeForm):
    class Meta:
        model = get_user_model()  # 현재 프로젝트에서 활성화 되어있는 유저를 반환 
        fields = [
            "first_name",
            "last_name",
            "email",
        ]
```
```python
# accounts/views.py
from .forms import CustomUserChangeForm

@require_http_methods(["GET", "POST"])
def update(request):
    if request.method == "POST":
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect("articles:index")
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {"form": form}
    return render(request, "accounts/update.html", context)
```
```html
<!-- accounts/update.html -->
<h1>Update Account</h1>

<form action="{% url 'accounts:update' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">Update</button>
</form>
```
```html
<!-- base.html -->
{% if request.user.is_authenticated %}
	...
	<a href="{% url 'accounts:update' %}"><button>User Update</button></a>
	...
{% else %}
```


### 비밀번호 변경
- Django의 기본 user 비밀번호 변경 ModelForm: `PasswordChangeForm`
- 상속받고 있는 `UserChangeForm`을 보면, 비밀번호를 수정하는 경로를 미리 적어둠
	- 비밀번호 수정 경로를 커스텀하기 위해, 경로가 적힌 부분을 가져와 `CustomUserChangeForm`에서  Overriding
- 비밀번호를 변경하면 기존 세션의 인증 정보와 일치하지 않아 자동으로 로그아웃 됨
	- `update_session_auth_hash()`함수를 사용해 로그인 상태를 유지

#### 구현 결과
```python
# accounts/urls.py
urlpatterns = [
    ...
    path("password/", views.change_password, name="change_password"),
    ...
]
```
```python
# accounts/forms.py
from django.urls import reverse

class CustomUserChangeForm(UserChangeForm):
    class Meta:
        model = get_user_model()  # 현재 프로젝트에서 활성화 되어있는 유저를 반환 
        fields = [
            "first_name",
            "last_name",
            "email",
        ]
    
    #  비밀번호 수정 경로를 커스텀 할 수 있도록 UserChangeForm의 __init__을 Overriding
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        if self.fields.get("password"):
            # reverse: url_name으로부터 url을 찾아가는 함수 
            password_help_text = (
                "You can change the password " '<a href="{}">here</a>.'
            ).format(f"{reverse('accounts:change_password')}")
            self.fields["password"].help_text = password_help_text
```
```python
# accounts/views.py
from django.contrib.auth import update_session_auth_hash
from django.contrib.auth.decorators import login_required
from django.contrib.auth.forms import (
    AuthenticationForm,
    UserCreationForm,
    PasswordChangeForm,
)

@login_required
@require_http_methods(["GET", "POST"])
def change_password(request):
    if request.method == "POST":
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            form.save()
            update_session_auth_hash(request, form.user)
            return redirect("articles:index")
    else:
        form = PasswordChangeForm(request.user)
    context = {"form": form}
    return render(request, "accounts/change_password.html", context)
```
```html
<!-- accounts/change_password.html -->
<h1>Change Password</h1>

<form action="{% url 'accounts:change_password' %}" method="POST">
	{% csrf_token %}
	{{ form.as_p }}
	<button type="submit">Change Password</button>
</form>
```


---

## 20. Django Static & Media
### Static Files
- 정적 파일(멈춰있는 파일)을 의미
- 서비스 로직에서 별도의 처리 없이 보여주기만 하면 되는 파일을 의미
	- 로고, 광고 이미지 등 서비스 이미지 파일
	- javascript file, CSS file 등
- 무조건 주기만 하면 되는 파일들이기 때문에 모아서 따로 제공 가능
- 각 앱의 `static` 디렉토리: **`<app_name>/static/<app_name>`**

- **`STATIC_URL`**: static 파일을 참조할 때 사용할 url
	- 개발 단계에서는 `app/static` 경로 및 settings의 `STATICFILES_DIRS`에 정의된 경로 참조
	- 실제 디렉토리 경로가 아님에 주의 (URL로만 존재하는 경로)

- **`STATIC_ROOT`**: 배포를 위해 정적 파일을 수집하는 디렉토리의 절대경로
	- Django 프로젝트에서 사용하는 모든 정적 파일을 이곳으로 모아서 적용
		- 단, `DEBUG=True`인 경우엔 동작 X (개발 단계라고 인식)
	- 추후 배포시 모든 정적파일을 다른 웹 서버가 직접 제공하기 위함


### Media Files
- 유저가 웹에서 업로드한 모든 파일 (static file을 제외 모든 파일)

- **`MEDIA_URL`**: 미디어를 처리를 위한 URL

- **`MEDIA_ROOT`**
	- 업로드한 파일이 저장되는 디렉토리 경로를 지정
	- 업로드 파일은 데이터베이스에 저장되지 않으며 실제 저장되는 것을 파일의 경로


### runserver
- `runserver` : ‘개발용’ 서버를 실행하는 것
- 순수 python으로 작성되었으며 django에 포함되어있어 개발을 편하게 도와주는 역할
- **절대로 개발 서버를 운영 환경에서 사용해서는 안됨**
    - 만약 `python [manage.py](<http://manage.py>) runserver`를 이용해서 배포한다면 문제가 발생
- Django는 Web Framework이며 실제 운영시에는 웹 서버를 앞쪽에 달아줘야함


---
## 21. Model Relationship (1:N)
### 1:N 관계
-  `Article`에 `Author`라는 개념을 둔다면,
    - 하나의 `Article`은 한 명의 `Author`를 가질 수 있음
    - **한 명의 `Author`는 여러개의 `Article`을 가질 수 있음 (1:N)**
- 만약 `Article`에 `Comment`라는 개념을 둔다면,
    - **하나의 `Article`은 여러개의 `Comment`를 가질 수 있음 (1:N)**
    - 하나의 `Comment`는 하나의 `Article`을 가질 수 있음


### Foreign Key
- **외래 키**를 의미
- 관계형 DB에서 한 테이블(A)의 필드 중 다른 테이블(B)의 행을 **유일하게 식별이 가능한 키**
- 테이블(A)에 설정되는 Foreign Key가 반드시 다른 테이블(B)의 Primary Key일 필요는 없으나 유일하게 식별이 가능해야함


### 정참조 & 역참조
- 정참조
	- Comment(N) → Article(1) 참조 == 정참조
	- Comment는 하나의 참조하는 Article이 존재하므로 `comment.article`로 쉽게 접근 가능 

- 역참조
	- Article(1) → Comment(N) 참조 == 역참조
	- Django는 역참조의 경우 참조하는 클래스명에 `_set`을 붙인 manager를 생성
	- manager 이름을 변경하려면 아래와 같이 변경 가능
```python
# 작성하는 댓글에 대한 모델
class Comment(models.Model):
    article = models.ForeignKey(
        Article, on_delete=models.CASCADE, related_name="comments" # manager 이름 변경
    )
    ...
```


---
