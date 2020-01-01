---
title: "[LeetCode/Sql] Game Play Analysis II"
categories: 
  - Sql
tags:
  - Sql
  - Game Play Analysis II
  - LeetCode
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Game Play Analysis II](https://leetcode.com/problems/game-play-analysis-ii)

어떻게 풀까?
-
게임의 첫번째 로그인에 대한 데이터를 출력하는 문제이다. 

DEVICE_ID도 출력하지만 조회 조건에 DEVICE_ID에 대한 요구사항이 없다. 조건절에 DEVICE_ID를 넣으면 PLAYER_ID, DEVICE_ID의 두 개값을 조합한 후 첫번째 로그인한 값을 찾게 된다. 조회 조건과 조회되는 컬럼의 값은 다를 수 있음을 생각할 것.

문제풀이
-
~~~sql
SELECT PLAYER_ID, DEVICE_ID
FROM ACTIVITY
WHERE (PLAYER_ID, EVENT_DATE) IN
(SELECT PLAYER_ID, MIN(EVENT_DATE) FROM ACTIVITY GROUP BY PLAYER_ID);
~~~