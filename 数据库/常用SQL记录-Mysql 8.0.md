连续数字

``` sql 
WITH RECURSIVE cte (num) AS
(
  SELECT 1
  UNION ALL
  SELECT num+1 FROM cte WHERE num < 30
)
SELECT *
FROM cte;

```

连续日期 年月日
```sql
SELECT * FROM  (
WITH RECURSIVE cte (date_for) AS
(
  SELECT DATE('2024-03-12')
  UNION ALL
  SELECT DATE_ADD(date_for,INTERVAL 1 DAY)  FROM cte WHERE date_for < DATE('2024-04-30')
)
SELECT date_for
FROM cte
) temp 

 LEFT JOIN table_name  ON  DATE_FORMAT(table_name.create_time,'%y-%m-%d')=temp.date_for

```    