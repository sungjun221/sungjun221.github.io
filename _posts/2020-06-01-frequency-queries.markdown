---
title: "[HackerRank] Frequency Queries"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Frequency Queries
  - HackerRank
  - Dictionaries and Hashmaps
  - Java
last_modified_at: 2020-06-01T12:04:24-04:00
toc: true
---
문제정보
-
- [HackerRank - Frequency Queries](https://www.hackerrank.com/challenges/frequency-queries/problem)

어떻게 풀까?
-
배열 2개 묶음으로 쿼리값들이 주어진다. 1번째는 쿼리의 타입으로 1-입력, 2-삭제, 3-체크이며, 2번째는 입력숫자값이다. 숫자값의 빈도수를 계산하고 3번쿼리가 들어왔을 때 빈도수가 동일한 값이 있는 경우 1, 아닌 경우 0을 출력한다.

이 문제는 Hashmap으로 숫자에 대한 빈도수를 저장하면 되지만 이것만으로는 부족하다. Timed out이 뜨기 때문이다. 반대로 빈도수에 대한 카운트를 배열로 저장하면 결과값을 출력할 때 별도의 연산없이 값을 출력할 수 있다.

- 주의사항
    - 이 문제는 Boilerplate code도 손을 대야 한다. 그렇지 않으면 최적화가 되지 않아 Timed out이 뜬다.

    - 빈도수에 대한 카운트를 할 때 queries.size()만큼 배열값을 확보한다. (+1은 값을 인덱스1부터 넣기 위해) queries의 값보다 더 빈도수가 많아질 순 없기 때문이다.

    - 결과값을 List에 담을 때 입력 숫자값이 최대 빈도수보다 더 크게 들어올 수도 있기 때문에 이에 대한 예외처리를 해야한다.


문제풀이(Java)
-
~~~java
import java.io.*;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import static java.util.stream.Collectors.joining;

public class Solution {

    static List<Integer> freqQuery(List<int[]> queries) {
        Map<Integer, Integer> frqs = new HashMap<>();
        int[] qts = new int[queries.size() + 1];
        List<Integer> rslt = new ArrayList<>();
        int frq, cmd, v;

        for (int[] query : queries) {
            cmd = query[0];
            v = query[1];
            if(cmd == 1){
                frq = frqs.getOrDefault(v, 0);
                frqs.put(v, frq+1);
                qts[frq]--;
                qts[frq+1]++;
            }else if(cmd == 2){
                frq = frqs.getOrDefault(v, 0);
                if(frq > 0){
                    frqs.put(v, frq-1);
                    qts[frq]--;
                    qts[frq-1]++;
                }
            }else if(cmd == 3){
                if(v > qts.length) rslt.add(0);
                else rslt.add(qts[v] > 0 ? 1 : 0);
            }
        }
        return rslt;
    }

    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = Integer.parseInt(bufferedReader.readLine().trim());
        List<int[]> queries = new ArrayList<>();

        for(int i=0; i<q; i++){
            String[] row = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");
            int cmd = Integer.parseInt(row[0]);
            int v = Integer.parseInt(row[1]);
            int[] query = new int[2];
            query[0] = cmd;
            query[1] = v;
            queries.add(query);
        }

        List<Integer> ans = freqQuery(queries);

        bufferedWriter.write(
                ans.stream()
                        .map(Object::toString)
                        .collect(joining("\n"))
                        + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
~~~
