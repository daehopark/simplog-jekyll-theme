---
layout: post
title: "[Leetcode] 704. Binary Search"
tags: ["leetcode"]
comments: true
---

오름차순으로 정렬된 배열과 `target`이 주어지면 배열에서 `target` 원소를 찾아 인덱스를 반환해야 한다. 만약 `target` 원소가 없다면 `-1`을 반환해야 한다.

자바에서 `Arrays` 객체를 이용하면 `binarySearch` 메서드를 지원한다.

```java
import java.util.Arrays;

class Solution {
  public int search(int[] nums, int target) {
    int targetIndex = Arrays.binarySearch(nums, target);
    return targetIndex >= 0 ? targetIndex : -1;
  }
}
```

이렇게만 풀면 너무 아쉬우니까 직접 `binarySearch` 메서드를 구현해보자.

```java
class Solution {
  public int search(int[] nums, int target) {
    int targetIndex = binarySearch(nums, target);
    return targetIndex >= 0 ? targetIndex : -1;
  }

  private int binarySearch(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
      int mid = (left + right) / 2;
      if (target == nums[mid]) {
        return mid;
      }

      if (target < nums[mid]) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    }
    return -1;
  }
}
```

이번에는 재귀로도 한 번 풀어보자.

```java
class Solution {
  public int search(int[] nums, int target) {
    int targetIndex = binarySearch(nums, 0, nums.length - 1, target);
    return targetIndex >= 0 ? targetIndex : -1;
  }

  private int binarySearch(int[] nums, int left, int right, int target) {
    if (left > right) {
      return -1;
    }

    int mid = (left + right) / 2;
    if (target == nums[mid]) {
      return mid;
    }

    if (target < nums[mid]) {
      return binarySearch(nums, left, mid - 1, target);
    } else {
      return binarySearch(nums, mid + 1, right, target);
    }
  }
}
```
