---
title: "[LeetCode/Sql] Game Play Analysis I"
categories: 
  - Sql
tags:
  - Sql
  - Game Play Analysis I
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Game Play Analysis I](https://leetcode.com/problems/game-play-analysis-i)

어떻게 풀까?
-
게임의 첫번째 로그인에 대한 데이터를 출력하는 문제이다. WHERE절에 2개의 값을 묶어 조건을 걸면 쉽게 풀린다.

문제풀이
-
~~~sql
SELECT PLAYER_ID, EVENT_DATE AS FIRST_LOGIN
FROM ACTIVITY
WHERE (PLAYER_ID, EVENT_DATE) IN
(SELECT PLAYER_ID, MIN(EVENT_DATE) AS EVENT_DATE FROM ACTIVITY GROUP BY PLAYER_ID);
~~~