---
title: "78. Subsets"
date: 2023-05-17T01:26:32+08:00
draft: false
author: 'Yang'
tags: ['backtracking', 'DFS']
categories: ['leetcode']
---
## 前言：
這題在school也有寫過，當時還負責主講，沒想到時到今日全都忘光了。這次看neetcode的作法選用DFS，赫然發現這是一個更好的作法。

---

## 題目：

**Given an integer array nums of unique elements, return all possible subsets(the power set).**

**The solution set must not contain duplicate subsets. Return the solution in any order.**

```
* Example 1:
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

* Example 2:
Input: nums = [0]
Output: [[],[0]]

```

---

## 解題思路：
給定一個list:
```
arr = [1,2]
```
用一個暫存的`subset`來存放當前可能的子集合，每個元素都有兩種選擇，要放入`subset`或是不要放入。
```
				     []
				   /    \
				  []    [1]
				 /  \   /  \
			        [] [2] [1] [1,2]
```
從上圖可以看出，每一層都是對於一個元素的取捨，從最上面的元素`1`開始，選擇左邊的路就是不放入當前元素，右邊的路就是放入，最後生成所有可能的子集

### 1. Base case
因為input是一個array，所以當array走到底時就要跳出，此時也代表走到**這條DFS路線上的終點**，所以可以將當前的`subset`加入到`res`當中

但要注意，必須回傳`subset.copy()`。假如回傳`subset`本人，因為list是pass by reference的特性，最後res中的所有結果都會指向同一個`subset`

### 2. 要或不要加入該元素
如果想加入元素，就直接對`subset` append新的元素即可。但考慮不加入該元素時需要先把該元素pop出去

---

# 程式碼：

```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        subset = []

        def dfs(i):
            if i >= len(nums):
                res.append(subset.copy())
                return
            
            # decide to include nums[i]
            subset.append(nums[i])
            dfs(i+1)

            # decide not to include nums[i]
            subset.pop()
            dfs(i+1)

        dfs(0)
        return res
```

