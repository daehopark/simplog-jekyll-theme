---
layout: post
title: "[Leetcode] 148. Sort List"
tags: ["leetcode"]
comments: true
---

주어진 링크드 리스트를 오름차순으로 정렬하되 시간 복잡도는 `O(NlogN)`을 준수하고 공간 복잡도는 `O(1)`을 준수해야 한다.

우선순위 큐를 사용하여 모든 노드를 큐에 넣은 다음, 하나씩 뽑아 순차적으로 연결시켜주면 되겠다고 생각했다.

```java
class Solution {
  public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
      return head;
    }

    PriorityQueue<ListNode> pq = new PriorityQueue<>(Comparator.comparingInt(o -> o.val));
    while (head != null) {
      pq.add(head);
      head = head.next;
    }// O(NlogN)

    ListNode headNode = pq.poll();
    ListNode tempNode = headNode;
    while (!pq.isEmpty()) {
      tempNode.next = pq.poll();
      tempNode = tempNode.next;
    }// O(NlogN)

    tempNode.next = null;// prevent cycle

    return headNode;
  }
}
```
