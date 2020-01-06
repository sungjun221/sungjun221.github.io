---
title: "[프로그래머스] 소수 찾기"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 소수 찾기
  - 프로그래머스
  - Brute Force
  - Java
last_modified_at: 2020-01-06T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)


문제풀이
-
주어진 숫자들의 순열 중 소수인 경우를 구하되 중복은 제거한다. 

0으로 시작하는 경우 앞의 0을 없애야 한다. 경우의 수를 모두 확인해야하기 때문에 재귀를 이용하여 풀었다.

~~~java
import java.util.*;

class Solution {
    private String[] sarr = null;
    private int cnt = 0;
    private String primes = ",";

    public int solution(String numbers) {
        sarr = new String[numbers.length()];
        boolean[] visited = new boolean[sarr.length];

        for(int i=0; i<sarr.length; i++){
            sarr[i] = numbers.substring(i, i+1);
        }

        permutation(visited, "");
        return cnt;
    }

    private void permutation(boolean[] visited, String s){
        while(s.indexOf("0") == 0 && s.length() > 1){
            s = s.substring(1);
        }

        if(!primes.contains("," + s + ",") && isPrime(s)){
            primes += s + ",";
            cnt++;
        }

        for(int i=0; i<sarr.length; i++){
            if(visited[i] == false){
                visited[i] = true;
                permutation(visited, s + sarr[i]);
                visited[i] = false;
            }
        }
    }

    private boolean isPrime(String s){
        if("".equals(s)){
            return false;
        }

        int n = Integer.parseInt(s);

        if(n < 2){
            return false;
        }

        for(int i=2; i<=(n/2); i++){
            if((n%i) == 0){
                return false;
            }
        }
        return true;
    }
}
~~~