---
title: Windows - 刪除不要的網路
date: 2018-09-16 10:00:00
tags:
categories:
- Windows
- 技巧
---
如果你常常用手機 USB 做網路共享，又或是常常掛 VPN 之類的，想必你會有一大堆「網路一」「網路二」，這篇文章就教你如何刪除這些在也用不到的網路
<!--more-->
# 開始刪除
首先按下 `Windows鍵 + R`，接著輸入 `regedit` 並開啟
接著到以下兩個路徑中將不要的網路刪除即可
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
```
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged
```
![]()
