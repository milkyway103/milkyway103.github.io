---
layout: post
title: 카펫 - 프로그래머스 레벨 2
categories : algorithm
tags : java programmers BP
comments : true
---

## 문제

Leo는 카펫을 사러 갔다가 중앙에는 빨간색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다.

Leo가 본 카펫에서 갈색 격자의 수 brown, 빨간색 격자의 수 red가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.

**제한사항**
- 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
- 빨간색 격자의 수 red는 1 이상 2,000,000 이하인 자연수입니다.
- 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

## 풀이

```java
import java.util.ArrayList;

class Solution {
    public int[] solution(int brown, int red) {
        int[] ret = new int[2];

        int sum = (brown + 4) / 2;
        // System.out.printf("sum: %d\n", sum);
        int row = -1, col = -1;
        ArrayList<Integer> candidates = new ArrayList<>();

        for ( int i=3 ; i<=sum/2 ; i++ ) {
            candidates.add(i);
        }

        for ( int candi : candidates ) {
            // System.out.printf("%d, %d\n", candi, sum-candi);
            if ( (candi-2) * (sum-candi-2) == red ) {
                row = sum - candi;
                col = candi;
                break;
            }
        }

        ret[0] = row;
        ret[1] = col;

        return ret;
    }
}
```

## 풀이 과정
- red와 brown 중 범위가 더 적은 brown을 기준으로 완전탐색을 진행하기로 했다.
- `(brown+4)/2` 는 `row`와 `col`를 더한 값이 된다.
- 1과 2는 가로, 세로의 값이 될 수 없으므로 (red가 최소 1) 3부터 시작하여 후보군을 만든다.
- 후보군을 대상으로 완전 탐색을 진행한다.

## 다른 사람의 풀이를 보고 느낀 점 & 아쉬웠던 점
1. 수학식으로 푸셨던 분이 계셨는데, 분명히 좋은 풀이라고 생각하지만 시험장에서 단기간에 생각해내기엔 어렵다고 느껴졌다.
2. 나와 비슷한 풀이방식이지만 모두 후보군에 저장하지 않고 하나의 for문에서 후보 뽑기와 정답인지 확인을 동시에 진행하신 분이 계셨다. 좋은 방법이라고 생각한다. 메모리와 시간효율 두 가지 면 모두에서 좋은 것 같다.
