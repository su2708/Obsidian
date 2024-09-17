[[달리기반 SQL]]
## 문제

**Employees** 테이블:

|EmployeeID|Name|Department|Salary|
|---|---|---|---|
|1|Alice|HR|5000|
|2|Bob|IT|7000|
|3|Charlie|IT|6000|
|4|David|HR|4500|
|5|Eve|Sales|5500|
|6|Frank|IT|7200|

**Projects** 테이블:

|ProjectID|ProjectName|Budget|
|---|---|---|
|101|Alpha|10000|
|102|Beta|15000|
|103|Gamma|12000|
|104|Delta|8000|

**EmployeeProjects** 테이블:

|EmployeeID|ProjectID|
|---|---|
|1|101|
|2|101|
|3|102|
|4|103|
|5|104|
|6|102|
|6|103|

---
### 요구사항

1. 각 직원이 속한 부서에서 가장 높은 월급을 받는 직원들만 포함된 결과를 조회하는 SQL 쿼리를 작성해주세요.
    a. 출력 결과에는 직원의 이름, 부서, 그리고 월급이 포함되어야 합니다.
    b. 기대결과

|Name|Department|Salary|
|---|---|---|
|Alice|HR|5000|
|Frank|IT|7200|
|Eve|Sales|5500|

2. 직원이 참여한 프로젝트 중 예산이 10,000 이상인 프로젝트만을 조회하는 SQL 쿼리를 작성해주세요.
    a. 출력 결과에는 직원 이름, 프로젝트 이름, 그리고 프로젝트 예산이 포함되어야 합니다.
    b. 기대결과

|Name|ProjectName|Budget|
|---|---|---|
|Bob|Beta|15000|
|Charlie|Beta|15000|
|Frank|Beta|15000|
|David|Gamma|12000|
|Frank|Gamma|12000|

---

## 내 답변

```sql
-- 문제 1번
select Name, Department, Salary
from
	(
	select Name,
		 Department ,
		 Salary , 
		 rank() over (partition by Department order by Salary desc) ranking
	from employees2 e
	) p
where ranking = 1

-- 문제 2번
select Name, ProjectName, Budget
from employeeprojects epj
join employees2 e2 on e2.EmployeeID = epj.EmployeeID
join projects p on p.ProjectID = epj.ProjectID
where Budget >= 10000
order by 2

```
