---
title: "[LeetCode/Sql] Duplicate emails"
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
- [LeetCode - Duplicate emails](https://leetcode.com/problems/duplicate-emails)

어떻게 풀까?
-
중복된 이메일을 찾는다.


문제풀이
-
~~~sql
SELECT EMAIL
FROM PERSON
GROUP BY EMAIL
HAVING COUNT(1) > 1;
~~~