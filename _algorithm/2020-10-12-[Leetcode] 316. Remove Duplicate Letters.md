---
layout: post
title: "[Leetcode] 316. Remove Duplicate Letters"
tags: ["leetcode"]
comments: true
---

문자열이 주어지고 중복되는 문자를 제거하되, 중복된 문자를 제거하고 만들어지는 문자열 중에서 사전 순으로 가장 빠른 문자열을 반환해야 한다. 예를 들어 `babc`가 있다면 중복되는 문자열 `b`를 제거하면 `abc`나 `bac`가 만들어질 수 있는데 이 중에서 사전 순으로 더 빠른 `abc`를 반환해야 한다.

가장 먼저 문자열에서 문자가 나타난 가장 마지막 인덱스를 저장한다.

```java
int[] lastIndex = new int[26];
for (int i = 0; i < s.length(); i++) {
  lastIndex[indexOfChar(s.charAt(i))] = i;
}
```

문자열을 순차적으로 탐색하면서 스택에 문자를 삽입하는데, 다음과 같은 과정을 거친다.

- 스택에 삽입된 문자를 저장하고 `특별한 경우`를 제외하고는 다시 삽입하지 않는다.
- 삽입될 문자가 스택의 최상단 문자보다 사전 순으로 뒤라면 그냥 삽입한다.
- 삽입될 문자가 스택의 최상단 문자보다 사전 순으로 앞이라면,
  - 스택의 최상단 문자가 삽입될 문자의 인덱스 뒤에 또 있다면 빼고 삽입한다. (반복)
  - 스택의 최상단 문자가 삽입될 문자의 인덱스 뒤에 없다면 그냥 삽입한다.

```java
boolean[] checked = new boolean[26];
Deque<Character> deque = new LinkedList<>();
for (int i = 0; i < s.length(); i++) {
  char ch = s.charAt(i);
  if (checked[indexOfChar(ch)]) {
    continue;
  }

  while (!deque.isEmpty()) {
    char compCh = deque.peekLast();
    if (compCh > ch && i < lastIndex[indexOfChar(compCh)]) {
      deque.removeLast();
      checked[indexOfChar(compCh)] = false;
      continue;
    }
    break;
  }

  deque.addLast(ch);
  checked[indexOfChar(ch)] = true;
}
```

위에서는 스택이라고 했지만 스택에 저장된 문자를 순차적으로 취할 수 있게 스택과 큐 기능이 모두 있는 `Deque`를 사용했다. 완성된 전체 코드는 다음과 같다.

```java
import java.util.Deque;
import java.util.LinkedList;

class Solution {
  public String removeDuplicateLetters(String s) {
    if (s == null || s.length() <= 1) {
      return s;
    }

    int[] lastIndex = new int[26];
    for (int i = 0; i < s.length(); i++) {
      lastIndex[indexOfChar(s.charAt(i))] = i;
    }

    boolean[] checked = new boolean[26];
    Deque<Character> deque = new LinkedList<>();
    for (int i = 0; i < s.length(); i++) {
      char ch = s.charAt(i);
      if (checked[indexOfChar(ch)]) {
        continue;
      }

      while (!deque.isEmpty()) {
        char compCh = deque.peekLast();
        if (compCh > ch && i < lastIndex[indexOfChar(compCh)]) {
          deque.removeLast();
          checked[indexOfChar(compCh)] = false;
          continue;
        }
        break;
      }

      deque.addLast(ch);
      checked[indexOfChar(ch)] = true;
    }

    StringBuilder sb = new StringBuilder();
    while (!deque.isEmpty()) {
      sb.append(deque.removeFirst());
    }

    return sb.toString();
  }

  private int indexOfChar(char ch) {
    return ch - 'a';
  }
}
```
