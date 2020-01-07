---
title: "[HackerRank/Sql] Placements"
categories: 
  - Sql
tags:
  - Sql
  - placements
  - HackerRank
last_modified_at: 2020-01-07T12:04:24-04:00
toc: true
---

문제정보
-
- [HackerRank - Placements](https://www.hackerrank.com/challenges/placements/problem)

어떻게 풀까?
-
평범한 문제이나 문제를 잘 읽자. 시키지도 않은 order by dsec를 했었음.


문제풀이
-
~~~sql
SELECT 
    T.NAME
FROM 
    (    
        SELECT S.ID, S.NAME, F.FRIEND_ID, P.SALARY, FP.F_SAL
        FROM STUDENTS S, FRIENDS F, PACKAGES P, (SELECT F.ID F_ID, P.SALARY F_SAL FROM FRIENDS F, PACKAGES P WHERE F.ID = P.ID) FP
        WHERE S.ID = F.ID (+)
        AND S.ID = P.ID (+)
        AND F.FRIEND_ID = FP.F_ID (+)
    ) T
WHERE
    T.SALARY < T.F_SAL
ORDER BY T.F_SAL;
~~~