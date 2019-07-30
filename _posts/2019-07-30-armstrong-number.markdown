---
title: "[LeetCode] Armstrong Number"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Armstrong Number
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-25T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Armstrong Number](https://leetcode.com/problems/armstrong-number)


어떻게 풀까?
-
주어진 숫자의 각 자릿수에 총자릿수만큼 거듭제곱을 하여 모두 더한 값이 처음에 주어진 값과 같은지 여부를 반환한다.

Number타입으로 주어진 값을 String타입으로 변환한 뒤 Array.from을 사용하여 배열로 만들면 map을 이용해서 순차적으로 처리할 수 있다.

> map, reduce는 Array타입에 사용할 수 있다.
> JavaScript의 Array의 length는 속성값이다.
> 유사배열객체(array-like object)에 대해서 알아두자.


문제풀이 (JavaScript)
-
~~~javascript
/**
 * @param {number} N
 * @return {boolean}
 */
var isArmstrong = function(N) {
    let tot = 0;
    let arr = Array.from(N+'');
    arr.map(x => tot += Math.pow(x, arr.length));
    
    if(tot == N) return true;
    return false;
};
~~~