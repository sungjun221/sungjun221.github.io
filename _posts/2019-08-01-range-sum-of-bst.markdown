---
title: "[LeetCode] Range Sum of BST"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Range Sum of BST
  - LeetCode
  - JavaScript
last_modified_at: 2019-08-01T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst)


어떻게 풀까?
-
Binaray Search Tree를 순회하여 L, R 범위 안에 있는 값들의 총합을 반환한다.

언제 왼쪽으로, 언제 오른쪽으로 가야할까? 그리고 범위 안에 속해 있을 때는 어떻게 움직여야 할까를 고민한다.

재귀로 푼다.
* L보다 작을 땐 오른쪽으로
* R보다 클 땐 왼쪽으로
* L과 R 사이에 있을 때는 왼쪽, 오른쪽 모두 순회하고 그 결과값에 현재 노드값을 더한다. (중요. 별도로 총합값을 선언하여 조건절로 더하는 것이 아니라 return 하는 부분에 순회하는 부분과 같이 녹일 수 있다.)

다른 방식의 풀이도 있다. 모든 노드를 순회하여 Array에 넣은 뒤 범위 안의 값이 있는지 검사하여 값을 더할 수도 있다. 효율은 좋지 않다.

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
 * @param {TreeNode} root
 * @param {number} L
 * @param {number} R
 * @return {number}
 */
var rangeSumBST = function(root, L, R) {
	if(root == null) return 0;

	if(root.val < L) return rangeSumBST(root.right, L, R);
	else if(root.val > R) return rangeSumBST(root.left, L, R);
	else return root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R);
};
~~~