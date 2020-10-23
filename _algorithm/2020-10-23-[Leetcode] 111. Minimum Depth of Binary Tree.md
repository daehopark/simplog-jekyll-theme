---
layout: post
title: "[Leetcode] 111. Minimum Depth of Binary Tree"
tags: ["leetcode"]
comments: true
---

주어진 이진 탐색 트리의 최소 `depth` 값을 구해 반환해야 한다.

## Solution

`BFS`를 사용하여 트리를 `Level Order`로 탐색하면서 `Leaf Node`를 찾는다.

```java
class Solution {
  public int minDepth(TreeNode root) {
    if (root == null) return 0;

    int level = 1;
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
      int qSize = q.size();
      for (int i = 0; i < qSize; i++) {
        if (isLeafNode(q.peek())) return level;
        TreeNode treeNode = q.poll();
        if (treeNode.left != null) q.add(treeNode.left);
        if (treeNode.right != null) q.add(treeNode.right);
      }
      level++;
    }

    return -1;
  }

  private boolean isLeafNode(TreeNode treeNode) {
    return treeNode.left == null && treeNode.right == null;
  }
}
```
