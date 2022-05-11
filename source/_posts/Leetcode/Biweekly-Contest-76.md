---
title: Leetcode Biweekly Contest 76
date: 2022-04-17 10:00:00
tags:
categories:
- Leetcode
---

這次的 Contest 對應 GMT+8 剛好是 22:30 ~ 24:00，但這天我到家時已經 23:00 多了，而且還沒洗澡，為了不影響成績那些的還是寫了一題。

<!-- more -->

# 2239. Find Closest Number to Zero

Difficulty: `Easy`

Given an integer array `nums` of size `n`, return the number with the value closest to `0` in `nums`. If there are multiple answers, return the number with the largest value.

```
Input: nums = [-4,-2,1,4,8]
Output: 1
Explanation:
The distance from -4 to 0 is |-4| = 4.
The distance from -2 to 0 is |-2| = 2.
The distance from 1 to 0 is |1| = 1.
The distance from 4 to 0 is |4| = 4.
The distance from 8 to 0 is |8| = 8.
Thus, the closest number to 0 in the array is 1.
```
```
Input: nums = [2,-1,1]
Output: 1
Explanation: 1 and -1 are both the closest numbers to 0, so 1 being larger is returned.
```

- `1 <= n <= 1000`
- `-105 <= nums[i] <= 105`

## Solution

1. 正負都要找最靠近 0 的，所以要一個 `正無限大` 與 `負無限大`
2. 接著把 list 走過一遍：
    若為正數，將該數和 `已儲存的最小正數` 比較，並存下小的；
    若為負數，將該數和 `已儲存的最大負數` 比較，並存下大的；
    若為 0，直接回傳 0，我相信沒有一個數可以比 0 更靠近 0。
3. 將 `最靠近 0 的正數` 與 `最靠近 0 的負數的絕對值` 比較：
    若 `最靠近 0 的正數` 比較大， 回傳 `最靠近 0 的負數的絕對值`；
    若 `最靠近 0 的負數` 大於等於， 回傳 `最靠近 0 的正數`；
4. Edge case: 正負數絕對值後相等，題目說此時要回傳較大的數，也就是正數，所以使用 `<=`。

```python
class Solution:
    def findClosestNumber(self, nums: List[int]) -> int:
        closest_positive = float("inf")
        closest_negative = float("-inf")
        for i in nums:
            if i > 0:
                closest_positive = min(closest_positive, i)
            elif i < 0:
                closest_negative = max(closest_negative, i)
            else:
                return 0
            
        if closest_positive > abs(closest_negative):
            return closest_negative
        elif closest_positive <= abs(closest_negative):
            return closest_positive
```