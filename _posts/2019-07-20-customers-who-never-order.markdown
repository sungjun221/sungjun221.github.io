---
title: "[LeetCode/Sql] Customers Who Never Order"
categories: 
  - Sql
tags:
  - Sql
  - Customers Who Never Order
  - LeetCode
last_modified_at: 2019-07-20T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order)

어떻게 풀까?
-
주문을 하지 않은 고객을 찾는다.
고객테이블에 대하 LEFT JOIN을 건다.


문제풀이
-
~~~sql
SELECT NAME AS CUSTOMERS
FROM CUSTOMERS C LEFT OUTER JOIN ORDERS O
ON C.ID = O.CUSTOMERID
WHERE O.CUSTOMERID IS NULL;
~~~