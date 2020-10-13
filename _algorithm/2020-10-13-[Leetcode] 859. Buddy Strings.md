---
layout: post
title: "[Leetcode] 859. Buddy Strings"
tags: ["leetcode"]
comments: true
---

두 문자열 `A`와 `B`가 주어지고 `A`의 문자 2개를 바꾸어 `B` 문자열을 만들 수 있는지를 반환해야 한다.

- `A`와 `B`의 길이가 다른 경우: `false`
- `A`와 `B`의 길이가 같은 경우:
  - `A`와 `B`가 동일한 경우 `A`의 의미없는 문자 교환을 위해 최소 1개의 문자는 2개 이상이어야 한다.
  - `A`와 `B`의 다른 문자가 1개 이거나 3개 이상인 경우는 성립할 수 없다.
  - `A`와 `B`의 다른 문자가 2개이며 두 문자를 교환하여 `A`와 `B`가 같은지 확인한다.

```java
class Solution {
  public boolean buddyStrings(String A, String B) {
    if (A.length() != B.length()) {
      return false;
    }

    int[] diff = new int[]{-1, -1};
    int[] k = new int[26];
    int maxK = 0;
    for (int i = 0; i < A.length(); i++) {
      char ch1 = A.charAt(i);
      k[ch1 - 'a']++;
      maxK = Math.max(maxK, k[ch1 - 'a']);

      char ch2 = B.charAt(i);
      if (ch1 != ch2) {
        if (diff[0] == -1) {
          diff[0] = i;
          continue;
        } else if (diff[1] == -1) {
          diff[1] = i;
          continue;
        }
        return false;// 3 or more different characters
      }
    }

    // A and B are same
    if (diff[0] == diff[1]) {
      return maxK >= 2;
    }

    // one different character
    if (diff[1] == -1) {
      return false;
    }

    return A.charAt(diff[0]) == B.charAt(diff[1]) && A.charAt(diff[1]) == B.charAt(diff[0]);
  }
}
```
