---
title: macOS 睡眠喚醒後沒聲音解決辦法
date: 2020-03-13 10:00:00
tags:
categories:
- macOS
---
這陣子把 Mac 叫醒時常遇到整台電腦怎麼調就是沒聲音，不管是插拔耳機還是再度睡眠，聲音就好像掛掉了似的。
<!-- more -->
這問題好像只會發生在特定版本，升上 10.15.5 後就沒發生過了。

## 問題點
macOS 處理程序中有一項叫做 `coreaudiod`，據了解這項程序負責電腦上聲音的處理，包括降噪等。

## 解法
1. `Command + Space` 開啟 Spotlight，並輸入 `monitor` 開啟活動監視器
2. 在活動監視器右上角搜尋 `audio`，這時應該會看到有一條程序名為 `coreaudiod`
3. 選擇 `coreaudiod` 並將其結束
4. Done，繼續享受音樂
![coreaudioImage](https://okewvg.bn.files.1drv.com/y4mo5arM8l1RuCzE3KZTfB89bzDrasII4k_YfhqOLA46Sn5WhqTzLBMCd42fKsrkw9gdrYls9CVm4bEz01azD-aTkcT2a8iye2OYPOd_bp7gH9TH-pjedvAxJGZYVB9vSht-7zAdt4Y3Nsn2PxlZ5PyRFhmN1Gcg90syTONEGk1Km05CG9XWTDPzXd55GTvUPIwOTpn6cOowir2aUMj5nXDog)