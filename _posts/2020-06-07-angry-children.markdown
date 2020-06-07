---
title: "[HackerRank] Max Min"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Max Min
  - HackerRank
  - Greedy Algorithm
  - Java
last_modified_at: 2020-06-07T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Max Min](https://www.hackerrank.com/challenges/angry-children/problem)

어떻게 풀까?
-
n개의 값을 가진 배열 arr에서 k개만큼 값을 선택했을 때 Max(arr) - Min(arr) 값이 가장 작은 경우의 값을 출력하는 문제이다.

배열을 정렬한 후에 각 배열간의 차이를 구한 배열을 만들고, Queue처럼 한칸씩 움직이며 Min값을 체크하는 식으로 구현하였다.

이번 문제는 꽤 수월히 풀려서 좋다.

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

    static int maxMin(int k, int[] arr) {
        Arrays.sort(arr);
        int[] diff = new int[arr.length-1];
        int unfairness = 0;
        int min = Integer.MAX_VALUE;

        for(int i=0; i<diff.length; i++){
            diff[i] = arr[i+1] - arr[i];
        }

        for(int i=0; i<k-1; i++){
            unfairness += diff[i];
        }
        min = unfairness;

        for(int i=(k-1); i<diff.length; i++){
            unfairness -= diff[i-(k-1)];
            unfairness += diff[i];

            if(min > unfairness){
                min = unfairness;
            }
        }

        return min;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int k = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        int[] arr = new int[n];

        for (int i = 0; i < n; i++) {
            int arrItem = scanner.nextInt();
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");
            arr[i] = arrItem;
        }

        int result = maxMin(k, arr);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~
