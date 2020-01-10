---
title: "[프로그래머스] 기능개발"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 기능개발
  - 프로그래머스
  - Stack/Queue
  - Java
last_modified_at: 2020-01-10T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586)

어떻게 풀까?
-
문제를 풀 때 한 발자국 더 생각하는 연습을 해야하겠다.

처음에 풀 때는 배열을 순차적으로 순회하면서 그때마다 남은날짜값을 비교하였다. 그래서 뒷값이 적으면 Queue에 담고, 뒷값이 크면 이제까지 쌓인 Queue의 Size값으로 Count를 세어 결과값에 담았다.

그러나 개발 후 다른코드를 참고해보니, 한 번에 남은날짜를 계산하여 Queue에 쭉 담은 뒤 Queue값을 쭉 비워가며 Count를 세면 더 간단하게 만들어진다.

그리고 개발하면서 실수한 점!

Progress / speed 를 나눠서 남은일정을 구하면 나머지가 남는 경우에 날짜가 하루 빠지게 된다. 이런 식의 문제를 풀 때 주의해야 겠다.

그리고 연산 중 괄호를 제대로 안써서 잘못된 값이 나오기도 했다. 연산자 우선순위를 고려하여 괄호를 평소에 잘 기입하는 버릇을 들이자.

컴파일을 하지 않고 에디터에 그대로 쭉 작성해보면서 문제를 풀다보니, 오탈자가 많다. 오탈자 없이 잘 작성하는 것을 연습하자. 


문제풀이
-
~~~java
import java.util.*;

class Solution{
  	public int[] solution(int[] progresses, int[] speeds){
	  	int[] answer = null;
	    Queue<Integer> queue = new LinkedList<>();
	    List<Integer> resultList = new LinkedList<>();
	    int cnt = 1;

	    for(int i=0; i<progresses.length; i++){
	    	int remain = (100 - progresses[i]) / speeds[i];
	    	int reminder = (100 - progresses[i]) % speeds[i];
	    	
	    	if(reminder > 0){
	    		remain += 1;
	    	}
	    	queue.add(remain);
	    }

	    int preRemain = queue.poll();
	    while(queue.size() > 0){
	    	int remain = queue.poll();
	    	if(preRemain < remain){
	    		resultList.add(cnt);
	    		cnt = 1;
	    		preRemain = remain;
	    	}else{
	    		cnt++;
	    	}
	    }

	    if(cnt > 0){
	    	resultList.add(cnt);
	    }

	    answer = new int[resultList.size()];
	    for(int i=0; i<answer.length; i++){
	    	answer[i] = resultList.get(i);
	    }

    	return answer;
    }
}
~~~