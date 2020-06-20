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

문제풀이(Java)
-
~~~java

~~~
