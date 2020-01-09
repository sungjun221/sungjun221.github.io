---
title: "[HackerRank/Sql] Symmetric Pairs"
categories: 
  - Sql
tags:
  - Sql
  - Symmetric Pairs
  - HackerRank
last_modified_at: 2020-01-09T12:04:24-04:00
toc: true
---

문제정보
-
- [HackerRank - Symmetric Pairs](https://www.hackerrank.com/challenges/symmetric-pairs/problem)

어떻게 풀까?
-
대칭이기 때문에 그냥 대칭인 값을 나열하면 반대쪽 대칭의 값까지 노출된다.
F1.X < F1.Y 로 대칭된 값은 쳐주고, F1.X = F2.Y인 경우는 COUNT(*)가 2인 경우에만 UNION 해준다.


문제풀이
-
~~~sql
SELECT F.X, F.Y 
FROM
(
    (
        SELECT F1.X, F1.Y
        FROM FUNCTIONS F1, 
             FUNCTIONS F2
        WHERE F1.X < F1.Y 
        AND F1.X = F2.Y
        AND F1.Y = F2.X)
    UNION
    (
        SELECT X, Y
        FROM FUNCTIONS
        WHERE X = Y
        GROUP BY X, Y
        HAVING COUNT(1) = 2
    ) 
) F
ORDER BY F.X;
~~~