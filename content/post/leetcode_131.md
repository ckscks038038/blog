---
title: "131. Palindrome Partitioning"
date: 2023-05-22T23:40:03+08:00
draft: false
author: 'Yang'
tags: ['tree','backtracking']
categories: ['leetcode']
---
## 前言：

一開始看很難，但其實是很典型的backtracking套路，不要想一次把整個遞迴結構想出來，應該以單層的角度，把下面子問題抽象來看。

---

## 題目：

Given a string s, partition s such that every 
substring
 of the partition is a 
palindrome
. Return all possible palindrome partitioning of s.

```
* Example 1:
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]

* Example 2:
Input: s = "a"
Output: [["a"]]

```

---

## 解題思路：
{{< figure src="/img/leetcode_131_decision_tree.png" width="450px" >}} 
這題也是決策樹問題，把圖畫出來如上。

### 1. 第一部份
字串**aab**在進入for迴圈可以將整個字串`s`切成兩一部份。

第一部份又可以分為三種可能：
- **i: 0 -> 0**: `"a"`
- **i: 0 -> 1**: `"aa"`
- **i: 0 -> 2**: `"aab"`
  
接著判斷是否第一部份為回文，是的話進入第二部份，並且將子字串加入`part`; 不是的話則不繼續往下找

```
for j in range(i, len(s)):
	if self.isPali(s, i, j):
		part.append(s[i : j + 1])
```
### 2. 第二部份
如果第一部份是回文，例如子字串`"aa"`是回文，則會考慮剩下的第二部份子字串`"b"`是否為回文。

以下列程式碼來判斷第二部份子字串是否為回文:
```
dfs(j + 1)
```

最後幫剛剛加入`part`的回文子字串`pop()`，再交給下一次for的對象使用
---

# 程式碼：

```
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res, part = [], []

        def dfs(i):
            if i >= len(s):
                res.append(part.copy())
                return
            for j in range(i, len(s)):
                if self.isPali(s, i, j):
                    part.append(s[i : j + 1])
                    dfs(j + 1)
                    part.pop()

        dfs(0)
        return res

    def isPali(self, s, l, r):
        while l < r:
            if s[l] != s[r]:
                return False
            l, r = l + 1, r - 1
        return True

```


