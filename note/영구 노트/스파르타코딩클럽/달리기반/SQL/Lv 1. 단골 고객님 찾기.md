#sql [[달리기반 SQL]] 
## 문제

- **상황**: 여러분들은 스파르타코딩클럽의 분석가로 취직했습니다. DBeaver를 테스트 해볼 겸 “김”씨로 시작하는 이용자들 수를 세어 보기로 했습니다.
- **문제**:다음과 같은 결과테이블을 만들어봅시다.
	- name_cnt: “김”씨 성을 가지고 있는 교육생의 수

| name_cnt |
| -------- |
| 100      |

---
### 데이터 설명

1. user 테이블은 스파르타 코딩클럽에 가입한 유저들의 정보를 날짜별로 기록한 테이블입니다.
	- user_id: 익명화된 유저들의 아이디(varchar255)
	- created_at: 아이디 생성 날짜(timestamp)
	- updated_at: 정보 업데이트 날짜(timestamp)
	- name: 익명화된 유저들의 이름(varchar255)
	- email: 이메일(varchar255)

---

## 내 답변

```sql
SELECT COUNT(DISTINCT user_id) as name_cnt
FROM users u
WHERE name like '김%'
```
