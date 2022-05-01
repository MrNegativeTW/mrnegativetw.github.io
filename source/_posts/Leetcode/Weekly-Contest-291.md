---
title: Leetcode Weekly Contest 291
date: 2022-05-01 10:00:00
tags:
categories:
- Leetcode
---

繼昨天 Biweekly Contest 77 心態崩掉後，今天 Weekly Contest 30 分鐘解兩題，雖然比不上演算法大神，但我已經很開心了。

<!-- more -->

# 2259. Remove Digit From Number to Maximize Result

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-291/problems/remove-digit-from-number-to-maximize-result/)

You are given a string `number` representing a positive integer and a character `digit`.

Return the resulting string after removing exactly one occurrence of `digit` from `number` such that the value of the resulting string in decimal form is maximized. The test cases are generated such that `digit` occurs at least once in `number`.

```
Input: number = "123", digit = "3"
Output: "12"
Explanation: There is only one '3' in "123". After removing '3', the result is "12".
```
```
Input: number = "1231", digit = "1"
Output: "231"
Explanation: We can remove the first '1' to get "231" or remove the second '1' to get "123".
Since 231 > 123, we return "231".
```
```
Input: number = "551", digit = "5"
Output: "51"
Explanation: We can remove either the first or second '5' from "551".
Both result in the string "51".
```

- `2 <= number.length <= 100`
- `number` consists of digits from `'1'` to `'9'`.
- `digit` is a digit from `'1'` to `'9'`.
- `digit` occurs at least once in `number`.

## 題目解釋

一次只能從 `number` 字串中移除一個 `digit`，回傳移掉後最大的 number。

## Solution

這題在開始寫之前就有想到 edge case，但暫時想不出來 edge case 實體長怎樣，所以先 brute force 直接移除第一個出現的 digit 並吃個 Wrong Answer 看吐出來的 edge case 長怎樣：

```
Input: "133235", "3"
Output: "13235"
Expected: "13325"
```

接著就改一下，把全部出現的 digit 移除並加入 result，最後用 max() 把 resutl list 中最大值回傳。

```python
class Solution:
    def removeDigit(self, number: str, digit: str) -> str:
        result = []
        for i, v in enumerate(number):
            if v == digit:
                temp = number[:i] + number[i + 1:]
                result.append(temp)
        return max(result)
```

# 2260. Minimum Consecutive Cards to Pick Up

Difficulty: `Medium`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-291/problems/minimum-consecutive-cards-to-pick-up/)

You are given an integer array `cards` where `cards[i]` represents the value of the `ith` card. A pair of cards are matching if the cards have the same value.  

Return the minimum number of consecutive cards you have to pick up to have a pair of matching cards among the picked cards. If it is impossible to have matching cards, return `-1`.

```
Input: cards = [3,4,2,3,4,7]
Output: 4
Explanation: We can pick up the cards [3,4,2,3] which contain a matching pair of cards with value 3. Note that picking up the cards [4,2,3,4] is also optimal.
```
```
Input: cards = [1,0,5,3]
Output: -1
Explanation: There is no way to pick up a set of consecutive cards that contain a pair of matching cards.
```

- `1 <= cards.length <= 10^5`
- `0 <= cards[i] <= 10^6`

## 題目解釋

相同的兩個數之間共間隔幾個數（含相同的數本身）。
例如題目的 `[3,4,2,3,4,7]`，3 與 3 之間有 2 個數，加上相同數共為 4。

## Solution

一開始當然是暴力解，也當然馬上吃 Wrong Answer，測資長這樣：

```
# Input: [77,10,11,51,69,83,33,94,0,42,86,41,65,40,72,8,53,31,43,22,9,94,45,80,40,0,84,34,76,28,7,79,80,93,20,82,36,74,82,89,74,77,27,54,44,93,98,44,39,74,36,9,22,57,70,98,19,68,33,68,49,86,20,50,43]
# Output: 15
# Expected: 3
```

回頭細看題目發現，題目沒有限定一個數能出現幾次，拿題目的 Example 來做說明，`[3,4,2,3,4,7]` 把中間擴充更多 3 變成 `[3,4,2,3,7,3,3,4,7]`，注意到了嗎？最小間隔是兩個相鄰的 3，答案應為 2。

1. 使用 `enumerate()` 來列舉 cards，除了需要數值外還需要 index 來計算間隔。
2. 若 value 不在 dict 中，新增這個 value 的 index。
3. 若 value 在 dict 中，使用當前的 index 減這個 value 儲存的 index 並 + 1，算出來後存到 list 中。
    接著更新這個 value 最後出現的 index。
4. 若儲存最小值的 list 不為空，使用 `min()` 叫出最小值，若為空回傳 -1。

```
class Solution:
    def minimumCardPickup(self, cards: List[int]) -> int:
        seen = {}
        min_consecutive = []
        for key, value in enumerate(cards):
            if value not in seen:
                seen[value] = key
            else:
                min_consecutive.append((key + 1) - seen[value])
                seen[value] = key
            
        if min_consecutive:
            return min(min_consecutive)
        
        return -1
```
