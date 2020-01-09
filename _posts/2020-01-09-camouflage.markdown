---
title: "[프로그래머스] 위장"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 위장
  - 프로그래머스
  - Hash
  - Java
last_modified_at: 2020-01-09T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 위장](https://programmers.co.kr/learn/courses/30/lessons/42578)

어떻게 풀까?
-
카테고리 별로 갯수를 구한 뒤 조합의 수를 구하기 위해 곱한다. 단 특정 카테고리를 선택하지 않는 경우도 고려해야하기 때문에 카테고리 가짓수에 +1을 한 뒤 서로 곱한다.

이렇게 구해진 값에는 모두 선택하지 않은 경우도 포함되어 있기 때문에 다시 -1을 한다. 

 


문제풀이
-
~~~java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
    	Map<String, Integer> m = new HashMap<>();
    	int answer = 1;

    	for(int i=0; i<clothes.length; i++){
    		String k = clothes[i][1];
    		if(m.containsKey(k)){
    			int v = (int)m.get(k);
    			m.put(k, v+1);
    		}else{
    			m.put(k, 1);
    		}
    	}

    	for(String k : m.keySet()){
    		int v = m.get(k);
    		answer *= (v+1);
    	}
    	answer -= 1;
        return answer;
    }
}
~~~