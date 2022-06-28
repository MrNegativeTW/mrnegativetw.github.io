---
title: Leetcode Biweekly Contest 81
date: 2022-06-25 10:00:00
tags:
categories:
- Leetcode
---

Graph 題我直接當場去世。

<!-- more -->

# 2315. Count Asterisks

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-81/problems/count-asterisks/)

You are given a string `s`, where every two consecutive vertical bars `'|'` are grouped into a pair. In other words, the 1st and 2nd `'|'` make a pair, the 3rd and 4th `'|'` make a pair, and so forth.

Return the number of `'*'` in `s`, excluding the `'*'` between each pair of `'|'`.

Note that each `'|'` will belong to exactly one pair.

## 題目解釋

以 `|` 為分隔下去 split，算出奇數 index 的  `*` 有幾個

## Solution

不知道算不算暴力。先用 `split()` 依照 `|` 分開字串，接著用 `count()` 計算奇數 index 中的 `*`，
加總後就是答案了。

```python
class Solution:
    def countAsterisks(self, s: str) -> int:
        s_arr = s.split("|")
        n = len(s_arr)
        result = 0
        for i in range(0, n, 2):
            result = result + s_arr[i].count("*")
            
        return result
```

來看另一種解法，跑過一次的同時紀錄 `|` 狀況，最後也是加總就答案。

```python
def countAsterisks(self, s: str) -> int:
    bar = False
    count = 0
    for x in s:
        if x=='|':
            bar = not bar
                
        if x=='*' and bar==False:
            count+=1
            
    return count
```