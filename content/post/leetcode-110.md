---
title: 'Balanced Binary Tree - Leetcode 110'
date: 2023-04-10T00:39:24+08:00
draft: false
tags: ['binary tree', 'tree', 'easy']
categories: ['leetcode']
---

## 前言：

這題被歸類為 easy，但目前一刷完體感上，並沒有很直覺地理解遞迴，要再多 trace 才可以體會 dfs 在這的含意(也可能我還沒有透徹搞懂 dfs)

---

## 題目：

給一個 binary tree，回傳該樹是否 balanced

---

## 解題思路：

### 1. 暴力解：

從 root node 開始，算出其左子樹與右子樹的長度，比較兩者長度相減的絕對值是否有`<=1`，如果有則對其左子樹與右子樹重複此動作直接走完所有 node，否則回傳 False

--- **此法可解(參考[賈考博](https://www.youtube.com/watch?v=7vRTOS2SMuk))，但因為每次都需要重複走過所有子節點，所以直接複雜度是 O(n^2)**。

### 2. 最優解：

暴力解的思路是由上往下，才會重複 traverse 位於下方的子節點們，那麼這次就從下往上 traverse，這樣每個子節點只需要經過一次，並且往上傳遞兩個資訊:

- 是否 balanced
- 高度

對每個子節點而言，都可以把自己視為當前的 root node，然後來檢查自己是否 balanced，以及自己的高度是多少。假設身為 left node，它的高度與 right node 的高度符合 balanced tree 的要求： `abs(height[left] - height[right]) <= 1`，也保證整棵樹是平衡的，因為 left node 或是 right node 自己可能就是 unbalanced tree(樹高跟是否 balanced 無關唷)。

因此，對於當前的 root node 而言，除了要檢查左右子樹的樹高是否平衡，也要檢查兩者自己本身是否是平衡的。

---

### 程式碼：

#### 暴力解：

```
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        if root is None:
            return True

        leftHeight = self.helper(root.left)
        rightHeight = self.helper(root.right)

        if abs(leftHeight - rightHeight) > 1:
            return False

        return self.isBalanced(root.left) and self.isBalanced(root.right)

    def helper(self, root: TreeNode) -> int:
        if root is None:
            return 0

        return 1 + max(self.helper(root.left), self.helper(root.right))
```

#### 最優解：

```
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:

        def dfs(root):
            if not root: return [True, 0]

            left, right = dfs(root.left), dfs(root.right)
            balance = abs(left[1] - right[1]) <= 1 and left[0] and right[0]

            return [balance, 1 + max(left[1], right[1])]

        return dfs(root)[0]

```
