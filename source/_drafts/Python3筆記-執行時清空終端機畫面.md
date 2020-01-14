---
title: Python 3 筆記 - 執行時清空終端機畫面
date: 2018-08-19 10:00:00
tags:
categories:
- Python 3 筆記
---
於終端機執行 Python 時常常會被眼花撩亂的字樣填滿眼前，又或者是需要美觀的程式執行畫面，這時就需要這兩行來輔助啦！
<!--more-->
# 重點程式碼
加入位置：頂端
為了讓程式一執行就先清空畫面，所以於頂端加入
```Python
import os
os.system('clear')
```

# 解釋
`import os`
載入 os 模組

`os.system('clear')``
使用模組功能對終端機發出 `clear` 指令，也就是清空畫面
