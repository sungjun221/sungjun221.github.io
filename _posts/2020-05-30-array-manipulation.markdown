---
title: "[HackerRank] Array Manipulation"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Array Manipulation
  - HackerRank
  - String Manipulation
  - Java
last_modified_at: 2020-05-29T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Array Manipulation](https://www.hackerrank.com/challenges/crush/problem)

어떻게 풀까?
-
쿼리값 start, end, k 값을 주어진 배열에 적용하여 그 중 최대값을 구하는 문제이다.

brute force로 풀면 간단해 보이는 문제이나, 당연히 이렇게 풀면 Timed out이 뜬다.

문제해결의 핵심은 값 자체를 다 구하는 것이 아니라 difference(변화)값만을 갖고 계산하는 것이다.

start에 k값을 더하고 end+1에 k값을 빼서 배열의 변화값을 표현한다.

그리고 이 배열을 순회하며 변화값을 더하고 빼서 max값을 구하는 것이다.
 

문제풀이(Java)
-
~~~java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    static int commonChild(String s1, String s2) {
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();

        int[][] t = new int[c1.length+1][c2.length+1];

        for(int i=0; i<t.length; i++){
            for(int j=0; j<t[0].length; j++){
                if(i == 0 || j == 0){
                    t[i][j] = 0;
                }else if(c1[i-1] == c2[j-1]){
                    t[i][j] = t[i-1][j-1] + 1;
                }else{
                    t[i][j] = Math.max(t[i-1][j], t[i][j-1]);
                }
            }
        }
        return t[c1.length][c2.length];
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String s1 = scanner.nextLine();

        String s2 = scanner.nextLine();

        int result = commonChild(s1, s2);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

- - -
* 참고
  - [Wikipedia - Longest common subsequence problem](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)
