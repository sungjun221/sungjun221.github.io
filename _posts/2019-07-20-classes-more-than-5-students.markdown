---
title: "[LeetCode/Sql] Classes More Than 5 Students"
categories: 
  - Sql
tags:
  - Sql
  - Classes More Than 5 Students
  - LeetCode
last_modified_at: 2019-07-20T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students)

어떻게 풀까?
-
클래스를 수강하는 학생이 5명 이상인 클래스명을 표시한다.
Row가 중복되는 경우도 처리해줘야 하기 때문에 SubQuery로 중복을 제거한 후 Grouping 하여 처리한다.


문제풀이
-
~~~sql
SELECT CLASS
FROM 
(SELECT DISTINCT STUDENT, CLASS
FROM COURSES) AS T
GROUP BY CLASS
HAVING COUNT(1) >= 5;
~~~