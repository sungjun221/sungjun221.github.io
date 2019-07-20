---
title: "[LeetCode/Sql] Second highest salary"
categories: 
  - Sql
tags:
  - Sql
  - Second highest salary
  - LeetCode
last_modified_at: 2019-07-20T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Second highest salary](https://leetcode.com/problems/second-highest-salary)

어떻게 풀까?
-
2번째로 높은 Salary의 값을 구하는 것이다.
특이한 점은 row가 하나만 있을 경우엔 null을 반환해야 한다.

SubQuery를 활용하여 푼다.


문제풀이(JavaScript)
-
~~~sql
SELECT MAX(SALARY) AS SECONDHIGHESTSALARY
FROM EMPLOYEE
WHERE SALARY < (SELECT MAX(SALARY) FROM EMPLOYEE);
~~~