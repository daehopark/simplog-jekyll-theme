---
layout: post
title: "[Leetcode] 836. Rectangle Overlap"
tags: ["leetcode"]
comments: true
---

주어진 두 사각형이 겹치면 `true`를 반환하는 문제다. 반대로 두 사각형이 겹치지 않는 조건을 찾아 문제를 해결할 수 있다. 두 사각형이 절대 겹치지 않는 조건들은 다음과 같다.

- Rec1 x1 >= Rec2 x2
- Rec1 x2 <= Rec2 x1
- Rec1 y1 >= Rec2 y2
- Rec1 y2 <= Rec2 y1

위 조건들이 아닌 경우에 `true`를 반환하면 된다.

```java
class Solution {
  public boolean isRectangleOverlap(int[] rec1, int[] rec2) {
    if (rec1[0] >= rec2[2]) {
      return false;
    } else if (rec1[2] <= rec2[0]) {
      return false;
    } else if (rec1[1] >= rec2[3]) {
      return false;
    } else if (rec1[3] <= rec2[1]) {
      return false;
    }
    return true;
  }
}
```
