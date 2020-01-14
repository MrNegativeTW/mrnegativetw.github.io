---
title: Hexo 架設教學 - 新增 Sitemap
date: 2018-08-06 10:00:00
tags:
categories:
- Hexo 架設相關
- 架設教學
---
總之加入 Sitemap 可以被 Google 排名的更前面一些，所以加就對了
<!--more-->
# 產生尋寶圖
`cd` 到 Hexo 的安裝資料夾，並於 `Terminal` 輸入
```
npm install hexo-generator-sitemap
```
接著於 Hexo 本身的 `_config.xml` 中加入以下字串
```
# Sitemap
sitemap:
    path: sitemap.xml
```
然後叫 Hexo 產生一次內容
```
hexo g
```
就這樣，你擁有了夢寐以求的 Sitemap，接下來就提交到 Google 去吧！
# 提交到 Google
打開 [Google Search Console](https://www.google.com/webmasters/tools/home?hl=zh-TW&authuser=0)
選擇新增內容，並填入網址
![hexo_sitemap_1](https://0enchq.bn.files.1drv.com/y4mODIhfFHWkiZEup2nDyTPxMmuDdz6Q3aa3I1MljXGMsZo9WRJ2PShYDz6_UWymz-gcVb24E3GlVlTfESLb-cU6rSTfcrudWnkrTlaolcIWmsADde6CrQOJso6QLo6HJlDg2nvzSFs3wSEkymr33U8WyF-oUBrqAzKAaIyDqkDhV8SrmfGaIJ_WJUhqvKelr87yjJXiHi1g-ibNi1EsccxZQ)
接著 Google 會要求你驗證所有權，下載 Google 給的 HTML 檔案後，放到 `source` 資料夾，這樣就算用過 `hexo clean` 也不會丟失此文件
![hexo_sitemap_2](https://0epdkq.bn.files.1drv.com/y4mQ269crIyU84gg-UP11PM45PqlJbFVzDF9qJHarNMl8YIWVXymfoFwz98gsUOawCBBYzmeAF9Cmtxu_32eD1WdVDIY4v40PagJvqREZKgm6MU8qxoirTDPiSFSZ4zpHP3nC5c_LHIJk-60-W1H8Q68Eu6hVmoH28Wtsdx-xLF2JhChd7VGb0hgzp2yKj7TX22Vy2OAbYshkMtpEjkZuHp-w)
放置完成後打開 HTML 檔，並於頂端加入以下字串，防止 Hexo 自動幫我們產生頁面
```
layout: false
---
```
驗證完成後進入剛剛新增的資源，然後在左側找到 `檢索 > Sitemap`
然後於右上角找到 `新增/測試 SITEMAP`，並填入 `sitemap`
![hexo_sitemap_3](https://0en4bg.bn.files.1drv.com/y4mYt2sb5lbgDY71H4C-yJFflcCogFW1qaKFVGBv2_mhfLdAr1gfbi4zqGHKUdI8JTn7Kps5uKaWyaADww2FU4t8NJ1XUTx40sbJqfIh2cB7g08_3G5wh4YmGoLPqXo3PScMsATAixsXfb7lvIlS6ReqpSgh9r4-rVnrN0LkX7uthJUPFO8OHu1409ayDtkvIWoZmpAeffSPy7edrGxX25NdQ)
接下來就靜待 Google 派猴子來抓資料吧！
