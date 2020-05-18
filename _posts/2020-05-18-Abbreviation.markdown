---
title: "[HackerRank] Abbreviation"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Abbreviation
  - HackerRank
  - Dynamic Programming
  - Java
last_modified_at: 2020-05-18T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Abbreviation](https://www.hackerrank.com/challenges/abbr/problem)

어떻게 풀까?
-
주어진 두 문자의 동일한 부분을 비교하는 LCS(Longest Common Subsequence)의 응용문제이다. Dynamic Programming은 Divide and Conquer(분할정복기법)으로 풀어야 하는 문제에 적용할 수 있다.

아래와 같은 문제유형은 Dynamic Programming으로 풀 수 있다.

- 막대기 자르기
- Longest Common Subsequence(최장 공통 부분 수열)
- 0/1 배낭문제

이름이 좀 이상하기로 유명한 알고리즘이다. 국내에선 '기억하기 알고리즘'으로 부르자고 제안한 교수님도 있다고 한다. 

쉽게 말해 테이블을 만들어서 이전의 값을 기억하여 재활용 하는 것이다.

그래서 점화식(漸化式, Recurrence relation, 재귀식)으로 풀어야 하는 문제에 적용할 수 있다. 수열 간 항의 관계를 표현한 점화식(예 : 피보나치 수열)이 이전의 값을 재활용 해야 하는 경우이기 때문이다.


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

    static boolean isEqual(char a, char b){
        return a == b || Character.toUpperCase(a) == b;
    }

    static String abbreviation(String a, String b) {
        boolean[][] dp = new boolean[a.length()+1][b.length()+1];
        dp[0][0] = true;

        for(int i=1; i < a.length()+1; i++){
            for(int j=0; j < b.length()+1; j++){
                if(j > 0 && dp[i-1][j-1] && isEqual(a.charAt(i-1), b.charAt(j-1))){
                    dp[i][j] = true;
                }
                if(Character.isLowerCase(a.charAt(i-1)) && dp[i-1][j]){
                    dp[i][j] = true;
                }
            }
        }
        return dp[a.length()][b.length()] ? "YES" : "NO";
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            String a = scanner.nextLine();

            String b = scanner.nextLine();

            String result = abbreviation(a, b);

            bufferedWriter.write(result);
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

- - -
* 참고
  - [zerocho - Dynamic Programming](https://www.zerocho.com/category/Algorithm/post/584b979a580277001862f182)