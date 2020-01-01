---
title: "[LeetCode] Jewels and Stones"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Jewels and Stones
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-31T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Jewels and Stones](https://leetcode.com/problems/jewels-and-stones)


어떻게 풀까?
-
J안에 속한 문자가 S에 몇개나 있는지 출력한다.

그냥 조회하여 표시하면 평범하지만 아래와 같이 푸는 방법도 있다. 몇개나 있는지를 확인해야 할 때는 filter()를 활용할 수 있다.


문제풀이 (JavaScript)
-
~~~javascript
/**
 * @param {number[]} A
 * @param {number[]} B
 * @return {number[]}
 */
const numJewelsInStones = (J, S) => S.split('').filter(char => J.indexOf(char) !== -1).length;
~~~