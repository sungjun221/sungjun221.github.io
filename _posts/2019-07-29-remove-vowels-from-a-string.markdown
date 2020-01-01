---
title: "[LeetCode] Remove Vowels from a String"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Remove Vowels from a String
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-29T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Remove Vowels from a String](https://leetcode.com/problems/remove-vowels-from-a-string)


어떻게 풀까?
-
주어진 문자에 aeiou를 제외한 값을 반환한다.

JavaScript에는 replaceAll()이 없다. replace()로 정규식을 이용하는 방법과 split()으로 나눈 뒤 join()으로 다시 합치는 방법이 있다.
> str.split(searchStr).join(replaceStr);

문제풀이 (JavaScript)
-
~~~javascript
/**
 * @param {string} S
 * @return {string}
 */
var removeVowels = function(S) {
    return S.replace(/[aeiou]/gi, "");
};
~~~