#본캠프 [[ch4_WebService]]

## 3. RESTful API와 JSON
### RESTful API
-  API란?
	- CLI (Command Line Interface) - 명령줄로 소통하는 방법
	- GUI (Graphic User Interface) - 그래픽으로 유저와 소통하는 방법
	- API (Application Programming Interface) - 프로그래밍으로 어플리케이션과 소통하는 방법

-  RESTful API란?
	- REST: 웹에 대한 소프트웨서 설계 방법론
	- 어플리케이션간 소통하는 방법에 REST적인 표현을 더한 것 → REST 원리를 따라 설계한 API


---
## 4. Response와 Serializer
### **Django Project 기본 세팅**
1. 가상환경 생성 및 활성화
2. Django 설치: `pip install django==4.2 (LTS version)`
3. `requirements.txt` 생성: `pip freeze > requirements.txt`
4. 프로젝트 생성: `django-admin startproject <프로젝트 이름> <생성 디렉토리>`
5. 앱 생성: `python manage.py startapp <앱 이름>`
	- `settings.py`의 `INSTALLED_APPS`에 생성한 앱 이름 추가
	- `<프로젝트 이름>/urls.py`에서 앱으로 향하는 url들을 분리해서 path 지정
	- `<앱 이름>/urls.py`에서 app_name을 주고 url마다 path 지정
6. `Custom UserModel` 생성
	- `accounts` 앱 생성
	- `settings.py`의 `INSTALLED_APPS`에 `accounts` 추가하고, `AUTH_USER_MODEL = "accounts.User"` 도 추가
	- `<프로젝트 이름>/urls.py`에서 앱으로 향하는 url들을 분리해서 path 지정
	- `accounts/urls.py`에서 app_name을 주고 url마다 path 지정
	- `accounts/models.py`에서 빈 User class 생성
	- `accounts/forms.py`을 생성해 `CustomUserCreationForm` 클래스 생성
7. 최초 `migration` 생성
	- `python manage.py makemigrations`
	- `python manage.py migrate`


### django seed
- 개인의 Django models에 맞게 test data를 생성해주는 라이브러리
- 적용 방법
	1) `pip install django-seed`
	2) `settings.py`에 `django_seed` 추가
	3) `python manage.py seed <앱 이름> --number=<개수>`
```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    # Third-party
    "django_seed",
    # Local
    "articles",
    "accounts",
]
```


### Response와 Serialization
1. Response
	- Response는 서버가 클라이언트 요청에 대해 반환하는 HTTP 응답
	
	- Django REST Framework에서 `Response` 객체는 JSON 형식의 데이터를 반환할 때 많이 사용
	- 주요 특징
		- Django의 기본 `HttpResponse`를 확장한 객체로, RESTful API에 적합한 JSON 응답을 줌
		- 데이터 직렬화와 콘텐츠 협상을 자동으로 처리
		- 상태 코드와 헤더를 쉽게 설정 가능
	
	- `JsonResponse`
		- JSON으로 인코딩된 response를 만드는 `HttpResponse`의 서브 클래스
		- dict 타입이 아닌 객체를 직렬화(Serialization)하기 위해서는 `safe=False`로 설정

2. Serialization
	- Python 객체를 JSON, XML 등의 포맷으로 변환하거나, 그 반대로 역직렬화(Deserialization)하는 과정
	- 데이터를 클라이언트와 서버 간에 주고받기 위한 중요한 과정
	
	- 주요 기능
		- **직렬화(Serialization)**: Python 데이터 구조(Python 객체, QuerySet 등)를 JSON 형식으로 변환
		- **역직렬화(Deserialization)**: 클라이언트로부터 받은 JSON 데이터를 Python 데이터로 변환

	- Django는 내부적으로 serializer를 제공하지만 유연하지 않고 모델 구조에 한정된 구조
	- 모델에 종속적이지 않고 유연하지만 사용하기 편한 Serializer가 필요


