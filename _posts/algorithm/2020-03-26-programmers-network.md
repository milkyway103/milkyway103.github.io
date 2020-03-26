---
layout: post
title: 카펫 - 프로그래머스 레벨 2
categories : algorithm
tags : java programmers BP
comments : true
---

## 문제
네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

**제한사항**
- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 n-1인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
computer[i][i]는 항상 1입니다.

## 풀이

```java
class Solution {

    public int solution(int n, int[][] computers) {
        int num = 0;
        boolean[] connected = new boolean[n];
        for (int i=0; i<n; i++) {
            if (connected[i]) continue;
            // System.out.println(i);
            num++;
            connected[i] = true;
            search(i, connected, computers);
        }
        return num;
    }

    public void search(int thiscom, boolean[] connected, int[][] computers) {
        for ( int i=0; i<computers.length; i++) {
            if (connected[i] || computers[thiscom][i] == 0) continue;
            connected[i] = true;
            search(i, connected, computers);
        }
        return;
    }
}
```

## 풀이 과정
- 0번 컴퓨터에서부터 시작하여 연결되어 있다면 표시하고, 연결된 컴퓨터에 연결된 컴퓨터를 재귀적으로 확인하였다.
- 재귀를 모두 돌았는데도 표시되지 않은 컴퓨터가 있다면 다른 네트워크가 있다는 뜻이므로 `num++` 하고 재귀를 반복한다.
