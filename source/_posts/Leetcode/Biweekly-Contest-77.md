---
title: Leetcode Biweekly Contest 77
date: 2022-04-30 10:00:00
tags:
categories:
- Leetcode
---

這次的 Contest 對應 GMT+8 剛好是 22:30 ~ 24:00，但這天...我沒事，終於可以好好寫了。至於心態崩掉後看別人解法發現自己思路正確又是另一回事了。

<!-- more -->

# 2255. Count Prefixes of a Given String

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-77/problems/count-prefixes-of-a-given-string/)

You are given a string array `words` and a string `s`, where `words[i]` and `s` comprise only of lowercase English letters.

Return the number of strings in `words` that are a prefix of `s`.

A prefix of a string is a substring that occurs at the beginning of the string. A substring is a contiguous sequence of characters within a string.

```
Input: words = ["a","b","c","ab","bc","abc"], s = "abc"
Output: 3
Explanation:
The strings in words which are a prefix of s = "abc" are:
"a", "ab", and "abc".
Thus the number of strings in words which are a prefix of s is 3.
```
```
Input: words = ["a","a"], s = "aa"
Output: 2
Explanation:
Both of the strings are a prefix of s. 
Note that the same string can occur multiple times in words, and it should be counted each time.
```

- `1 <= words.length <= 1000`
- `1 <= words[i].length, s.length <= 10`
- `words[i]` and `s` consist of lowercase English letters only.

## 題目解釋

把 `s` 拆開從只有一個字慢慢加上去，例如：s = "fool"，那就會是 "f", "fo", "foo" 和 "fool"。接著看看 `words` 這個 list 中有幾個符合的 string。

## Solution

1. oh yes brute force
2. for loop 把 `s` 從第一個字到最後一次都取出來
3. for loop 跑過 `words` list 和取出的 `s` 比對，若一樣的話計數器加一

```python
class Solution:
    def countPrefixes(self, words: List[str], s: str) -> int:
        count = 0
        for i in range(len(s)):
            a = s[0:i + 1]
            for j in words:
                if a == j:
                    count += 1
        return count
```


# 2256. Minimum Average Difference

Difficulty: `Medium`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-77/problems/minimum-average-difference/)

You are given a 0-indexed integer array `nums` of length `n`.

The average difference of the index `i` is the absolute difference between the average of the first `i + 1` elements of `nums` and the average of the last `n - i - 1` elements. Both averages should be rounded down to the nearest integer.

Return the index with the minimum average difference. If there are multiple such indices, return the smallest one.

```
Input: nums = [2,5,3,9,5,3]
Output: 3
Explanation:
- The average difference of index 0 is: |2 / 1 - (5 + 3 + 9 + 5 + 3) / 5| = |2 / 1 - 25 / 5| = |2 - 5| = 3.
- The average difference of index 1 is: |(2 + 5) / 2 - (3 + 9 + 5 + 3) / 4| = |7 / 2 - 20 / 4| = |3 - 5| = 2.
- The average difference of index 2 is: |(2 + 5 + 3) / 3 - (9 + 5 + 3) / 3| = |10 / 3 - 17 / 3| = |3 - 5| = 2.
- The average difference of index 3 is: |(2 + 5 + 3 + 9) / 4 - (5 + 3) / 2| = |19 / 4 - 8 / 2| = |4 - 4| = 0.
- The average difference of index 4 is: |(2 + 5 + 3 + 9 + 5) / 5 - 3 / 1| = |24 / 5 - 3 / 1| = |4 - 3| = 1.
- The average difference of index 5 is: |(2 + 5 + 3 + 9 + 5 + 3) / 6 - 0| = |27 / 6 - 0| = |4 - 0| = 4.
The average difference of index 3 is the minimum average difference so return 3.
```
```
Input: nums = [0]
Output: 0
Explanation:
The only index is 0 so return 0.
The average difference of index 0 is: |0 / 1 - 0| = |0 - 0| = 0.
```

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 105`

## 題目解釋

1. 前面 `i + 1` 個數的平均減掉後面 `n - i - 1` 個數的平均
2. 算出來的平均取 floor 後相減，並取絕對值
3. return 算出來絕對值最小的那個 index，aka 第幾次迴圈。

## Solution

1. oh yes brute force with time limit exceeded

```python
import math
class Solution:
    def minimumAverageDifference(self, nums: List[int]) -> int:
        min_diff_index = 0
        min_diff_value = float("inf")
        avg_diff = []
        n = len(nums)
        for i in range(n):
            left_count = i + 1
            right_count = n - i - 1
            
            avg_diff_left = sum(nums[:i + 1]) / left_count
            
            if right_count != 0:
                avg_diff_right = sum(nums[i + 1:]) / right_count
            else:
                avg_diff_right = 0
                
            current_avg_diff = abs(math.floor(avg_diff_left) - math.floor(avg_diff_right))
            avg_diff.append(current_avg_diff)
        return avg_diff.index(min(avg_diff))
```

2.  Brute force 吃到 Time Limit Exceeded 不意外，仔細想了想發現在每次 for loop 時都會重新把數字加過一次，但這步驟其實很不必要，我們可以將「上次的數」加上/減掉「下一個 `nums[i]`」，這樣可以省下很多重複運算的時間。

But，人生就是這個 but，我寫完暴力後吃 TLE，然後又去看第三題，但又放不下第二題，想完上述說的優化後，心態有點崩掉直接躺下去滑手機，直到結束看 Discuss 才知道，「幹，原來我的思路是正確的，只是沒有實作」。乾。

最終 Accepted 的 code 長這樣：

```python
import math
class Solution:
    def minimumAverageDifference(self, nums: List[int]) -> int:
        min_diff_index = 0
        min_diff_value = float("inf")
        
        sum_start = 0
        sum_end = sum(nums)
        n = len(nums)
        for i in range(n):
            left_count = i + 1
            right_count = n - i - 1
            
            sum_start += nums[i]
            sum_end -= nums[i]
            
            avg_diff_left = sum_start / left_count
            
            if right_count != 0:
                avg_diff_right = sum_end / right_count
            else:
                avg_diff_right = 0
                
            current_avg_diff = abs(math.floor(avg_diff_left) - math.floor(avg_diff_right))
            if current_avg_diff < min_diff_value:
                min_diff_index = i
                min_diff_value = current_avg_diff
        
        return min_diff_index
```