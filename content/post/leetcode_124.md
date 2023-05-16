---
title: "124. Binary Tree Maximum Path Sum"
date: 2023-05-17T00:06:44+08:00
draft: False
author: 'Yang'
tags: ['tree', 'DFS']
categories: ['leetcode']
---
## 前言：
這題雖然是hard，但看完山景城跟neetcode的解釋後便頓然開悟，比起前幾題，124反而在解題方向上更明確。
做完這題應該要去做leetcode_543，解題思路幾乎相同。

---

## 題目：

**A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.**

**The path sum of a path is the sum of the node's values in the path.**

**Given the root of a binary tree, return the maximum path sum of any non-empty path.**

```
* Example 1:
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

* Example 2:
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

```

---

## 解題思路：
假設所有node的value都是正數且value都一樣，則路徑越長越好。但因為tree特性受限於只能從root出發，所以要用DFS從leaf node開始找出區域最佳解，更新**左右子樹穿過自己連通後**的最佳解後，回傳**選擇其中一條路(或左右都是負則都不選)**的最佳解為何。


### 1. base case
如果遇到節點為null，則沒有value，**能回傳的最佳解是0**

### 2. 區域最佳解
把自己跟左右子樹連起來以後加總，跟當前全域變數中最佳解比較，看有沒有比較大，有的話就更新

### 3. 回給父節點不能回區域最佳解
但回傳時，只能選擇一邊(左or右子樹)與自己相加較大的值。因為站在父節點的角度，它已經會考慮左右子樹了，所以作為子節點，就不可以再同時考慮左右子樹，否則就不是一個`path`了



---

# 程式碼：

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        res = [root.val]

        # return max path sum without split
        def dfs(root):
            if not root:
                return 0
            
            leftMax = dfs(root.left)
            rightMax = dfs(root.right)
            leftMax = max(leftMax, 0)
            rightMax = max(rightMax, 0)

            currPathSum = root.val + leftMax + rightMax
            res[0] = max(res[0], currPathSum)
            
            return max(leftMax, rightMax) + root.val
        
        dfs(root)
        
        return res[0]
```

