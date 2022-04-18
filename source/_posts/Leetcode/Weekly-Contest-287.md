---
title: Leetcode Weekly Contest 287
date: 2022-04-03 10:00:00
tags:
categories:
- Leetcode
---

我的第一場 Leetcode Contest，這天看我爸搭平台搭到一半才想到跑回來寫。這場好玩的是，我先寫了 Medium 才回去寫 Easy，然後 Easy 還卡超久，結果是因為題目沒看清楚 lol。

<!-- more -->

# 2224. Minimum Number of Operations to Convert Time

Difficulty: `Easy`

You are given two strings `current` and `correct` representing two 24-hour times.

24-hour times are formatted as `"HH:MM"`, where `HH` is between `00` and `23`, and `MM` is between `00` and `59`. The earliest 24-hour time is `00:00`, and the latest is `23:59`.

In one operation you can increase the time `current` by `1`, `5`, `15`, or `60` minutes. You can perform this operation **any** number of times.

Return the ***minimum number of operations*** needed to convert `current` to `correct`.

```
Input: current = "02:30", correct = "04:35"
Output: 3
Explanation:
We can convert current to correct in 3 operations as follows:
- Add 60 minutes to current. current becomes "03:30".
- Add 60 minutes to current. current becomes "04:30".
- Add 5 minutes to current. current becomes "04:35".
It can be proven that it is not possible to convert current to correct in fewer than 3 operations.
```
```
Input: current = "11:00", correct = "11:01"
Output: 1
Explanation: We only have to add one minute to current, so the minimum number of operations needed is 1.
```

- `current` and `correct` are in the format `"HH:MM"`
- `current <= correct`

## 我的解法

1. 既然是要步驟，先初始一個 counter `operations`
2. 把字串拆成數字的 list 方便操作
3. 把時間全部換算為 `分`
4. 相減算出差異時間 `diff_time` 後，除以 `60`、`15`、`5`、`1`，並加至 `operations`；
    同時將 `diff_time` 指定為除法後的餘數。

```python
class Solution:
    def convertTime(self, current: str, correct: str) -> int:
        operations = 0
        
        current_time_arr = [int(x) for x in current.split(":")]
        correct_time_arr = [int(x) for x in correct.split(":")]
        
        current_time = current_time_arr[0] * 60 + current_time_arr[1]
        correct_time = correct_time_arr[0] * 60 + correct_time_arr[1]
        
        diff_time = correct_time - current_time
        operations += int(diff_time / 60)
        diff_time = diff_time % 60
        operations += int(diff_time / 15)
        diff_time = diff_time % 15
        operations += int(diff_time / 5)
        diff_time = diff_time % 5
        operations += int(diff_time / 1)
        diff_time = diff_time % 1
        
        return operations
```

# 2225. Find Players With Zero or One Losses

Difficulty: `Medium`

You are given an integer array `matches` where `matches[i] = [winner_i, loser_i]` indicates that the player `winner_i` defeated player `loser_i` in a match.

Return a list `answer` of size `2` where:

- `answer[0]` is a list of all players that have not lost any matches.
- `answer[1]` is a list of all players that have lost exactly one match.

The values in the two lists should be returned in increasing order.

Note:
- You should only consider the players that have played at least one match.
- The testcases will be generated such that no two matches will have the same outcome.

```
Input: matches = [[1,3],[2,3],[3,6],[5,6],[5,7],[4,5],[4,8],[4,9],[10,4],[10,9]]
Output: [[1,2,10],[4,5,7,8]]
Explanation:
Players 1, 2, and 10 have not lost any matches.
Players 4, 5, 7, and 8 each have lost one match.
Players 3, 6, and 9 each have lost two matches.
Thus, answer[0] = [1,2,10] and answer[1] = [4,5,7,8].
```
```
Input: matches = [[2,3],[1,3],[5,4],[6,4]]
Output: [[1,2,5,6],[]]
Explanation:
Players 1, 2, 5, and 6 have not lost any matches.
Players 3 and 4 each have lost two matches.
Thus, answer[0] = [1,2,5,6] and answer[1] = [].
```

- `1 <= matches.length <= 105`
- `matches[i].length == 2`
- `1 <= winneri, loseri <= 105`
- `winneri != loseri`
- All `matches[i]` are unique.

## 我的解法

1. 把每一組跑過一次，並將 winner 和 loser 的編號與出現次數存在各自的 dict 中
2. 將 `winner_dict` 跑過一次，只要 key 沒在 `loser_dict` 中出現，代表沒輸過，並存到 `no_lost`
3. 將 `loser_dict` 跑過一次，只要 value == 1，表示只輸過一次，並存到 `lost_one`
4. 最後將兩個 list 排序後存到 `result` 中回傳

```python
class Solution:
    def findWinners(self, matches: List[List[int]]) -> List[List[int]]:
        winner_dict = {}
        loser_dict = {}
        no_lost = []
        lost_one = []
        result = []
        
        for i in range(len(matches)):
            winner = matches[i][0]
            loser = matches[i][1]
            if winner in winner_dict:
                winner_dict[winner] += 1
            else:
                winner_dict[winner] = 1
                
            if loser in loser_dict:
                loser_dict[loser] += 1
            else:
                loser_dict[loser] = 1
                
        for key, value in winner_dict.items():
            if key not in loser_dict:
                no_lost.append(key)
        
        for key, value in loser_dict.items():
            if value == 1:
                lost_one.append(key)

        no_lost.sort()
        lost_one.sort()
        
        result.append(no_lost)
        result.append(lost_one)
        
        return result
```

# 我就暴力

![我就暴力](https://bnz05pap001files.storage.live.com/y4mqKdVDUSY-2JUIWS0AwUkOJAC3jyA5Blz2juHB89TvVorWcNP2f9DmAGl8rouDhQC6KvDTpUmPHYsd36QMXFop0UoWtQ5ta6_ri6zdtJmWAoLLGgJ9u2GB52zO_OZsgNjpR4VUdQe0fIrtNpHZ5K5IqPPNNJ5RnKkCzaJtepfqk2FVGnERVO-s5wspWGXXuOG?width=600&height=591&cropmode=none)