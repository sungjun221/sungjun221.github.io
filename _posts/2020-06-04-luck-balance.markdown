---
title: "[HackerRank] Luck Balance"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Luck balance
  - HackerRank
  - Greedy Algorithm
  - Java
last_modified_at: 2020-06-04T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Luck Balance](https://www.hackerrank.com/challenges/luck-balance/problem)

어떻게 풀까?
-
컨테스트 참여하여 떨어질 경우는 운이 상승하고, 이길 경우는 운이 감소한다. 중요한 컨테스트는 k번 이하로 질 수 있다. 이 때 가장 큰 운의 값을 구하는 문제이다.

Java의 경우, 중요한 컨테스트는 PriorityQueue로 입력값을 내림차순 한 뒤 값을 더한다. k번 이후에는 뺀다. 중요하지 않은 컨테스트는 그냥 더한다.

- (주의) PriorityQueue와 같이 값을 넣고 빼는 자료구조는 for문으로 돌릴 경우 size값이 달라지기 때문에 기대한대로의 동작을 하지 않게 된다. 


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
    
    static int luckBalance(int k, int[][] contests) {
        Queue<Integer> q = new PriorityQueue<>(Collections.reverseOrder());
        int max = 0;
    	
        for(int i=0; i<contests.length; i++){
            if(contests[i][1] == 1){
            	q.add(contests[i][0]);
            }else {
            	max += contests[i][0];
            }
        }
        
        int i=0;
        while(!q.isEmpty()) {
        	if(i < k) {
        		max += q.poll();	
        	}else {
        		max -= q.poll();
        	}
        	i++;
        }
        return max;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nk[0]);

        int k = Integer.parseInt(nk[1]);

        int[][] contests = new int[n][2];

        for (int i = 0; i < n; i++) {
            String[] contestsRowItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int j = 0; j < 2; j++) {
                int contestsItem = Integer.parseInt(contestsRowItems[j]);
                contests[i][j] = contestsItem;
            }
        }

        int result = luckBalance(k, contests);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
~~~