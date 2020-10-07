---
layout: post
title: "[Leetcode] 701. Insert into a Binary Search Tree"
tags: ["leetcode"]
comments: true
---

이진 탐색 트리(`Binary Search Tree`)와 값이 주어지는데 해당 값을 트리의 적절한 곳에 입력해주는 문제다.

풀이는 그리 어렵지 않았다. `root`가 비어있으면 그곳에 해당 값을 넣어주면 되고 임의의 서브 트리에 대해 해당 `root` 노드의 값보다 작으면 왼쪽 서브 트리에, 크면 오른쪽 서브 트리에서 작업할 수 있도록 재귀를 하면 된다.

```java
class Solution {
  public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) {
      return root = new TreeNode(val);
    }

    if (val < root.val) {
      if (root.left == null) {
        root.left = new TreeNode(val);
      } else {
        insertIntoBST(root.left, val);
      }
    } else {
      if (root.right == null) {
        root.right = new TreeNode(val);
      } else {
        insertIntoBST(root.right, val);
      }
    }

    return root;
  }
}
```

풀이는 완료했지만 어쩐지 `if else` 문을 너무 많이 쓰는 것 같아 마음이 불편했다. 저 부분의 코드를 조금 더 줄일 수 없을까? `insertIntoBST` 메서드가 각 서브 트리의 `root`를 반환하니 그냥 할당해주면 되겠다고 생각했다.

```java
if (val < root.val) {
  root.left = insertIntoBST(root.left, val);
} else {
  root.right = insertIntoBST(root.right, val);
}
```

완성된 코드는 다음과 같다.

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
  public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) {
      return new TreeNode(val);
    }

    if (val < root.val) {
      root.left = insertIntoBST(root.left, val);
    } else {
      root.right = insertIntoBST(root.right, val);
    }

    return root;
  }
}
```
