---
title: "[HackerRank] Balanced Forest"
categories: 
  - Algorithm
tags:
  - Algorithm
  - HackerRank
  - Tree
  - Java
last_modified_at: 2021-04-01T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Balanced Forest](https://www.hackerrank.com/challenges/balanced-forest/problem)

주어진 Tree의 edge 2개를 잘라 3개의 Tree를 만든다. 이 때 만들어진 3개의 Tree는 같은 Total값을 가져야 한다. 
위의 조건을 만족하기 위해 노드 하나를 추가할 수 있는데 이 값을 최소값으로 하려고 한다. 최소값을 반환하고 불가능할 경우 -1을 반환하라.

생각해 보기
-
- 나눠진 Tree의 영역 합계값을 어떻게 구할까?
    - 주어진 데이터는 부모, 자식 관계가 없는 undirected graph이다. 부모/자식 관계를 가진 Tree로 재구성하여 부모가 자식 value의 sum값을 가지도록 구성한다.
      그럼 잘린 간선의 노드 밑으로 합계값을 알 수 있다.
- edge를 자르고 최소값을 구하는 로직을 어떻게 구현할까?
    - Tree를 순회하며 해당 노드위의 edge를 잘랐을 경우를 판단한다.
    - 자르고 나면 나머지 영역에서 2개의 Tree를 구할 수 있는지, 구한다면 최소값은 얼마나 나올지를 판단한다.
    - 어떤 케이스들이 있을지 생각해본다.
- 잘린 영역 이외의 영역에서 구하고자 하는 target sum값을 가진 노드의 무리가 있는지 어떻게 판단할까?
    - Tree에서 나를 제외한 영역의 값을 구해 판단한다.
    - target sum값이 잘린 영역안에 있지 않은지를 판단하기 위해 DFS 조회 시의 in-out 순번을 활용할 수 있다.

![Tree](https://user-images.githubusercontent.com/4060030/113262926-21bf2e00-930c-11eb-9938-9cc6d29d7ef6.png)
  
어떻게 풀까?
-
1. 주어진 데이터를 인접관계 정보를 표현한 Graph로 구성한다.
2. Graph에서 임의의 Root node를 정한 후, 부모/자식 관계를 가진 Tree로 구성한다.
3. DFS 순회를 통해 Tree를 구성하며, 이 때 해당 node의 enter-leave 순번을 기록한다. 이 정보를 cutting 시 유효성 확인을 위해 사용한다.  
4. Tree를 순회(DFS)하며 각 node가 가진 자신을 포함한 자식노드들의 sum값을 계산한다. (이 값이 해당 node 위의 edge를 잘랐을 때 나눠진 Tree가 가지는 sum값이 된다)
5. 이 때 각 node의 sum값에 대응하는 (앞에서 구한) enter-leave 순번을 저장한다. 이 값을 이용해 cutting 할 node를 판정 시, 선택 된 node로 자른 Tree 외의 영역에서 원하는 sum값이 존재하는지 판단할 수 있다. (Root로부터 선택된 node까지의 path에 해당되지 않는 영역)
6. 5번까지 구축된 정보를 활용해 Tree를 순회하며 각 node에서 cutting 시의 최소값을 판단하며, 가장 적은 최소값을 저장한 뒤 가장 마지막에 반환한다.

- - -
* 참고
    - [charles-wangkai](https://github.com/charles-wangkai/hackerrank/blob/master/balanced-forest)
