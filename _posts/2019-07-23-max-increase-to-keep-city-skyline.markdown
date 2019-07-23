---
title: "[LeetCode] Max Increase to Keep City Skyline"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Max Increase to Keep City Skyline
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-23T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Max Increase to Keep City Skyline](https://leetcode.com/problems/max-increase-to-keep-city-skyline)


어떻게 풀까?
-
Grid를 건물의 높이라고 할 때 위아래, 양옆에서 보는 스카이라인이 변하지 않도록 하면서 건물의 높이를 최대로 올릴 경우에 추가분의 총합을 구하는 문제다. map()으로 row별로 최대값을 뽑아 배열을 구성한다. 

- arrow function expression의 경우 함수 몸체가 한줄의 구문이면 암묵적으로 return한다.
- map()의 return은 배열이다.
- ...는 전개구문이다.


문제풀이(JavaScript)
-
~~~javascript
let transpose = g => g[0].map((v,i) => g.map(row => row[i]));

let maxIncreaseKeepingSkyline = function(grid){
    let sideview = grid.map(row => Math.max(...row));
    let topview = transpose(grid).map(row => Math.max(...row));
    let sum = 0;
    for(let i=0; i<grid.length; i++){
        for(let j=0; j<grid[0].length; j++){
            sum += Math.min(topview[i], sideview[j]) - grid[i][j];
        }
    }
    return sum;
}
~~~