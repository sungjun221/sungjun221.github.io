---
title: "[LeetCode/Sql] Rising Temperature"
categories: 
  - Sql
tags:
  - Sql
  - Rising Temperature
  - LeetCode
last_modified_at: 2019-07-20T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Rising Temperature](https://leetcode.com/problems/rising-temperature)

어떻게 풀까?
-
전날보다 높은 온도를 가진 날의 ID를 출력한다.
ORACLE SQL과 MYSQL SQL가 다르다.

ORACLE SQL은 DATE에 + 1을 하면 다음날로 비교할 수 있지만 MYSQL은 TO_DAYS()로 비교하거나 DATE_ADD()를 사용해야 한다.

문제풀이 (ORACLE)
-
~~~sql
SELECT W1.ID AS ID
FROM WEATHER W1, WEATHER W2
WHERE W1.RECORDDATE = W2.RECORDDATE + 1
AND W1.TEMPERATURE > W2.TEMPERATURE;
~~~

문제풀이 (MySQL - TO_DAYS() 사용)
-
~~~sql
SELECT W1.ID AS ID
FROM WEATHER W1, WEATHER W2
WHERE TO_DAYS(W1.RECORDDATE) = TO_DAYS(W2.RECORDDATE) + 1
AND W1.TEMPERATURE > W2.TEMPERATURE;
~~~

문제풀이 (MySQL - DATE_ADD() 사용)
-
~~~sql
SELECT W1.ID AS ID
FROM WEATHER W1, WEATHER W2
WHERE W1.RECORDDATE = DATE_ADD(W2.RECORDDATE, INTERVAL 1 DAY)
AND W1.TEMPERATURE > W2.TEMPERATURE;
~~~