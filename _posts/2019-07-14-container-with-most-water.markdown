---
title: "[LeetCode] Containter With Most Water"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Containter With Most Water
  - LeetCode
  - Java
last_modified_at: 2019-07-14T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Containter With Most Water](https://leetcode.com/problems/container-with-most-water)

문제풀이(Java)
-
문제 자체는 어렵지 않다. O(N^2)으로 풀면 평범하다. O(N)으로 풀면 괜찮다.

~~~java
public int maxArea(int[] height) {
    int maxWater=0, left=0, right=height.length-1;
    while(left<right) {
        maxWater = Math.max(maxWater,(right-left)*Math.min(height[left], height[right]));
        if(height[left]<height[right]) left++;
        else right--;
    }
    return maxWater;
}
~~~