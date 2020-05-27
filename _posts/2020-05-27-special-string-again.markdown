---
title: "[HackerRank] Special String Again"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Special String Again
  - HackerRank
  - String Manipulation
  - Java
last_modified_at: 2020-05-27T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Special String Again](https://www.hackerrank.com/challenges/special-palindrome-again/problem)

어떻게 풀까?
-
주어진 배열에 대해 특정 조건을 만족하는 substring의 갯수를 구하는 문제이다.

문제 그대로 풀어내는 방법도 있지만 1..N까지를 더해가는 가우스의 공식을 활용하면 문제는 더 쉽게 풀린다.

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

class NumCount {
    char c;
    int n;

    NumCount(char c, int n){
        this.c = c;
        this.n = n;
    }
}

public class Solution {

    static long substrCount(int n, String s) {
        char[] arr = s.toCharArray();
       
        List<NumCount> list = new ArrayList<>();
        // 1
        char cur = arr[0];
        int cnt = 1;
        for(int i=1; i<n; i++){
            if(arr[i] == cur){
                cnt++;
            }else{
                list.add(new NumCount(cur, cnt));
                cur = arr[i];
                cnt = 1;
            }
            cur = arr[i];
        }
        list.add(new NumCount(cur, cnt));

        // 2
        int rslt = 0;
        for(int i=0; i<list.size(); i++){
            int v = list.get(i).n;
            rslt += (v * (v+1)) / 2;
        }

        // 3
        for(int i=1; i<list.size()-1; i++){
            if(list.get(i-1).c == list.get(i+1).c && list.get(i).n == 1){
                rslt += Math.min(list.get(i-1).n, list.get(i+1).n);
            }
        }
        return rslt;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        String s = scanner.nextLine();

        long result = substrCount(n, s);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~
