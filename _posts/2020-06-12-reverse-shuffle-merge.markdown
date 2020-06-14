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
  - Python
  - JavaScript
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
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {

    static String reverseShuffleMerge(String s) {
        int[] addCnt = new int[26];
        int[] skipCnt = new int[26];
        Stack<Character> st = new Stack<>();
        String rslt = "";

        char[] ss = s.toCharArray();

        for(int i=0; i<ss.length; i++){
            addCnt[ss[i] - 'a']++;
        }

        for(int i=0; i<addCnt.length; i++){
            addCnt[i] /= 2;
        }

        skipCnt = addCnt.clone();

        for(int i=ss.length-1; i>=0; i--){
            while(!st.empty() && st.peek() > ss[i] && addCnt[ss[i] - 'a'] > 0 && skipCnt[st.peek() - 'a'] > 0){
                char c = st.pop();
                addCnt[c - 'a']++;
                skipCnt[c - 'a']--;
            }

            if(addCnt[ss[i] - 'a'] > 0){
                st.push(ss[i]);
                addCnt[ss[i] - 'a']--;
            }else{
                skipCnt[ss[i] - 'a']--;
            }
        }

        while(!st.empty()){
            rslt = st.pop() + rslt;
        }
        return rslt;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String s = scanner.nextLine();

        String result = reverseShuffleMerge(s);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

문제풀이(Python)
--
~~~python
#!/bin/python3
import copy
import math
import os
import random
import re
import sys


# Complete the reverseShuffleMerge function below.
def reverseShuffleMerge(s):
    addCnt = [0] * 26
    st = []
    rslt = ""

    ss = list(s)

    for i in range(len(ss)):
        addCnt[ord(ss[i]) - ord('a')] += 1

    for i in range(len(addCnt)):
        addCnt[i] = addCnt[i] // 2

    skipCnt = copy.deepcopy(addCnt)

    for i in range(len(ss) - 1, -1, -1):
        while st and st[-1] > ss[i] and addCnt[ord(ss[i]) - ord('a')] > 0 and skipCnt[ord(st[-1]) - ord('a')] > 0:
            c = st.pop()
            addCnt[ord(c) - ord('a')] += 1
            skipCnt[ord(c) - ord('a')] -= 1

        if addCnt[ord(ss[i]) - ord('a')] > 0:
            st.append(ss[i])
            addCnt[ord(ss[i]) - ord('a')] -= 1
        else:
            skipCnt[ord(ss[i]) - ord('a')] -= 1

    while st:
        rslt = st.pop() + rslt

    return rslt


if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    s = input()

    result = reverseShuffleMerge(s)

    fptr.write(result + '\n')

    fptr.close()
~~~

문제풀이(JavaScript)
--
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

process.stdin.on('end', _ => {
    inputString = inputString.replace(/\s*$/, '')
        .split('\n')
        .map(str => str.replace(/\s*$/, ''));

    main();
});

function readLine() {
    return inputString[currentLine++];
}

Array.prototype.last = function(){
    return this[this.length - 1];
};

function reverseShuffleMerge(s) {
    let addCnt = new Array(26).fill(0);
    let skipCnt;
    let st = [];
    let rslt = "";

    for(let i=0; i<s.length; i++){
        addCnt[charIdx(s[i])]++;
    }

    for(let i=0; i<addCnt.length; i++){
        addCnt[i] = Math.floor(addCnt[i]/2);
    }

    skipCnt = [...addCnt];

    for(let i=s.length-1; i>=0; i--){
        while(st.length > 0 && st.last() > s[i]  && addCnt[charIdx(s[i])] > 0 && skipCnt[charIdx(st.last())] > 0){
            let c = st.pop();
            addCnt[charIdx(c)]++;
            skipCnt[charIdx(c)]--;
        }

        if(addCnt[charIdx(s[i])] > 0){
            st.push(s[i]);
            addCnt[charIdx(s[i])]--;
        }else{
            skipCnt[charIdx(s[i])]--;
        }
    }

    while(st.length > 0){
        rslt = st.pop() + rslt;
    }
    return rslt;
}

function charIdx(c){
    return c.charCodeAt(0) - 97;
}

function main() {
    const ws = fs.createWriteStream(process.env.OUTPUT_PATH);

    const s = readLine();

    let result = reverseShuffleMerge(s);

    ws.write(result + "\n");

    ws.end();
}
~~~
