---
layout: post
title: "이중 연결 리스트(Doubly Linked List)"
tags: ["Java", "Data Structure"]
comments: true
---

연결 리스트(`Linked List`)는 데이터가 선형적(`Linear`)으로 연결되어 있는 자료구조입니다. 배열과는 달리 메모리 상의 흩어진 데이터들이 참조의 형태로 서로 연결되어 있습니다.

오늘은 이중 연결 리스트를 구현해보겠습니다. 이중 연결 리스트는 양끝 노드를 참조하는 `HEAD` 노드와 `TAIL` 노드를 기점으로 리스트의 노드들이 양방향으로 연결되어 있습니다. 때문에 임의의 노드를 탐색하려면 `HEAD` 노드 또는 `TAIL` 노드부터 차례대로 탐색해야 합니다.

## Implementation

아래와 같이 `Node` 클래스를 정의합니다. 노드는 데이터를 가지고 있으며 다음 노드를 참조할 수 있는 `next` 필드와 이전 노드를 참조할 수 있는 `prev` 필드를 가지고 있습니다.

```java
public class Node {
  private int data;
  private Node prev;
  private Node next;

  public Node(int data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }

  public Node(int data, Node prev, Node next) {
    this.data = data;
    this.prev = prev;
    this.next = next;
  }

  public int getData() {
    return data;
  }

  public void setData(int data) {
    this.data = data;
  }

  public Node getPrev() {
    return prev;
  }

  public void setPrev(Node prev) {
    this.prev = prev;
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
  private Node tail;
  private int size;

  public LinkedList();
  public boolean empty();
  public int size();
  public int head();
  public int tail();
  public void pushFront(int data);
  public void pushBack(int data);
  public int popFront();
  public int popBack();
}
```

### Fields

| Name | Description                               |
| :--: | ----------------------------------------- |
| head | 리스트의 한쪽 끝을 참조하는 `HEAD` 노드   |
| tail | 리스트의 반대쪽 끝을 참조하는 `TAIL` 노드 |
| size | 리스트의 사이즈                           |

### Constructor

`head`와 `tail`을 `null`로 `size`를 `0`으로 할당하여 리스트를 초기화합니다.

```java
public LinkedList() {
  this.head = null;
  this.tail = null;
  this.size = 0;
}
```

### empty()

`size` 필드를 사용하여 리스트가 비어있는지 알 수 있습니다.

```java
public boolean empty() {
  return size == 0;// or head == null or tail == null
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

### tail()

`TAIL` 노드의 데이터를 반환합니다. 만약 비어있다면 예외 처리를 해줘야 합니다.

```java
public int tail() {
  if (empty()) {
    throw new NoSuchElementException();
  }
  return tail.getData();
}
```

### pushFront()

리스트가 비어있다면 `HEAD` 노드와 `TAIL` 노드에 새로운 노드를 추가합니다. 그렇지 않다면 `HEAD` 노드 앞에 새로운 노드를 추가하고 `HEAD` 노드를 새롭게 추가된 노드로 재할당합니다.

```java
public void pushFront(int data) {
  if (empty()) {
    head = tail = new Node(data);
  } else {
    Node node = new Node(data, null, head);
    head.setPrev(node);
    head = node;
  }
  size++;
}
```

### pushBack()

리스트가 비어있다면 `HEAD` 노드와 `TAIL` 노드에 새로운 노드를 추가합니다. 그렇지 않다면 `TAIL` 노드 뒤에 새로운 노드를 추가하고 `TAIL` 노드를 새롭게 추가된 노드로 재할당합니다.

```java
public void pushBack(int data) {
  if (empty()) {
    head = tail = new Node(data);
  } else {
    Node node = new Node(data, tail, null);
    tail.setNext(node);
    tail = node;
  }
  size++;
}
```

### popFront()

리스트에 노드가 하나라면, 즉 `HEAD` 노드 와 `TAIL` 노드가 같다면 둘 다 `null`을 바라보게 합니다. 리스트에 노드가 2개 이상이라면 `HEAD` 노드를 그 다음 노드로 재할당하여 리스트에 노드를 하나 삭제합니다. 만약 리스트가 비어있다면 예외 처리를 해줘야 합니다.

```java
public int popFront() {
  int ret = head();// if empty, throw exception
  if (head == tail) {
    head = tail = null;
  } else {
    head = head.getNext();
  }
  size--;
  return ret;
}
```

### popBack()

리스트에 노드가 하나라면, 즉 `HEAD` 노드 와 `TAIL` 노드가 같다면 둘 다 `null`을 바라보게 합니다. 리스트에 노드가 2개 이상이라면 `TAIL` 노드를 그 이전 노드로 재할당하여 리스트에 노드를 하나 삭제합니다. 만약 리스트가 비어있다면 예외 처리를 해줘야 합니다.

```java
public int popBack() {
  int ret = tail();// if empty, throw exception
  if (head == tail) {
    head = tail = null;
  } else {
    tail = tail.getPrev();
  }
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
|   tail()    |      O(1)       |
| pushFront() |      O(1)       |
| pushBack()  |      O(1)       |
| popFront()  |      O(1)       |
|  popBack()  |      O(1)       |

## Full Source Code

```java
public class LinkedList {
  private Node head;
  private Node tail;
  private int size;

  public LinkedList() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  public boolean empty() {
    return head == null || tail == null;
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

  public int tail() {
    if (empty()) {
      throw new NoSuchElementException();
    }
    return tail.getData();
  }

  public void pushFront(int data) {
    if (empty()) {
      head = tail = new Node(data);
    } else {
      Node node = new Node(data, null, head);
      head.setPrev(node);
      head = node;
    }
    size++;
  }

  public void pushBack(int data) {
    if (empty()) {
      head = tail = new Node(data);
    } else {
      Node node = new Node(data, tail, null);
      tail.setNext(node);
      tail = node;
    }
    size++;
  }

  public int popFront() {
    int ret = head();// if empty, throw exception
    if (head == tail) {
      head = tail = null;
    } else {
      head = head.getNext();
    }
    size--;
    return ret;
  }

  public int popBack() {
    int ret = tail();// if empty, throw exception
    if (head == tail) {
      head = tail = null;
    } else {
      tail = tail.getPrev();
    }
    size--;
    return ret;
  }
}
```
