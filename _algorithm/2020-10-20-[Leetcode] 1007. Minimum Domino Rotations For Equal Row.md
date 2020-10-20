---
layout: post
title: "[Leetcode] 1007. Minimum Domino Rotations For Equal Row"
tags: ["leetcode"]
comments: true
---

`A` 혹은 `B`의 주사위가 모두 같은 숫자가 될 수 있도록 주사위를 바꾸는 최소한의 횟수를 구해야 한다.

## Solution 1

다음 4가지 경우의 수를 모두 구한 다음 최소한의 횟수를 구하면 된다.

- `A`의 첫 번째 숫자가 기준점인 경우
- `A`의 주사위가 모두 같은 숫자가 되기 위한 최소한의 횟수
- `B`의 주사위가 모두 같은 숫자가 되기 위한 최소한의 횟수
- `B`의 첫 번째 숫자가 기준점인 경우
- `A`의 주사위가 모두 같은 숫자가 되기 위한 최소한의 횟수
- `B`의 주사위가 모두 같은 숫자가 되기 위한 최소한의 횟수

```java
class Solution {
  public int minDominoRotations(int[] A, int[] B) {
    int answer = Integer.MAX_VALUE;
    int a = minDominoRotations(A, B, A[0]);
    int b = minDominoRotations(A, B, B[0]);
    int c = minDominoRotations(B, A, A[0]);
    int d = minDominoRotations(B, A, B[0]);
    if (a != -1) answer = Math.min(answer, a);
    if (b != -1) answer = Math.min(answer, b);
    if (c != -1) answer = Math.min(answer, c);
    if (d != -1) answer = Math.min(answer, d);
    return answer == Integer.MAX_VALUE ? -1 : answer;
  }

  private int minDominoRotations(int[] A, int[] B, int target) {
    int count = 0;
    for (int i = 0; i < A.length; i++) {
      if (A[i] == target) continue;
      if (B[i] == target) {
        count++;
        continue;
      }
      return -1;
    }
    return count;
  }
}
```

## Solution 2

반드시 4번의 계산을 해야하는가? `A`의 첫 번째 주사위 숫자와 `B`의 첫 번째 주사위 숫자가 같다면 1번만 수행하면 된다. 또한 `A`의 모든 주사위의 숫자가 같기 위해 바꿔야 하는 횟수와 `B`의 모든 주사위의 숫자가 같기 위해 바꿔야 하는 횟수를 동시에 구할 수 있다.

```java
class Solution {
  public int minDominoRotations(int[] A, int[] B) {
    if (A[0] == B[0]) return minDominoRotations(A, B, A[0]);
    int x = minDominoRotations(A, B, A[0]);
    int y = minDominoRotations(B, A, B[0]);
    if (x == -1 || y == -1) return Math.max(x, y);
    return Math.min(x, y);
  }

  private int minDominoRotations(int[] A, int[] B, int target) {
    int cnt1 = 0, cnt2 = 0;
    for (int i = 0; i < A.length; i++) {
      if (A[i] != target && B[i] != target) return -1;
      if (A[i] != target) cnt1++;
      if (B[i] != target) cnt2++;
    }
    return Math.min(cnt1, cnt2);
  }
}
```
