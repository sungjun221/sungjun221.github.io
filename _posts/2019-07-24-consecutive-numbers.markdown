---
title: "[LeetCode/Sql] Consecutive Numbers"
categories: 
  - Sql
tags:
  - Sql
  - Consecutive Numbers
  - LeetCode
last_modified_at: 2019-07-24T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers)

어떻게 풀까?
-
숫자가 3개 연속으로 이어진 ID값을 출력한다.
해당 테이블을 3개 선언하여 id값을 비교한다.

WHERE l1.id = l2.id + 1 AND l1.id = l3.id + 2 와 같이 한 테이블을 기준으로 join하면 제 값이 안나온다. 아래와 같이 세 테이블의 id값을 연결해야 한다.

문제풀이
-
~~~sql
SELECT SCORE,
    ( SELECT COUNT(1)
      FROM
         ( SELECT DISTINCT SCORE S
           FROM SCORES ) AS T
      WHERE S >= SCORE ) AS RANK  -- S is inner's, SCORE is outer's
FROM SCORES
ORDER BY SCORE DESC;
~~~