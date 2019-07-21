---
title: "[LeetCode/Sql] Exchange Seats"
categories: 
  - Sql
tags:
  - Sql
  - Exchange Seats
  - LeetCode
last_modified_at: 2019-07-21T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Exchange Seats](https://leetcode.com/problems/exchange-seats)

어떻게 풀까?
-
홀수와 짝수 ID의 순서를 바꿔서 출력한다.

문제를 잘 읽어야 한다. UPDATE 하는 것으로 잘못 이해하고 문제를 풀려고 했다.
CASE문을 이용한다. CASE를 이용하면 해당 column만 적용되는 것이 아니라 row에 적용된다. (당연한 말이지만..)

3가지 케이스가 있다. ID가 홀수, 짝수, 그리고 마지막에 혼자 남은 홀수.

문제풀이
-
~~~sql
SELECT
    CASE
        WHEN ID % 2 <> 0 AND ID = (SELECT COUNT(1) FROM SEAT) THEN ID
        WHEN ID % 2 = 0 THEN ID - 1
        ELSE ID + 1
    END AS ID,
    STUDENT
FROM SEAT
ORDER BY ID;
~~~