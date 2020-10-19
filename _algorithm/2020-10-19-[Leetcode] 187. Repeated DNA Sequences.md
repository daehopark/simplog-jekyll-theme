---
layout: post
title: "[Leetcode] 187. Repeated DNA Sequences"
tags: ["leetcode"]
comments: true
---

주어진 `DNA` 문자열에서 2번 이상 반복되는 10자리 시퀀스 문자열을 찾아 반환해야 한다.

## Solution 1

`DNA` 문자열에서 찾을 수 있는 10자리 시퀀스 문자열을 모두 찾아 `Map`에 담은 뒤, 2개 이상인 문자열을 추출한다.

```java
class Solution {
  public List<String> findRepeatedDnaSequences(String s) {
    Map<String, Integer> map = new HashMap<>();
    for (int i = 0; i <= s.length() - 10; i++) {
      String temp = s.substring(i, i + 10);
      map.put(temp, map.getOrDefault(temp, 0) + 1);
    }

    List<String> list = new ArrayList<>();
    map.forEach((k, v) -> {
      if (v >= 2) {
        list.add(k);
      }
    });

    return list;
  }
}
```

## Solution 2

`Set` 자료구조를 활용하여 2번 이상 나오는 문자열만 정답 리스트에 담는다.

```java
class Solution {
  public List<String> findRepeatedDnaSequences(String s) {
    List<String> list = new ArrayList<>();

    Set<String> set = new HashSet<>();
    for (int i = 0; i <= s.length() - 10; i++) {
      String temp = s.substring(i, i + 10);
      if (set.contains(temp)) {
        list.add(temp);
        continue;
      }
      set.add(temp);
    }

    return list.stream().distinct().collect(Collectors.toList());
  }
}
```

## Solution 3

2번 이상 나오는 문자열을 `List`에 담지 않고 `Set`에 담아 유일성을 확보하면 리스트를 바로 추출할 수 있다.

```java
class Solution {
  public List<String> findRepeatedDnaSequences(String s) {
    Set<String> repeated = new HashSet<>();

    Set<String> set = new HashSet<>();
    for (int i = 0; i <= s.length() - 10; i++) {
      String temp = s.substring(i, i + 10);
      if (set.contains(temp)) {
        repeated.add(temp);
        continue;
      }
      set.add(temp);
    }

    return new ArrayList<>(repeated);
  }
}
```
