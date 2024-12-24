#본캠프 [[ch4_WebService]]

## 4. Response와 Serializer
### Django Project 기본 세팅
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

1. `pip install django-seed`
2. `settings.py`에 `django_seed` 추가
```python

```