---
title: "[프로그래머스] 타겟 넘버"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 타겟 넘버
  - target number
  - 프로그래머스
  - Programmers
  - Java
last_modified_at: 2020-01-01T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 타겟 넘버](https://programmers.co.kr/learn/courses/30/lessons/43165)


문제풀이
-
재귀호출을 이용해서 풀어본 버전.
~~~java
class Solution {
    private static int[] numbers = null;
    private static int target = 0;
    private static int answer = 0;
    public int solution(int[] numbers, int target) {
        this.numbers = numbers;
        this.target = target;
        dfs(0, 0);
        return answer;
    }
    
    private static void dfs(int idx, int prev){
        if((target == prev) && (idx == numbers.length)){
            answer++;
            return;
        }
        
        if(idx >= numbers.length){
            return;
        }
        dfs(idx+1, prev+numbers[idx]);
        dfs(idx+1, prev-numbers[idx]);    
    }
}
~~~