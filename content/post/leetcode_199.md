---
title: "199. Binary Tree Right Side View"
date: 2023-05-16T00:17:01+08:00
draft: false
author: 'Yang'
---
## 前言：
這題BFS觀念 `level order traveral`, 學習如何在python中使用deque來解決BFS問題，並且學習在traversal時考慮子節點有`null`的情況

---

## 題目：

**Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.**

```
* Example 1:
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]

* Example 2:
Input: root = [1,null,3]
Output: [1,3]

* Example 3:
Input: root = []
Output: []
```

---

## 解題思路：

如果把樹的每一層都當作一個一維陣列，從上往下、從左往右，每次把traverse到的新節點加入到一維陣列中，並找出一維陣列的最右邊元素(rightSide)，就可以得到結果。

### 1. 創建deque, 比list更適合

為什麼更適合呢？因為當使用list走完一個level時，需要把已經遍歷的元素pop掉(左邊)，同時前面新訪問到的子節點又需要append(右邊)，因此使用deque就可以用`popleft()`以及`append`達成兩個要求。

### 2. 考慮空子節點的情況
除非是完全二元樹(complete binary tree)，否則要考慮到子節點為空的情況避免error。在當前level中取出一個`node`時，要先判斷這個`node`是否為`null`，如果是空的話就不要往下找了；不是空的話，則可以將他的左右子樹append到deque當中。

並且，只有該node**有值**，才有可能是`rightSide` value(空的node沒有value得跳過嘛)。並且依據每個level的traverse都是**由左往右**的特性，最後的`rightSide`就是traverse該level最後一次遇到的*有值* node

另外需要注意有可能整個level都是空節點，這時候traverse完整個level都不會找到`rightSide` value，所以要加上一個判斷確保append的數值是合理的。



---

# 程式碼：

```
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        q = collections.deque([root])

        while q:
            rightSide = None
            qLen = len(q)

            # qLen will be updated during this for loop
            # node could be null(since a node could have null children)
            # use a if condition to decide if there's need to append children
            # also, only if there's node then we can decide "rightSide"
            for i in range(qLen):
                node = q.popleft()
                if node:
                    rightSide = node
                    q.append(node.left)
                    q.append(node.right)

            if rightSide:
                res.append(rightSide.val)
        return res
```

