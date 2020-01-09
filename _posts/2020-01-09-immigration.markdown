---
title: "[프로그래머스] Immigration"
categories: 
  - Algorithm
tags:
  - Algorithm
  - immigration
  - 프로그래머스
  - Binary Search
  - Java
last_modified_at: 2020-01-09T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - Immigration](https://www.hackerrank.com/challenges/placements/problem)

어떻게 풀까?
-
어렵게 생각했는데 생각하지 못한 방식으로 쉽게 풀 수 있는 문제였다. 복잡하게 심사관을 선택하는 경우의 수를 모두 탐색하는 문제가 아니다.

심사관 한 명이 N분동안 처리할 수 있는 사람의 수는 (N/심사관의 걸리는 시간)이다. 이 수들을 더하면 모든 심사관이 N분동안 처리할 수 있는 사람의 수가 된다.

주어진 n명의 사람을 처리할 수 있는 최소의 N분이 얼마인지를 확인하면 되는 문제이다. 제일느린심사관*n명을 하면 가장 느리게 처리했을 때의 max 시간값이 나온다. 

최소값 min을 1로 가정하고 이진탐색으로 중간값의 민원인수를 확인해가며 최소시간을 구하면 된다.

 


문제풀이
-
~~~java
import java.util.*;

class Solution {    
    public long solution(int n, int[] times) {
    	Arrays.sort(times);
    	long min = 1;
    	long max = (long)times[times.length-1] * n;
    	long answer = max;
    	long sum = 0;
    	long mid = 0;

    	while(min <= max){
			sum = 0;
    		mid = (min+max)/2;
			for(int i=0; i<times.length; i++){
				sum += (mid / times[i]);
			}
			if(n > sum){
				min = mid + 1;
			}else{
				if(mid < answer){
					answer = mid;
				}
				max = mid - 1;
			}
    	}
    	return answer;
    }
}
~~~