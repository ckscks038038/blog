---
title: "90. Subsets II"
date: 2023-05-19T16:24:10+08:00
draft: false
author: 'Yang'
tags: ['tree', 'backtracking']
categories: ['leetcode']
---
## 前言：
這題跟**78. Subsets** 是幾乎一樣的問題，唯一的差別是這題給的input `nums`會有重複元素出現，但是output `res`不可有重複組合。

---

## 題目：

Given an integer array `nums` that may contain duplicates, return all possible 
subsets
 (the power set).

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

```
* Example 1:
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]

* Example 2:
Input: nums = [0]
Output: [[],[0]]

```

---

## 解題思路：
一樣用決策樹的思路，一邊是放入當前元素、另一邊是不放入當前元素，只是放入如果已經放入了`nums[i]`(**#1**)，在`pop()`後並且跳過`nums[i]`，打算在下次考慮`nums[i+1]`前，要先判斷目前的`nums[i]`與`nums[i+1]`是否一樣，如果一樣的話就跳過。這樣才可避免走到跟 **#1** 一樣的決策情況。

一樣，決策的題目畫出決策樹是最好理解的。


---

# 程式碼：

```
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort()

        def dfs(i, subset):
            if i == len(nums):
                res.append(subset.copy())
                return
        
            # All subsets that include nums[i]
            subset.append(nums[i])
            dfs(i + 1, subset)
            subset.pop()

            # All subsets that don't include nums[i]
            while i + 1 < len(nums) and nums[i] == nums[i + 1]:
                i += 1
            dfs(i + 1, subset)

        dfs(0, [])
        return res
```


