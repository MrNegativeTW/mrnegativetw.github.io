---
title: Leetcode Biweekly Contest 80
date: 2022-06-11 10:00:00
tags:
categories:
- Leetcode
---

這禮拜變成 Easy Medium Hard Hard，難度直接倍增，Easy 當然也是暴力解的啦，至於第二題 Medium 就吃了 Time Limit Exceeded 和 Memory Limit Exceeded。

QAQ

<!-- more -->

# 2299. Strong Password Checker II

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-80/problems/strong-password-checker-ii/)

A password is said to be **strong** if it satisfies all the following criteria:

- It has at least `8` characters.
- It contains at least **one lowercase** letter.
- It contains at least **one uppercase** letter.
- It contains at least **one digit**.
- It contains at least **one special character**. The special characters are the characters in the following string: `"!@#$%^&*()-+"`.
- It does **not** contain `2` of the same character in adjacent positions (i.e., `"aab"` violates this condition, but `"aba"` does not).

Given a string `password`, return `true` if it is a **strong** password. Otherwise, return `false`.



## 題目解釋

就是跑過一次 password 的每個字，確認這個 string 中的字有達到需求。

## Solution

基本上就是很醜的暴力解，用各種 `True` 和 `False`  組成的。

```python
def strongPasswordCheckerII(self, password: str) -> bool:
    upper = False
    lower = False
    digits = False
    special = False
    special_list = [i for i in "!@#$%^&*()-+"]
    
    n = len(password)
    
    if n < 8:
        return False
    last_char = ""
    for i in password:
        if i.isnumeric():
            digits = True
        if i.isupper():
            upper = True
        if i.islower():
            lower = True
        if i in special_list:
            special = True
            
        if i == last_char:
            return False
        else: 
            last_char = i
    
    if upper == True and lower == True and digits == True and special == True:
        return True
    else:
        return False
```

讓我們來觀摩一下其他大神的解法：
[[Java/Python 3] Use HashSet to count conditions, w/ brief explanation and analysis](https://leetcode.com/problems/strong-password-checker-ii/discuss/2138969/JavaPython-3-Use-HashSet-to-count-conditions-w-brief-explanation-and-analysis)

利用 `s` `u` `l` `d` 表示 special characters, lower case, upper case 和 digits 並在判斷正確的時候把這些字元加入 set() 當中，最後只要確認 set() 長度是否為 4 即可。

```python
def strongPasswordCheckerII(self, password: str) -> bool:
    seen = set()
    for i, c in enumerate(password):
        if i > 0 and c == password[i - 1]:
            return False
        if c in "!@#$%^&*()-+":
            seen.add('s')
        elif c.isupper():
            seen.add('u')
        elif c.islower():
            seen.add('l')
        elif c.isdigit():
            seen.add('d')             
    return  len(password) > 7 and len(seen) == 4
```

幹，好猛 = =

# 2300. Successful Pairs of Spells and Potions

Difficulty: `Easy`, [Open in Leetcode](https://leetcode.com/contest/biweekly-contest-80/problems/strong-password-checker-ii/)

Total Accepted: 7291，Total Submissions: 33118，看來這題我不孤單，大家都在吃各種 Limit exceeded。

You are given two positive integer arrays `spells` and `potions`, of length `n` and `m` respectively, where `spells[i]` represents the strength of the `ith` spell and `potions[j]` represents the strength of the `jth` potion.

You are also given an integer `success`. A spell and potion pair is considered **successful** if the **product** of their strengths is **at least** `success`.

Return an integer array `pairs` of length `n` where `pairs[i]` is the number of **potions** that will form a successful pair with the `ith` spell.

## 題目解釋

給你一串魔咒 `spells` 和藥水 `potions` 和成功合成的數值 `success`，我們要計算每一個 `spells` 去乘上每一個 `potions` 後有大於等於 `success` 的數量。

回傳的 `pairs` 是 `spells` 的數量。

## Solution

我第一次只用一個 dict，不意外吃 Time Limit Exceeded，第二次改用兩個 dict，直接吃 Memory Limit Exceeded。

```python
def successfulPairs(self, spells: List[int], potions: List[int], success: int) -> List[int]:
    pairs = []
    spells_dict = {}
    
    for i in spells:
        count = 0
        potions_dict = {}
        
        if i in spells_dict:
            potions_dict = spells_dict[i]
        
        for j in potions:
            compare = 0
            if j not in potions_dict:
                compare = i * j
                potions_dict[j] = compare
            else:
                compare = potions_dict[j]
            
            if compare >= success:
                count = count + 1
            
        pairs.append(count)
        if i not in spells_dict:
            spells_dict[i] = potions_dict
    
    return pairs
```

所以我們來看看大神的解法：
[[JavaC++/Python] Straignt Forward with Explantion
](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/discuss/2138875/JavaC%2B%2BPython-Straignt-Forward-with-Explantion)

使用數學基本概念，`spell[i]` * `potions[j]` 要等於 success，那 `potions[j]` 一定要大於某個數，相乘時才會大於等於 success。

所以先算出這個數到底要多大 `need = success * 1.0 / spell`

接著用 bisect_left 取得數字要插入在哪個 index，

最後算出右邊有幾個數即可。

```python
def successfulPairs(self, spells: List[int], potions: List[int], success: int) -> List[int]:
    potions.sort()
    return [len(potions) - bisect_left(potions, (success + a - 1) // a) for a in spells]
```