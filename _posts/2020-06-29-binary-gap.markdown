---
title: "[Codility] BinaryGap"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Java
last_modified_at: 2020-06-25T12:04:24-04:00
toc: true
---
문제정보
-
- [Codility - BinaryGap](https://app.codility.com/programmers/lessons/1-iterations/binary_gap)

어떻게 풀까?
-
1이 연속으로 나올 경우의 처리를 고려한다.

문제풀이(Java)
-
~~~java
import java.util.*;

class Solution {
    public int solution(int N) {
        char[] arr = Integer.toBinaryString(N).toCharArray();
        int max = 0;
        int length = 0;
        boolean ready = false;
        boolean gap = false;
        for(int i=0; i<arr.length; i++){
            if(arr[i] == '1'){
               if(ready == false){
                   ready = true;
               }else if(ready == true && gap == true){
                   if(max < length){
                       max = length;
                   }
                   length = 0;
                   gap = false;
               }
            }else if(arr[i] == '0'){
                if(ready == true){
                    gap = true;
                    length++;
                }
            }
        }
        return max;
    }
}
~~~
