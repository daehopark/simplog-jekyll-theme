---
layout: post
title: "[Leetcode] 1009. Complement of Base 10 Integer"
tags: ["leetcode"]
comments: true
---

주어진 10진수를 2진수로 표현한 다음 반전시켜 나온 값을 10진수로 반환해야 한다. 예를 들어 5를 이진수로 표현하면 `101`인데 이를 반전시키면 `010`이 나오고 이는 10진수로 2가 되니 2를 반환하면 된다.

풀이는 간단한다. 임의의 수를 2진수로 표현했을 때 `1000101`과 같은 수가 나왔다면 자리수에 맞춰 `1111111`와 `XOR` 연산을 해주면 된다. 1은 같은 숫자끼리 연산이 되어 0이 되고 0은 다른 숫자와의 연산이 되어 1이 된다. 중요한건 1로 채워진 숫자를 찾는 것인데, 비트 마스크와 `&` 연산자를 사용하여 문제를 해결할 수 있다.

```java
class Solution {
  public int bitwiseComplement(int N) {
    int bitMast = 1;
    while ((N & bitMast) != N) {
      bitMast = (bitMast << 1) | 1;
    }
    return N ^ bitMast;
  }
}
```
