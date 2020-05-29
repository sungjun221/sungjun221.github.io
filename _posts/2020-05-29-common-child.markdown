---
title: "[HackerRank] Common Child"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Common Child
  - HackerRank
  - String Manipulation
  - Java
last_modified_at: 2020-05-29T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Common Child](https://www.hackerrank.com/challenges/common-child/problem)

어떻게 풀까?
-
이 문제는 LCS(Longest Common Subsequence)문제이다. LCS 문제는 Dynamic Programming을 이용해서 풀 수 있다.

공통된 값의 count를 계산하는 테이블을 만들고 각 위치의 가장 큰 count값을 기록해 나가는 것이다.

자세한 내용은 참고란에 URL을 첨부해둔다.
 

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