---
## 6. DRF Single Model CRUD
### DRF Single Model CRUD
- ModelSerializer
	- Model의 여러가지 필드들을 어떻게 직렬화해서 데이터의 포맷을 잡을지가 핵심
	- Django의 Model → ModelForm 사용과 굉장히 유사한 형태

- API 설계

| Name          | Method | Endpoint                   |
| :------------ | :----- | :------------------------- |
| Article 목록 조회 | GET    | /articles/                 |
| Article 상세 조회 | GET    | /articles/<int:article_id> |
| Article 생성    | POST   | /articles/                 |
| Article 수정    | PUT    | /articles/<int:article_id> |
| Article 삭제    | DELETE | /articles/<int:article_id> |
```python
# api_pjt/urls.py
urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/v1/articles/", include("articles.urls")),
]
```


### Article 목록 조회 (List)
- `articles/urls.py`
```python
app_name = "articles"
urlpatterns = [
    path("", views.article_list, name="article_list"),
]
```

- `articles/views.py`
```python
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .serializers import ArticleSerializer
from django.shortcuts import render, get_object_or_404
from .models import Article

@api_view(["GET", "POST"])  # DRF의 method는 api_view라는 decorator가 필요 
def article_list(request):
    if request.method == "GET":
        # 1. article들을 다 가져오기 
        articles = Article.objects.all()
        # 2. 가져온 articles를 json으로 직렬화하는 serializer 선언 
        serializer = ArticleSerializer(articles, many=True)
        # 3. data를 직렬화해서 Response로 반환 
        return Response(serializer.data)
    
    elif request.method == "POST":
        serializer = ArticleSerializer(data=request.data)
        # raise_exception=True: serializer가 유효하지 않으면 에러 발생 
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
```

- `@api_view()`
	- 아무것도 적지 않으면 `GET` 만 허용, 이외의 요청일경우 `405 Method Not Allowed`
	- DRF의 함수형 뷰의 경우 필수적으로 작성이 필요


### Article 상세 조회 (Detail)
- `articles/urls.py`
```python
app_name = "articles"
urlpatterns = [
    path("", views.article_list, name="article_list"),
    path("<int:pk>/", views.article_detail, name="article_detail"),
]
```

- `articles/views.py`
```python
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .serializers import ArticleSerializer
from django.shortcuts import render, get_object_or_404
from .models import Article

@api_view(["GET", "PUT", "DELETE"])
def article_detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    if request.method == "GET":
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    
    elif request.method == "PUT":
        # partial=True: 모든 field를 수정하는 것이 아니라 몇몇의 필드만 수정 
        serializer = ArticleSerializer(article, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
    
    elif request.method == "DELETE":
        article.delete()
        data = {"pk": f"{article.pk} is deleted."}
        return Response(data, status=status.HTTP_204_NO_CONTENT)
```

- from rest_framework import status
	- 조금 더 읽기 쉬운 status code 구성을 제공
	- 상태코드를 정수로 명시해도 되지만 아래와 같은 형식을 권장


---
## 7. DRF Class Based View
### Class Based View (CBV)
- views의 함수들을 class 안의 함수들로 정의해서 사용하는 것
- 클래스형 뷰에서는 특정 Http Method에 대한 처리를 함수로 분리 가능
    → GET요청에 대한 처리는 `get()`에서, POST 요청에 대한 처리는 `post()`에서 정의
- 클래스를 사용하기 때문에 코드의 재사용성과 유지보수성이 향상
- 기본 `APIView`외에도 여러 편의를 제공하는 다양한 내장 CBV가 존재


### **Class Based View 종류**
- `APIView`
	- DRF CBV의 베이스 클래스
- `GenericAPIView`
    - 일반적인 API 작성을 위한 기능이 포함된 클래스
    - 보통 CRUD 기능이 대부분인 상황을 위해 여러가지 기능이 미리 내장되어 있음
