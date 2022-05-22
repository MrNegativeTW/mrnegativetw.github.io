---
title: Leetcode Weekly Contest 294
date: 2022-05-22 10:00:00
tags:
categories:
- Leetcode
---

今天是第一次在內湖租屋處寫 Leetcode Contest，其他就沒啥特別的，就是覺得兩題 medium 著重在不同 list 排序同時保持相對位置。

<!-- more -->

# 2278. Percentage of Letter in String

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-294/problems/percentage-of-letter-in-string/)

Given a string `s` and a character `letter`, return the **percentage** of characters in `s` that equal `letter` rounded down to the nearest whole percent.

```
Input: s = "foobar", letter = "o"
Output: 33
Explanation:
The percentage of characters in s that equal the letter 'o' is 2 / 6 * 100% = 33% when rounded down, so we return 33.
```
```
Input: s = "jjjj", letter = "k"
Output: 0
Explanation:
The percentage of characters in s that equal the letter 'k' is 0%, so we return 0.
```

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters.
- `letter` is a lowercase English letter.

## 題目解釋

特定字元在字串出現次數的比例。

## Solution

直接對 string 進行 count，取得出現次數，接著直接算比例。

```python
class Solution:
    def percentageLetter(self, s: str, letter: str) -> int:
        n = len(s)
        count = 0
        
        count = s.count(letter)
        
        if count == 0:
            return 0
        else:
            return floor(100 * (float(count) / float(n)))
```

# 2279. Maximum Bags With Full Capacity of Rocks

Difficulty: `Medium`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-294/problems/maximum-bags-with-full-capacity-of-rocks/)

You have `n` bags numbered from `0` to `n - 1`. You are given two 0-indexed integer arrays `capacity` and `rocks`. The ith bag can hold a maximum of `capacity[i]` rocks and currently contains `rocks[i]` rocks. You are also given an integer `additionalRocks`, the number of additional rocks you can place in any of the bags.

Return the maximum number of bags that could have full capacity after placing the additional rocks in some bags.

- `n == capacity.length == rocks.length`
- `1 <= n <= 5 * 104`
- `1 <= capacity[i] <= 109`
- `0 <= rocks[i] <= capacity[i]`
- `1 <= additionalRocks <= 109`

## 題目解釋



## Solution

這次寫完忘記想 edge case 直接吃 Wrong Answer，仔細看了才發現，「快滿的袋子也有可能在 list 中間」，因此不能從頭找起。

1. 首先把袋子的空容量算出來，並用新 list 紀錄起來
2. 用新 list 當 key 把全部的 list 排序，讓最少空間袋子的排在最前，最多空間的袋子排在最後。
3. 進入比較環節，若袋子空容量為 0，表示為滿袋， `full_bag` 加 1；
    若袋子容量為 1 或以上，表示仍未滿袋，就將這個容量記錄起來，並與「多餘的 rock 總數比較」，
    若累計的空容量小於等於「多餘的石頭總數」，表示此袋子裝滿但石頭還有剩，`full_bag` 加 1，若累計的空容量大於「多餘的石頭總數」，表示此袋子裝完了石頭，但袋子還有容量。


```python
class Solution:
    def maximumBags(self, capacity: List[int], rocks: List[int], additionalRocks: int) -> int:
        n = len(capacity)
        full_bag = 0
        empty_count = 0
        
        # Calculate difference between capacity and rocks
        diff_list = []
        for i in range(n):
            diff_list.append(capacity[i] - rocks[i])
        
        # Sort three list at once, use diff_list as key.
        diff_list, capacity, rocks = zip(*sorted(zip(diff_list, capacity, rocks)))
        
        for i in range(n):
            if diff_list[i] == 0:
                full_bag = full_bag + 1
            else:
                empty_count = empty_count + diff_list[i]
                if empty_count <= additionalRocks:
                    full_bag  = full_bag + 1
        
        return full_bag
```

```
Edge case:
[91,54,63,99,24,45,78]
[35,32,45,98,6,1,25]
17
Expected: 1, output: 0
```