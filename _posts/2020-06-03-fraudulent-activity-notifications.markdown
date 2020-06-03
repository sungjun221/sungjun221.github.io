---
title: "[HackerRank] Fraudulent Activity Notifications"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Fraudulent Activity Notifications
  - HackerRank
  - Sorting
  - Java
  - Python
last_modified_at: 2020-06-03T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Fraudulent Activity Notifications](https://www.hackerrank.com/challenges/fraudulent-activity-notifications/problem)

어떻게 풀까?
-
주어진 날짜배열에서 현재날짜의 거래금액이 d만큼 이전날짜들 금액의 중간값 2배 이상이 되는 경우가 몇 번이나 있는지 구하는 문제이다.

역시나 그냥 구하면 Timed out이 뜬다. 중간값을 구하는 것을 효율화해야한다. 문제의 제약사항에서 expenditure[i]의 값이 0~200 사이라는 것이 포인트이다.

200개 크기의 배열을 만들고 대상 날짜들의 갯수만큼 count 를 올려놓고 이 값을 이용하는 것이다. 또한 날짜를 이동할 때마다 Queue와 같이 이전값은 빼고 새로운 값은 추가한다.

실수를 많이 한 문제이다..


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
    
    static double findMedian(int[] cntArr, int d){
        int cnt = 0;
        double rslt = 0;

        if(d%2 != 0){
            for(int i=0; i<cntArr.length; i++){
                cnt += cntArr[i];

                if(cnt > d/2){
                    rslt = i;
                    break;
                }
            }
        }else{
            int first = 0;
            int second = 0;

            for(int i=0; i<cntArr.length; i++){
                cnt += cntArr[i];
                if(first == 0 && cnt >= d/2){
                    first = i;
                }
                if(second == 0 && cnt >= d/2 + 1){
                    second = i;
                    break;
                }
            }
            rslt = (first + second) / 2.0;
        }
        return rslt;
    }
    
    static int activityNotifications(int[] expenditure, int d) {
        int noti = 0;
        int[] cntArr = new int[201];

        for(int i=0; i<d; i++){
            cntArr[expenditure[i]]++;
        }

        for(int i=d; i<expenditure.length; i++){
            double median = findMedian(cntArr, d);

            if(2*median <=expenditure[i]){
                noti++;
            }

            cntArr[expenditure[i-d]]--;
            cntArr[expenditure[i]]++;
        }

        return noti;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nd = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nd[0]);

        int d = Integer.parseInt(nd[1]);

        int[] expenditure = new int[n];

        String[] expenditureItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            int expenditureItem = Integer.parseInt(expenditureItems[i]);
            expenditure[i] = expenditureItem;
        }

        int result = activityNotifications(expenditure, d);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~

문제풀이(Python)
-
~~~python
#!/bin/python3

import math
import os
import random
import re
import sys

def findMedian(cntarr, d):
    cnt = 0
    rslt = 0

    if d%2 is not 0:
        for i in range(len(cntarr)):
            cnt += cntarr[i]

            if cnt > d//2:
                rslt = i
                break
    else:
        first = 0
        second = 0

        for i, _ in enumerate(cntarr):
            cnt += cntarr[i]
            if first == 0 and cnt >= d//2:
                first = i
            if second == 0 and cnt >= d//2 + 1:
                second = i
                break
        rslt = (first+second) / 2
    return rslt


def activityNotifications(expenditure, d):
    noti = 0
    cntarr = [0 for _ in range(201)]
    for i in range(d):
        cntarr[expenditure[i]] += 1

    for i in range(d, len(expenditure)):
        median = findMedian(cntarr, d)

        if 2*median <= expenditure[i]:
            noti += 1

        cntarr[expenditure[i-d]] -= 1
        cntarr[expenditure[i]] += 1

    return noti


if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    nd = input().split()

    n = int(nd[0])

    d = int(nd[1])

    expenditure = list(map(int, input().rstrip().split()))

    result = activityNotifications(expenditure, d)

    fptr.write(str(result) + '\n')

    fptr.close()
~~~