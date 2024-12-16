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