- `Mixin`
    - 재사용 가능한 여러가지 기능을 담고있느 클래스
    - 말그대로 여러 클래스를 섞어서 사용하기 위한 클래스
        - `ListModelMixin` - 리스트 반환 API를 만들기 위해 상속 받는 클래스
        - `CreateModelMixin` - 새로운 객체를 생성하는 API를 만들기위해 상속 받는 클래스
- `ViewSets`
    - 여러 엔드포인트(endpoint)를 한 번에 관리할 수 있는 클래스
    - RESTful API에서 반복되는 구조를 더 편리하게 작성할 수 있는 방법을 제공


### CBV 사용해보기
- ArticleListAPIView
	- `as_view()` 메서드를 사용해서 URL 패턴에 연결
```python
# articles/urls.py
app_name = "articles"
urlpatterns = [    
    path("", views.ArticleListAPIView.as_view(), name="article_list_create"),
]
```
```python
# articles/views.py
from rest_framework.views import APIView

class ArticleListAPIView(APIView):
    def get(self, request):
        # 1. article들을 다 가져오기 
        articles = Article.objects.all()
        # 2. 가져온 articles를 json으로 직렬화하는 serializer 선언 
        serializer = ArticleSerializer(articles, many=True)
        # 3. data를 직렬화해서 Response로 반환 
        return Response(serializer.data)
    
    def post(self, request):
        serializer = ArticleSerializer(data=request.data)
        # raise_exception=True: serializer가 유효하지 않으면 에러 발생 
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
```

- ArticleDetailAPIView
```python
# articles/urls.py
app_name = "articles"
urlpatterns = [    
    path("", views.ArticleListAPIView.as_view(), name="article_list_create"),
    path("<int:pk>/", views.ArticleDetailAPIView.as_view(), name="article_detail"),
]
```
```python
# articles/views.py
from rest_framework.views import APIView

class ArticleDetailAPIView(APIView):
    def get_object(self, pk):
        return get_object_or_404(Article, pk=pk)

    def get(self, request, pk):
        article = self.get_object(pk)
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

    def put(self, request, pk):
        article = self.get_object(pk)
        serializer = ArticleSerializer(article, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)

    def delete(self, request, pk):
        article = self.get_object(pk)
        article.delete()
        data = {"pk": f"{pk} is deleted."}
        return Response(data, status=status.HTTP_200_OK)
```


---
## 9. Serializer
### Article에 Comment 추가하기
- Nested Relationships
	- Serializer는 기존 필드를 override하거나 추가적인 필드를 구성 가능
	- 이때 모델 사이에 참조 관계가 있다면 해당 필드를 포함하거나 중첩 가능
- 즉, Serializer를 조작하여 Article에 Comment를 추가할 수 있음
```python
from rest_framework import serializers
from .models import Article, Comment


class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = "__all__"
        read_only_fields = ("article",)


class ArticleSerializer(serializers.ModelSerializer):
    comments = CommentSerializer(many=True, read_only=True)

    class Meta:
        model = Article
        fields = "__all__"
```


### 커스텀 필드 추가하기
- `source`
	- SerializerField의 속성으로 해당 필드를 채우는데 사용하는 속성을 지정
	- 점 표기법을 이용하여 내부 속성에 접근 가능
```python
class ArticleSerializer(serializers.ModelSerializer):
    comments = CommentSerializer(many=True, read_only=True)
    # comments_count는 직접 필드를 추가해 주는 것(Django가 알아서 추가해주는 것이 아님!)
    # source 속성을 이용하여 데이터 값을 전달
    comments_count = serializers.IntegerField(source="comments.count", read_only=True)
    
    class Meta:
        model = Article
        fields = "__all__"
```


### 응답 구조만 변경하기
- **`to_representation()`**
	- Serialization 이후 보여지는 결과에 대해 자동으로 내부적으로 불리는 함수
	- 이 메서드를 override하여 커스텀 형식으로 변경 가능
```python
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = "__all__"
        read_only_fields = ("article",)
    
    def to_representation(self, instance):
        ret = super().to_representation(instance)
        ret.pop("article")
        return ret
```


