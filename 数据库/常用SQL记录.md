连续数字

``` sql
-- 连续数字 mysql8.0
WITH RECURSIVE cte (num) AS
(
  SELECT 1
  UNION ALL
  SELECT num+1 FROM cte WHERE num < 30
)
SELECT *
FROM cte;

```