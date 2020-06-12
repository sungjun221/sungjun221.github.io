---
title: "[HackerRank] Reverse Shuffle Merge"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Reverse Shuffle Merge
  - HackerRank
  - Greedy Algorithm
  - Java
last_modified_at: 2020-06-12T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Reverse Shuffle Merge](https://www.hackerrank.com/challenges/reverse-shuffle-merge/problem)

어떻게 풀까?
-
문자 A에 함수들이 아래와 같이 적용된다.
- reverse(A) : 반대로 뒤집는다. ex) abc -> cba
- shuffle(A) : 랜덤하게 섞는다. ex) abc -> abc, acb, bac, bca, cab, cba
- merge(A1, A2) : A1과 A2를 각각 서로의 문자 순서를 유지한채 병합한다. ex) A1=abc, A2=def, merge(A1, A2) -> abcdef, adbecf, deabfc

문자 s는 merge(reverse(A), shuffle(A))일 때, 사전적으로 앞선 알파벳이 우선하도록 하는 가장 작은 A를 구하는 문제이다.

문제를 제대로 이해하는 것에서 애를 먹었다. 예제를 보고 merge(A1, A2)의 경우 A1의 값이 A2에 앞선 것으로 잘못 이해했다. A2의 두번째 값이 A1의 두번째값보다 먼저 나올 수 있다.

다시 문제를 보면, reverse와 shuffle을 하는데 shuffle의 경우는 랜덤하기 떄문에 무시한다.

reverse 되어 있기 때문에 s값을 뒤에서부터 순회하며 대상이 되는 문자를 찾아낸다. 각 알파벳 문자가 노출될 수 있는 count값을 구하여 배열에 저장하고, merge 되어 있기 때문에 각 값을 1/2 한다.

이 배열을 결과값 A에 추가할 수 있는 문자 별 수를 저장하는 count배열과, 추가하지 않고 넘길 수 있는 count배열로 따로 저장한다. 

그리고 s를 순회하며 해당 문자가 A값에 추가될 수 있는지를 판별하기 위해 Stack을 만들고 뒤에 오는 s의 값들과 사전적 순서를 비교한다.

그래서 순회하는 s의 현재값의 사전적 순서가 뒤에 있으면 Stack에 쌓여 있는 값은 모두 제거하고, 앞선다면 그 위에 현재 값을 추가한다.

문제풀이(Java)
-
~~~java
    // 작성 중
~~~