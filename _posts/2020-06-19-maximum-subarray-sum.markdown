---
title: "[HackerRank] Maximum Subarray Sum"
categories: 
  - Algorithm
tags:
  - Algorithm
  - HackerRank
  - Search
  - Java
last_modified_at: 2020-06-19T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Maximum Subarray Sum](https://www.hackerrank.com/challenges/maximum-subarray-sum/problem)

어떻게 풀까?
-
이 문제는 (나한테) 어렵다. 물론 난이도가 Hard이지만 역시 어렵다. Brute Force로 푸는 것은 간단하지만 그렇게 풀게 놔둘리가 없다.

이 문제는 주어진 배열에서 연이어진 값들을 고를 때, 이 값들의 합을 m으로 나눈 값이 가장 큰 경우가 무엇인지 찾는 것이다. (가장 큰 부분누적합의 나머지)

가장 큰 연속한 부분의 합을 구하는 이런 유형의 문제를 Maximum Subarray Problem라고 부르는데 이 문제는 거기서 m으로 나눈 값 중 큰 값을 구하게 함으로서 문제를 한 번 더 꼬았다.

이 문제를 풀 때 아래와 같은 점을 생각해보아야 한다. 

- 중간에 음수값이 있지 않는 한 배열은 뒤로 갈수록 누적합이 커진다.

- i..j까지 누적합의 나머지를 구하려면 0..j까지의 누적합의 나머지에서 0..i까지의 누적합의 나머지를 빼면 된다.

- 그런데 0..i까지의 누적합의 나머지가 0..i까지의 누적합보다 크면 -값이 나오게 된다. 이 경우는 m값을 더한 후 빼면 된다.

- 선택된 누적합의 나머지보다 +1 이상인 값 중 가장 작은 값이 -값이 나오는 경우 중 가장 큰 max값을 갖게 된다.


솔직히 푸는 방식을 이해하는데도 꽤 시간이 걸렸다. 여러번 복습해야할 것 같다.

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

    static long maximumSum(long[] a, long m) {
        long max = 0;
        long modSum = 0;
        TreeSet<Long> modSums = new TreeSet<>();

        for(long v : a){
            modSum = (modSum + v) % m;
            Long closesetLargerSum = modSums.ceiling(modSum + 1);
            if(closesetLargerSum == null) closesetLargerSum = 0L;

            if(closesetLargerSum > 0){
                max = Math.max(max, modSum - closesetLargerSum + m);
            }

            max = Math.max(max, modSum);
            modSums.add(modSum);
        }
        return max;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            String[] nm = scanner.nextLine().split(" ");

            int n = Integer.parseInt(nm[0]);

            long m = Long.parseLong(nm[1]);

            long[] a = new long[n];

            String[] aItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                long aItem = Long.parseLong(aItems[i]);
                a[i] = aItem;
            }

            long result = maximumSum(a, m);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

- - -
* 참고
    - [brokensandals.net](https://brokensandals.net/technical/programming-challenges/hackerrank-maximum-subarray-sum)
    - [geeksforgeeks](https://www.geeksforgeeks.org/maximum-subarray-sum-modulo-m)
