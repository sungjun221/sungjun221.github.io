---
title: "[프로그래머스] 가장 큰 수"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 가장 큰 수
  - 프로그래머스
  - sort
  - Java
last_modified_at: 2020-01-05T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)


문제풀이
-
[3, 30, 34, 5, 9]의 가장 큰 값은 "9534330"이다.
하지만 앞자리 숫자간 비교를 위해 String으로 변환 후 desc 정렬하여 덧붙이면 "9534303"이 된다.
그래서 Comparator를 아래와 같이 구현하여 두 개의 값을 붙였을 때의 값을 비교한다.

~~~java
import java.util.*;

class Solution {
    public String solution(int[] numbers) {
        String answer = "";
        String[] sarr = new String[numbers.length];
        for(int i=0; i<numbers.length; i++){
            sarr[i] = String.valueOf(numbers[i]);
        }
        
        Arrays.sort(sarr, new Comparator<String>(){
            @Override
            public int compare(String s1, String s2){
                return (s2+s1).compareTo(s1+s2);
            }
        });

        if(sarr[0].startsWith("0")){
            answer = "0";
        }else{
            for(String s : sarr) answer += s;        
        }

        return answer;
    }
}
~~~