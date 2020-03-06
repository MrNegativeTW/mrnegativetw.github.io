---
title: Kotlin 筆記 - 印出 IntArray 的內容
date: 2020-03-06 10:00:00
tags:
categories:
- Kotlin 筆記
---
IntArray 若是直接 print 會得到一串類似這樣的字串 `[I@4783da3f`
<!-- more -->
# 問題範例
IntArray 若直接使用 print() 印出，會產生一坨奇怪的東西。

```kotlin
val list = intArrayOf(0, 1, 2, 3, 4, 5)
println(list)
// 印出 I@[4783da3f
```

# 解法
這時只要加上 `contentToString()` 即可將 IntArray 正常印出。
```kotlin
println(list.contentToString())
```