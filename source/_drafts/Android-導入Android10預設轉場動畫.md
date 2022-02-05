---
title: Android 筆記 - 導入 Android 10 預設轉場動畫
date: 2020-10-22 10:00:00
tags:
categories:
- Android 筆記
---
Android 10 預設轉場動畫與先前的截然不同，這篇文章教你如何在 APP 內導入 10 的轉場動畫。
<!--more-->
# 效果預覽
Android 10:
<img src="https://9to5google.com/wp-content/uploads/sites/4/2019/06/android_q_beta_4_transition_1.gif" width="320">

Android 9:
<img src="https://9to5google.com/wp-content/uploads/sites/4/2019/06/android_q_beta_3_transition_1.gif" width="320">

有沒有覺得縮放很潮？
雖然 Android 9 前的預設轉場還是上下滑，但人就是會犯賤，想在舊版本加入炫泡一點的東西。

# 改起來各位
## 新增 dependencies
首先在 APP 等級的 `build.gradle` 加入 `fragment`。
```
implementation 'androidx.navigation:navigation-fragment-ktx:2.3.1'
```

## 修改 style.xml
首先在 `style.xml` 中最上層加入這些東西。
```xml
<style name="Animation.Activity.Transition" parent="android:Animation.Activity">
    <item name="android:activityOpenEnterAnimation">@anim/fragment_open_enter</item>
    <item name="android:activityOpenExitAnimation">@anim/fragment_open_exit</item>
    <item name="android:activityCloseEnterAnimation">@anim/fragment_close_enter</item>
    <item name="android:activityCloseExitAnimation">@anim/fragment_close_exit</item>
</style>
```

在各主題中加入 `android:windowAnimationStyle`，並指向剛剛設定的 `Animation.Activity.Transition`。
```xml
<style name="AppTheme" parent="Theme.MaterialComponents.Light">
    <!-- Other Attr -->
    <item name="android:windowAnimationStyle">@style/Animation.Activity.Transition</item>
</style>
```

完成，你現在有炫泡轉場動畫了

# 後記
我肖想這個動畫很久了，為了這動畫我還去拆 Reddit 的 APK，然後複製貼到自己的專案。後來有天我在試 Jetpack 的 Navigation 時意外發現這個動畫已經存在於 `fragment` 套件中，我們只需引入套用即可，繞了一大圈冤忘路  。