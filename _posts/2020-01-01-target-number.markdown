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
DFS 재귀호출을 이용해서 풀어본 버전.
~~~java
class Solution {
    private int[] numbers = null;
    private int target = 0;
    private int answer = 0;
    public int solution(int[] numbers, int target) {
        this.numbers = numbers;
        this.target = target;
        dfs(0, 0);
        return answer;
    }
    
    private void dfs(int idx, int prev){
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
<br>

DFS Stack을 이용해서 풀어본 버전.
~~~java
import java.util.*;

class Solution {
    private int[] numbers = null;
    private int target = 0;
    private int answer = 0;
    private Stack<Elem> stk = new Stack<>();
    
    public int solution(int[] numbers, int target) {
        this.numbers = numbers;
        this.target = target;
        stk.push(new Elem(0,0));
        
        while(stk.size() != 0){
            dfs();            
        }
        return answer;
    }
    
    private void dfs(){        
        Elem e = stk.pop();
        int idx = e.getIdx();
        int prev = e.getPrev();
        
        if((target == prev) && (idx == numbers.length)){
            System.out.println("ANSWER++");
            answer++;
            return;
        }
        
        if(idx >= numbers.length){
            return;
        }
        
        stk.push(new Elem(idx+1, prev+numbers[idx]));
        stk.push(new Elem(idx+1, prev-numbers[idx]));
    }
}

class Elem{
    private int idx;
    private int prev;
    
    public Elem(int idx, int prev){
        this.idx = idx;
        this.prev = prev;
    }
    
    public int getIdx(){
        return this.idx;
    }
    
    public int getPrev(){
        return this.prev;
    }
}
~~~