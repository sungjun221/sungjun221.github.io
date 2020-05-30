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
last_modified_at: 2020-05-30T12:04:24-04:00
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

start에 k값을 더하고 end+1에 k값을 빼서 배열의 변화값을 표현한다.c

그리고 이 배열을 순회하며 변화값을 더하고 빼서 max값을 구하는 것이다.
 

문제풀이(Java)
-
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    static long arrayManipulation(int n, int[][] queries) {
        long max = 0;
        long x = 0;
        int[] arr = new int[n+1];

        for(int i=0; i<queries.length; i++){
            int a = queries[i][0];
            int b = queries[i][1];
            int k = queries[i][2];
            
            arr[a] += k;
            if((b+1) <= n){
                arr[b+1] -= k;
            }
        }

        for(int i=1; i<(n+1); i++){
            x += arr[i];
            if(max < x) max = x;
        }

        return max;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nm = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nm[0]);

        int m = Integer.parseInt(nm[1]);

        int[][] queries = new int[m][3];

        for (int i = 0; i < m; i++) {
            String[] queriesRowItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 3; j++) {
                int queriesItem = Integer.parseInt(queriesRowItems[j]);
                queries[i][j] = queriesItem;
            }
        }

        long result = arrayManipulation(n, queries);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~
