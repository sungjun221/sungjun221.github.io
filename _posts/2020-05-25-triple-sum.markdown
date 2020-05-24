---
title: "[HackerRank] Triple sum"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Triple sum
  - HackerRank
  - Search
  - Java
last_modified_at: 2020-05-25T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Triple sum](https://www.hackerrank.com/challenges/triple-sum/problem)

어떻게 풀까?
-
a,b,c의 배열에서 각각 p,q,r을 구성할 때, p<=q, r<=q에 해당하는 중복없는 경우수를 구하는 문제이다.

이 문제는 쉬워보이지만 함정이 있는 문제이다. 그리고 나는 함정에 걸렸다..

- 일일이 해당케이스를 찾아 카운트 하는 것이 아니라, a와 c가 b보다 작은 경우의 가짓수를 구해 수식으로 계산하면 된다..!

- a와 b 배열의 수가 10^5까지 가능하기 때문에 최대의 경우 두 가짓수를 곱하면 int형을 초과한 값이 나오게 된다. 그래서 a와 b를 곱하기 전에 long형으로 각각 형변환 해주어야 한다...!

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

    static long triplets(int[] a, int[] b, int[] c) {
        long rslt = 0;
        a = Arrays.stream(a).sorted().distinct().toArray();
        b = Arrays.stream(b).sorted().distinct().toArray();
        c = Arrays.stream(c).sorted().distinct().toArray();

        int ai = 0;
        int bi = 0;
        int ci = 0;

        while(bi < b.length){
            while(ai < a.length && a[ai] <= b[bi]) ai++;
            while(ci < c.length && c[ci] <= b[bi]) ci++;

            rslt += (long)ai * (long)ci;
            bi++;
        }

        return rslt;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] lenaLenbLenc = scanner.nextLine().split(" ");

        int lena = Integer.parseInt(lenaLenbLenc[0]);

        int lenb = Integer.parseInt(lenaLenbLenc[1]);

        int lenc = Integer.parseInt(lenaLenbLenc[2]);

        int[] arra = new int[lena];

        String[] arraItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < lena; i++) {
            int arraItem = Integer.parseInt(arraItems[i]);
            arra[i] = arraItem;
        }

        int[] arrb = new int[lenb];

        String[] arrbItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < lenb; i++) {
            int arrbItem = Integer.parseInt(arrbItems[i]);
            arrb[i] = arrbItem;
        }

        int[] arrc = new int[lenc];

        String[] arrcItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < lenc; i++) {
            int arrcItem = Integer.parseInt(arrcItems[i]);
            arrc[i] = arrcItem;
        }

        long ans = triplets(arra, arrb, arrc);

        bufferedWriter.write(String.valueOf(ans));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~
