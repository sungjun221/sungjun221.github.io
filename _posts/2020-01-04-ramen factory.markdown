---
title: "[프로그래머스] 라면공장"
categories: 
  - Algorithm
tags:
  - Algorithm
  - 라면공장
  - 프로그래머스
  - Heap
  - Java
last_modified_at: 2020-01-04T12:04:24-04:00
toc: true
---

문제정보
-
- [프로그래머스 - 라면공장](https://programmers.co.kr/learn/courses/30/lessons/42629)


문제풀이
-
왜 Heap을 써서 풀어야 하는가?
문제를 있는 그대로 해석하면 DFS를 이용해 라면공장 재고가 떨어지지 않는 경우의 수들을 찾아 그 중 가장 적은 값을 찾아야 할 것 같다.

하지만 이 문제는 한 번 더 생각을 해야한다. Heap을 이용해 공급을 받을 수 있는 케이스들을 추가해놓고, 재고가 0이 된 시점에 최대의 공급을 받을 수 있었던 경우를 선택하는 것이다.

그러면 가장 적은 횟수의 공급을 받을 수 있다. 이런 발상의 전환이 쉽지 않다. 연습하면 늘겠지.

(참고 : Heap은 자료구조(Data Structure), 우선순위큐는 ADT(Abstract Data Type, 추상자료형)이다. ADT는 구현을 명시하지 않고 자료와 자료에 대한 연산을 명기한 것이다.

반면에 Java의 PriorityQueue는 Queue인터페이스의 구현클래스이다.)

~~~java
import java.util.*;

class Solution {
    private Queue<Integer> q = new PriorityQueue<>(Comparator.reverseOrder());
    
    public int solution(int stock, int[] dates, int[] supplies, int k) {
        int scnt = 0;
        int idx = 0;
        
        for(int i=0; i<k; i++){
            if(idx < dates.length && dates[idx] == i){
                q.add(supplies[idx++]);
            }
            if(stock - i <= 0){
                stock += q.poll();
                scnt++;
            }
        }
        return scnt;
    }
}
~~~