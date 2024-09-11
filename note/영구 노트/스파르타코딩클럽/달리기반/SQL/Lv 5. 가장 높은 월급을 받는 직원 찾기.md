## 문제

**Employees** 테이블:

|EmployeeID|Name|Department|Salary|ManagerID|
|---|---|---|---|---|
|1|Alice|HR|70000|NULL|
|2|Bob|IT|90000|1|
|3|Charlie|IT|80000|2|
|4|David|IT|85000|2|
|5|Eve|HR|75000|1|
|6|Frank|Finance|95000|NULL|
|7|Grace|Finance|80000|6|
|8|Heidi|IT|95000|2|

---
### 요구사항

1. 각 직원의 이름, 부서, 월급, 그리고 그 직원이 속한 부서에서 가장 높은 월급을 받고 있는 직원의 이름과 월급을 조회하는 SQL 쿼리를 작성해주세요.
    a. 기대결과

|Name|Department|Salary|Top_Earner|Top_Salary|
|---|---|---|---|---|
|Alice|HR|70000|Eve|75000|
|Bob|IT|90000|Heidi|95000|
|Charlie|IT|80000|Heidi|95000|
|David|IT|85000|Heidi|95000|
|Eve|HR|75000|Eve|75000|
|Frank|Finance|95000|Frank|95000|
|Grace|Finance|80000|Frank|95000|
|Heidi|IT|95000|Heidi|95000|

2. 부서별로 평균 월급이 가장 높은 부서의 이름과 해당 부서의 평균 월급을 조회하는 SQL 쿼리를 작성해주세요.
    a. 기대결과

|Department|Avg_Salary|
|---|---|
|IT|87500|

---

## 내 답변

```sql

```
