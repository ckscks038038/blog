---
title: "46. Permutations"
date: 2023-05-19T01:40:26+08:00
draft: false
author: 'Yang'
tags: ['tree', 'backtracking']
categories: ['leetcode']
---
## 前言：
這題看neetcode的作法感覺像是逆序而行，不太直觀，目前為止還沒看懂。先以之前寫過的解法來做題，下次再做到這題再用neetcode的解法試試。

---

## 題目：

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

```
* Example 1:
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

* Example 2:
Input: nums = [0,1]
Output: [[0,1],[1,0]]

* Example 3:
Input: nums = [1]
Output: [[1]]
```

---

## 解題思路：
假設一開始組合為空，那就是用for迴圈考慮當前的所有選擇，進入每個選擇時都先將當前的元素加入組合中，並且把當前數字移除，使得之後可以考慮的數字剩下除了剛剛加入的元素之外的所有元素，接著再重新進入下一個迴圈


### 1. Base case
當目前組合的長度已經等於原本nums的長度，則表示已經選完所有數字，即將當前組合append到res中



---

# 程式碼：

```
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        comb = []
        n = len(nums)
        def backtracking(nums):
            if len(comb) == n:
                res.append(comb.copy())
                return
            for i in nums:
                comb.append(i)
                copy = nums.copy()
                copy.remove(i)
                backtracking(copy)
                comb.pop()

        backtracking(nums)
        return res
```


