[[달리기반 SQL]]
## 문제

- **상황**: 이번에는 이용자들이 잘 활동하고 있는지 보고자 합니다. 포인트가 많을수록 활동을 잘하고 있다고 생각 할 수 있습니다. 날짜별로 획득한 포인트가 점점 늘어나는지 줄어드는지 확인해 봅시다.
- **문제**: 다음과 같은 결과테이블을 만들어봅시다.
	- created_at: 익명화된 유저들의 아이디(varchar255)
	- average_points: 유저가 획득한 날짜별 평균 포인트(int), 반올림 필수

---
### 데이터 설명

1. point_users 테이블은 스파르타코딩클럽 가입 유저들의 포인트에 대한 정보를 기록한 테이블입니다.
	- point_user_id: user_point 행을 구별하기 위한 key(varchar255)
	- created_at: 아이디 생성 날짜(timestamp)
	- updated_at: 정보 업데이트 날짜(timestamp)
	- user_id: 익명화된 유저들의 아이디(varchar255)
	- point: 보유하고 있는 포인트(int)

---

## 내 답변

```sql
SELECT date(created_at) created_at, ROUND(avg(`point`)) average_points
FROM point_users pu
group by 1
```
