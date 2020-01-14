---
title: Python 3 筆記 - 自動安裝所需的 Module
date: 2018-09-28 10:00:00
tags:
categories:
- Python 3 筆記
---
模組一多加上換電腦開發時，重裝模組總是那麼的浪費時間
<!--more-->
# 重點程式碼
首先開啟你的終端機，並 `cd` 到 Python 專案的目錄
接著讓 pip 為我們產生所需模組列表
```
python -m pip freeze > requirements.txt
```

接著我們可以在要操作的電腦上輸入下面這行，讓 pip 為我們安裝所需要的模組
```
pip install -r requirements.txt
```
