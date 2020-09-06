---
layout: post
title: "[Leetcode] 108. Convert Sorted Array to Binary Search Tree"
tags: ["leetcode"]
comments: true
---

오름차순으로 정렬된 배열이 주어지고 해당 배열을 균형 이진 탐색 트리(`Balanced Binary Search Tree`)로 변환해야 한다. 배열이 오름차순이기 때문에 배열의 중앙 인덱스를 기점으로 배열을 좌우로 나눈다. 배열의 중앙 인덱스는 `root`가 되고 좌측 배열은 왼쪽 서브 트리가, 우측 배열은 오른쪽 서브 트리가 된다. 좌우가 동일한 구조로 이진 탐색 트리가 생성되기 때문에 항상 균형 잡힌 상태가 된다.

```java
class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode() {
  }

  TreeNode(int val) {
    this.val = val;
  }

  TreeNode(int val, TreeNode left, TreeNode right) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

class Solution {
  public TreeNode sortedArrayToBST(int[] nums) {
    return sortedArrayToBST(nums, 0, nums.length - 1);
  }

  private TreeNode sortedArrayToBST(int[] nums, int start, int end) {
    if (start > end) {
      return null;
    }
    if (start == end) {
      return new TreeNode(nums[start]);
    }
    int mid = (start + end) / 2;
    TreeNode left = sortedArrayToBST(nums, start, mid - 1);
    TreeNode right = sortedArrayToBST(nums, mid + 1, end);
    return new TreeNode(nums[mid], left, right);
  }
}
```
