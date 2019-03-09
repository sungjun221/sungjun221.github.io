---
title: "[HackerRank] Min Max Riddle"
categories: 
  - Algorithm
  - Main
tags:
  - Algorithm
  - Min Max Riddle
  - HackerRank
last_modified_at: 2019-03-08T12:04:24-04:00
toc: true
---
> 공부하면서 쓴 글이라 잘못된 내용이 있을 수 있습니다. 그런 부분은 댓글로 알려주시면 감사합니다.

문제정보
-
- [HackerRank - Min Max Riddle](https://www.hackerrank.com/challenges/min-max-riddle/problem)

어떻게 풀까?
-
이 문제는 Array에서 Window Size 1부터 n까지 증가할 때, 각 Size값의 최소값들 중 최대값을 구하는 것이다. 처음엔 문제에서 설명한 그대로 배열을 순회하며 값을 찾았더니 역시 Timed Out.. 복잡도가 O(n^2)으로는 문제가 풀리지 않는다. 

이 문제를 풀기 위해선 반대의 관점에서 생각해봐야 한다. 문제를 읽으면 Array의 값들 중 최소값을 구하고..다시 최대값을 구하고.. 하기 때문에 Array의 값들에 집중하게 된다. 그러나 반대로 Window Size를 기준으로 삼는다면 어떨까? 나는 이 생각을 못했다. Discussions 게시판에서 글을 읽다가 이 설명을 읽고 깜짝 놀랐다. 세상에 머리 좋은 사람들이 이렇게 많다.

먼저 Array의 각 값들이 최소값이 될 수 있는 Window Size 중에 가장 큰 값이 얼마인지를 찾아보자. 예를 들어 {11, 2, 3, 14, 5, 2, 11, 12}의 값이 주어졌다고 하자. 그렇다면 number -> window_size라고 할 때 max_window = {11: 2, 2: 8, 3: 3, 14: 1, 5: 2, 12: 1}가 된다. 11이 최소값이 될 수 있는 최대 Window Size가 2인 것이다.

그럼 이제 반대로 뒤집어보자. Key를 Window Size 기준으로 하는 것이다. 뒤집은 값 중 중복되는 Window Size는 가진 값이 최대인 것만 남겨놓자. 그러면 window_size -> max_value가 되며 inverted_windows = {1: 14, 8:2, 3:3, 2:11}가 된다. 즉 Window Size가 1일 때 가질 수 있는 최대값은 14가 되는 것이다.

구하고자 하는 최대값은 Window Size가 작을수록 커진다. Window Size가 작을수록 큰 값이 최소값이 되기 때문이다. Window Size의 큰 값부터 작은값까지 값을 나열해보자. Window Size가 8일 때는 최대값은 2가 나온다. 7일 때는 따로 구해진 값이 없기 때문에 8과 같은 최대값 2이다. 6,5,4...도 마찬가지이다. 그러다 Window Size 3의 최대값은 3이 나온다. 이렇게 정리해보면 result = [2, 2, 2, 2, 2, 3, 11, 14]가 나온다.

이 값을 Widnow Size 1부터 n까지의 최대값으로 표시하기 위해 뒤집어보면 return [14, 11, 3, 2, 2, 2, 2, 2]라는 최종값이 나온다.


Stock Span Problem
-
험난한(?) 여정이었다. 하지만 여기서 생각해 볼 것이 아직 하나 더 있다. Array의 각 값들이 최소값이 될 수 있는 최대 Window Size는 어떻게 구할 수 있을까? 이것을 구하기 위해서는 Stock Span Problem을 푸는 방법을 이해해야 한다.

Stock Span Problem은 Stock Prices 배열에서 k일째인 price[k]가, 0부터 k일까지 중 k번째일이 최고가격인 날이 연속으로 며칠째 이어졌는지 구하는 문제다. 써놓고보니 나도 나중에 읽으면 무슨 말인지 모를 것 같다. 대신 잘 설명해둔 url을 링크로 걸어둔다.

- [GeeksforGeeks - Stock Span Problem](https://www.geeksforgeeks.org/the-stock-span-problem/)

자신과 근접한 값들 중 특정조건에 부합하는 연속된 값이 얼마나 있는지 확인할 때는 Stack을 활용하면 좋다. Array를 순회하며 뒷쪽에서 index의 값을 꾸준히 Stack에 쌓고, 앞쪽에서는 Stack의 top에 값이 있을 경우 해당 값(이전 index)을 가져와서(peek) 현재 index값과 체크하려는 조건을 비교하여 true이면 Stack에서 해당값을 pop한다.

이렇게 하면 Stack의 top에 남은 값은, 더 작은 index값들 중 조건에 맞지 않는 가장 근접한 index값이 된다. 이 값을 현재 index의 값에서 빼주면 조건에 부합하는 근접한 index값의 숫자가 된다. 이 값을 Stock Span size를 저장할 임의의 Array에 저장하면 된다.


응용
-
그렇다면 이 Stock Span Problem을 푸는 방식을 어떻게 문제에 적용할 수 있을까? Stock Span Problem에서는 특정기간 중 최대값인 기간을 구하는거지만 이 문제에서는 일정 기간 중 최소값을 구해야 하기 때문에 최소값을 구하는 것으로 적용해야 한다. 여기서 해당 index가 최소값이었던 근접하고 연속적인 '최대 기간'이 해당 index의 값이 최소값이 될 수 있는 최대 Window Size가 되는 것이다.

하지만 대상범위는 현재 index의 왼쪽에 위치한 값들만이다. 그렇기 때문에 반대로 오른쪽부터 Window Size도 계산해야 한다. 그래서 특정 index의 최소값이 될 수 있는 최대 Window Wize = (왼쪽부터 계산한 Window Size) + (오른쪽부터 계산한 Window Size) + 자기자신 1 이 되는 것이다.

코드는 아래와 같다.

~~~java
import java.io.IOException;
import java.util.Collections;
import java.util.Iterator;
import java.util.Scanner;
import java.util.SortedMap;
import java.util.Stack;
import java.util.TreeMap;

public class Solution { 
  public static void main(String[] args) throws IOException { 
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    int[] arr = new int[n];
    
    for(int i=0; i<arr.length; i++){
       arr[i] = sc.nextInt();
    }
    
    int[] result = riddle(arr);
    
    for(int i=0; i<result.length; i++){
       if(i != 0){
         System.out.print(" ");
       }
       System.out.print(result[i]);
    }
    sc.close();
  }
  
  /* Array에서 Window Size 1부터 n까지 증가할 때, 각 Size값의 최소값들 중 최대값을 구한다. */
  static int[] riddle(int[] arr) {
    int[] lefts = leftWinSize(arr);
    int[] rights = rightWinSize(arr);

    SortedMap<Integer, Integer> numberToWinSize = new TreeMap<>(Collections.reverseOrder());
    
    // 각 값에 대한 최대 length값을 구한다.
    for(int i=0; i<arr.length; i++){
      // 동일한 값이 있을 수 있기 때문에 기존값이 있다면 업데이트함.
      numberToWinSize.put(
        arr[i],
        Math.max(numberToWinSize.getOrDefault(arr[i], -1),lefts[i] + rights[i] + 1));
    }
    
    Iterator<Integer> iter = numberToWinSize.keySet().iterator();
    int number = iter.next();
    int[] result = new int[arr.length];
    
    for(int i=0; i<result.length; i++){
    // 해당 index의 값이 가질 수 있는 Window Size가 가질 수 있는 가장 큰 값을 구한다.  
      while(numberToWinSize.get(number) <= i){
        number = iter.next();
      }
      result[i] = number;
    }
    return result;
  }
  
  /* Array의 각 값에서 해당값이 최소값이 되는 좌측부터 연속한 Window Size를 구한다. */
  static int[] leftWinSize(int[] arr){
    int[] lefts = new int[arr.length];
    Stack<Integer> indices = new Stack<>();
    
    for(int i=0; i<arr.length; i++){
      while(!indices.isEmpty() && arr[i] <= arr[indices.peek()]){
        indices.pop();
      }
      lefts[i] = i - (indices.isEmpty() ? 0 : indices.peek() + 1);
      indices.push(i);
    }
    
    return lefts;
  }
  
  /* Array의 각 값에서 해당값이 최소값이 되는 우측부터 연속한 Window Size를 구한다. */
  static int[] rightWinSize(int[] arr){
    int[] rights = new int[arr.length];
    Stack<Integer> indices = new Stack<>();
    
    for(int i=arr.length-1; i>=0; i--){
      while(!indices.isEmpty() && arr[i] <= arr[indices.peek()]){
        indices.pop();
      }
      
      rights[i] = (indices.isEmpty() ? arr.length : indices.peek()) - i - 1;
      indices.push(i);
    }
    return rights;
  }
}
~~~

- - -
* 참고
  - [GeeksforGeeks - Stock Span Problem](https://www.geeksforgeeks.org/the-stock-span-problem/)
  - [charles-wangkai/hackerrank](https://github.com/charles-wangkai/hackerrank/tree/master/min-max-riddle)