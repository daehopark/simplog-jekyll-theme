---
layout: post
title: "[Leetcode] 213. House Robber II"
tags: ["leetcode"]
comments: true
---

집을 털었을 때 얻을 수 있는 금액이 배열로 주어진다. 임의의 집을 털었을 때, 이웃집은 털 수 없다. 집이 원형으로 배치되어 있기 때문에 첫 번째 집은 마지막 집과 이웃이다. 털 수 있는 최대 금액을 반환해야 한다.

임의의 `i`번째 집부터 시작했을 때 `i`번째 집을 털 수도 있고 털지 않을 수도 있는데, 2가지 경우의 수 중 더 큰 금액을 취하면 된다. 단, 반복된 연산을 피하기 위해 `cache` 배열을 선언하여 한 번 구한 값은 재사용한다.

```java
private int rob(int[] cache, int[] nums, int index, int maxIndex) {
  if (index >= maxIndex) return 0;
  if (cache[index] != -1) return cache[index];
  int case1 = nums[index] + rob(cache, nums, index + 2, maxIndex);
  int case2 = rob(cache, nums, index + 1, maxIndex);
  return cache[index] = Math.max(case1, case2);
}
```

문제점은 첫 번째 집과 마지막 집을 동시에 터는 경우의 수를 배제할 수 없다는 데에 있었다. 그래서 2가지 경우로 또 나눴다.

- 마지막 집을 무조건 털지 않는 경우
- 첫 번째 집을 무조건 털지 않는 경우

```java
class Solution {
  public int rob(int[] nums) {
    if (nums.length == 1) return nums[0];
    if (nums.length == 2) return Math.max(nums[0], nums[1]);

    int[] cache = new int[nums.length];

    Arrays.fill(cache, -1);
    int case1 = rob(cache, nums, 0, nums.length - 1);// without last house

    Arrays.fill(cache, -1);
    int case2 = rob(cache, nums, 1, nums.length);// without first house

    return Math.max(case1, case2);
  }

  private int rob(int[] cache, int[] nums, int index, int maxIndex) {
    if (index >= maxIndex) return 0;
    if (cache[index] != -1) return cache[index];
    int case1 = nums[index] + rob(cache, nums, index + 2, maxIndex);
    int case2 = rob(cache, nums, index + 1, maxIndex);
    return cache[index] = Math.max(case1, case2);
  }
}
```
