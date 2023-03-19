---
title: 'Climbing the Leaderboard'
date: 2023-03-18T14:13:31+08:00
draft: false
tags: ['binary search', 'array']
categories: ['leetcode', 'HackerRank']
---

## 前言：

這題來自 **HackerRank** 的 **_1 Month Preparation Kit - Week 3_**，結合了 Leetocde `35. Search Insert Position`以及`26. Remove Duplicates from Sorted Array`兩題的解題套路。

---

## 題目：

![](/img/climbing_the_leaderboard.png)

> 題目提到的**Dense Ranking**指在一數組中，重複的數值會獲得一樣的名次 `n`，且下一個較小的數值會獲得`n+1`的名次。

---

## 解題思路：

以題目給的 input 為例：

```
ranked = [100, 90, 90, 80]
player = [70, 80, 105]
```

### 1. 排除重複分數

已知 數組 ranked 中重複的分數獲得的是相同名次，因為有重複分數的緣故，index 與排名無法找出關聯。例如兩個 90 (一個`index = 1`, 另一個`index = 2`)在不同的 index 上但排名卻一樣。

如果將數組濃縮為：

```
ranked = [100, 90, 80]
```

則各分數在 leaderboard 中的排名剛好等於其所在位置的`index+1`。例如`score = 90`對應`index = 1`，則排名為第二名。這樣一來後續要將 player 的分數加入數組時只需要根據當前`index`即可算出排名。

### 2. 找出適當插隊位置

> 想像今天如果要排隊入場遊樂園，且優先順序跟先來後到無關，而是看誰手上的號碼牌數字比較大，握有較大號碼的玩家可以插隊排在數字較小的玩家前面(好不公平阿)。

上述的情境就是我們現在要解決的問題：

--- **讓新來的玩家拿手上的分數去跟隊伍（ranked）中的分數比較，並找出適合的插隊位置**。

目前的 input 在濃縮了 ranked 以後變成：

```
ranked = [100, 90, 80]
player = [70, 80, 105]
```

例如，考慮第一位玩家 `player[0]`握有`score = 70`要加入 ranked 這個數組，最終他應該會被排在 80 後面：

```
ranked = [100, 90, 80, 70]
```

剛開始看到這樣的問題，最直觀的想法應該是用一個 for 迴圈遍歷數組 ranked，直到找出**第一個** `ranked[i] <= score` 的元素。

因為 ranked 為 descending order，所以**第一個`ranked[i] <= 70` 的元素**是**所有小於等於 score 之中最大的數字**，也就是我們要找的位置：

```
for i in range(len(ranked)):
	if ranked[i] <= 70:
		return i
```

但再仔細想想會發現，如果只是為了找到 `ranked[i] <= score` 之中最大的數字，檢查整個數組其實有點太多餘，因為跟 score 相比起來太大或太小的數字都不需要被考慮才對。這時候就可以用 binary search 來省略多餘的步驟：

```
l, r = 0, len_ranked - 1

while l < r:
	mid = (l + r) // 2

	if score >= ranked[mid]:
		r = mid
	else:
		l = mid + 1

if r == len_ranked - 1 and score < ranked[r]:
	res.append(r + 2)
else:
	res.append(r + 1)
```

寫到這邊其實問題差不多解決了。最後需要注意的是，如果玩家手上握有的分數比整個數組中的所有分數都還小，那此玩家就會被排在數組最後一個元素的後一位;且已知數組最後一個元素的排名為`index + 1`，則此玩家的排名會是`index + 2`，例如 `score = 50`要加入 `ranked = [100, 90, 80, 70]` 中，會被排在最後一個數字 70 的後面，變成`ranked = [100, 90, 80, 70, 50]`。

而其他情況下排名就會是`index + 1`，例如 `score = 80`，或是`score = 85`要加入 `ranked = [100, 90, 80, 70]` 中，找到的第一個 `ranked[i] <= score` 的數字是 80，則該 score 可以被排在原本的 80 這個位置上，變成`ranked = [100, 90, 85(80), 80, 70]`。

---

# 程式碼：

```
def climbingLeaderboard(ranked, player):
    # remove duplicates from ranked
    left = 1
    for i in range(1, len(ranked)):
        if ranked[i - 1] != ranked[i]:
            ranked[left] = ranked[i]
            left += 1
    len_ranked = left

    # insert each score in the player list indenpently
    res = []

    for score in player:
        l, r = 0, len_ranked -  1

        while l < r:
            mid = (l + r) // 2

            if score >= ranked[mid]:
                r = mid
            else:
                l = mid + 1

        if r == len_ranked - 1 and score < ranked[r]:
            res.append(r + 2)
        else:
            res.append(r + 1)

    return res
```
