---
title: "39. Combination Sum
"
date: 2023-05-19T01:07:13+08:00
draft: false
author: 'Yang'
tags: ['tree', 'BFS', 'backtracking']
categories: ['leetcode']
---
## 前言：
這題的決策樹(decision tree)比較比較特別，左邊代表繼續選原本的，右邊代表啥都不選，就給你空著。只要把這個概念想清楚，這題也就迎刃而解了。

---

## 題目：

Given an array of **distinct** integers candidates and a target integer target, return a list of all **unique combinations** of candidates where the chosen numbers sum to target. You may return the combinations in **any order**.

The **same** number may be chosen from candidates an **unlimited number of times**. Two combinations are unique if the **frequency** of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than 150 combinations for the given input.

給定一個整數`target`，找出`list`中可以組合成target的不重複組合，且組合中的數字可以重複出現。
```
* Example 1:
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

* Example 2:
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]

* Example 3:
Input: candidates = [2], target = 1
Output: []

```

---

## 解題思路：
直接上核心的code比較快:
```
// decide to include nums[i]
cur.append(nums[i])
dfs(i, cur, total + nums[i])

// decide not to include nums[i]
cur.pop()
dfs(i+1, cur, total) 
```
`cur`代表目前決策走到現在，放入了什麼數字。同前提所提到的，可以假設**走左邊表示放入當前數字，走右邊表示選擇不放入**，且放入數字後，**下一次也繼續對同樣的數字做一樣的考慮**。也就是說，如果當`i=0`得到`nums[0]=2`，那選擇走左邊就是把一個數字`2`放入`cur`中，並且保持`i=0`，下一次就會對相同數字做決策。

這樣做就可以達成題目所要求，可以重複數字的條件。

既然左邊選擇不更新`i`，在子節點上對同數字做決策，那麼選擇右邊就是不選擇放入`nums[i]`，並且直接跳過`i=0`的可能，去考慮`i+1`情況的決策。

一開始心中有個疑問:

**那既加入`nums[0]=2`又不保持`i=0`，更新i+=1，使下一次對`i=1`的數值做決策的情況去哪了呢？**

其實就在第一次選擇左邊後(`i=0`, 此時`cur=[nums[0]]`)，第二次選擇往右走(`i=0`,此時`cur=[nums[0]]`)，第三次選擇往左(`i=1`,此時`cur=[nums[0], nums[1]]`)的情況。

講起來有點複雜，拿出紙筆畫圖就會理解了

### 1. Base case
如果可以找到跟`target`相等的組合，當然立馬加入`res`中

如果決策樹已經走到底，也就是`index i >= len(nums)`，或是者是當前組合的總數已經超越目標值`target`了，那就直接return跳出stack即可

```
if total == target:
	res.append(cur.copy())
	return
if i >= len(nums) or total > target:
	return
```
### 2. total這個變數
因為total會隨著當前組合改變，所以如果使用全域變數的話，理論上pop()的時候還需要再減掉之前append的數值，不如就直接當成argument來傳遞。一樣的道理也適用於另外兩個變數`i`, `cur`

---

# 程式碼：

```
class Solution:
    def combinationSum(self, nums: List[int], target: int) -> List[List[int]]:
        res = []

        def dfs(i, cur, total):
            if total == target:
                res.append(cur.copy())
                return
            if i >= len(nums) or total > target:
                return
            
            # decide to include nums[i]
            cur.append(nums[i])
            dfs(i, cur, total + nums[i])

            # decide not to include nums[i]
            cur.pop()
            dfs(i+1, cur, total) 

        dfs(0, [], 0)
        return res
```

