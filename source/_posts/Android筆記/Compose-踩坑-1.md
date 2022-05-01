---
title: Compose 踩坑記 (1)
date: 2022-04-27 10:00:00
tags:
categories:
- Android 筆記
- Jetpack Compose 踩坑日記
---

最近發現 Jetpack Compose 版本已經到了 release 1.1.1，也是時候來學學現在很熱門的宣告式 UI。

目前踩到的坑有：

- Row 與 Column 中的 Arrangement 與 Alignment

<!--more-->

# Row 與 Column 中的 Arrangement 與 Alignment

看了些教學後開始嘗試 CenterHorizontally 這個屬性，但怎麼弄都顯示 Unresolved reference: CenterHorizontally，確認過 compose 版本也沒問題，最後在 [Compose layout basics](https://developer.android.com/jetpack/compose/layouts/basics#standard-layouts) 中發現一段話：

> To set children's position within a Row, set the `horizontalArrangement` and `verticalAlignment` arguments. For a Column, set the `verticalArrangement` and `horizontalAlignment` arguments.

這兩個詞很像，沒仔細看還真的沒發現，有 `Arrangement` 和 `Alignment`，當要使用 `Center...` 這個屬性時，依照 Row 或 Column 把前面換成對應的 `...Alignment` 即可

**Row 要這樣用**

```kotlin
Row(
    modifier = Modifier.fillMaxSize(),
    horizontalArrangement = Arrangement.Center, 
    verticalAlignment = Alignment.CenterVertically
) {
    // ...
}
```

**Column 要這樣用**

```kotlin
Column(
    modifier = Modifier.fillMaxSize(),
    verticalArrangement = Arrangement.Center,
    horizontalAlignment = Alignment.CenterHorizontally
) {
    // ...
}
```