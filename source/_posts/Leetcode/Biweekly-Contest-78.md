---
title: Leetcode Biweekly Contest 78
date: 2022-05-14 10:00:00
tags:
categories:
- Leetcode
---

這次兩題大概花 25 分鐘，而且都是第一次 submit 就吃 Accept，WTF

題外話，今天換到了 Windows 上做題目，各種鍵盤快捷鍵都超不順手，以後乖乖用 Mac 做題目比較實在。

<!-- more -->

# 2269. Find the K-Beauty of a Number

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-78/problems/find-the-k-beauty-of-a-number/)

The **k-beauty** of an integer `num` is defined as the number of **substrings** of `num` when it is read as a string that meet the following conditions:

- It has a length of `k`.
- It is a divisor of `num`.

Given integers `num` and `k`, return the k-beauty of `num`.

```
Input: num = 240, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
- "24" from "240": 24 is a divisor of 240.
- "40" from "240": 40 is a divisor of 240.
Therefore, the k-beauty is 2.
```
```
Input: num = 430043, k = 2
Output: 2
Explanation: The following are the substrings of num of length k:
- "43" from "430043": 43 is a divisor of 430043.
- "30" from "430043": 30 is not a divisor of 430043.
- "00" from "430043": 0 is not a divisor of 430043.
- "04" from "430043": 4 is not a divisor of 430043.
- "43" from "430043": 43 is a divisor of 430043.
Therefore, the k-beauty is 2.
```
- `1 <= num <= 109`
- `1 <= k <= num.length` (taking `num` as a string)


## 題目解釋

從 `num` 的第一位開始，每次往後拿 `k` 個數，若拿到的數當 **除數** 時可以把 `num` 除盡就是 k-beauty。

## Solution

oh yes brute force

1. 每次拿 k 個數，依序往後拿
2. 拿出來的數轉 int 後去做比較，符合條件 count ++

```python
class Solution:
    def divisorSubstrings(self, num: int, k: int) -> int:
        n = len(str(num))
        num_str = str(num)
        count = 0
        start = 0
        end = k
        
        while end <= n:
            k_beauty = num_str[start:end]
            if int(k_beauty) != 0 and num % int(k_beauty) == 0:
                count = count + 1
            start = start + 1
            end = end + 1
            
        return count
```

# 2270. Number of Ways to Split Array

Difficulty: `Medium`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-78/problems/number-of-ways-to-split-array/)

You are given a **0-indexed** integer array `nums` of length `n`.

`nums` contains a **valid split** at index `i` if the following are true:

- The sum of the first `i + 1` elements is greater than or equal to the sum of the last `n - i - 1` elements.
- There is **at least one element** to the right of `i`. That is, `0 <= i < n - 1`.
Return the number of **valid splits** in `nums`.

```
Input: nums = [10,4,-8,7]
Output: 2
Explanation: 
There are three ways of splitting nums into two non-empty parts:
- Split nums at index 0. Then, the first part is [10], and its sum is 10. The second part is [4,-8,7], and its sum is 3. Since 10 >= 3, i = 0 is a valid split.
- Split nums at index 1. Then, the first part is [10,4], and its sum is 14. The second part is [-8,7], and its sum is -1. Since 14 >= -1, i = 1 is a valid split.
- Split nums at index 2. Then, the first part is [10,4,-8], and its sum is 6. The second part is [7], and its sum is 7. Since 6 < 7, i = 2 is not a valid split.
Thus, the number of valid splits in nums is 2.
```
```
Input: nums = [2,3,1,0]
Output: 2
Explanation: 
There are two valid splits in nums:
- Split nums at index 1. Then, the first part is [2,3], and its sum is 5. The second part is [1,0], and its sum is 1. Since 5 >= 1, i = 1 is a valid split. 
- Split nums at index 2. Then, the first part is [2,3,1], and its sum is 6. The second part is [0], and its sum is 0. Since 6 >= 0, i = 2 is a valid split.
```

- `2 <= nums.length <= 10^5`
- `-10^5 <= nums[i] <= 10^5`

## 題目解釋

看到題目覺得有點眼熟嗎？沒錯，上周有一題也很類似：
[2256. Minimum Average Difference](https://leetcode.com/contest/biweekly-contest-77/problems/minimum-average-difference/)

1. 前面 `i + 1` 的總和 與 後面 `n - i - 1` 的總和
2. 如果前面的總和大於後面的，valid splits ++

## Solution

有了上次吃 Lime Limit Exceeded 的經驗這次就不會傻傻的每次 for loop 都 sum 一次。

1. 首先把兩邊的總和用工人智慧定義出來
2. 接著每次 for loop 對雙邊總和做加法與減法，仔細看看題目邊界會發現數字大小達到 `正負10^5`，list 長度也達到 `10^5`，若使用 `sum()` 一定吃 LTE
3. 最後比較，只要左邊大就 count++

```python
class Solution:
    def waysToSplitArray(self, nums: List[int]) -> int:
        n = len(nums)
        count = 0
        left = 0
        right = sum(nums)
        
        for j in range(n - 1):
            i = nums[j]
            left = left + i
            right = right - i

            if left >= right:
                count = count + 1
                        
        return count
```