---
title: "[LeetCode/Sql] Delete Duplicate Emails"
categories: 
  - Sql
tags:
  - Sql
  - Delete Duplicate Emails
  - LeetCode
last_modified_at: 2019-07-20T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails)

어떻게 풀까?
-
중복된 이메일 중에 가장 작은 ID값을 제외하고 삭제한다.
SubQuery를 쓰지 않고 푸는 고급진 방법과 SubQuery를 사용하는 방법이 있다.


문제풀이 (non-SubQuery)
-
Person 테이블을 별명을 붙여 서로 매핑한 뒤 p1.Id > p2.Id로 골라낸다.
p2.Id보다 큰 값이 모두 빠지게 되어 결과적으로 가장 작은 값만 빼고 삭제된다.
~~~sql
DELETE P1
FROM PERSON P1, PERSON P2
WHERE P1.EMAIL = P2.EMAIL 
AND P1.ID > P2.ID;
~~~

문제풀이 (SubQuery)
-
서브쿼리로 EMAIL을 Grouping 한 후 MIN(ID)를 제외한 값들을 삭제하게 한다.
서브쿼리를 두 번 감싼 이유는 한 번만 감싸면 아래와 같은 오류가 발생한다.

> You can't specify target table 'person' for update in FROM clause

FROM절에 있는 내용을 타게팅 할 수 없기 때문에 서브쿼리로 한 번 더 감싼다.

~~~sql
DELETE 
FROM PERSON 
WHERE ID NOT IN 
(
    SELECT T.ID FROM
    (
        SELECT MIN(ID) AS ID FROM PERSON GROUP BY EMAIL
    ) AS T
)
~~~