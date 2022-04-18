---
title: Python 3 筆記 - 利用 Requests 與 Beautiful Soup 爬取需登入的 ASPX 頁面
date: 2018-10-13 10:00:00
tags:
categories:
- Python 3 筆記
---
前幾天在爬學校的考試系統，不過用 Requests 操作登入不管怎樣就是進不去，後來發現表單送出資料中還有三欄隱藏的神秘資訊，Google 後才發現，原來 ASPX 頁面要登入可不是單單打密碼這麼簡單，這篇文章就讓我們來教學如何爬 ASPX 頁面吧。
<!--more-->
# 隱藏資訊
首先我們要找到表單的隱藏資訊，使用 Chrome 打開登入頁面，並右鍵 > 檢查，切換到 Network 頁面，接著，輸入 **錯誤** 的帳號密碼，並送出，送出後會發現旁邊的 Network 頁面多了一些紀錄資訊出來
圖中的 Form Data 欄位，就是我們登入所需要的所有資料啦
![requests_1](https://pbawbq.bn.files.1drv.com/y4maqvsowB2aEytjI1DRUnTAn2TqXudLfLtr-1nNqR5Tv1LzYAxWmlu24fprz10arGJJP1EZW12_HTkulFgONYrypCoMj071RscH7JdnopbMuAztlvtgRIhfuEE3y9g2POgA8mXKS8nZ63UstIGhJl6v1ZRN_PwmO6rjKP8mYZ_ql8frLq8plxJbrH89KOoGiYbZwlFfg-ExunQ9VbkT6W_Ew)

上圖以學校考試系統為例，表單的送出資訊還包含了`__VIEWSTATE`
`__VIEWSTATEGENERATOR` `__EVENTVALIDATION`，這三個隱藏資訊，拿到這些東西之後就可以準備繼續動工了

# 繼續動工
首先開頭我們先引入 BeautifulSoup 和 requests
我使用的版本為：Python 3.7、BeautifulSoup4

```Python
from bs4 import BeautifulSoup
import requests
```

接著是重點部分

```Python
with requests.Session() as s:
	page = s.get('https://portal.stust.edu.tw/examseat/login.aspx')
	soup = BeautifulSoup(page.content, 'lxml')

	payload_loginPage = {
		'txtStud_No': 'YOUR USERNAME',
		'txtPasswd': 'YOUR PASSWORD',
		'Button1': '登入'
	}

	payload_loginPage["__VIEWSTATE"] = soup.select_one("#__VIEWSTATE")["value"]
	payload_loginPage["__VIEWSTATEGENERATOR"] = soup.select_one("#__VIEWSTATEGENERATOR")["value"]
	payload_loginPage["__EVENTVALIDATION"] = soup.select_one("#__EVENTVALIDATION")["value"]
	s.post('https://portal.stust.edu.tw/examseat/login.aspx', data=payload_loginPage)
```
第 1 行：首先我們把 Requests 模組中的 session 功能叫出並簡稱為 `s`
第 2 行：利用 HTTP Requests Methods 中的 GET 方法取得登入頁面，並存於 `page` 當中
> 關於 HTTP Requests Methods，可以看這邊的說明 [英文版](https://www.w3schools.com/tags/ref_httpmethods.asp)、[中文版](http://www.w3school.com.cn/tags/html_ref_httpmethods.asp)

第 3 行：使用 Beautiful Soup 對 `page` 的內容(content) 進行處理
第 5-9 行：表單的可視化部分，冒號前方為表單欄位名稱，後方為欄位數值
> `'Button1': '登入'` 就是按下送出扭的概念

第 11-13 行：自動帶入這一坨隱藏資訊的數值，
第 14 行：最後就是把上面一坨的資料用 POST 的方式送交到表單進行驗證囉

# 成功與否
如何知道成功與否呢？
最簡單的方式就是透過印出下一頁的資料來確認，所以可在最後面加上
```python
page = s.get('https://portal.stust.edu.tw/examseat/Default.aspx')
print(page.text)
```

第 1 行：取得下一頁
第 2 行：印出頁面原始碼
