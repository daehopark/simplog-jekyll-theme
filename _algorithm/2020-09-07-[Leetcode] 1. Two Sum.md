---
layout: post
title: "[Leetcode] 1. Two Sum"
tags: ["leetcode"]
comments: true
---

원소가 정수인 배열이 주어진다. 임의의 두 원소의 합이 `target`이 되는 두 원소의 인덱스를 반환하면 된다. 무조건 한 쌍의 원소의 합만 `target`이 되며 다른 경우는 없다. 또한 같은 원소가 2번 반복되지 않는다.

`HashMap`을 생성하고 배열을 순차적으로 탐색하면서 원소의 값을 `key`로 인덱스를 `value`로 저장한다. 임의의 원소 `nums[i]`에 대해 `target - nums[i]`가 `HashMap`에 존재하면 두 원소의 합이 `target`임을 의미하므로 두 원소의 인덱스를 반환한다.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
  private final Map<Integer, Integer> map = new HashMap<>();

  public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
      if (map.containsKey(target - nums[i])) {
        return new int[]{map.get(target - nums[i]), i};
      }
      map.put(nums[i], i);
    }// O(N)
    return null;
  }
}
```
