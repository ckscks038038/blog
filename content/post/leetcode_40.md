---
title: "40. Combination Sum II"
date: 2023-05-19T22:44:45+08:00
draft: false
author: 'Yang'
tags: ['tree']
categories: ['leetcode']
---
## 前言：
這題跟剛剛做完的`90. Subsets II` 解法幾乎完全一樣，只差了這次需要考慮total，但值得注意的是，兩個題目是完全不同的呈現，所以有時仔細思考後，會發現不同問題其實本質是一致的。

另外，這次neetcode是用for迴圈解，我現在覺得決策樹問題用dfs思考會比較直觀，所以就用了底下留言給的程式碼做參考。

---

## 題目：

Given a collection of **candidate** numbers (**candidates**) and a target number (target), find all unique combinations in **candidates** where the candidate numbers sum to `target`.

Each number in candidates may only be used **once** in the combination.

**Note**: The solution set must not contain duplicate combinations.

```
* Example 1:
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

* Example 2:
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]

```

---

## 解題思路：



### 1. 用決策樹分析
以dfs的方式，對每個`candidates`中的元素做決策，這次要加入、或是不要加入，並且只要加入了，total就要扣掉加入的數值，直到達成total剛好等於`target`，或是已經把total減到負數。

### 2. 避免重複的組合被考慮
題目有提出，**Note**: The solution set must not contain duplicate combinations.

也就是`res`中任何組合只接受出現一次，因此要避免出現重複組合，以決策樹的角度就是，一邊選擇加入當前元素，另外一邊拒絕加入當前元素，且如果當前元素與下一個元素重複，也要先透過更新`i = i + 1`跳過此重複出現的元素，這樣就可以避免相同組合重複出現。

---

# 程式碼：

```
class Solution:
    def combinationSum2(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()

        res = []
        def dfs(i, cur, total):
            if total == 0:
                res.append(cur.copy())
                return
            if  i >= len(nums) or total < 0:
                return

            cur.append(nums[i])
            dfs(i + 1, cur, total - nums[i])

            while i + 1 < len(nums) and nums[i] == nums[i+1]:
                i += 1
            
            cur.pop()
            dfs(i + 1, cur, total)
        
        dfs(0, [], target)
        return res

```


