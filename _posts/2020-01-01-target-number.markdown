---
title: "[LeetCode/Sql] Product Sales Analysis III"
categories: 
  - Sql
tags:
  - Sql
  - Product Sales Analysis III
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii)

어떻게 풀까?
-
상품의 첫번째 판매연도에 대한 데이터를 출력하는 문제이다.

테이블이 2개 주어졌으나 하나만 사용하면 된다..! WHERE절에 2개의 값을 묶어 조건을 걸면 쉽게 풀린다.

문제풀이
-
~~~sql
SELECT SALE_ID, YEAR AS FIRST_YEAR, QUANTITY, PRICE
FROM SALES
WHERE (PRODUCT_ID, YEAR) IN
(SELECT PRODUCT_ID, MIN(YEAR) AS YEAR FROM SALES GROUP BY PRODUCT_ID);
~~~