---
layout: post
title: "[Leetcode] 449. Serialize and Deserialize BST"
tags: ["leetcode"]
comments: true
---

이진 탐색 트리(`Binary Search Tree`)를 문자열과 호환할 수 있도록 `Serialize`와 `Deserialize` 메서드를 구현해야 한다.

## Serialize

`BST`를 `Level Order`하게 문자열로 저장한다. 각 데이터를 구분할 수 있게 띄어쓰기를 해줘서 나중에 띄어쓰기를 기준으로 데이터를 파싱할 수 있게 한다.

```java
// Encodes a tree to a single string.
public String serialize(TreeNode root) {
  if (root == null) return null;

  Queue<TreeNode> q = new LinkedList<>();
  q.add(root);

  StringBuilder sb = new StringBuilder();
  while (!q.isEmpty()) {
    TreeNode node = q.poll();
    sb.append(node.val).append(' ');
    if (node.left != null) q.add(node.left);
    if (node.right != null) q.add(node.right);
  }

  return sb.toString().trim();
}
```

## Deserialize

전달 받은 문자열을 띄어쓰기를 기준으로 파싱하여 `BST`에 넣을 데이터 배열을 얻는다. 그리고 순차적으로 `BST`에 데이터를 넣어주면 된다. `Level Order`로 데이터를 넣었기 때문에 `BST`에 데이터를 삽입하면 원래의 형태를 그대로 유지할 수 있게 된다.

```java
// Decodes your encoded data to tree.
public TreeNode deserialize(String data) {
  if (data == null) return null;

  String[] temp = data.split("\\s+");
  TreeNode root = null;
  for (String e : temp) {
    root = push(root, Integer.parseInt(e));// O(logN)
  }// O(NlogN)

  return root;
}

private TreeNode push(TreeNode node, int data) {
  if (node == null) {
    return new TreeNode(data);
  }

  if (data < node.val) {
    node.left = push(node.left, data);
  } else {
    node.right = push(node.right, data);
  }

  return node;
}// O(logN)
```

## Full Source Code

```java
import java.util.LinkedList;
import java.util.Queue;

class TreeNode {
  int val;
  TreeNode left;
  TreeNode right;

  TreeNode(int x) {
    val = x;
  }
}

class Codec {

  // Encodes a tree to a single string.
  public String serialize(TreeNode root) {
    if (root == null) return null;

    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);

    StringBuilder sb = new StringBuilder();
    while (!q.isEmpty()) {
      TreeNode node = q.poll();
      sb.append(node.val).append(' ');
      if (node.left != null) q.add(node.left);
      if (node.right != null) q.add(node.right);
    }

    return sb.toString().trim();
  }

  // Decodes your encoded data to tree.
  public TreeNode deserialize(String data) {
    if (data == null) return null;

    String[] temp = data.split("\\s+");
    TreeNode root = null;
    for (String e : temp) {
      root = push(root, Integer.parseInt(e));// O(logN)
    }// O(NlogN)

    return root;
  }

  private TreeNode push(TreeNode node, int data) {
    if (node == null) {
      return new TreeNode(data);
    }

    if (data < node.val) {
      node.left = push(node.left, data);
    } else {
      node.right = push(node.right, data);
    }

    return node;
  }// O(logN)
}
```
