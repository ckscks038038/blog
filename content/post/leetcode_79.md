---
title: "79. Word Search"
date: 2023-05-23T22:44:27+08:00
draft: false
author: 'Yang'
tags: ['tree', 'backtracking']
categories: ['leetcode']
---
## 前言：
這題也是經典`backtracking`套路，可以分層理解。假設大目標是`ABCD`，那找到A以後，目標變成找`BCD`，一直延續下去。

---

## 題目：

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

```
* Example 1: 
{{< figure src="/img/leetcode_79example_1.png" width="300px" >}}
 Input: board = [["A","B","C","E"],
["S","F","C","S"],
["A","D","E","E"]],
word = "ABCCED"
Output: true

* Example 2:
{{< figure src="/img/leetcode_79example2.png" width="300px" >}}
Input: board = 
[["A","B","C","E"],
["S","F","C","S"],
["A","D","E","E"]], 
word = "SEE"
Output: true

* Example 3:

```

---

## 解題思路：
兩個核心概念，邊界條件以及bfs機制。

### 1. 考慮邊界條件
如果碰到邊界、找到不是當前`word[i]`的值或是已經重複走過的路，都表示這個path是不通的那就return false
```
if (r < 0 or c < 0 or
	r >= ROWS or c >= COLS or
	word[i] != board[r][c] or
	(r, c) in path):
	return False
```


### 2. 用dfs找路徑
看起來比之前複雜，但其實差異只有
- 從二元決策樹變成四元決策樹
- 需要return 該路徑的成功或失敗

還記得之前寫**78. Subset**時，在跑dfs是不需要return失敗或成功的，因為只需要把所有可能都列到底，然後全部的可能跑完(`i == len(nums)`)，就結束了。

但這次需要判斷input最後的結果是True or False, 所以需要隨時遇到**邊界條件**就先回傳False
```
path.add((r, c))
res = (dfs(r + 1, c, i + 1) or
	dfs(r - 1, c, i + 1) or
	dfs(r, c + 1, i + 1) or
	dfs(r, c - 1, i + 1))
path.remove((r, c))
return res
```


---

# 程式碼：

```
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        path = set()

        def dfs(r, c, i):
            if i == len(word):
                return True
            if (r < 0 or c < 0 or
                r >= ROWS or c >= COLS or
                word[i] != board[r][c] or
                (r, c) in path):
                return False

            path.add((r, c))
            res = (dfs(r + 1, c, i + 1) or
                   dfs(r - 1, c, i + 1) or
                   dfs(r, c + 1, i + 1) or
                   dfs(r, c - 1, i + 1))
            path.remove((r, c))
            return res
        
        for r in range(ROWS):
            for c in range(COLS):
                if dfs(r, c, 0): return True
        return False

```


