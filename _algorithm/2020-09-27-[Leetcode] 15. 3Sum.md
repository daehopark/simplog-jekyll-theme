---
layout: post
title: "[Leetcode] 15. 3Sum"
tags: ["leetcode"]
comments: true
---

주어진 배열에서 임의의 원소 3개의 합이 0이 되는 조합을 찾되 중복은 없어야 한다. 모든 조합을 다 찾은 뒤에 중복을 제거해주는 방법은 시간이 너무 오래 걸릴 것 같다는 생각이 들어 중복이 될 조합은 미리 제거해주기 위해 배열을 오름차순으로 정렬했다.

정렬된 배열을 순차적으로 탐색하면서 임의의 원소를 하나 택하면 나머지 배열에서 2개의 원소를 찾는 문제로 축소시킬 수 있다. 배열이 이미 정렬되어 있기 때문에 이진 탐색을 통해 2개의 원소를 찾아 하나씩 반환하면 된다.

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
  public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);// O(NlogN)

    List<List<Integer>> answer = new ArrayList<>();
    for (int i = 0; i < nums.length - 2; i++) {
      // prevent from duplication case
      if (i - 1 >= 0 && nums[i] == nums[i - 1]) {
        continue;
      }

      for (int j = i + 1; j < nums.length - 1; j++) {
        // prevent from duplication case
        if (j - 1 >= i + 1 && nums[j] == nums[j - 1]) {
          continue;
        }

        int twoSum = nums[i] + nums[j];
        int tempIndex = Arrays.binarySearch(nums, j + 1, nums.length, -twoSum);
        if (tempIndex >= 0) {
          answer.add(Arrays.asList(nums[i], nums[j], -twoSum));// O(logN)
        }
      }
    }// O(N^2logN)

    return answer;
  }
}
```
