---
layout: post
title: "[Leetcode] 452. Minimum Number of Arrows to Burst Balloons"
tags: ["leetcode"]
comments: true
---

2차원 공간에 펼쳐진 풍선들이 배열로 주어지고 모든 풍선들을 타 터뜨릴 수 있는 화살의 최소 개수를 구해야 한다.

임의의 풍선을 터뜨릴 수 있는 좌표를 `(x1, x2)`라고 하면 풍선들을 `x2`를 기준으로 오름차순 정렬한다. 이제부터 그룹을 지어줘야 하는데, 임의의 풍선이 기준이 된다면 그 다음에 오는 풍선들의 `x1`이 기준 풍선의 `x2`의 값보다 같거나 작다면 한 그룹으로 묶일 수 있다. 왜냐하면 같은 그룹의 풍선 모두 `x2` 좌표를 향해 화살을 쏘면 다 터지기 때문이다.

위의 프로세스를 반복해주면 여러 개의 그룹이 나오고 그룹의 개수가 곧 화살의 개수가 된다.

```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
  public int findMinArrowShots(int[][] points) {
    Arrays.sort(points, (Comparator.comparingInt(o -> o[1])));

    int shots = 0;
    for (int i = 0; i < points.length; i++) {
      int[] point = points[i];
      for (int j = i + 1; j < points.length; j++) {
        int[] compPoint = points[j];
        if (compPoint[0] > point[1]) {
          break;
        }
        i++;
      }
      shots++;
    }

    return shots;
  }
}
```
