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
- [LeetCode - Find Anagram Mappings](https://leetcode.com/problems/find-anagram-mappings)


어떻게 풀까?
-
B가 A의 Anagram일 때, A의 각 자리에 대한 B의 위치를 매핑한 배열을 반환한다.


문제풀이 (JavaScript)
-
~~~javascript
/**
 * @param {number[]} A
 * @param {number[]} B
 * @return {number[]}
 */
var anagramMappings = function(A, B) {
    let reducer = (acc, cur) => {
        for(let i=0; i<B.length; i++){
            if(B[i] == cur){
                acc.push(i);
                break;
            }
        }
        return acc;
    } 
    return A.reduce(reducer, []);
};
~~~