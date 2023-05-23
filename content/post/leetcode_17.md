---
title: "17. Letter Combinations of a Phone Number"
date: 2023-05-24T00:19:06+08:00
draft: false
author: 'Yang'
tags: ['tree', 'backtracking']
categories: ['leetcode']
---
## 前言：
這題應該是backtracking寫到現在最簡單的一題，不需要考慮`pop()`，只管把所有可能列出來即可。

---

## 題目：

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
  
{{< figure src="/img/leetcode_17example.png" width="550px" >}}

---

## 解題思路：
{{< figure src="/img/leetcode_17pic_1.png" width="350px" >}}
看著圖片，把所有可能按照給的digits依序`append()`到`curStr`即可

唯一需要注意的是，如果遇到空字串`digits = ""`，需要return 的是空list`[]`，但進入迴圈的話會在一開始就append一個`""`

為了避免此問題要先判斷`digits`是否為空字串


---

# 程式碼：

```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        res = []
        map = {
            "2":"abc", 
            "3":"def",
            "4":"ghi",
            "5":"jkl",
            "6":"mno",
            "7":"pqrs",
            "8":"tuv",
            "9":"wxyz",
            }
        
        def dfs(curStr, i):
            if i >= len(digits):
                res.append(curStr)
                return

            for c in map[digits[i]]:
                dfs(curStr + c, i+1)
        if len(digits):
            dfs("", 0)

        return res
```


