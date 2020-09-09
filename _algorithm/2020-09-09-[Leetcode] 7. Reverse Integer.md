---
layout: post
title: "[Leetcode] 7. Reverse Integer"
tags: ["leetcode"]
comments: true
---

어떤 수의 역수를 구할 때는 낮은 자리수부터 자리수를 올리면서 순차적으로 더하고 10을 곱하면서 얻을 수 있다. 다만 `overflow`를 방지하기 위해서 역수의 데이터 타입을 `long`으로 설정하고 마지막에 `int` 형으로 명시적 타입 변경을 한 수와 비교함으로써 `overflow` 발생 여부를 확인할 수 있다.

```java
class Solution {
  public int reverse(int x) {
    long reversed = 0;
    while (x != 0) {
      reversed = reversed * 10 + x % 10;
      x /= 10;
    }

    // overflow
    if (reversed != (int) reversed) {
      return 0;
    }

    return (int) reversed;
  }
}
```
