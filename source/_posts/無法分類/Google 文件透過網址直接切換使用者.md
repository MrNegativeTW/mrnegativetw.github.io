---
title: Google 文件透過網址直接切換使用者
date: 2020-03-25 10:00:00
tags:
categories:
- 無法分類
---
適用於 Google 文件/試算表/簡報/表單/雲端硬碟...等服務，透過網址直接切換使用者，免於使用者錯誤麻煩。
<!--more-->
# 適應症
1. 開啟別人分享的連結時 Google 顯示帳號錯誤
2. 開啟簡報範本時的帳號不對(登入不只一個 Google 帳號)
反正類似情況都適用

# 解法
於網址的 `/d/` 之前加入 `/u/2`
```
https://docs.google.com/presentation/d/1sytMyYV91ICRJI6JJFvJid9vATK6nqLQ8a1lmSUemdw/template/preview
```

上述網址加上 `/u/2` 變成
```
https://docs.google.com/presentation/u/2/d/1sytMyYV91ICRJI6JJFvJid9vATK6nqLQ8a1lmSUemdw/template/preview
```