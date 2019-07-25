---
title: "[LeetCode] Maximum Binary Tree"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Maximum Binary Tree
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-25T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree)


어떻게 풀까?
주어진 배열의 값을 트리로 작성하되 Root의 값은 가장 큰 값이 된다.
트리의 형태는 문제의 지문을 참고한다.

범위의 최대값을 찾아 노드를 생성하고 재귀를 통해 트리를 만들어 나간다.


문제풀이 (JavaScript)
-
~~~javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var constructMaximumBinaryTree = function(nums, low = 0, high = nums.length - 1) {
    if(low > high) return null;
    let i = low;
    for(let j=i+1; j<= high; j++){
        if(nums[j] > nums[i]) i = j;
    }
    const root = new TreeNode(nums[i]);
    root.left = constructMaximumBinaryTree(nums, low, i-1);
    root.right = constructMaximumBinaryTree(nums, i+1, high);
    return root;
};
~~~