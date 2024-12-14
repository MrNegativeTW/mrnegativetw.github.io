---
title: macOS 下使用 Serial Port 連接 Cisco 設備
date: 2020-07-29 10:00:00
tags:
categories:
- macOS
---
看很多文章好像都很舊很複雜，其實一行指令就能解決的。
<!--more-->

# 確認
看其他文章好像還要什麼驅動啥的，搞得有夠麻煩，反正我目前的 macOS 10.15.5 讀得到
 
順帶一提，我這條線是從蝦皮買的，NT$ 39 的樣子，不懂怎麼會有人可以賣到幾百塊，又沒有比較快lol
![](macSerial_0.png)

# 連接
一行指令即可解決。
```
screen /dev/tty.usbserial-1420 9600
```
![](macSerial_1.png)

# 硬啦
![](macSerial_2.png)