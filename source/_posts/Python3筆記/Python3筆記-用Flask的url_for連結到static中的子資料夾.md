---
title: Python 3 筆記 - 用 Flask 的 url_for 連結到 static 中的子資料夾
date: 2018-09-01 10:00:00
tags:
categories:
- Python 3 筆記
---
我知道，標題唸起來很饒舌...
怪了，怎麼用 `url_for` 指定了路徑，卻還是抓不到檔案呢？
<!--more-->
#  重點程式碼
你是否像我一樣把路徑全打在一起，然後把單獨打檔名呢
事實上，你要這樣打
```
url_for('static', filename='PATH/TO/FILE')
```
# 解釋
`'static'`
靜態檔案的資料夾位置，管他什麼 CSS 還是 JS，通通放在這邊

`filename='PATH/TO/FILE'``
這就是重點了，你可以在 static 底下新建資料夾分門別類存放 CSS 或 JS，這時候檔案的路徑就是靠這行處理啦

# 範例
舉例來說，我在 `static` 資料夾裡面放了子資料夾 `css` ，然後將網頁的主要 CSS 檔案 `style.css` 放在其中，那我就可以用如下寫法成功讀取 CSS 檔案
```
<link rel="stylesheet" type="text/css" href="{{url_for('static', filename='css/style.css')}}">
```
