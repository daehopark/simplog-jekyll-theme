---
layout: post
title: "SimpLog Style Guide"
tags: ["SimpLog"]
comments: false
---

## Heading Level 2
### Heading Level 3
#### Heading Level 4
##### Heading Level 5
###### Heading Level 6

## Paragraph

`Lorem Ipsum` is simply dummy text of the printing and typesetting industry. `Lorem Ipsum` has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing `Lorem Ipsum` passages, and more recently with desktop publishing software like Aldus PageMaker including versions of `Lorem Ipsum`.

## Link

[This is the link](/)

## Unordered List

- unordered list item
- unordered list item
- unordered list item
    - unordered list item
    - unordered list item
- unordered list item

## Ordered List

1. ordered list item
2. ordered list item
3. ordered list item
    1. ordered list item
    2. ordered list item
4. ordered list item

## Quote

> Neque porro quisquam est qui dolorem ipsum quia dolor sit amet, consectetur, adipisci velit... There is no one who loves pain itself, who seeks after it and wants to have it, simply because it is pain...

## Code

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return null;
    }
}
```

## Table

| Tables   |      Are      |  Cool |
|----------|:-------------:|------:|
| col 1 is |  left-aligned | $1600 |
| col 2 is |    centered   |   $12 |
| col 3 is | right-aligned |    $1 |
