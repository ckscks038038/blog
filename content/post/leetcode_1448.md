---
title: "1448. Count Good Nodes in Binary Tree"
date: 2023-05-16T19:33:00+08:00
draft: False
author: 'Yang'
tags: ['tree', 'DFS', 'Microsoft']
categories: ['leetcode']
---
## 前言：
這裡練習DFS的觀念。每次寫DFS都會有新的體會、或想起之前對它的見解，例如這次就想起之前有想到過DFS的遞迴關係其實就只是重複call DFS function，所以理解為每個`非null`節點都被當作參數call一次DFS

這次又另外了解到，在知道要用DFS來解決問題後，可以先把DFS基本雛型先寫出來(分成**process**跟**iterative call**兩部份)，再來改**process**確切要做什麼、以及**iterative call**的`return value`要如何處理。

---

## 題目：

**Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.**

**Return the number of good nodes in the binary tree.**


```
* Example 1:
Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.

* Example 2:
Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

* Example 3:
Input: root = [1]
Output: 1
Explanation: Root is considered as good.
```

---

## 解題思路：
題目定義是在**走到該節點時，沿路都沒有比他大的節點**的條件下，就`res += 1`
既然要是依照**沿路遇到的節點**作為參考，用DFS遍歷整棵樹就可以達成**沿路**的概念，並且把一路更新遇到的最大值，只要當前節點的value沒有比當前的最大值還小(也就是一路上中的最大值，`node >= maxVal`)，則該節點是一個**good node**


### 1. 先寫出DFS雛型

要注意DFS有分三種：
- in-order
- pre-order
- post-order

這題用in-order來解，才可以由根節點開始，由上往下(`pre-order`, `post-order`都是由下往上)

1. **process**的部份用來處理更新最大值以及判斷當前節點是否為good node

2. **interative call**則會回傳從子節點們的出發所找到的good node數量

### 2. process: 沿途更新最大值、該節點是否是good node?
沿途更新最大值很容易：
maxVal = max(maxVal, root.val)

判斷該節點是否是good node，參考neetcode乾淨的python寫法，用一行就可以搞定if else
只有遇到比自己的數值還大的node才使得自己不是good node，意即遇到**比自己小**、**跟自己一樣大**的節點都沒關係:
```
res = 1 if root.val >= maxVal else 0 
```
### 3. 即使當前節點不是good node，也不影響他的子節點
這題用DFS遍歷要走到底，因為即使當前節點不是good node，**後面還是可以找到good node**，只是當前節點不會更新最大值而已。


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
    def goodNodes(self, root: TreeNode) -> int:

        def dfs(root, maxVal):
            if not root:
                return 0

            # process
            maxVal = max(maxVal, root.val)
            res = 1 if root.val >= maxVal else 0    
            
            res += dfs(root.left, maxVal)
            res += dfs(root.right, maxVal)

            return res
        
        return dfs(root, root.val) 
    
```
