---
layout: post
title: "[Leetcode] 532. K-diff Pairs in an Array"
tags: ["leetcode"]
comments: true
---

배열과 `K`가 주어지고 배열에서 임의의 두 원소를 뽑아 그 차이가 `K`인 조합을 모두 찾아야 한다. 원소는 중복으로 사용할 수 없지만 같은 값이 여러 번 등장할 수는 있다.

가장 먼저 떠오른 해결책은 배열을 오름차순으로 정렬한 다음 순차적으로 탐색하면서 임의의 원소 `a`에 대해 `a + b = k`를 만족하는 수를 찾았다. 배열이 오름차순으로 정렬되었기 때문에 `b`를 이진 탐색으로 찾을 수 있어 빠를 것이라 생각했다. 중요한 것은 중복을 방지하기 위해 한 번 등장한 `a`는 두 번 사용하지 않아야 한다.

```java
import java.util.Arrays;

class Solution {
  public int findPairs(int[] nums, int k) {
    Arrays.sort(nums);// O(NlogN)

    int pairsCount = 0;
    for (int i = 0; i < nums.length; i++) {
      if (i > 0 && nums[i - 1] == nums[i]) {
        continue;
      }
      int pairIndex = Arrays.binarySearch(nums, i + 1, nums.length, nums[i] + k);// O(logN)
      if (pairIndex >= 0) {
        pairsCount++;
      }
    }// O(NlogN)

    return pairsCount;
  }// O(NlogN)
}
```

두 번째 해결책은 `HashMap`을 사용하여 시간 복잡도를 줄이는 것이다. `HashMap`의 키는 원소의 값이고 값은 원소가 출현한 횟수를 저장한다. `HashMap`의 키셋을 순차적으로 탐색하면서 `a + b = k`를 만족하는 경우의 수를 찾는다.

중복을 방지하기 위해 임의의 원소 `a`에 대해 `a + b = k`를 만족하는 원소만 찾아야 한다. `a - b = k`를 만족하는 원소를 찾으면 `b`를 기준값으로 했을 때에 중복이 발생할 수 있다. 또한 `a`와 `b`가 같은 경우 해당 원소가 2번 이상 들어있는지 확인해야 한다.

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
  public int findPairs(int[] nums, int k) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
      map.put(num, map.getOrDefault(num, 0) + 1);
    }// O(N)

    int pairCount = 0;
    for (Integer num : map.keySet()) {
      int pairNum = num + k;
      if (pairNum == num && map.get(num) >= 2) {
        pairCount++;
        continue;
      }
      if (pairNum != num && map.containsKey(pairNum)) {
        pairCount++;
      }
    }// O(N)

    return pairCount;
  }// O(N)
}
```

시간 복잡도는 두 번째 풀이법이 더 좋아보였지만 실제로는 첫 번째 풀이법이 훨씬 더 빠른 시간 안에 해결되었다. `HashMap`의 내부 구현이 복잡해서 그런걸까?
