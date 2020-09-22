---
layout: post
title: "[Leetcode] 5. Longest Palindromic Substring"
tags: ["leetcode"]
comments: true
---

주어진 문자열의 `Substring` 중에서 가장 긴 `Palindrome`을 찾는 문제이다. `Palindrome`은 크게 길이가 홀수인 것과 짝수인 것으로 나눌 수 있는데, 문자열을 순차적으로 탐색하면서 모든 경우의 수를 다 찾아보되 최적화를 통새 시간을 최대한 단축시켜 문제를 해결했다.

```java
class Solution {
  private String answer = "";

  public String longestPalindrome(String s) {
    int sLen = s.length();
    for (int i = 0; i < sLen; i++) {
      // 남은 문자열로 만들 수 있는 Palindrome의 최대 길이가 정답보다 작다면 중지
      int remainMaxLen = (sLen - i) * 2 + 1;
      if (remainMaxLen < answer.length()) {
        break;
      }
      bruteForce(s, i, i);
      bruteForce(s, i, i + 1);
    }
    return answer;
  }

  private void bruteForce(String s, int i, int j) {
    if (i < 0 || j >= s.length()) {
      return;
    }
    if (s.charAt(i) == s.charAt(j)) {
      // 정답보다 길 때만 문자열을 갱신
      int tmpLen = j - i + 1;
      if (tmpLen > answer.length()) {
        answer = s.substring(i, j + 1);
      }
      bruteForce(s, i - 1, j + 1);
    }
  }
}
```
