#n8n #Nomad_Coder [[Nomad Coder]]

## 1단계: Docker 및 Docker Compose 설치
- 가장 먼저 라즈베리파이 시스템을 업데이트하고 도커를 설치합니다.
```
# 1. 시스템 업데이트
sudo apt update && sudo apt upgrade -y

# 2. 도커 자동 설치 스크립트 실행
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 3. 현재 사용자(pi)에게 도커 권한 부여 (sudo 없이 쓰기 위해)
sudo usermod -aG docker $USER

# 4. 권한 적용 (로그아웃 후 로그인 효과)
newgrp docker

# 5. 설치 확인 (버전이 나오면 성공)
docker compose version
```

## 2단계: 디렉토리 생성 및 권한 설정
- n8n 데이터를 저장할 디렉토리를 만들고, 도커가 파일을 쓸 수 있도록 권한을 맞춰줍니다.
```
# 1. 폴더 생성
mkdir -p ~/n8n/n8n_data

# 2. n8n 폴더로 이동
cd ~/n8n

# 3. 데이터 폴더 권한 설정 (중요: 도커 내부 사용자인 node(1000번)가 쓸 수 있게 함)
sudo chown -R 1000:1000 ~/n8n/n8n_data
sudo chmod -R 777 ~/n8n/n8n_data
```

## 3단계: .env 작성
- `nano .env`를 입력하여 아래 내용을 붙여넣으세요.
```
# --- [기본 접속 보안 설정] ---
# 접속 시 아이디/비번 입력창 띄우기 (true/false)
N8N_BASIC_AUTH_ACTIVE=true

# 아이디 (원하는 걸로 변경하세요)
N8N_BASIC_AUTH_USER=admin

# 비밀번호 (반드시 복잡한 걸로 변경하세요!)
N8N_BASIC_AUTH_PASSWORD=my_strong_password_123

# --- [네트워크 설정] ---
# 컨테이너 내부 호스트 설정 (0.0.0.0 유지)
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=http

# HTTP(IP주소) 접속 시 로그인 풀림 방지
N8N_SECURE_COOKIE=false

# --- [시간대 설정] ---
GENERIC_TIMEZONE=Asia/Seoul
TZ=Asia/Seoul

# --- [성능 최적화 및 경고 해결] ---
# SQLite 읽기 성능 향상
DB_SQLITE_POOL_SIZE=50

# 복잡한 작업 시 멈춤 방지 (Task Runner 활성화)
N8N_RUNNERS_ENABLED=true

# 보안 경고 메시지 해결
N8N_BLOCK_ENV_ACCESS_IN_NODE=false
N8N_GIT_NODE_DISABLE_BARE_REPOS=true
```

## 4단계: docker-compose.yml 작성
- `nano docker-compose.yml`을 입력하여 아래 내용을 작성합니다.
```
services:
  n8n:
    # n8n 공식 권장 이미지 (다운로드 제한 없음)
    image: docker.n8n.io/n8nio/n8n
    
    # 재부팅 시 자동 실행
    restart: always
    
    # 포트 연결 (외부 5678 : 내부 5678)
    ports:
      - "5678:5678"
    
    # 아까 작성한 .env 파일 불러오기
    env_file:
      - .env
    
    # 데이터 저장소 연결 (내 라즈베리파이 폴더 : 컨테이너 내부 폴더)
    volumes:
      - ./n8n_data:/home/node/.n8n
```

## 5단계: 실행 및 접속

#### 1. 실행하기
- `docker compose up -d`

#### 2. 로그 확인 (에러 없는지 체크)
- `docker compose logs -f`

#### 3. 접속하기
- 브라우저 주소 창에: `http://[라즈베리파이IP]:5678`