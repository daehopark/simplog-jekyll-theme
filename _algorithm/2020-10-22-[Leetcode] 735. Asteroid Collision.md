---
layout: post
title: "[Leetcode] 735. Asteroid Collision"
tags: ["leetcode"]
comments: true
---

운석들의 움직이는 방향과 세기(?)가 배열로 주어지고 운석 충돌 시뮬레이션 결과를 배열로 반환해야 한다.

## Solution

`Stack`을 사용하여 운석을 순차적으로 쌓으면서 충돌 시뮬레이션을 구현한다. 임의의 운석을 `Stack`에 넣으려는데 해당 운석이 왼쪽 방향으로 이동하는 중이고 `Stack`에 마지막으로 쌓인 운석이 오른쪽으로 이동하는 중이라면 충돌이 발생한다.

```java
class Solution {
  public int[] asteroidCollision(int[] asteroids) {
    Stack<Integer> stack = new Stack<>();
    for (int asteroid : asteroids) {
      if (asteroid > 0) {
        stack.add(asteroid);
        continue;
      }
      crush(stack, asteroid);
    }
    return stack.stream().mapToInt(i -> i).toArray();
  }

  private void crush(Stack<Integer> stack, int leftMoveAsteroid) {
    while (true) {
      if (stack.isEmpty() || stack.peek() < 0) {
        stack.add(leftMoveAsteroid);
        return;
      }

      int rightMoveAsteroid = stack.peek();
      int crushed = rightMoveAsteroid + leftMoveAsteroid;
      if (crushed > 0) return;
      stack.pop();
      if (crushed == 0) return;
    }
  }
}
```
