---
title: OpenWrt 筆記 - TP-Link Archer C7 V2 網路介面設置
date: 2020-03-07 10:00:00
tags:
categories:
- OpenWrt 筆記
---
OpenWrt 刷上去興奮的開機了，結果換來的是網路無法聯外，僅能在本地自嗨，究竟是什麼問題呢？
<!-- more -->
## 問題
OpenWrt 刷完後會發現，明明一樣接著 WAN 口，但是卻無法上網。

會像我一樣大概是因為介面設定出問題，首先進入 OpenWrt 設定頁面，並導航到如下位置：
{% note info %} 
網路 > 介面
{% endnote %}

## 實體介面設置
### LAN
接著將 LAN 介面如下打勾：
{% note info %} 
Switch VLAN: "eth1.1" (lan)
無線網路: Master "SSID" (lan)
無線網路: Master "SSID"  (lan)
{% endnote %}
![Openwrt_interfaceSetting_0](https://okhzwa.bn.files.1drv.com/y4mZrbamB76IQh_xpRMoQV7d-963Un7V22QroYhfIxLP_aEEFmFGkjB3Upw6H-zqGEOCZJjNVrqkI2ubVaXBHHCiniaMM9Mrwwn_bApWmiNWogzsR3E0sPcuPyt9v2plE8AWVsq0h0GwSd-2QSlKpaLMeLgrkH-kg7g3xjZH7edxHlqoNzM7HUGuIt-E4FyTRpPhZmkwEsBpW3ugXr6wkTvBQ)

### WAN
接著將 WAN 介面如下打勾：
{% note info %} 
Switch VLAN: "eth0.1" (wan, wan6)
{% endnote %}
![Openwrt_interfaceSetting_1](https://okf0nq.bn.files.1drv.com/y4mVmF4lH79vHg7WmXT1PJ4tuCUTRZxUc_jbzAzdKeWrKkd8fhCngqiMbqBr_eAWbrtkKVM8SUGgE_pomEfMTzBlGA9XMW1Q1lhzQeJFI0oIcS6dKB_ybUQvqUAb0M_ieL1KEktWQo0zXSBwAWuYeUuiszM1uaz9b4CGErkRx6lc8VY2RxDqi-SqenjKLeX2tPW1KF1bxjAhCVw1xWAj2XL1A)

## 防火牆設置
防火牆設定上也請注意，別為了讓路由可以上網把 WAN 跟 LAN 指定到同一區域了，我的設定如下：
![Openwrt_interfaceSetting_2](https://okepta.bn.files.1drv.com/y4mW1YQx15uucGPF5TXSDZrMQiBh8CGieJqiYfUeJDOYllCfiGSITjaG0OxWjOuKae9lhevlcm6TzPD8v3yMoF1mCsgziYweFkB1YkonKGfcN2lJAXKvKUV-IsDDtzuiU8dSBjKy94jDHOzua5987pFcrwct1U7GojetwDI3DzBTL1p2tx6Z7ufBbZVAW8JEMg920jVgU5K-TW3GYH1sgfC4g)

## 完工圖
設定都正確的話理論上會呈現如下圖，WAN 顏色都為紅色，LAN 顏色都為綠色。
![Openwrt_interfaceSetting_3](https://okgj1w.bn.files.1drv.com/y4mABQouAKqyfSabiMbTVKD3aGfn6ekbiOtyt9B8NJSRkEbSwhj_-d0MOK1mvJ8cVk-TjnH_76Dev62wAPlHj209lsKHz90timxPI5Ks6E3LrRrYppdTBvUlNmATe-yDjF2t1P0DmF1Q_GIKYkI23dKhNlLPxG5aNSBLdJ-KcS2Pldp5E0JpgfnpEZMtGl_a2S36-DL6Voz8gRhLD-kphVxTQ)