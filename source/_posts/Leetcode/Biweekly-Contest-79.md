---
title: Leetcode Biweekly Contest 79
date: 2022-05-28 10:00:00
tags:
categories:
- Leetcode
---

這禮拜的 Weekly and Biweekly Contest 合計 8 題解出 1 題，超爛，但這次也發現如果 contest 有贊助商就會比較難QQ

<!-- more -->

# 2283. Check if Number Has Equal Digit Count and Digit Value

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-79/problems/check-if-number-has-equal-digit-count-and-digit-value/)

You are given a 0-indexed string num of length n consisting of digits.

Return true if for every index i in the range 0 <= i < n, the digit i occurs num[i] times in num, otherwise return false.

```
Input: num = "1210"
Output: true
Explanation:
num[0] = '1'. The digit 0 occurs once in num.
num[1] = '2'. The digit 1 occurs twice in num.
num[2] = '1'. The digit 2 occurs once in num.
num[3] = '0'. The digit 3 occurs zero times in num.
The condition holds true for every index in "1210", so return true.
```
```
Input: num = "030"
Output: false
Explanation:
num[0] = '0'. The digit 0 should occur zero times, but actually occurs twice in num.
num[1] = '3'. The digit 1 should occur three times, but actually occurs zero times in num.
num[2] = '0'. The digit 2 occurs zero times in num.
The indices 0 and 1 both violate the condition, so return false.
```

## 題目解釋

給你一串字，第 i 個字是否在字串中出現過 num[i] 次

## Solution

oh yes brute force，其實可用 dict 儲存算過的 key

```python
class Solution:
    def digitCount(self, num: str) -> bool:
        for key, value in enumerate(num):
            if int(value) != num.count(str(key)):
                return False
        return True
```