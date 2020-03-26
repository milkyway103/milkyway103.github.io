---
layout: post
title: 타겟 넘버 - 프로그래머스 레벨 2
categories : algorithm
tags : java programmers DFS
comments : true
---

## 문제
n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다.

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

**제한사항**
- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

## 풀이
```java
class Solution {

    int cnt = 0;

    public int solution(int[] numbers, int target) {
        dfs(0, numbers, 0, target);
        return cnt;
    }

    public void dfs(int index, int[] numbers, int temp, int target) {
        if (index == numbers.length) {
            if (temp == target) cnt++;
            return;
        }
        dfs(index+1, numbers, temp+numbers[index], target);
        dfs(index+1, numbers, temp-numbers[index], target);
    }
}
```

## 풀이 과정
- 깊이 우선 탐색을 사용하였다.
- index를 하나씩 전진시켜나가면서 빼거나 더해 가며 다시 재귀적으로 `dfs` 함수를 호출한다.
- 만약 모든 배열을 다 돌았다면 return하되 임시 변수 `temp`가 `target`과 같다면 `cnt`를 증가시켜준다.
