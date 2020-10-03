---
layout: post
title: "[Leetcode] 39. Combination Sum"
tags: ["leetcode"]
comments: true
---

주어진 배열의 원소를 조합하여 그 합이 `target`이 되는 경우의 수를 모두 찾아 반환해야 한다. 배열의 각 원소는 중복 사용이 가능하며 조합의 중복은 허용하지 않는다.

처음에는 모든 경우의 수를 다 찾기 위해 완전 탐색을 사용했다. 배열의 첫 번째 원소부터 시작해서 사용하지 않는 경우, 한 번 사용하는 경우, 두 번 사용하는 경우와 같은 방식으로 모든 경우의 수를 다 찾은 다음 그 합이 `target`이 되면 정답에 추가했다.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

class Solution {
  private final List<List<Integer>> answer = new ArrayList<>();

  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    combinationSum(candidates, target, 0, new ArrayList<>());
    return answer;
  }

  private void combinationSum(int[] candidates, int target, int index, List<Integer> combination) {
    if (target == 0) {
      answer.add(new ArrayList<>(combination));
      return;
    }

    if (target < 0 || index >= candidates.length) {
      return;
    }

    combinationSum(candidates, target, index + 1, new ArrayList<>(combination));

    while (target > 0) {
      target -= candidates[index];
      combination.add(candidates[index]);
      combinationSum(candidates, target, index + 1, new ArrayList<>(combination));
    }
  }
}
```

문제는 `combinationSum` 메서드를 호출할 때마다 새로운 `List`를 할당하여 메모리를 많이 낭비하는 것이다. 하나의 `List`를 재사용하면서 문제를 해결할 수 없을까? 바로 `DFS`를 떠올려 `Backtracking`으로 `List`를 재사용했다.

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.stream.Collectors;

class Solution {
  private final List<List<Integer>> answer = new ArrayList<>();

  public List<List<Integer>> combinationSum(int[] candidates, int target) {
    backtrack(candidates, target, 0, new LinkedList<>());
    return answer;
  }

  private void backtrack(int[] candidates, int target, int index, LinkedList<Integer> combination) {
    if (target == 0) {
      answer.add(new ArrayList<>(combination));
      return;
    }

    if (target < 0) {
      return;
    }

    for (int i = index; i < candidates.length; i++) {
      combination.add(candidates[i]);
      backtrack(candidates, target - candidates[i], i, combination);
      combination.removeLast();
    }
  }
}
```
