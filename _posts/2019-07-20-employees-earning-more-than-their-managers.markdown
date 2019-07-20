---
title: "[LeetCode/Sql] Employees earning more than their managers"
categories: 
  - Sql
tags:
  - Sql
  - Employees earning more than their managers
  - LeetCode
last_modified_at: 2019-07-20T12:05:2-04:00
toc: true
---

문제정보
-
- [LeetCode - Employees earning more than their managers](https://leetcode.com/problems/employees-earning-more-than-their-managers)

어떻게 풀까?
-
자신의 매니저보다 더 돈을 많이 보는 피고용인을 구하는 문제이다.
동일한 테이블에 별명을 주어 ManagerID에 ID를 매핑하여 구한다.

문제풀이
-
~~~sql
SELECT E1.NAME AS EMPLOYEE 
FROM EMPLOYEE AS E1, EMPLOYEE AS E2
WHERE E1.MANAGERID = E2.ID AND E1.SALARY > E2.SALARY;
~~~