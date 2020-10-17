---
layout: post
title: "[Leetcode] 189. Rotate Array"
tags: ["leetcode"]
comments: true
---

배열과 `k`가 주어지면 배열을 오른쪽으로 `k`만큼 이동시켜야 한다.

기본적으로 배열의 길이가 1보다 같거나 작은 경우, `k`가 0인 경우는 배열을 그냥 반환하면 된다. 또한 `k`의 값이 배열의 길이와 같거나 초과할 경우는 불필요한 연산을 방지하기 위해 `k`의 값을 배열의 길이로 나눈 나머지 값으로 만들어 준다.

```java
class Solution {
  public void rotate(int[] nums, int k) {
    if (nums.length <= 1) return;

    k %= nums.length;
    if (k == 0) return;

    /* ... */
  }
}
```

## Solution 1

주어진 배열과 똑같은 길이의 배열을 복사한 다음, 배열을 순환 배열로 생각하면서 `k`번째 이전의 인덱스를 가진 배열의 값부터 채워나가면 된다.

```java
class Solution {
  public void rotate(int[] nums, int k) {
    if (nums.length <= 1) return;

    k %= nums.length;
    if (k == 0) return;

    int[] copied = Arrays.copyOf(nums, nums.length);
    for (int i = 0; i < nums.length; i++) {
      nums[i] = copied[(i - k + nums.length) % nums.length];
    }
  }
}
```

## Solution 2

문제를 해결하기 위해 필요한 여분의 배열의 길이가 꼭 주어진 배열의 길이와 같아야 할 필요가 있을까? `k` 만큼의 길이를 가진 배열을 만들어 블록 단위로 배열을 이동시키면 될 것 같다는 생각이 들었다.

```java
class Solution {
  public void rotate(int[] nums, int k) {
    if (nums.length <= 1) return;

    k %= nums.length;
    if (k == 0) return;

    int[] kArr = Arrays.copyOf(nums, k);
    for (int i = 0; i < nums.length; i++) {
      int index = (i + k) % nums.length;
      int temp = nums[index];
      nums[index] = kArr[i % k];
      kArr[i % k] = temp;
    }
  }
}
```

## Solution 3

발상을 전환해서 배열을 반대로 뒤집는 연산을 통해 문제를 해결할 수도 있다. 다음과 같은 순서로 배열을 뒤집으면 된다.

- 주어진 배열을 뒤집는다.
- `0`부터 `k-1`번째 인덱스까지 배열을 뒤집는다.
- `k`부터 배열의 마지막 인덱스까지 배열을 뒤집는다.

```java
class Solution {
  public void rotate(int[] nums, int k) {
    if (nums.length <= 1) return;

    k %= nums.length;
    if (k == 0) return;

    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
  }

  private void reverse(int[] nums, int l, int r) {
    while (l < r) {
      int temp = nums[l];
      nums[l] = nums[r];
      nums[r] = temp;
      l++;
      r--;
    }
  }
}
```
