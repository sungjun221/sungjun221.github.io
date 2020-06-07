---
title: "[HackerRank] Greedy Florist"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Greedy Florist
  - HackerRank
  - Greedy Algorithm
  - Java
  - Python
  - Javascript
last_modified_at: 2020-06-06T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Greedy Florist](https://www.hackerrank.com/challenges/greedy-florist/problem)

어떻게 풀까?
-
k명의 사람이 가격이 c[i]인 n개의 꽃을 다 사야하는데, x번째 살수록 (x+1) * c[i] 가 된다. 즉 점점 비싸진다. 

꽃의 가격이 점점 비싸지기 때문에 반대로 처음에 가장 비싼 것을 골라야 나중에 좀 더 싼 값에 꽃을 구입할 수 있게 된다.

Array를 정렬하여 비싼가격부터 구입하는 탐욕알고리즘으로 풀어야 한다. 풀고 나서 생각하니 쉬운데 과정중에 꽤 헷갈렸다..

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

    static int getMinimumCost(int k, int[] c) {
        Arrays.sort(c);
        int cost = 0;
        int idx = 0;
        int t = 0;
        
        while(idx < c.length){
            t = idx / k;
            cost += (t + 1) * c[c.length-1-idx];
            idx++;
        }
        return cost;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nk[0]);

        int k = Integer.parseInt(nk[1]);

        int[] c = new int[n];

        String[] cItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            int cItem = Integer.parseInt(cItems[i]);
            c[i] = cItem;
        }

        int minimumCost = getMinimumCost(k, c);

        bufferedWriter.write(String.valueOf(minimumCost));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

문제풀이(Python)
-
Python에서 /를 하면 실수형으로 반환되기 때문에 //를 하여 정수값을 얻는다.
~~~python
#!/bin/python3

import math
import os
import random
import re
import sys

# Complete the getMinimumCost function below.
def getMinimumCost(k, c):
	c.sort()
	cost = 0
	idx = 0
	t = 0

	while idx < len(c):
		t = idx // k
		cost += (t + 1) * c[len(c)-1-idx]
		idx += 1

	return cost

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    nk = input().split()

    n = int(nk[0])

    k = int(nk[1])

    c = list(map(int, input().rstrip().split()))

    minimumCost = getMinimumCost(k, c)

    fptr.write(str(minimumCost) + '\n')

    fptr.close()
~~~

문제풀이(Javascript)
-
Javascript에서 array sort()를 하면 ASCII 문자 순서로 정렬되어 크기순대로 정렬되지 않는다. (ex: 1, 11, 2, 3, 33, 4, ...)

sortFunction을 넘겨주어 정렬을 시켜야한다.
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

function getMinimumCost(k, c) {
    c.sort((a,b)=> a-b);
    let cost = 0;
    let idx = 0;
    let t = 0;

    while(idx < c.length){
        t = Math.floor(idx / k);
        cost += (t + 1) * c[c.length-1-idx];
        idx += 1;
    }

    return cost;
}

function main() {
    const ws = fs.createWriteStream(process.env.OUTPUT_PATH);

    const nk = readLine().split(' ');

    const n = parseInt(nk[0], 10);

    const k = parseInt(nk[1], 10);

    const c = readLine().split(' ').map(cTemp => parseInt(cTemp, 10));

    const minimumCost = getMinimumCost(k, c);

    ws.write(minimumCost + '\n');

    ws.end();
}
~~~
