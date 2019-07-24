---
title: "[LeetCode/Sql] Department Highest Salary"
categories: 
  - Sql
tags:
  - Sql
  - Department Highest Salary
  - LeetCode
last_modified_at: 2019-07-24T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Department Highest Salary](https://leetcode.com/problems/department-highest-salary)

어떻게 풀까?
-
각 부서에서 가장 많은 급여를 받는 사람을 출력한다.
SubQuery로 각 부서의 가장 많은 급여를 조회한 뒤 그 값을 엮어서 조회한다.


문제풀이
-
~~~sql
SELECT 
    D.NAME AS DEPARTMENT,
    E.NAME AS EMPLOYEE,
    E.SALARY AS SALARY
FROM 
   (SELECT DEPARTMENTID, MAX(SALARY) AS SALARY
    FROM EMPLOYEE
    GROUP BY DEPARTMENTID) AS M,
    EMPLOYEE AS E,
    DEPARTMENT AS D
WHERE E.DEPARTMENTID = D.ID
  AND D.ID = M.DEPARTMENTID
  AND E.SALARY = M.SALARY;
~~~