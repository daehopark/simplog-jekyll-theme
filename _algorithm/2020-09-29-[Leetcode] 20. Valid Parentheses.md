---
layout: post
title: "[Leetcode] 20. Valid Parentheses"
tags: ["leetcode"]
comments: true
---

주어진 문자열의 괄호가 유효한지를 반환하면 된다. 괄호 문제는 바로 스택을 떠올리면 된다. 여는 괄호는 스택에 넣어주고 닫는 괄호는 스택의 최상단 문자와 비교하여 짝이 맞는지 확인한다. 모든 과정이 끝나고 스택에 남은 괄호가 없는지 확인하는 작업만 주의하면 된다.

```java
import java.util.Stack;

class Solution {
  private final Stack<Character> stack = new Stack<>();

  public boolean isValid(String s) {
    if (s.length() % 2 != 0) {
      return false;
    }

    for (char ch : s.toCharArray()) {
      if (ch == '(' || ch == '{' || ch == '[') {
        stack.push(ch);
        continue;
      }
      if (stack.isEmpty() || !isValid(stack.pop(), ch)) {
        return false;
      }
    }// O(N)

    return stack.isEmpty();// check if open character remains
  }// O(N)

  private boolean isValid(char open, char close) {
    if (open == '(' && close == ')') return true;
    if (open == '{' && close == '}') return true;
    if (open == '[' && close == ']') return true;
    return false;
  }
}
```
