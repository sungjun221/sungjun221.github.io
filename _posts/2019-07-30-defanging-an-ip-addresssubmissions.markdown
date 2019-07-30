---
title: "[LeetCode] Find Anagram Mappings"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Find Anagram Mappings
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-30T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Find Anagram Mappings](https://leetcode.com/problems/defanging-an-ip-addresssubmissions)


어떻게 풀까?
-
IP주소의 .을 [.]으로 변환한다.

정규표현식에 대해 한번씩 연습해보자. JavaScript에서 정규표힌식이 쓰이는 곳은 RegExp(exec(), test(), match())와 String(search(), replace(), split())의 객체이다.
    - [정규식에서 쓰이는 메소드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%8B%9D#%EC%A0%95%EA%B7%9C%EC%8B%9D_%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)


문제풀이 (JavaScript)
-
~~~javascript
/**
 * @param {string} address
 * @return {string}
 */
var defangIPaddr = function(address) {
    return address.replace(/\./g, "[.]");
};
~~~