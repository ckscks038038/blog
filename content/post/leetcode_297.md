---
title: "297. Serialize and Deserialize Binary Tree"
date: 2023-05-17T00:38:17+08:00
draft: false
author: 'Yang'
tags: ['tree']
categories: ['leetcode']
---
## 前言：
這題用DFS或BFS來做serialize跟deserialize，但因為DFS比較容易想，code也比較少，就選擇用DFS來寫。

---

## 題目：

**Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.**

**Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.**

**Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.**

有夠長，其實就是叫你找個方法把一個tree編碼，再找個方法解碼。
```
* Example 1:
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
* Example 2:
Input: root = []
Output: []

* Example 3:

```

---

## 解題思路：
可以分成serialize跟deserialize兩部份，但因為serialize只是對tree跑一次preorder比較trivial，所以只討論deserialize


### 1. 第一個值一定是root node
恩對，因為preorder的順序就是先process `root node`。此時先建立一個node，接著找出它的左子樹與右子樹。

```
node = TreeNode(int(vals[self.i]))
self.i += 1
```
### 2. 找左子樹與右子樹
因為剛剛找到跟節點後會更新list中目前指向的index `i`，因此找出`node.left`跑dfs的時候，`i`此時會指向根節點的左子節點，並從這個節點作為新的根節點繼續重複找出其左右子樹；等到剛剛的左子節點(或說左子樹)找完以後，`i`也已經指向根節點的右子節點了
```
node.left = dfs()
node.right = dfs()
```

### 3. Base case
當遇到N時，就回傳空節點`None`
```
if vals[self.i] == "N":
	self.i += 1
	return None
```


---

# 程式碼：

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        res = []
        
        def dfs(node):
            if not node:
                res.append("N")
                return
            
            res.append(str(node.val))
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return ",".join(res)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        vals = data.split(",")
        self.i = 0

        def dfs():
            if vals[self.i] == "N":
                 self.i += 1
                 return None
            node = TreeNode(int(vals[self.i]))
            self.i += 1
            node.left = dfs()
            node.right = dfs()
            
            return node

        return dfs()
# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```

