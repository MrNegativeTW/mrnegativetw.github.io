---
title: 開發階段如何快速 debug 系統簽名的 APP
date: 2023-09-06 10:00:00
tags:
categories:
- Android 筆記
---

眾所皆知，系統簽名的 APP 可以拿到比較屌的權限，但開發階段總不可能每次都傳到主板商的伺服器去做簽名，然後載回來安裝測試，太麻煩

<!--more-->

將 `android.uid.system` 加入 AndroidManifest 的 manifest 標籤中，然後 Android Studio 就會跟你說簽名不符，不能裝

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.yourmom.zip"
    android:sharedUserId="android.uid.system" >
```

這時就需要 WeChat 去跟主板商的 FAE 拿東西了，以我們公司合作的亮鑽科技來說，FAE 給了兩個方案：

1. 簽保密協議，拿簽名文件

打包階段可使用簽名文件，然後就可以裝了

2. 使用「可以略過簽名驗證」的系統補丁刷入系統

開發階段當然會選這個，加完 `android.uid.system` 直接裝，直接提高效率

