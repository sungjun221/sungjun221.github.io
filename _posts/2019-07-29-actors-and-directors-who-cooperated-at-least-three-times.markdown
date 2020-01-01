---
title: "[LeetCode/Sql] Actors and Directors Who Cooperated At Least Three Times"
categories: 
  - Sql
tags:
  - Sql
  - Actors and Directors Who Cooperated At Least Three Times
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Actors and Directors Who Cooperated At Least Three Times](https://leetcode.com/problems/actors-and-directors-who-cooperated-at-least-three-times)

어떻게 풀까?
-
최소 3번 이상 함께 작업한 배우와 감독을 출력한다.

HAVING절에 COUNT()로 집계한다. SELECT에 COUNT()를 출력할 필요가 없다.

문제풀이
-
~~~sql
SELECT ACTOR_ID, DIRECTOR_ID
FROM ACTORDIRECTOR
GROUP BY ACTOR_ID, DIRECTOR_ID
HAVING COUNT(1) >= 3;
~~~