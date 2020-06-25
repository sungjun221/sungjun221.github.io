---
title: "특정 수의 노출 횟수 구하기"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Java
last_modified_at: 2020-06-25T12:04:24-04:00
toc: true
---
문제정보
-
Integer형인 두 숫자 v와 f가 있다. 한자리 숫자값인 f가 v에서 몇 번이나 노출되는지 리턴하는 프로그램을 짜라.

- v는 0 ~ 2,100,000,000
- f는 0 ~ 9

문제풀이(Java)
-
~~~java
import java.io.IOException;
import java.util.Scanner;

public class Solution {
	private static final Scanner scanner = new Scanner(System.in);
	
	private static int count(int v, int f) {
		int cnt = 0;
		while(v > 0) {
			if((v % 10) == f) cnt++;
			v /= 10;
		}
		return cnt;
	}
	
	public static void main(String[] args) throws IOException {
		System.out.println("Please enter any number (0-2,100,000,000)");
        int v = scanner.nextInt();
		System.out.println("Enter the number(0-9) you want to know how many times it appers from the number you entered previously");
        int f = scanner.nextInt();
        
        int rslt = count(v,f);
        System.out.println("result : " + rslt);

        scanner.close();
    }
}
~~~