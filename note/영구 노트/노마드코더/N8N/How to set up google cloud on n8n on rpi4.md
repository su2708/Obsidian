#n8n #Nomad_Coder [[Nomad Coder]]

## 1단계: Cloudflare Tunnel로 외부 주소 발급받기
- 라즈베리파이와 외부 세계를 잇는 안전한 통로를 만드는 과정입니다.
```
# 1. 터미널에서 터널 실행 (-d 옵션으로 백그라운드 실행)
docker run -d --name <이름> --network host cloudflare/cloudflared:latest tunnel --url http://<라즈베리파이 주소>:<n8n 포트 번호>

# 2. 도메인 주소 확인
	- docker logs <이름> 실행
	- 로그 중간에서 https://random-words-here.trycloudflare.com 형태의 주소를 찾아 복사
```

## 2단계: n8n 환경 설정 (.env 수정)
- 발급받은 도메인을 n8n이 인식하도록 설정 파일을 업데이트해야 합니다.
```
# 1. .env 파일 수정
N8N_HOST=random-words-here.trycloudflare.com
N8N_PROTOCOL=https
WEBHOOK_URL=https://random-words-here.trycloudflare.com/
N8N_SECURE_COOKIE=true (HTTPS 환경을 위한 보안 설정)

# 2. 수정한 설정으로 n8n 재시작
docker compose up -d

# 3. 접속 확인
	- https://random-words-here.trycloudflare.com 에 접속하여 n8n 화면이 뜨는지 확인
```

## 3단계: Google Cloud Console 설정
- 구글이 내 n8n의 접근을 허용하도록 '열쇠'를 만드는 과정입니다.
```
# 1. google cloud console 접속 후 프로젝트 생성.

# 2. API 활성화: gmail api 검색 후 [사용] 클릭

# 3. 좌측 화면에서 [대상] 클릭
	- User Type: 외부(External) 선택
	- 앱 정보: 이름과 이메일만 입력 후 저장
	- 테스트 사용자: Test users 섹션에 본인 gmail 주소를 추가

# 4. 좌측 화면에서 [클라이언트] 클릭 후 [클라이언트 만들기] 클릭
	- 애플리케이션 유형: 웹 애플리케이션
	- 승인된 리디렉션 URI에 n8n 노드 설정창(Credential 생성창)의 OAuth Redirect URL를 복붙
	- 위의 설정을 마치고 [만들기] 클릭
	- Client ID와 Client Secret 확인

# 5. n8n에 Credential 등록하기
	- Credential 생성창의 Client ID와 Client Secret에 구글 콘솔에서 확인한 값을 붙여넣기
	- [Connect my account] 버튼을 눌러 구글 계정과 연동
```

## ==주의 사항==
1. **주소 일치**
	- `.env`의 `WEBHOOK_URL`, 브라우저 접속 주소, 구글 콘솔의 `리디렉션 URI` 세 곳의 주소가 동일해야 합니다.

2.  **터널 유지**
	- `cloudflared` 터미널이 꺼지면 주소가 만료되어 접속되지 않습니다.
	- 주소가 만료되면 1~3단계를 다시 진행해야 합니다.

3. **권한 체크**
	- 구글 계정 연동 단계에서 권한 체크박스가 보인다면 반드시 모드 체크해야 합니다.