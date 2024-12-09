#본캠프 [[ch4_WebService]]

## 01. CI/CD란 무엇인가
### CI/CD의 정의
![[CI_CD.webp]]
- Continuous Integration / Continuous Deployment(Delievery)의 약자로, 지속적인 통합과 지속적인 제공을 의미

- 기본 개념
    - 지속적인 통합(Continuous Integration)
    - 지속적인 서비스 제공 (Continuous Delivery)
    - 지속적인 배포(Continuous Deployment)

- 지속적인 통합(Continuous Integration) : 코드의 지속적인 통합
    - 자동화된 빌드와 자동화된 테스트를 제공
    - 안정적인 코드를 빠르게 제공할 수 있는 밑거름

- 지속적인 서비스 제공(Continuous Delivery)

- 지속적인 배포(Continuous Deployment)
    - 배포를 자동화하여 배포 시간을 단축하고 코드 결과물을 빠르게 지속적으로 제공

- 단계
    - **코드 작성**: 개발자들은 소스 코드를 작성하고 저장소(repository)에 업로드
    - **빌드**: 저장소에서 최신 소스 코드를 가져와 빌드를 수행. 빌드는 소스 코드를 컴파일하고, 라이브러리를 추가하고, 필요한 파일을 생성하는 과정.
    - **테스트**: 빌드된 결과물을 대상으로 테스트를 수행. 테스트는 기능이 정상적으로 작동하는지 확인하고, 버그를 발견하고 수정하는 과정.
    - **배포**: 테스트를 통과한 결과물을 배포. 배포는 서버에 업로드하거나, 사용자에게 제공하는 과정


### 현대적인 개발 과정
- 애자일 개발
	- 특정 주기마다 개발, 테스트 및 프로덕션에 통합된 기능을 출시
	![[agile.svg]]

- Docker를 통해 서버를 표준화하고 같은 환경에서 테스트 및 배포 테스트를 진행하고 이 과정을 자동화
    - 테스트로 검증된 자동화 배포를 사용하여 실패 확률 저하

- 자동화된 과정으로 지속적으로 코드를 통합하여 지속적으로 자동 배포

- 컨테이너와 빌드/테스트 도구의 발전에 따라 Docker가 테스트 뿐만 아니라 실제 배포도 담당
![[Kubernetes.webp]]


---
## 02. Docker 기초
### 왜 Docker 인가
- 애플리케이션 개발과 배포가 편함
	- Docker Container 내부에서 여러 소프트웨어를 설치해도 호스트 OS에는 영향이 없음
	- CI/CD에서 지속적인 통합 과정의 테스트에서 Docker를 활용
	- 어떤 서버에 올리더라도 같은 환경으로 구성된 컨테이너로 동작하기 때문에 표준화된 배포를 구성 가능
- 여러 애플리케이션의 독립성과 확장성이 높아짐
- Docker가 가상화에서 사실상 표준의 위치를 차지


---
## 03. Docker Image 관리
### docker image 이해와 구조 확인
- Docker 이미지 이해
    - Docker Container 서비스를 위한 이미지는 Container 런타임에 필요한 바이너리, 라이브러리 및 설정 값 등을 포함하고, 변경되는 상태값을 보유하지 않고(stateless) 변하지 않음(Immutable, Read-Only)
    - **상태 저장 없음(Stateless)**: 애플리케이션과 관련된 모든 파일과 라이브러리를 포함하고 있기 때문에, 다른 환경에서도 동일한 애플리케이션을 실행 가능
    - **불변성(Immutable)**: 이미지가 한번 생성되면 변경할 수 없는 것을 의미
    - 도커 이미지는 필요한 파일만 포함하고 있기 때문에, 용량이 작으며, 이미지를 변경할 필요가 있을 경우에는 새로운 이미지를 생성 필요
	![[Docker Image.webp]]

