---
title: ADATA SX8200 Pro 512G 裝 MacBook Pro
date: 2020-12-15 10:00:00
tags:
categories:
- 開箱
---
專題製作的經費可以購買相關的物品，然後我就拿來買硬碟給 MacBook Pro 13 (Early 2015, A1502) 升級了。
![](cover.jpg)
<!--more-->

MacBook Pro 對硬碟很挑，當初口袋名單有 Samsung、Intel、Micron 這些廠牌，但綜合一堆結果下來，這顆好像在性能、耗電、問題都是最平均的，不然我大概也不會選這顆樂透。

# 主控與顆粒
購買日期：2020 年 12 月 14 日，主控：`SM2262G AB`，顆粒：`ADATA 自封`。
顆粒的序號我有 P 過，P 掉的部分都是一樣的數字。
![](ssd-closer-look.jpg)

簡單來說買到了 SX8200，買這顆跟抽抽樂一樣，威剛你好樣的。

# Adapter 開箱
蝦皮隨便找了個 209 的轉接頭就買了，正面印有 `2008A6` `LOTES`，背面印有 `NFHK N-941A` `www.sznfhk.com`
![](adapter_0.jpg)

硬碟插上去的樣子，這裡有個問題，如果硬碟插到底，會卡到硬碟上的電容，如果不插到底，不曉得會不會出問題。待觀察。
![](adapter_1.jpg)


# 速度
## Ryzen Desktop
CPU：Ryzen R7 1700
主機板：AB350-Gaming 3
![](Test_Windows.png)


## MacBook Pro 13 Early 2015
CPU：i5-5257U
頻寬：4x lanes PCIe 2.0
Adapter：蝦皮 209 那個
![](Test_MacBook.png)

# 總結

使用上接近正常。

- 休眠
耗電，充到 84 後拔電源放置整晚，早上起來剩下 65。問題很詭異，透過 Log 檔案可以看到有東西持續每 15 分鐘喚醒電腦，直到時間到電腦真正去睡覺，目前我是把深度睡眠時間改 2 小時，讓他儘早去睡覺。值得一提的是插上電源時不會每 15 分鐘喚醒，真的詭異。

- 溫度
正常使用大概 60 度上下，但透過 iStat Menu 看硬碟溫度有時可達 70 度。

- DriveDX 
可正常抓到資訊，連壽命都有，順帶一提，這顆型號為 `SX8200PNP`。

### 花費
| 品名 | 價格 | 購買地 |
| --- | --- | --- |
| SX8200 Pro 512G | NTD 1,799 | 欣亞數位 |
| Adapter | NTD 209 | 蝦皮 |
| 仿小米 24 合 1 工具組| NTD 169 | 蝦皮 |