---
layout: post
title: "[Leetcode] 133. Clone Graph"
tags: ["leetcode"]
comments: true
---

그래프가 주어지면 해당 그래프의 깊은 복사를 반환해야 한다.

## Solution

모든 노드의 복제된 노드 배열을 선언한다. 그리고 기존 그래프를 `BFS` 탐색으로 돌면서 이어진 노드들을 모두 복제된 노드로 교체해준다.

```java
class Solution {
  public Node cloneGraph(Node node) {
    if (node == null) return null;
    if (node.neighbors.isEmpty()) return new Node(1);

    boolean[] visited = new boolean[1001];
    Node[] cloned = new Node[1001];

    Queue<Node> q = new LinkedList<>();
    q.add(node);
    visited[node.val] = true;

    while (!q.isEmpty()) {
      Node tempNode = q.poll();
      Node copied = getCloned(cloned, tempNode);
      for (Node e : tempNode.neighbors) {
        if (!visited[e.val]) {
          visited[e.val] = true;
          q.add(e);
        }
        copied.neighbors.add(getCloned(cloned, e));
      }
    }

    return getCloned(cloned, node);
  }

  private Node getCloned(Node[] cloned, Node node) {
    if (cloned[node.val] == null) {
      return cloned[node.val] = new Node(node.val);
    }
    return cloned[node.val];
  }
}
```