- `docker pull`: Docker 이미지 내려받기
    ![[Docker-hub-registry.webp]]
    - [hub.docker.com](http://hub.docker.com/) 에서 이미지를 제공받거나 해당 사이트로 이미지를 제공
    - Private Registry 서버를 통해 이미지를 제공받거나 제공 가능

    - 예제
        ```bash
        # docker [image] pull [options] name:[tag]
        
        # 최초에는 docker.io가 default registry로 설정됨.
        docker pull debian[:latest]
        docker pull library/debian:10
        docker pull docker.io/library/debian:10
        docker pull index/docker.io/library/debian:10
        docker pull nginx:latest
        
        # private registry 나 클라우드 저장소의 이미지를 받는 경우
        docker pull 192.168.0.101:5000/debian:10 # 현재는 실제로 동작하지 않음
        docker pull gcr.io/google-samples/hello-app:1.0
        ```
        
- `docker image inspect`: Docker 이미지 구조 확인
    ```bash
    docker image inspect nginx:latest
    [
        {
            "Id": "sha256:593aee2afb642798b83a85306d2625fd7f089c0a1242c7e75a237846d80aa2a0",
            "RepoTags": [
                "nginx:latest"
            ],
            "RepoDigests": [
                "nginx@sha256:add4792d930c25dd2abf2ef9ea79de578097a1c175a16ab25814332fe33622de"
            ],
            "Parent": "",
            "Comment": "",
            "Created": "2023-10-25T01:21:47.343274012Z",
            "Container": "1e4063a23e5d6d56cbf5478ff7227b8c6940152770a0770585c3ae9480478b66",
            "ContainerConfig": {
                "Hostname": "1e4063a23e5d",
                "Domainname": "",
                "User": "",
                "AttachStdin": false,
    …
    ```
    
- `docker image inspect`: 생성된 이미지의 내부 구조 정보를 json 형태로 제공
    ```bash
    docker image inspect --format="{{.Os}}" nginx:latest
    linux
    docker image inspect --format="{{.RepoTags}}" nginx:latest
    [nginx:latest]
    docker image inspect --format="{{.ContainerConfig.ExposedPorts}}" nginx:latest
    map[80/tcp:{}]
    docker image inspect --format="{{.RepoTags}} {{.Os}}" nginx:latest
    [nginx:latest] linux
    ```
    
- `docker image history`: `Dockerfile`에 대한 정보
    - 여러 개의 계층 구조로 구성
    ```bash
    docker image history nginx:latest
    IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
    593aee2afb64   4 days ago    /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
    <missing>      4 days ago    /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
    <missing>      4 days ago    /bin/sh -c #(nop)  EXPOSE 80                    0B
    <missing>      4 days ago    /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
    <missing>      4 days ago    /bin/sh -c #(nop) COPY file:9e3b2b63db9f8fc7…   4.62kB
    <missing>      4 days ago    /bin/sh -c #(nop) COPY file:57846632accc8975…   3.02kB
    <missing>      4 days ago    /bin/sh -c #(nop) COPY file:3b1b9915b7dd898a…   298B
    <missing>      4 days ago    /bin/sh -c #(nop) COPY file:caec368f5a54f70a…   2.12kB
    <missing>      4 days ago    /bin/sh -c #(nop) COPY file:01e75c6dd0ce317d…   1.62kB
    <missing>      4 days ago    /bin/sh -c set -x     && groupadd --system -…   112MB
    <missing>      4 days ago    /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bookworm   0B
    <missing>      4 days ago    /bin/sh -c #(nop)  ENV NJS_VERSION=0.8.2        0B
    <missing>      4 days ago    /bin/sh -c #(nop)  ENV NGINX_VERSION=1.25.3     0B
    <missing>      2 weeks ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
    <missing>      2 weeks ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
    <missing>      2 weeks ago   /bin/sh -c #(nop) ADD file:55ad846fa191e603f…   74.8MB
    ```
    
- `docker pull`: 이미지가 계층 구조인 것을 확인
    - 다운로드된 계층들 정보는 전용 경로에 저장
    ```bash
    docker pull nginx:latest
    latest: Pulling from library/nginx
    a378f10b3218: Pull complete
    5b5e4b85559a: Pull complete
    508092f60780: Pull complete
    59c24706ed13: Pull complete
    1a8747e4a8f8: Pull complete
    ad85f053b4ed: Pull complete
    3000e3c97745: Pull complete
    Digest: sha256:add4792d930c25dd2abf2ef9ea79de578097a1c175a16ab25814332fe33622de
    Status: Downloaded newer image for nginx:latest
    docker.io/library/nginx:latest
    ```
    
- Docker login/logout : hub.docker.com에서 회원가입 후 실행
    - 예제
    ```bash
    docker login
    Log in with your Docker ID or email address to push and pull images from Docker Hub. If you dont have a Docker ID, head over to <https://hub.docker.com/> to create one.
    You can log in with your password or a Personal Access Token (PAT). Using a limited-scope PAT grants better security and is required for organizations using SSO. Learn more at <https://docs.docker.com/go/access-tokens/>
    
    Username: {자신의 계정}
    Password: {자신의 암호}
    Login Succeeded
    
    $ docker logout
    Removing login credentials for <https://index.docker.io/v1/>
    ```
    
    - 명령을 통해 등록 후 관리되는 Access Token 리스트
        -  [https://hub.docker.com/settings/security](https://hub.docker.com/settings/security)
        ![[hub_docker_security.webp]]
        
- Docker Desktop 확인
    ![[check_docker_desktop.webp]]
    

## 04. Docker Container와 Container 를 다루는 CLI
### Docker Image와 Docker Container 사이의 관계
- Docker Image와 Docker Container의 관계
    
    - Image: 컨테이너에 대한 OS, Application, Library 등등의 정보를 담고 있음
    - Container: Image를 실행한 상태. 1개의 Image로 부터 N개의 Container를 생성할 수 있는 1:N의 관계.
        - Image는 내가 만들고 싶은 붕어빵의 정보를 갖고 있는 틀이라면, Container는 구워낸 붕어빵
    ![[DockerImage_DockerContainer.webp]]
    

### Docker Container를 다루는 명령어
- Docker 이미지/Container 관련 명령어
	![[Container 명령어.webp]]
    [https://segmentfault.com/a/1190000005802339](https://segmentfault.com/a/1190000005802339)
    
- Docker 생애 주기(Lifecycle)
    ![[Docker_Lifecycle.webp]]
    [https://docker-saigon.github.io/post/Docker-Internals/](https://docker-saigon.github.io/post/Docker-Internals/)
    
- Docker Container 수동 생성
    ```bash
    docker pull ubuntu:22.04
    docker images
    
    # docker create 은 실제 실행하지 않고 컨테이너 생성만
    docker create –ti --name ubuntu2204test ubuntu:22.04
    docker ps –a
    CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS    PORTS     NAMES
    2ccc1b2a1144   ubuntu:22.04   "/bin/bash"   4 seconds ago   Created             ubuntu2204test
    
    docker start ubuntu2204test
    Ubuntu2204test
    docker attach ubuntu2204test
    
    # docker run 은 create/start/attach 를 순차적으로 한 번에 실행하는 것과 같음 
    docker run -ti --name=ubuntu2204test2 ubuntu:22.04 /bin/bash
    root@57a1a1c759b6:/#
    ```
    
- Docker Container 는 프로세스
    ```bash
    docker run -ti --name=ubuntu2204test3 ubuntu:22.04 /bin/bash
    root@1cd125b32870:/#
    
    # 터미널을 한 개 더 열고 
    ps -ef | grep ubuntu2204test3
    user   9710  7637  0 17:17 pts/4    00:00:00 docker run -ti --name=ubuntu2204test3 ubuntu:22.04 /bin/bash
    user   9921  9377  0 17:17 pts/5    00:00:00 grep --color=auto ubuntu2204test3
    ```
    
- Container 명령 테스트
    - 샘플 파일
        [container-sample.zip](https://prod-files-secure.s3.us-west-2.amazonaws.com/83c75a39-3aba-4ba4-a792-7aefe4b07895/70d5dd95-9e85-4ee2-a6cb-803b38652ab0/container-sample.zip)
        
    - 컨테이너 명령 테스트  
    ```bash
    cd ~
    mkdir nodejsapp
    cd nodejsapp
    vi app.js # 테스트용 nodejs 앱
    vi Dockerfile # 새로운 도커 이미지를 위한 Dockerfile
    docker buildx build -t node-test:1.0 . # 1.0 태그를 추가하여 node-test라는 이미지를 빌드 
    docker images | grep node-test  # 빌드 완료한 이미지 보기
    docker image history node-test:1.0 # 1.0으로 태그 추가한 이미지의 Dockerfile history
    docker run -itd -p 6060:6060 --name=node-test -h node-test node-test:1.0
    docker ps | grep node-test
    curl <http://localhost:6060>
    ```
    
- docker run 자주 사용하는 옵션
    - `-d`: detached mode; 백그라운드 모드
    - `-p`: 호스트와 컨테이너의 포트를 연결(포워딩)
    - `-v`: 호스트와 컨테이너의 디렉토리를 연결(마운트)
    - `-e`: 컨테이너 내에서 사용할 환경변수 설정
    - `-name`: 컨테이너 이름 설정
    - `-rm`: 프로세스 종료 시 컨테이너 자동 삭제
    - `-ti`: -i 와 -t 를 동시에 사용한 것으로 터미널 입력을 위한 옵션

- 실행 중인 Container에 대한 정보
    ```bash
    # 컨테이너에서 실행 중인 프로세스 조회
    docker top node-test 
    UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
    root                2398                2378                0                   08:37               ?                   00:00:00            /sbin/tini -- node runapp.js
    root                2421                2398                0                   08:37               ?                   00:00:00            node runapp.js
    
    # 컨테이너에 매핑된 포트 조회
    docker port node-test
    8080/tcp -> 0.0.0.0:8080
    
    # 컨테이너 리소스 통계 출력 (1회)
    docker stats node-test --no-stream
    CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O        BLOCK I/O   PIDS
    14c475f7ac09   node-test   0.01%     9.035MiB / 15.45GiB   0.06%     1.5kB / 518B   0B / 0B     11
    
    # 컨테이너 리소스 통계 출력 (스트림)
    docker stats node-test
    ```
    
- `docker logs`
    ```bash
    # 표준 출력(stdout), 표준에러(stderr) 출력
    docker logs node-test 
    …
     
    
    # 로그를 계속 출력
    docker logs –f node-test
    …
    …
    
    # 출력된 로그는 파일로 관리되기 때문에 HostOS 의 disk 를 사용
    docker info | grep -i log
    ```
    
- `docker [container] inspect`
    ```bash
    # 컨테이너 내부 확인
    docker inspect node-test
    [
        {
            "Id": "2ccc1b2a114495e2d0b67f84e55c9c21e79c6bfff6355ae1fc2caa5225698bba",
            "Created": "2023-10-29T08:04:36.295616146Z",
            "Path": "/bin/bash",
            "Args": [],
            "State": {
                "Status": "running",
                "Running": true,
                "Paused": false,
                "Restarting": false,
                "OOMKilled": false,
                "Dead": false,
                "Pid": 1814,
                "ExitCode": 0,
                "Error": "",
                "StartedAt": "2023-10-29T08:05:15.974255879Z",
                "FinishedAt": "0001-01-01T00:00:00Z"
            },
            "Image": "sha256:e4c58958181a5925816faa528ce959e487632f4cfd192f8132f71b32df2744b4",
            "ResolvConfPath": "/var/lib/docker/containers/2ccc1b2a114495e2d0b67f84e55c9c21e79c6bfff6355ae1fc2caa5225698bba/resolv.conf",
            "HostnamePath": "/var/lib/docker/containers/2ccc1b2a114495e2d0b67f84e55c9c21e79c6bfff6355ae1fc2caa5225698bba/hostname",
    ```
    
- `docker stop` | `start` | `pause` | `unpause`
    ```bash
    # 터미널1, 도커 상태 확인
    docker stats
    
    # 터미널2, 도커 프로세스 이벤트 확인
    docker events
    
    # 터미널3, docker start
    docker stop node-test
    docker ps –a
    docker start node-test
    
    # 
    docker pause node-test
    docker unpause node-test
    docker ps -a
    ```
    
- `docker exit code`
    - 0
        - Docker Process가 수행해야 할 모든 Command 또는 Shell을 실행하고 정상 종료
    - 255
        - Docker Image에 정의된 EntryPoint 또는 CMD가 수행이 완료되었을 경우 발생
    - 125
        - Docker run 명령어의 실패로 실제 docker process가 기동되지 않음
    - 126
        - Docker Container 내부에서 Command를 실행하지 못할 경우 발생
    - 127
        - Docker Container 내부에서 Command를 발견하지 못하였을 경우 발생
    - 137
        - kill -9로 인해 종료 됨
    - 141
        - 잘못된 메모리 참조하여 종료 됨
    - 143
        - Linux Signal로 정상 종료 됨
    - 147
        - 터미널에서 입력된 정지 시그널로 종료 됨
    - 149
        - 자식 프로세스가 종료 되어 종료 됨


### Docker Container를 정리하는 방법
- `docker container prune`
    - 실행 중이 아닌 모든 컨테이너를 삭제
    ```bash
    # 중지된 컨테이너를 포함하여 모든 컨테이너 리스트
    docker container ls -a
    
    # 
    docker container prune
    ```
    
- `docker image prune`
    - 태그가 붙지 않은(dangling) 모든 이미지 삭제
    ```bash
    # 
    docker image prune
    
    # 남아 있는 이미지 리스트 확인 – 실행 중인 컨테이너의 이미지 등
    docker image ls
    ```
    
- `docker system prune`
    - 사용하지 않는 도커 이미지, 컨테이너, 볼륨, 네트워크 등 모든 도커 리소스를 일괄적으로 삭제
    ```bash
    docker system prune
    ```