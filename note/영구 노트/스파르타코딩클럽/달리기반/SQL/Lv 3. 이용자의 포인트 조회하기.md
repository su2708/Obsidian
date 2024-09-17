[[달리기반 SQL]]
## 문제

- **상황**: 이번에는 이용자들 별로 획득한 포인트를 학생들에게 이메일로 보내려고 합니다. 이를 위한 자료를 가공해봅시다. 특히 users 테이블에는 있으나 point_users 에는 없는 유저가 있어요. 이 유저들의 경우 point를 0으로 처리합시다.
- **문제**: 다음과 같은 결과테이블을 만들어봅시다.
	- user_id: 익명화된 유저들의 아이디
	- email: 유저들의 이메일
	- point: 유저가 획득한 포인트
		- users 테이블에는 있지만 point_users에는 없는 user는 포인트가 없으므로 0 으로 처리
		- users 테이블에는 있지만 point_users에는 없는 user는 포인트가 없으므로 0 으로 처리
---
### 데이터 설명

1. users 테이블은 스파르타 코딩클럽에 가입한 유저들의 정보를 날짜별로 기록한 테이블입니다.
	- user_id: 익명화된 유저들의 아이디(varchar255)
	- created_at: 아이디 생성 날짜(timestamp)
	- updated_at: 정보 업데이트 날짜(timestamp)
	- name: 익명화된 유저들의 이름(varchar255)
	- email: 이메일(varchar255)
2. point_users 테이블은 스파르타코딩클럽 가입 유저들의 포인트에 대한 정보를 기록한 테이블입니다.
	- point_user_id: point_users 테이블의 행을 구별하기 위한 key(varchar255)
	- created_at: 아이디 생성 날짜(timestamp)
	- updated_at: 정보 업데이트 날짜(timestamp)
	- user_id: 익명화된 유저들의 아이디(varchar255)
	- point: 보유하고 있는 포인트(int)

---

## 내 답변

```sql
SELECT u.user_id as user_id,
	u.email as email,
	if(pu.user_id is NULL, coalesce(pu.`point`, 0), pu.point) as point
FROM users u left join point_users pu on u.user_id = pu.user_id
order by 3 desc
```
