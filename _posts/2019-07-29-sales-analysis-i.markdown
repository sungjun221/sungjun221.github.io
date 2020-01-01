---
title: "[LeetCode/Sql] Sales Analysis I"
categories: 
  - Sql
tags:
  - Sql
  - Sales Analysis 
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Sales Analysis I](https://leetcode.com/problems/sales-analysis-i)

어떻게 풀까?
-
물건을 가장 많이 판매한 판매자를 출력한다.

판매자별 판매가총액을 Grouping 한 후 그 중 최고값을 구해 그 값과 같은 판매가를 가진 판매자를 조회한다.
MySql의 경우 ORDER BY SUM(PRICE) DESC한 후 LIMIT 1을 하여 발라낼 수 있다.

문제풀이
-
~~~sql
SELECT SELLER_ID
FROM SALES
GROUP BY SELLER_ID
HAVING SUM(PRICE) = 
	(
	 SELECT MAX(TOTAL_PRICE)
	 FROM 
	 	(
	 	 SELECT SUM(PRICE) AS TOTAL_PRICE 
	 	 FROM SALES 
	 	 GROUP BY SELLER_ID
	 	)
	); 
~~~