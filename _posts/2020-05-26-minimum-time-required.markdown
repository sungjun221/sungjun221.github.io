---
title: "[HackerRank] Minimum Time Required"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Minimum Time Required
  - HackerRank
  - Search
  - Java
last_modified_at: 2020-05-26T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Minimum Time Required](https://www.hackerrank.com/challenges/minimum-time-required/problem)

어떻게 풀까?
-
주어진 배열만큼의 기계가 있을 때, 각 기계는 제품 하나를 생산하는데 해당 배열의 값만큼의 날짜가 필요하다. 이 때 생산목표수만큼 제품을 생산하려면 며칠이 걸리는지 구하는 문제이다.

이 문제는 Binary Search를 이용해 풀 수 있다. 모든 기계가 가장 느린 기계라고 가정하면 이 가장 느린 날짜와 가장 빠른 날짜 사이에서 BS를 통해 좁혀가는 문제이다.

BS 문제는 재귀적으로 풀거나 순회하며 푸는 2가지 방식으로 풀 수 있다.

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
    /* method 1 */
    static long binarySearchIter(long[] machines, long goal, long minDay, long maxDay){
        long mid = 0;
        long prd = 0;

        while(minDay < maxDay){
            prd = 0;
            mid = (minDay + maxDay) / 2;

            for(int i=0; i<machines.length; i++){
                prd += Math.floor(mid/machines[i]);
            }

            if(prd >= goal){
                maxDay = mid;
            }else if(prd < goal){
                minDay = mid + 1;
            }
        }
        return minDay;
    }

    /* method 2 */
    static long binarySearchRecursive(long[] machines, long goal, long minDay, long maxDay){
        if(minDay == maxDay) return minDay;

        long mid = (minDay + maxDay) / 2;
        long prd = 0;

        for(int i=0; i<machines.length; i++){
            prd += Math.floor(mid/machines[i]);
        }

        if(prd >= goal){
            return binarySearchRecursive(machines, goal, minDay, mid);
        }else if(prd < goal){
            return binarySearchRecursive(machines, goal, mid+1, maxDay);
        }
        return -1;
    }

    static long minTime(long[] machines, long goal) {
        Arrays.sort(machines);
        return binarySearchIter(machines, goal, 1, machines[machines.length-1] * goal);             /* iter */
        //return binarySearchRecursive(machines, goal, 1, machines[machines.length-1] * goal);      /* recursive */
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nGoal = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nGoal[0]);

        long goal = Long.parseLong(nGoal[1]);

        long[] machines = new long[n];

        String[] machinesItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            long machinesItem = Long.parseLong(machinesItems[i]);
            machines[i] = machinesItem;
        }

        long ans = minTime(machines, goal);

        bufferedWriter.write(String.valueOf(ans));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~
