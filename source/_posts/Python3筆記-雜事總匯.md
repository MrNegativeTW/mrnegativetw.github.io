---
title: Python 3 筆記 - 雜事總匯
date: 2018-10-13 10:00:00
tags:
categories:
- Python 3 筆記
---
收錄一些小技巧，給我自己看。
<!--more-->

# 指令部分
## 自動安裝所需的 Module
```
# 匯出 Module 清單
python -m pip freeze > requirements.txt

# 安裝 Module 清單內的東西
pip install -r requirements.txt
```

## 找不到 pip 指令
有時候會說找不到 `pip`，那就在前面加個 `python -m`
```
python -m pip install ...
```

# 畫面部分
## input() 不換行

加個 `end=''`

```Python
print('YOUR TEXT', end='')
```

## 清空終端機畫面
```Python
import os
os.system('clear')
```


# 其他
## 陣列中隨機取值
```Python
import random
random.choice(YOURNAME)
```

## 產生 Secret Key
```Python
import secrets
secrets.token_hex(16)
```