---
title: "[LeetCode/Sql] Rank Scores"
categories: 
  - Sql
tags:
  - Sql
  - Rank Scores
  - LeetCode
last_modified_at: 2019-07-21T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Rank Scores](https://leetcode.com/problems/rank-scores)

어떻게 풀까?
-
점수 순서대로 랭크를 매긴다. 같은 점수가 있으면 그 다음 랭킹이 비지 않게한다. Correlated query를 활용한다.

다음 순서로 쿼리를 짠다.
* SCORE의 중복을 없앤 데이터 SELECT
* 작성한 INNER SCORE와 출력할 OUTER SCORE를 비교하여 RANK를 만든다
* 그 RANK를 SCORE와 함께 조합하여 최종 결과 쿼리를 작성

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