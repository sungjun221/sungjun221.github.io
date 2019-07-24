---
title: "[LeetCode/Sql] Nth Highest Salary"
categories: 
  - Sql
tags:
  - Sql
  - Nth Highest Salary
  - LeetCode
last_modified_at: 2019-07-24T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Nth Highest Salary](https://leetcode.com/problems/nth-highest-salary)

어떻게 풀까?
-
n번째로 많은 급여액을 구하는 함수를 작성하는 문제.
Oracle함수인 DENSE_RANK()를 활용하였다.

문제풀이 (Oracle)
-
~~~sql
CREATE FUNCTION GETNTHHIGHESTSALARY(N IN NUMBER) RETURN NUMBER IS
RESULT NUMBER;
BEGIN
	SELECT DISTINCT SALARY INTO RESULT
	FROM
	(SELECT DENSE_RANK() OVER (ORDER BY SALARY DESC) AS RANK, SALARY
	 FROM EMPLOYEE) T
	 WHERE T.RANK = N;
RETURN RESULT;
END;
~~~