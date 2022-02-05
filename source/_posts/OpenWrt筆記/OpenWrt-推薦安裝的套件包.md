---
title: OpenWrt 推薦安裝的套件包
date: 2020-03-07 10:01:00
tags:
categories:
- OpenWrt 筆記
---
OpenWrt 裡面的套件包真的是又多又雜，開一篇文章來記錄一下都裝了什麼。
<!-- more -->
## 中文化
`*-zh-TW`
雖然中文化有時候怪怪的，不過可以更快找到目標，不裝嗎？
而且我有在幫忙翻譯，所以會越來越好的。

## 網路喚醒 / WOL
`luci-app-wol`

## WiFi 排程 / WiFi Schedule
`luci-app-wifischedule`

## USB 網路共享 / USB Tethering
`kmod-usb-net`
`kmod-usb-net-cdc-ether`
`kmod-usb-net-rndis`

For Android, iPhone 需要需要其他[額外的套件](https://openwrt.org/docs/guide-user/network/wan/smartphone.usb.tethering)。

## 統計 / Statistics
`luci-app-statistics`
