---
layout: post
title: "2개의 스택(Stack)으로 큐(Queue) 구현"
tags: ["java", "Data Structure", "Algorithm"]
comments: true
---

오늘은 2개의 스택을 사용하여 어떻게 큐를 구현할 수 있는지 알아보려고 합니다. 예전에 면접에서 이 문제를 받고 당황해서 풀지 못한 기억이 있는데, 같은 실수를 반복하지 않기 위해 글을 작성하게 되었습니다.

2개의 스택을 가지고 큐를 구현할 때 가장 중요한 것은 각 스택의 역할입니다. 하나는 오로지 데이터를 입력하는 용도로만 사용해야 하고, 나머지 하나는 데이터를 출력하는 용도로 사용해야 합니다. 편의상 전자를 `stack1`, 후자를 `stack2`라 부르겠습니다.

## push

`push` 메서드의 구현은 간단합니다. `stack1`에 데이터를 입력하면 됩니다.

```java
public void push(int e) {
  stack1.push(e);
}
```

## pop

`pop` 메서드는 기본적으로 `stack2`에서 데이터를 꺼내쓰면 됩니다. 하지만 `stack2`가 비어있다면 어떻게 해야 할까요? `stack1`의 데이터를 모조리 `stack2`로 옮겨줍니다. 그러면 데이터가 입력했던 순서와 반대인 역순으로 `stack2`에 쌓이게 되며, 이는 큐의 선입선출(`FIFO`)의 구조를 가지게 됩니다.

```java
public int pop() {
  if (!stack2.isEmpty()) {
    return stack2.pop();
  }

  while (!stack1.isEmpty()) {
    stack2.push(stack1.pop());
  }

  return stack2.pop();
}
```

`pop` 메서드에서 중요한 것은 `stack2`가 비어있지 않는 동안은 `stack1`에서 `stack2`로 데이터를 이동시키면 안된다는 것입니다. 이는 큐의 선입선출 구조를 망가뜨리게 됩니다.

## Full Source Code

```java
public class CustomQueue {
  private final Stack<Integer> stack1 = new Stack<>();
  private final Stack<Integer> stack2 = new Stack<>();

  public void push(int e) {
    stack1.push(e);
  }

  public int pop() {
    if (!stack2.isEmpty()) {
      return stack2.pop();
    }

    while (!stack1.isEmpty()) {
      stack2.push(stack1.pop());
    }

    return stack2.pop();
  }
}
```

## References

- [[알고리즘] 스택2개로 큐 구현하기](https://limkydev.tistory.com/185)
