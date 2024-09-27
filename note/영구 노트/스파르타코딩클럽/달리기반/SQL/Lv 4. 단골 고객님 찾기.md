#sql [[달리기반 SQL]]
## 문제

**Orders** 테이블:

|OrderID|CustomerID|OrderDate|TotalAmount|
|---|---|---|---|
|101|1|2024-01-01|150|
|102|2|2024-01-03|200|
|103|1|2024-01-04|300|
|104|3|2024-01-04|50|
|105|2|2024-01-05|80|
|106|4|2024-01-06|400|

**Customers** 테이블:

|CustomerID|CustomerName|Country|
|---|---|---|
|1|Alice|USA|
|2|Bob|UK|
|3|Charlie|USA|
|4|David|Canada|

---
### 요구사항

1. 고객별로 주문 건수와 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.
    1. 출력 결과에는 고객 이름, 주문 건수, 총 주문 금액이 포함되어야 합니다. 단, 주문을 한 적이 없는 고객도 결과에 포함되어야 합니다.
    2. 기대결과

| CustomerName | OrderCount | TotalSpent |
| ------------ | ---------- | ---------- |
| Alice        | 2          | 450        |
| Bob          | 2          | 280        |
| Charlie      | 1          | 50         |
| David        | 1          | 400        |

2. 나라별로 총 주문 금액이 가장 높은 고객의 이름과 그 고객의 총 주문 금액을 조회하는 SQL 쿼리를 작성해주세요.
    1. 기대결과

|Country|Top_Customer|Top_Spent|
|---|---|---|
|USA|Alice|450|
|UK|Bob|280|
|Canada|David|400|

---

## 내 답변

```sql
문제 1번
select CustomerName,
	count(CustomerID) as OrderCount,
	sum(TotalAmount) as TotalSpent
from
	(
	select c.CustomerName,
		o.CustomerID,
		o.TotalAmount
	from 단골고객님찾기_orders o left join 단골고객님찾기_customer c on o.CustomerID = c.CustomerID
	) a
group by CustomerName

문제 2번
select Country,
	Top_Customer,
	Top_spent
from
	(
	select Country,
		CustomerName Top_Customer,
		Top_spent,
		rank() over (partition by Country order by Top_spent desc) rn
	from
		(
		select Country,
			CustomerName,
			sum(TotalAmount) Top_spent
		from
			(
			select c.Country,
				c.CustomerName,
				o.TotalAmount
			from 단골고객님찾기_orders o left join 단골고객님찾기_customer c on o.CustomerID = c.CustomerID
			) p
		group by 1, 2
		) q
	) r
where rn = 1
order by 1 desc
```
