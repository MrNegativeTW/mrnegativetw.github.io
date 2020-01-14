---
title: Python 3 筆記 - 產生 Secret Key
date: 2018-09-06 10:00:00
tags:
categories:
- Python 3 筆記
---
如何在 Python 中快速產生一組 Secret Key 呢？繼續看下去就知道了
<!--more-->
# 重點程式碼
首先在 Terminal 輸入 `Python`，進入環境
接著依序輸入這兩行，然後就會有一組鑰匙囉
```Python
import secrets
secrets.token_hex(16)
```
最後輸入 `exit()` 離開 Python 環境就好囉
