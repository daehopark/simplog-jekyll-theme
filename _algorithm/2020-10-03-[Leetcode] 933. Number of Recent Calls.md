---
layout: post
title: "[Leetcode] 933. Number of Recent Calls"
tags: ["leetcode"]
comments: true
---

주어진 문제의 조건에 맞게 `RecentCounter` 클래스를 구현해야 한다. `t` 시간에 요청이 들어오면 `[t - 3000, t]` 시간 범위에 유효한 요청의 개수를 반환해야 하는데, 요청이 선입선출의 형태를 띄는 것에 착안하여 큐를 사용했다.

`ping` 메서드가 호출되면 큐에 파라미터로 들어온 시간을 삽입한다. 그리고 `[t - 3000, t]` 시간 범위에 유효하지 않은 모든 요청들을 삭제하고, 남은 요청들의 개수를 반환하면 된다.

```java
import java.util.LinkedList;
import java.util.Queue;

class RecentCounter {
  private final Queue<Integer> requests = new LinkedList<>();

  public RecentCounter() {
    // constructor
  }

  public int ping(int t) {
    requests.add(t);
    while (!requests.isEmpty() && requests.peek() < t - 3000) {
      requests.poll();
    }
    return requests.size();
  }
}
```
