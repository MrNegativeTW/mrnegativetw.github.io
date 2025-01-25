---
title: 開箱 - 小米路由器 AX1500
date: 2025-01-24 10:00:00
tags:
categories:
- 開箱
---

全 Giga 網孔 + Wi-Fi 6 只要 NT$ 695 就能買到，如果只是要用來做中繼當 AP，這價位是真的無敵了。

<!--more-->

家中網路前面主路由是 TP-Link C7 刷 OpenWrt，所以這台只是單純做有線中繼，沒用到啥特殊功能，也不追求效能。

這篇只是簡單記錄一下，不詳細開箱。

![](cover.jpg)

# 外觀

外盒

![](box_1.jpg)


型號 RD12

![](box_2.jpg)

電源是 12V 1A

![](power-supply.jpg)

# Web 管理介面

![](web_main.png)

# 有線中繼模式改 IP

有線中繼模式下有些設定是看不到的，網址把 `apsetting` 改成 `setting` 就行

## 修改前

```
http://192.168.1.168/cgi-bin/luci/;stok=xxxxxxxxxxxxxxxxxxxxxx/web/apsetting/wifi
```

![](web_apsetting.png)

## 修改後

```
http://192.168.1.168/cgi-bin/luci/;stok=xxxxxxxxxxxxxxxxxxxxxx/web/setting/wifi
```

![](web_setting.png)

然後就可以改這台設備在內網的 IP 了

![](web_setting_ip.png)