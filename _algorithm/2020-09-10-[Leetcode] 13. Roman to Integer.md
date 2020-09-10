---
layout: post
title: "[Leetcode] 13. Roman to Integer"
tags: ["leetcode"]
comments: true
---

문제에 주어진 조건대로 로마어를 십진수로 변환해주면 된다.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
  private final Map<Character, Integer> map = new HashMap<>() {{
    put('I', 1);
    put('V', 5);
    put('X', 10);
    put('L', 50);
    put('C', 100);
    put('D', 500);
    put('M', 1000);
  }};

  public int romanToInt(String s) {
    int answer = 0;
    for (int i = 0; i < s.length(); i++) {
      char temp = s.charAt(i);
      if (i + 1 < s.length()) {
        if (temp == 'I' && s.charAt(i + 1) == 'V') {
          answer += 4;
          i++;
          continue;
        } else if (temp == 'I' && s.charAt(i + 1) == 'X') {
          answer += 9;
          i++;
          continue;
        } else if (temp == 'X' && s.charAt(i + 1) == 'L') {
          answer += 40;
          i++;
          continue;
        } else if (temp == 'X' && s.charAt(i + 1) == 'C') {
          answer += 90;
          i++;
          continue;
        } else if (temp == 'C' && s.charAt(i + 1) == 'D') {
          answer += 400;
          i++;
          continue;
        } else if (temp == 'C' && s.charAt(i + 1) == 'M') {
          answer += 900;
          i++;
          continue;
        }
      }
      answer += map.get(temp);
    }
    return answer;
  }
}
```
