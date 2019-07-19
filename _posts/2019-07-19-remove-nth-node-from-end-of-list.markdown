---
title: "[LeetCode] Remove nth node from end of list"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Remove nth node from end of list
  - LeetCode
  - Javascript
last_modified_at: 2019-07-16T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - remove-nth-node-from-end-of-list](https://leetcode.com/problems/remove-nth-node-from-end-of-list)

어떻게 풀까?
-
뒤에서 n번째의 노드를 삭제하는 문제다.
예전에 코드인터뷰 책에서 비슷한 유형의 문제를 풀어봐서 어렵진 않았다.

포인터용 변수 2개를 선언하여 n개만큼 거리를 두어 이동하면
뒤의 포인터가 끝을 가리킬 때 나머지 노드는 지워야 할 노드의 바로 이전 노드가 된다.
(n번째라는 것은 마지막 노드에서 n-1번째 노드이기 때문)

만일 n만큼 이동한 뒤의 포인터 값이 null이 되었다면 삭제할 노드는 맨 앞 노드란 의미가 된다.


재귀적인 방법
-
~~~javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }`
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
let removeNthFromEnd = function(head, n) {
    let endNode = head;
    let nthBefNode = head;

    for(let i=0; i<n; i++){
        endNode = endNode.next;
    }

    if(endNode == null){
        head = head.next;
        return head;
    }

    while(endNode.next != null){
        endNode = endNode.next;
        nthBefNode = nthBefNode.next;
    }

    nthBefNode.next = nthBefNode.next.next;
    return head;
};
~~~