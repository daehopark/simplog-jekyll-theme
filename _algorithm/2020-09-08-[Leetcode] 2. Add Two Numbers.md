---
layout: post
title: "[Leetcode] 2. Add Two Numbers"
tags: ["leetcode"]
comments: true
---

음이 아닌 두 정수가 링크드 리스트로 주어진다. 두 정수의 합을 링크드 리스토로 구현한 다음 `Head` 노드를 반환하면 된다. 링크드 리스트의 가장 낮은 자리수는 `Head` 노드이기 때문에 두 링크드 리스트를 `Head` 노드부터 순차적으로 탐색하면서 더해간다.

주의할 점은 두 링크드 리스트의 길이가 다르기 때문에 어느 한 리스트의 노드가 `null`일 수 있다. 이때는 `0`으로 변환해서 더해주면 된다. 또한 각 자리수를 더할 때 발생하는 올림수를 항상 알고 있어야 하며 연산이 다 끝난 후에도 마지막 올림수가 있는지 체크해서 노드를 추가해줘야 한다.

```java
class ListNode {
  int val;
  ListNode next;

  ListNode() {
  }

  ListNode(int val) {
    this.val = val;
  }

  ListNode(int val, ListNode next) {
    this.val = val;
    this.next = next;
  }
}

class Solution {
  public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode headNode = null;
    ListNode tempNode = null;

    int carry = 0;
    while (l1 != null || l2 != null) {
      int x = l1 == null ? 0 : l1.val;
      int y = l2 == null ? 0 : l2.val;
      int sum = x + y + carry;
      carry = sum / 10;

      if (headNode == null) {
        headNode = tempNode = new ListNode(sum % 10);
      } else {
        tempNode.next = new ListNode(sum % 10);
        tempNode = tempNode.next;
      }

      l1 = l1 == null ? l1 : l1.next;
      l2 = l2 == null ? l2 : l2.next;
    }

    if (carry == 1) {
      tempNode.next = new ListNode(1);
    }

    return headNode;
  }
}
```
