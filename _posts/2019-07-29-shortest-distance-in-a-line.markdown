---
title: "[LeetCode/Sql] Shortest Distance in a Line"
categories: 
  - Sql
tags:
  - Sql
  - Shortest Distance in a Line
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Shortest Distance in a Line](https://leetcode.com/problems/shortest-distance-in-a-line)

어떻게 풀까?
-
주어진 X좌표의 거리가 가장 짧은 것을 출력한다.

절대값 함수를 쓸 필요가 없다. POINT 테이블 2개를 선언하고 조건절에 P1.X > p2.X를 하면 된다.

문제풀이
-
~~~sql
SELECT MIN(P1.X - P2.X) AS SHORTEST
FROM POINT P1, POINT P2
WHERE P1.X > P2.X;
~~~