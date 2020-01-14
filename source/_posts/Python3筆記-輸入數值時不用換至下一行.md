---
title: Python 3 筆記 - 輸入數值時不用換至下一行
date: 2018-08-21 10:00:00
tags:
categories:
- Python 3 筆記
---
使用 input() 要輸入數值時，游標總會跑到下一行，造成版面雜亂，但是只要簡單加入這幾個字就可以解決這問題！
<!--more-->
# 重點程式碼
加入位置：print() 後方

```Python
print('YOUR TEXT', end='')
```
# 解釋
`end=''`
這就是不會換到下一行的關鍵
