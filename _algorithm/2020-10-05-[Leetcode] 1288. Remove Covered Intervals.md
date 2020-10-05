---
layout: post
title: "[Leetcode] 1288. Remove Covered Intervals"
tags: ["leetcode"]
comments: true
---

인터벌 배열이 주어지고 임의의 인터벌이 다른 인터벌의 범위를 커버할 수 있다면 합쳐 최종적으로 남는 인터벌의 개수를 반환해야 한다.

문제를 해결하기 위해 인터벌 배열을 정렬한다. 인터벌의 범위가 `[a, b]`라고 하면 `a`를 기준으로 오름차순 `b`를 기준으로 내림차순으로 정렬한다. 그 이유는 더 많은 범위를 커버할 수 있는 인터벌을 기준으로 다른 인터벌을 커버하면서 반복문을 처리하기 때문이다.

```java
import java.util.Arrays;

class Solution {
  public int removeCoveredIntervals(int[][] intervals) {
    Arrays.sort(intervals, (o1, o2) - > o1[0] == o2[0] ? o2[1] - o1[1] : o1[0] - o2[0]); // O(NlogN)

    int count = 0;
    for (int i = 0; i < intervals.length; i++) {
      int[] interval = intervals[i];
      for (int j = i + 1; j < intervals.length; j++) {
        int[] tempInterval = intervals[j];
        if (tempInterval[0] >= interval[0] && tempInterval[1] <= interval[1]) {
          i++;
          continue;
        }
        break;
      }
      count++;
    } // O(N)

    return count;
  }
}
```
