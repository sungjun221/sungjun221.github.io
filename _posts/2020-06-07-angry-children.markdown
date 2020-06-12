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
  - Python
  - Javascript
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

문제풀이(Python)
-
Python의 List는 append()로 값을 담는다.
~~~python
#!/bin/python3

import math
import os
import random
import re
import sys

# Complete the maxMin function below.
def maxMin(k, arr):
    arr.sort()
    diff = []
    unfairness = 0
    min = sys.maxsize

    for i in range(len(arr)-1):
        diff.append(arr[i+1] - arr[i])

    for i in range(k-1):
        unfairness += diff[i]

    min = unfairness

    for i in range(k-1, len(diff)):
        unfairness -= diff[i-(k-1)]
        unfairness += diff[i]

        if min > unfairness:
            min = unfairness

    return min

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(input())

    k = int(input())

    arr = []

    for _ in range(n):
        arr_item = int(input())
        arr.append(arr_item)

    result = maxMin(k, arr)

    fptr.write(str(result) + '\n')

    fptr.close()
~~~

문제풀이(Javascript)
-
Javascript의 array는 push()로 값을 담는다.
~~~javascript
'use strict';

const fs = require('fs');

process.stdin.resume();
process.stdin.setEncoding('utf-8');

let inputString = '';
let currentLine = 0;

process.stdin.on('data', inputStdin => {
    inputString += inputStdin;
});

process.stdin.on('end', function() {
    inputString = inputString.replace(/\s*$/, '')
        .split('\n')
        .map(str => str.replace(/\s*$/, ''));

    main();
});

function readLine() {
    return inputString[currentLine++];
}

function maxMin(k, arr) {
    arr.sort((a,b)=> a-b);
    let diff = [];
    let unfairness = 0;
    let min = Number.MAX_SAFE_INTEGER;

    for(let i=0; i<arr.length-1; i++){
        diff.push(arr[i+1] - arr[i]);
    }

    for(let i=0; i<k-1; i++){
        unfairness += diff[i];
    }
    min = unfairness;

    for(let i=(k-1); i<diff.length; i++){
        unfairness -= diff[i-(k-1)];
        unfairness += diff[i];

        if(min > unfairness){
            min = unfairness;
        }
    }

    return min;
}

function main() {
    const ws = fs.createWriteStream(process.env.OUTPUT_PATH);

    const n = parseInt(readLine(), 10);

    const k = parseInt(readLine(), 10);

    let arr = [];

    for (let i = 0; i < n; i++) {
        const arrItem = parseInt(readLine(), 10);
        arr.push(arrItem);
    }

    const result = maxMin(k, arr);

    ws.write(result + '\n');

    ws.end();
}
~~~
