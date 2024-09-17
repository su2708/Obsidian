[[달리기반 SQL]]
## 문제

**Products** 테이블:

| ProductID | ProductName  | Category    | Price |
| --------- | ------------ | ----------- | ----- |
| 1         | Laptop       | Electronics | 1000  |
| 2         | Smartphone   | Electronics | 800   |
| 3         | Headphones   | Electronics | 150   |
| 4         | Coffee Maker | Home        | 200   |
| 5         | Blender      | Home        | 100   |

**Orders** 테이블:

|OrderID|ProductID|OrderDate|Quantity|CustomerID|
|---|---|---|---|---|
|101|1|2024-02-01|2|1|
|102|3|2024-02-02|1|2|
|103|2|2024-02-03|1|1|
|104|4|2024-02-04|3|3|
|105|1|2024-02-05|1|2|
|106|5|2024-02-06|2|3|
**Customers** 테이블:

|CustomerID|CustomerName|Country|
|---|---|---|
|1|Alice|USA|
|2|Bob|UK|
|3|Charlie|USA|

---
### 요구사항

1. 각 고객이 구매한 모든 제품의 총 금액을 계산하고, 고객 이름, 총 구매 금액, 주문 수를 출력하는 SQL 쿼리를 작성해주세요.
    a. 기대결과

| CustomerName | TotalAmount | OrderCount |
| ------------ | ----------- | ---------- |
| Alice        | 2800        | 2          |
| Bob          | 1150        | 2          |
| Charlie      | 800         | 2          |

2. 각 제품 카테고리별로 가장 많이 팔린 제품의 이름과 총 판매량을 조회하는 SQL 쿼리를 작성해주세요.
    a. 기대결과

|Category|Top_Product|TotalSold|
|---|---|---|
|Electronics|Laptop|3|
|Home|Coffee Maker|3|

---

## 내 답변

```sql
-- 문제 1번
select c.CustomerName ,
	sum(p.Price * o.Quantity) as TotalAmount ,
	count(o.OrderID) as OrderCount
from orders o
join customers c on o.CustomerID = c.CustomerID
join products p on o.ProductID = p.ProductID
group by 1
  

-- 문제 2번
select Category, ProductName, TotalSold
from
	(
	select p.Category ,
		p.ProductName ,
		sum(o.Quantity) TotalSold,
		rank() over (partition by p.Category order by sum(o.Quantity) desc) ranking
	from orders o
	join products p on p.ProductID = o.ProductID
	group by 1, 2
	) p
where ranking = 1
```
