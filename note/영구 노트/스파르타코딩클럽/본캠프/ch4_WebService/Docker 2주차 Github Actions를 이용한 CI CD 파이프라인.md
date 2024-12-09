#본캠프 [[ch4_WebService]]

## 1. Github Actions 을 활용한 CI/CD 파이프라인 (1/3)
### Github Actions
- Github에 내장된 CI/CD 도구
![[Github Actions.webp]]

- Github에 내장되어 있는 CI/CD라 github와 통합이 쉽고, CI/CD 서버가 내장 되어 CI/CD서버를 따로 구축할 필요 없으며, 일정 수준까지 가격이 무료
    - 무료 버전 : 스토리지 500MB, 월 2000분

- Github Actions 동작 방법
    - repository의 `.github/workflows` 디렉토리에 필요한 Actions 파일들을 yaml 형식으로 작성
        - 반드시 `.github/workflows` 폴더 아래에 YAML 파일이 위치
    - 작성된 actions 파일들을 github에서 자동으로 실행


### Github Actions의 CI
- test를 통과한 코드만 `develop` 브랜치와 `main` 브랜치에 merge되도록 하여 오류를 방지하고 안정적인 코드가 배포되고 버그를 빠르게 발견

- 활용 예
    - `develop` 브랜치에 merge 된 경우, gradle test 를 진행
    - `feature/**`(feature하위 ) 브랜치가 push 된 경우, gradle test 를 진행
    - 만약 gradle test가 실패한 경우, slack 등 알림을 보내 코드를 수정하도록 개발자에게 안내
        - 이메일은 자동으로 발송되도록 기본 설정
```yaml
# Actions 이름 github 페이지에서 볼 수 있다.
name: 'CI'

# Event Trigger 특정 액션 (Push, Pull_Request)등이 명시한 Branch에서 일어나면 동작을 수행한다.
on: 
    push:
        # 배열로 여러 브랜치를 넣을 수 있다.
        branches: [ develop, feature/* ]
    # github pull request 생성시
    pull_request:
        branches: 
            - develop # -를 쓴 여러 줄로 여러 브랜치를 명시하는 것도 가능

    # 실제 어떤 작업을 실행할지에 대한 명시
jobs:
  ci:
  # 스크립트 실행 환경 (OS)
  # 배열로 선언시 개수 만큼 반복해서 실행한다. ( 예제 : 1번 실행)
    runs-on: [ ubuntu-latest ] 

    # 실제 실행 스크립트
    steps: 
      # uses는 github actions에서 제공하는 플러그인을 실행.(git checkout 실행)
      - name: checkout
        uses: actions/checkout@v4

      # with은 plugin 파라미터 입니다. (java 11버전 셋업)
      - name: java setup
        uses: actions/setup-java@v2
        with:
          distribution: 'adopt' # See 'Supported distributions' for available options
          java-version: '17'

      # run은 사용자 지정 스크립트 실행
      - name: run unittest
        run: |
          ./gradlew clean test
```


### Github Actions의 CD
- 배포를 자동화하는 작업을 기술해서 빠르고 간편하게 배포
- `main` 브랜치에 코드가 통합된 경우 운영 환경에 빠르게 배포할 수 있게 함

- 활용 예
    - `main` 브랜치에 merge된 경우, `gradle test` 를 실행
	- `main` 브랜치의 코드 기준으로 `jar` 파일을 생성
	- 생성된 `jar`파일을 특정 환경(AWS, GCP 등)에 배포
```yaml
name: 'CD'

on: 
    push:
        branches: [ main ]
jobs:
  cd:
    runs-on: [ ubuntu-latest ] 

    steps: 
      - name: checkout
        uses: actions/checkout@v4

      - name: java setup
        uses: actions/setup-java@v3
        with:
          distribution: 'adopt' # See 'Supported distributions' for available options
          java-version: '17'

      - name: run unittest
        run: |
          ./gradlew clean test
		  
      - name: deploy to heroku
	      uses: akhileshns/heroku-deploy@v3.12.12
		    with:
          heroku_api_key: ${{secrets.HEROKU_API_KEY}}
          heroku_app_name: "sampleapp-github-actions" #Must be unique in Heroku
          heroku_email: "nbcdocker@proton.me"
```


### event, runner, job, step
