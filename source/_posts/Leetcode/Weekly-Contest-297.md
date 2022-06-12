---
title: Leetcode Weekly Contest 297
date: 2022-06-12 10:00:00
tags:
categories:
- Leetcode
---

今天嘗試解了 Hard，有一題 Medium 是圖形相關，題目太長直接懶得看，另一題分餅乾的需要 Greedy Algorithm，直接超出知識範圍。最後想說來試試看 Hard。

<!-- more -->

# 2303. Calculate Amount Paid in Taxes

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-297/problems/calculate-amount-paid-in-taxes/)

You are given a **0-indexed** 2D integer array `brackets` where `brackets[i] = [upperi, percenti]` means that the `ith` tax bracket has an upper bound of `upperi` and is taxed at a rate of `percenti`. The brackets are **sorted** by upper bound (i.e. `upperi-1 < upperi` for `0 < i < brackets.length`).

Tax is calculated as follows:

- The first `upper0` dollars earned are taxed at a rate of `percent0`.
- The next `upper1 - upper0` dollars earned are taxed at a rate of `percent1`.
- The next `upper2 - upper1` dollars earned are taxed at a rate of `percent2`.
And so on.

You are given an integer `income` representing the amount of money you earned. Return the amount of money that you have to pay in taxes. Answers within `10^5` of the actual answer will be accepted.

## 題目解釋

計算你的收入 `income` 要被抽多少稅

以題目來說 `[[3,50],[7,10],[12,25]]`，前面 3 塊要 50%，接下來的 4 塊要 10%，接下來的 5 塊要 25%

## Solution

就很暴力，先把 brackets 從後面跑過一遍算出每個級距的錢，接著再跑過一遍算錢。

```python
def calculateTax(self, brackets: List[List[int]], income: int) -> float:
    total = 0
    for i in range(len(brackets) - 1, 0, -1):
        brackets[i][0] = brackets[i][0] - brackets[i - 1][0]
    print(brackets)
    
    for i in brackets:
        if income - i[0] >= 0:
            total = total + i[0] * (i[1] / 100)
            income = income - i[0]
        else:
            total = total + income * (i[1] / 100)
            break
    return total
```

# 2306. Naming a Company

Difficulty: `Hard`, [Open in Leetcode](https://leetcode.com/contest/weekly-contest-297/problems/naming-a-company/)

You are given an array of strings `ideas` that represents a list of names to be used in the process of naming a company. The process of naming a company is as follows:

1. Choose 2 **distinct** names from `ideas`, call them `ideaA` and `ideaB`.
2. Swap the first letters of `ideaA` and `ideaB` with each other.
3. If **both** of the new names are not found in the original `ideas`, then the name `ideaA` `ideaB` (the **concatenation** of `ideaA` and `ideaB`, separated by a space) is a valid company name.
4. Otherwise, it is not a valid name.

Return the number of **distinct** valid names for the company.

## 題目解釋



## Solution

看看題目限制寫到 `2 <= ideas.length <= 5 * 10^4`，吃到 TLE 完全不意外

```python
def distinctNames(self, ideas: List[str]) -> int:
    v = set()
    for i in range(len(ideas)):
        for j in range(len(ideas)):
            if i == j:
                continue
            n1 = ideas[j][0:1] + ideas[i][1:]    
            n2 = ideas[i][0:1] + ideas[j][1:]
            if n1 not in ideas and n2 not in ideas:
                v.add(n1 + " " + n2)
        
    return len(v)
```

所以讓來看看大神的思路：[[Python 3] Explanation with pictures
](https://leetcode.com/problems/naming-a-company/discuss/2141038/Python-3-Explanation-with-pictures)

1. 用第一個字母把 `ideas` 分組
2. 挑兩個出來比的時候，如果同一個字在這兩個 set() 都有出現，那這個字一定不會是 valid name
3. 把這些字從各自的 list 中扣掉，算出 list 中獨特的字有幾個
4. 把兩個 list 中獨特的數乘起來，就是一半的 valid name，另一半可以再 x2 ，也就是由 `A B` 變成 `B A` 的意思。


```python
def distinctNames(self, ideas: List[str]) -> int:
    # Group strings by their initials
    A = [set() for _ in range(26)]
    for idea in ideas:
        A[ord(idea[0]) - ord('a')].add(idea[1:])
    
    ans = 0
    # Calculate number of valid names from every initial pair.
    for i in range(25):
        for j in range(i + 1, 26):
            k = len(A[i] & A[j]) # Number of duplicated suffixes
            ans += 2 * (len(A[i]) - k) * (len(A[j]) - k)
    return ans
```