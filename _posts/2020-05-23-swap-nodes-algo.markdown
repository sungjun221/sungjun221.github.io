---
title: "[HackerRank] Swap Nodes [Algo]"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Swap Nodes
  - Swap Nodes Algo
  - HackerRank
  - Search
  - Java
last_modified_at: 2020-05-23T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Swap Nodes [Algo]](https://www.hackerrank.com/challenges/swap-nodes-algo/problem)

어떻게 풀까?
-
주어진 배열로 트리를 만들고, 쿼리에 주어진 k 배수에 해당하는 노드에서 Swap을 한 뒤 중위순회로 표기하는 문제이다.

배열을 트리로 만들 땐 다음 대상 노드를 큐에 넣어가며 Depth별로 노드를 연결할 수 있다.

문제풀이(Java)
-
~~~java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.*;

class Node{
    int d;
    Node l;
    Node r;

    Node(int d, Node l, Node r){
        this.d = d;
        this.l = l;
        this.r = r;
    }
}

public class Solution {
    static void inorder(Node cur, List<Integer> list){
        if(cur == null) return;

        inorder(cur.l, list);
        list.add(cur.d);
        inorder(cur.r, list);
    }

    static void swap(Node cur, int depth, int k){
        if(cur == null) return;

        if (depth >=k && depth % k == 0 ) {
            Node t = cur.l;
            cur.l = cur.r;
            cur.r = t;
        }

        swap(cur.l, depth+1, k);
        swap(cur.r, depth+1, k);
    }

    static int[][] swapNodes(int[][] indexes, int[] queries) {
        int[][] result = new int[queries.length][indexes.length];
        Node root = new Node(1, null, null);
        Node cur = root;

        Queue<Node> nodes = new LinkedList<Node>();
        nodes.offer(cur);

        int N = 0;
        while(N < indexes.length){
            cur = nodes.poll();

            cur.l = (indexes[N][0] == -1) ? null : new Node(indexes[N][0], null, null);
            cur.r = (indexes[N][1] == -1) ? null : new Node(indexes[N][1], null, null);

            if(cur.l != null && cur.l.d != -1) nodes.offer(cur.l);
            if(cur.r != null && cur.r.d != -1) nodes.offer(cur.r);
            N++;
        }

        for(int i=0; i<queries.length; i++){
            swap(root, 1, queries[i]);
            List<Integer> list = new ArrayList<>();
            inorder(root, list);
            result[i] = list.stream().mapToInt(v -> v).toArray();
        }

        return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(scanner.nextLine().trim());

        int[][] indexes = new int[n][2];

        for (int indexesRowItr = 0; indexesRowItr < n; indexesRowItr++) {
            String[] indexesRowItems = scanner.nextLine().split(" ");

            for (int indexesColumnItr = 0; indexesColumnItr < 2; indexesColumnItr++) {
                int indexesItem = Integer.parseInt(indexesRowItems[indexesColumnItr].trim());
                indexes[indexesRowItr][indexesColumnItr] = indexesItem;
            }
        }

        int queriesCount = Integer.parseInt(scanner.nextLine().trim());

        int[] queries = new int[queriesCount];

        for (int queriesItr = 0; queriesItr < queriesCount; queriesItr++) {
            int queriesItem = Integer.parseInt(scanner.nextLine().trim());
            queries[queriesItr] = queriesItem;
        }

        int[][] result = swapNodes(indexes, queries);

        for (int resultRowItr = 0; resultRowItr < result.length; resultRowItr++) {
            for (int resultColumnItr = 0; resultColumnItr < result[resultRowItr].length; resultColumnItr++) {
                bufferedWriter.write(String.valueOf(result[resultRowItr][resultColumnItr]));

                if (resultColumnItr != result[resultRowItr].length - 1) {
                    bufferedWriter.write(" ");
                }
            }

            if (resultRowItr != result.length - 1) {
                bufferedWriter.write("\n");
            }
        }

        bufferedWriter.newLine();

        bufferedWriter.close();
    }
}
~~~
