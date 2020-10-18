---
layout: post
title: "[Leetcode] 74. Search a 2D Matrix"
tags: ["leetcode"]
comments: true
---

2차원 배열과 `target`이 주어지고 2차원 배열의 원소 중 `target`이 존재하는지를 반환하는 문제다.

## Solution 1

2차원 배열을 1차원 배열처럼 생각하고 문제를 해결하면 된다. 1차원 배열 인덱스를 `(m * n)` 크기의 2차원 배열의 인덱스를 변환하려면 아래처럼 하면 된다.

```java
private int getValue(int[][] matrix, int index, int n) {
  return matrix[index / n][index % n];
}
```

2차원 배열을 1차원 배열처럼 생각하면 모든 원소가 오름차순으로 정렬되어 있으니 이진 탐색을 통해 문제를 해결할 수 있다.

```java
class Solution {
  public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0) return false;
    if (matrix.length == 1) return Arrays.binarySearch(matrix[0], target) >= 0;

    int m = matrix.length;
    int n = matrix[0].length;
    int l = 0;
    int r = m * n - 1;
    while (l <= r) {
      int mid = (l + r) / 2;
      int midVal = getValue(matrix, mid, n);
      if (target == midVal) return true;
      if (target < midVal) {
        r = mid - 1;
      } else {
        l = mid + 1;
      }
    }

    return false;
  }

  private int getValue(int[][] matrix, int index, int n) {
    return matrix[index / n][index % n];
  }
}
```
