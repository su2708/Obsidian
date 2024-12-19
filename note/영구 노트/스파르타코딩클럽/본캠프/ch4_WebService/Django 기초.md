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
# articles/forms.py
from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

#### Form 적용하기
- 새 글을 작성할 때 적용
```html
<!-- articles/templates/new.html -->
<h1>New Articles</h1>

    <form action="{% url 'create' %}" method="POST">
        {% csrf_token %}
        {{ form.as_p }}  <!-- form 적용 -->
        <label for="title">제목:</label>
        <input type="text", id="title", name="title", placeholder="Enter messagaes.">
        <br/>
        <br/>

        <label for="content">내용</label>
        <textarea name="content" id="content" cols="30" rows="10"></textarea>
        <br/>
        <br/>

        <button type="submit">저장</button>
    </form>
```
```python
# articles/views.py
from.forms import ArticleForm
...
def new(request):
	form = ArticleForm()
	context = {
		"form": form,
	}
	return render(request, "new.html", context)
...
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
    class Meta:
        model = Article
        fields = "__all__"
        # exclude = ["title"]
```


---
