---
layout: post
title: "[Leetcode] 3. Longest Substring Without Repeating Characters"
tags: ["leetcode"]
comments: true
---

동적으로 `Substring`의 길이를 계산하기 위해 다음과 같이 문자열을 순차적으로 탐색한다.

- 시작 인덱스를 `0`으로 초기화하고 문자열을 순차적으로 탐색하면서 문자의 인덱스를 갱신한다.
- 임의의 문자가 시작 인덱스 이후에 등장했으면 중복을 의미하므로 시작 인덱스의 값을 갱신한다.
- `Substring`의 최대 길이를 매번 갱신한다.

```java
import java.util.Arrays;

class Solution {
  private final int[] indexMap = new int[128];

  public int lengthOfLongestSubstring(String s) {
    if (s.length() <= 1) {
      return s.length();
    }

    Arrays.fill(indexMap, -1);

    int maxLength = 1;
    int startIndex = 0;
    char[] chArr = s.toCharArray();
    for (int i = 0; i < chArr.length; i++) {
      char ch = chArr[i];
      if (indexMap[ch] != -1 && indexMap[ch] >= startIndex) {
        startIndex = indexMap[ch] + 1;
      }
      indexMap[ch] = i;
      maxLength = Math.max(maxLength, i - startIndex + 1);
    }

    return maxLength;
  }
}
```
