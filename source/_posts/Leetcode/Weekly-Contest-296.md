---
title: Leetcode Weekly Contest 296
date: 2022-06-09 10:00:00
tags:
categories:
- Leetcode
---

原本 contest 296 的時間是 6/5 12:30，但我人在家樂福直接沒辦法寫，拖著拖著就到今天了。

<!-- more -->

# 2293. Min Max Game

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-296/problems/min-max-game/)

You are given a **0-indexed** integer array `nums` whose length is a power of `2`.

Apply the following algorithm on `nums`:

1. Let `n` be the length of `nums`. If `n == 1`, end the process. Otherwise, create a new 0-indexed integer array `newNums` of length `n / 2`.
2. For every **even** index `i` where `0 <= i < n / 2`, **assign** the value of `newNums[i]`as `min(nums[2 * i], nums[2 * i + 1])`.
3. For every **odd** index `i` where `0 <= i < n / 2`, **assign** the value of `newNums[i]` as `max(nums[2 * i], nums[2 * i + 1])`.
4. **Replace** the array `nums` with `newNums`.
5. **Repeat** the entire process starting from step 1.

Return the last number that remains in `nums` after applying the algorithm.

## 題目解釋



## Solution

照著題目打就過了，這題老實說蠻神奇的，至於時間複雜度：我就暴力！

```python
class Solution:
    def minMaxGame(self, nums: List[int]) -> int:
        while(len(nums) != 1):
            n = len(nums)
            half = n / 2
            if n == 1:
                return

            newNums = [0] * int(half)

            for i, v in enumerate(nums):
                if i % 2 == 0 and i < half:
                    newNums[i] = min(nums[2 * i], nums[2 * i + 1])
                elif i % 2 == 1 and i < half:
                    newNums[i] = max(nums[2 * i], nums[2 * i + 1])
            nums = newNums
                
        return nums[0]
```