---
layout: post
title: "2개의 스택(Stack)으로 큐(Queue) 구현"
tags: ["java"]
comments: true
---

예전에 면접에서 2개의 스택을 가지고 큐를 구현해야하는 문제가 나왔었는데 풀이를 제대로 못한 기억이 있습니다. 그래서 오늘은 2번의 실수를 하지 않기 위해 2개의 스택으로 큐를 구현해보려 합니다.

2개의 스택을 가지고 큐를 구현할 때 가장 중요한 것은 각 스택의 역할입니다. 하나는 데이터 축적을 위한 스택의 용도로 사용해야 하며 다른 하나는 꺼내쓰기 위한 용도로 큐를 사용해야 합니다.

## push

`push` 메서드의 구현은 간단합니다. `stack1`에 계속 데이터를 쌓아주면 됩니다.

```java
public void push(int e) {
  stack1.push(e);
}
```

## pop

`pop` 메서드는 `stack2`의 상태에 따라 달라집니다. `stack2`가 비어있다면 `stack1`의 데이터를 `stack2`로 옮겨줍니다. 그러면 `stack2`는 역순으로 데이터가 축적되어 자연스럽게 큐와 같은 선입선출의 구조를 가지게 됩니다.

```java
public int pop() {
  while (!stack1.isEmpty()) {
    stack2.push(stack1.pop());
  }

  return stack2.pop();
}
```

만약 `stack2`가 비어있지 않다면 `stack2`는 위에서 말한대로 데이터가 역순으로 저장되어 있어 `stack2`의 최상위 데이터를 꺼내면 됩니다.

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

만약 `stack1`과 `stack2` 모두 비어있다면 예외 처리를 해주면 됩니다.

## Full Source Code

```java
import java.util.Stack;

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

    if (stack1.isEmpty()) {
      // handle exception
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
