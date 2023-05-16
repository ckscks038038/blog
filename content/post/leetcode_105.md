---
title: "105. Construct Binary Tree from Preorder and Inorder Traversal"
date: 2023-05-16T20:19:20+08:00
draft: False
author: 'Yang'
tags: ['tree']
categories: ['leetcode']
---
## 前言：
這題目考研寫過，但只要一步一步想就可以得到結果。這次用遞迴實作才發現其實很難。

---

## 題目：

**Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.**

```
* Example 1:
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]

* Example 2:
Input: preorder = [-1], inorder = [-1]
Output: [-1]

```

---

## 解題思路：
先備條件為：所有list中元素都是唯一的。

1. **Preorder list**中，第一個元素必定是root node，此時就可以先create一個node作為出發點，繼續生成他的左子樹與右子樹
2. 接著看到**Inorder list**，可以透過剛剛找到的root node來找出root node在Inorder中的位置(`index`)，而且這個index非常重要，它代表root node左子樹中所有node的總數量。所以如果`index=3`，就表示剛剛找到的root node的左子樹一共有三個節點。
3. 在得到`index`後，回到**Preorder**，從剛剛建立好的root node往右走**index**步，就是左子樹於**Preorder**中最後一個元素，`index+1`開始接著就都是右子樹的元素了
4. 而此時**Inorder**中，`index+1`開始也都是右子樹的元素

### 1. base case的條件
在跑遞迴切割出sub array時，如果剩下一個元素了(代表他就是leaf node)，則它的左右子樹都是null，此時切出來的sub array會是空的，要觸發base case

```
root.left = self.buildTree(preorder[1:mid + 1], inorder[:mid])
root.right = self.buildTree(preorder[mid + 1:], inorder[mid + 1:])
```


### 2. 



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
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder or not inorder:
            return None
        
        root = TreeNode(preorder[0])
        mid = inorder.index(preorder[0])
        root.left = self.buildTree(preorder[1:mid + 1], inorder[:mid])
        root.right = self.buildTree(preorder[mid + 1:], inorder[mid + 1:])
        return root
```