---
## 10. Token Auth with JWT
### JSON Web Token (JWT)
#### JWT 간단 개요
- Token: 랜덤하게 생긴 문자열
- **일정한 규칙을 가지고 있고 간단한 서명을 더한 문자열**로 토큰 자체에 유저에 대한 간단한 정보가 들어있는 형태
- JWT 방식으로 Auth를 처리하면 Session DB나 인증을 위한 여러가지 로직 처리가 필요 없음

#### JWT 처리 방식
1. 클라이언트가 ID/PW를 서버로 전송
2. 서버에서 ID/PW를 검증하고 유효하다면 일정한 형식으로 서명 처리된 Token을 응답
3. 이후 클라이언트는 모든 요청 헤더에 토큰을 담아 서버로 요청을 전송
4. 서버는 해당 토큰의 유효성을 검증하고 유저의 신원과 권한을 확인 후 요청을 처리

#### 세션과의 차이
- 세션 데이터베이스가 존재하지 않으며 데이터베이스가 필요 없음
- 토큰 자체가 하나의 인증 데이터
- 서버는 토큰이 유효한지만 검증하여 처리

#### 장점과 단점
- 장점
	- 서버에서 관리하는 데이터가 없으므로 복잡한 처리 로직이 필요 없음
	- 세션이나 DB 없이 유저를 인증하는 것이 가능
- 단점
	- 일방적으로 로그인을 무효화하는 등의 처리가 불가능(세션 테이블 X)
	- Token 자체가 데이터를 담고있는 정보이므로 탈취당할 시 보안이 취약

#### JWT의 구조
- `.`을 기준으로 `HEADER`.`PAYLOAD`.`VERIFY SIGNATURE` 세 부분으로 구성

- `HEADER`
	- 토큰의 타입(jwt) 또는 서명 부분의 생성에 어떤 알고리즘(alg)이 사용되었는지 등을 저장

- `PAYLOAD`
	- 토근 발급자, 토큰 대상자, 토큰 만료시간, 활성날짜, 발급시간 등등 여러 데이터가 담겨있는 부분
	- 이곳에 서비스가 유저 정보를 담아서 인증
	- 누구나 풀어볼 수 있기 때문에 민감한 정보를 담지않고 최소의 정보만 저장
	- 각각의 데이터는 Claim이라고도 하며 Key-Value 형태로 구성됨

- `SIGNATURE`
	- `HEADER` + `PAYLOAD` + 서버의 비밀키 값을 `HEADER`에 명시된 암호 알고리즘 방식으로 생성한 값
	- `PAYLOAD`의 글자 하나만 달라져도` SIGNATURE`는 완전히 다른 문자열로 변환되어 서버의 비밀키 값을 모른다면 유효한 서명값을 만들어내는 것이 불가능
	- 서버는 토큰을 받으면 `HEADER` + `PAYLOAD` + 비밀키로 생성한 서명값이 토큰의 서명값과 일치하는지를 확인하는 과정을 거쳐서 유효성 여부를 확인
	- 서명의 유효여부 + 유효기간 내의 토큰인지 확인하여 Auth 과정을 처리

#### **Access Token과 Refresh Token**
- Access Token
	- 요청할 때 인증을 위해 헤더에 포함해야하는 토큰
	- 매 요청시 보내는 토큰이므로 보안이 취약
	- 만료 기한을 짧게 잡아 만약 탈취 당해도 짧은 시간안에 유효하지 않은 토큰 형태가 되도록 함

- Refresh Token
	- Access Token이 만료되었을때 새로 Access Token을 발급받기 위한 Token
	- Access Token 보다 긴 유효 기간을 가짐
	- 주로 사용자의 기기에 저장해두고 사용되며 만약 Refresh Token까지 만료되었다면 다시 인증(로그인) 과정이 필요
	- Refresh Token의 탈취를 보완하기 위해 DB 리소스를 사용하는 다양한 방식도 존재(BlackList 등)