---
title: Leetcode Weekly Contest 290
date: 2022-04-24 10:00:00
tags:
categories:
- Leetcode
---

這場開始後 25 分鐘我才注意到，然後剛寫完一題又被拉出去買東西，所以就把這場 Contest 當作平常刷題吧。

<!-- more -->

# 2248. Intersection of Multiple Arrays

Difficulty: `Easy`

Given a 2D integer array `nums` where `nums[i]` is a non-empty array of distinct positive integers, return the list of integers that are present in each array of `nums` sorted in ascending order.

```
Input: nums = [[3,1,2,4,5],[1,2,3,4],[3,4,5,6]]
Output: [3,4]
Explanation: 
The only integers present in each of nums[0] = [3,1,2,4,5], nums[1] = [1,2,3,4], and nums[2] = [3,4,5,6] are 3 and 4, so we return [3,4].
```
```
Input: nums = [[1,2,3],[4,5,6]]
Output: []
Explanation: 
There does not exist any integer present both in nums[0] and nums[1], so we return an empty list [].
```

- `1 <= nums.length <= 1000`
- `1 <= sum(nums[i].length) <= 1000`
- `1 <= nums[i][j] <= 1000`
- All the values of `nums[i]` are **unique**.

## 題目解釋

求出所有 nums[i] 的交集，沒錯，就是 Easy。

## 我的解法

第一直覺就是用交集，但還真的沒遇過兩個 `list` 以上的交集，所以爬了一下文發現可以用 `map` 將 `nums[i]` 一口氣轉為 set，接著使用 `*` 俗稱 Unpacking Operators，將所有 `set` 放出來進行交集比對，最後排序一下就可以回傳了。

```python
class Solution:
    def intersection(self, nums: List[List[int]]) -> List[int]:
        result = set.intersection(*map(set, nums))
        result = list(result)
        result.sort()
        return result
```

# 2250. Count Number of Rectangles Containing Each Point

Difficulty: `Medium`

詳細題目請前往 [Leetcode](https://leetcode.com/contest/weekly-contest-290/problems/count-number-of-rectangles-containing-each-point/) 上觀看

## 題目解釋

給你很多很多長方形 `rectangles`，再給你很多 `points`，找出每一個 point 在有被包含在多少個 rectangles 中。
換句話說，算出每一個 `point` 有小於等於多少個 `rectangle`。

## 我的解法

全部跑過一遍的暴力解這次碰壁了，直接吃 Time Limit Exceeded。

```python
# Time Limit Exceeded
class Solution:
    def countRectangles(self, rectangles: List[List[int]], points: List[List[int]]) -> List[int]:
        result = []
        for i in points:
            count = 0
            for j in rectangles:
                if j[0] >= i[0] and j[1] >= i[1]:
                    count += 1
            result.append(count)
                
        return result
```

事後上 Discuss 看大神怎麼解，發現都是用 Binary Search 處理。

[
✅ [Java/ C++/ Python] Detailed Explanation, fully commented  Binary Search l among points of h](https://leetcode.com/problems/count-number-of-rectangles-containing-each-point/discuss/1976969/Java-C%2B%2B-Python-Detailed-Explanation-fully-commented-Binary-Search-l-among-points-of-h)
