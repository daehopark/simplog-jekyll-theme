---
layout: post
title: "단일 연결 리스트(Singly Linked List)"
tags: ["Java", "Data Structure"]
comments: true
---

연결 리스트(`Linked List`)는 데이터가 선형적(`Linear`)으로 연결되어 있는 자료구조입니다. 배열과는 달리 메모리 상의 흩어진 데이터들이 참조의 형태로 서로 연결되어 있습니다.

오늘은 단일 연결 리스트를 구현해보겠습니다. 단일 연결 리스트는 리스트의 최신 노드를 참조하는 `HEAD` 노드를 기점으로 리스트의 노드들이 단방향으로 연결되어 있습니다. 때문에 임의의 노드에 접근하려면 항상 `HEAD` 노드부터 차례대로 탐색해야 합니다.

## Implementation

아래와 같이 `Node` 클래스를 정의합니다. 노드는 데이터를 가지고 있으며 다음 노드를 참조할 수 있는 `next` 필드를 가지고 있습니다.

```java
public class Node {
  private int data;
  private Node next;

  public Node(int data) {
    this.data = data;
    this.next = null;
  }

  public Node(int data, Node next) {
    this.data = data;
    this.next = next;
  }

  public int getData() {
    return data;
  }

  public void setData(int data) {
    this.data = data;
  }

  public Node getNext() {
    return next;
  }

  public void setNext(Node next) {
    this.next = next;
  }
}
```

아래와 같이 `LinkedList` 클래스의 필드와 메소드를 정의합니다.

```java
public class LinkedList {
  private Node head;
  private int size;

  public LinkedList();
  public boolean empty();
  public int size();
  public int head();
  public void pushFront(int data);
  public int popFront();
}
```

### Fields

| Name | Description                               |
| :--: | ----------------------------------------- |
| head | 리스트의 최신 노드를 참조하는 `HEAD` 노드 |
| size | 리스트의 사이즈                           |

### Constructor

`head`를 `null`로 `size`를 `0`으로 할당하여 리스트를 초기화합니다.

```java
public LinkedList() {
  this.head = null;
  this.size = 0;
}
```

### empty()

`size` 필드를 사용하여 리스트가 비어있는지 알 수 있습니다.

```java
public boolean empty() {
  return size == 0;// or head == null
}
```

### size()

`size` 필드를 사용하여 리스트의 사이즈를 알 수 있습니다.

```java
public int size() {
  return size;
}
```

### head()

`HEAD` 노드의 데이터를 반환합니다. 만약 비어있다면 예외 처리를 해줘야 합니다.

```java
public int head() {
  if (empty()) {
    throw new NoSuchElementException();
  }
  return head.getData();
}
```

### pushFront()

새로운 노드를 추가하고 `HEAD` 노드와 연결합니다. `HEAD` 노드는 항상 최신 노드를 바라봐야함으로 새롭게 추가된 노드로 재할당합니다.

```java
public void pushFront(int data) {
  head = new Node(data, head);
  size++;
}
```

### popFront()

`HEAD` 노드를 최신 직전의 노드로 갱신시켜 줍니다. 만약 비어있다면 예외 처리를 해줘야 합니다.

```java
public int popFront() {
  int ret = head();// if empty, throw exception
  head = head.getNext();
  size--;
  return ret;
}
```

## Time Complexity

`LinkedList` 클래스의 각 메서드의 시간 복잡도는 다음과 같습니다.

|   Method    | Time Complexity |
| :---------: | :-------------: |
|   empty()   |      O(1)       |
|   size()    |      O(1)       |
|   head()    |      O(1)       |
| pushFront() |      O(1)       |
| popFront()  |      O(1)       |

## Full Source Code

```java
public class LinkedList {
  private Node head;
  private int size;

  public LinkedList() {
    this.head = null;
    this.size = 0;
  }

  public boolean empty() {
    return size == 0;// or head == null
  }

  public int size() {
    return size;
  }

  public int head() {
    if (empty()) {
      throw new NoSuchElementException();
    }
    return head.getData();
  }

  public void pushFront(int data) {
    head = new Node(data, head);
    size++;
  }

  public int popFront() {
    int ret = head();// if empty, throw exception
    head = head.getNext();
    size--;
    return ret;
  }
}
```
