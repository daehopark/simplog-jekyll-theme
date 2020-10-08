---
layout: post
title: "[Leetcode] 61. Rotate List"
tags: ["leetcode"]
comments: true
---

링크드 리스트와 `k`가 주어지고 링크드 리스트를 `k`만큼 회전한 결과 리스트를 반환해야 한다.

`k` 값이 링크드 리스트의 사이즈보다 큰 경우 무의미한 연산을 최소화하기 위해 링크드 리스트의 사이즈가 필요하다. 때문에 리스트를 새로 선언하고 링크드 리스트의 모든 값을 다 넣어주었다.

```java
List<Integer> list = new ArrayList<>();
while (head != null) {
    list.add(head.val);
    head = head.next;
}

k %= list.size();
```

다음 `k` 값을 이용하여 새로운 `Head` 노드의 인덱스를 찾은 뒤, 리스트를 순환하면서 새로운 링크드 리스트를 구축해 나간다.

```java
ListNode headNode = null;
ListNode tempNode = null;
for (int i = 0; i < list.size(); i++) {
    int index = (list.size() - k + i) % list.size();
    if (headNode == null) {
        headNode = tempNode = new ListNode(list.get(index));
        continue;
    }
    tempNode.next = new ListNode(list.get(index));
    tempNode = tempNode.next;
}
```

완성된 코드는 다음과 같다.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }

        List<Integer> list = new ArrayList<>();
        while (head != null) {
            list.add(head.val);
            head = head.next;
        }

        k %= list.size();

        ListNode headNode = null;
        ListNode tempNode = null;
        for (int i = 0; i < list.size(); i++) {
            int index = (list.size() - k + i) % list.size();
            if (headNode == null) {
                headNode = tempNode = new ListNode(list.get(index));
                continue;
            }
            tempNode.next = new ListNode(list.get(index));
            tempNode = tempNode.next;
        }

        return headNode;
    }
}
```

문제를 풀다보니 메모리를 너무 낭비하고 있다는 생각이 들었다. 새로운 `Head` 노드의 인덱스를 알 수 있다면 기존의 링크드 리스트를 활용할 수 있지 않을까?

아이디어는 이렇다. 기존의 링크드 리스트의 처음과 끝을 이어 순환 링크드 리스트로 만든 다음, 새로운 링크드 리스트의 `Head`와 `Tail` 노드를 찾아 둘 사이를 끊어준다.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }

        List<ListNode> list = new ArrayList<>();
        while (true) {
            list.add(head);
            if (head.next == null) {
                head.next = list.get(0);
                break;
            }
            head = head.next;
        }

        k %= list.size();
        int headIndex = (list.size() - k) % list.size();
        int tailIndex = headIndex != 0 ? headIndex - 1 : list.size() - 1;
        list.get(tailIndex).next = null;
        return list.get(headIndex);
    }
}
```
