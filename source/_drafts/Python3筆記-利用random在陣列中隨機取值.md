---
title: Python 3 筆記 - 利用 random 在陣列中隨機取值
date: 2018-08-20 10:00:00
tags:
categories:
- Python 3 筆記
---
想要隨機取值嗎？有一個好用的模組叫做 random，可以幫你在超短的時間內隨機取出一個值，讓你擺脫選擇障礙。
<!--more-->
# 重點程式碼
加入位置：不定
```Python
random.choice(YOURNAME)
```
當然，還是要指定一個數值給來存放選出來的數值，於前方加入 `SOMETHING = ` ，並且於印出時使用 `SOMETHING` 即可（SOMETHING 請替換為你可以辨認的名稱）

# 解釋
`random.choice`
使用 random 模組中的選擇功能

`YOURNAME`
你所定義的陣列名稱

# 範例

```Python
list = ['1', '2', '3']
wow = random.choice(list)
print(wow)
```
