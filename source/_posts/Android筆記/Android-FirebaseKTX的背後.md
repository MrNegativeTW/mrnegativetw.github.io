---
title: Android 筆記 - Firebase KTX 的背後
date: 2020-09-05 10:00:00
tags:
categories:
- Android 筆記
---

今天在移動 Firebase 的 dependencies 到 *-ktx 時發現了些東西。

<!--more-->

事情是這樣的，Firebase 發布 *-ktx 的 Android SDK 好一段時間了，但我開始做這專案時還是 java 的版本，直到這幾天我才想把 dependencies 全部移動到 *-ktx 上，然後移著移著就發現了 ktx 只是包上糖衣的 java，我還以為 Google 有重寫過呢。

# 正文

首先改 build.gradle(app) 裡面的 dependencies，基本上加上 -ktx 就完成了

```
// Java
com.google.firebase:firebase-firestore:21.6.0

// Kotlin
com.google.firebase:firebase-firestore-ktx:21.6.0
```

第二步就是改 code，這邊開始我就發現了些問題，把 java 版本的 SDK 注解掉之後 Code 竟然沒跳錯誤？咦？

```java
// Java
// 照理來說換個 SDK 這裡就應該跳錯誤了
val db = FirebaseFirestore.getInstance()
```

```kotlin
// Kotlin
val db = Firebase.firestore
```

# 拆起來各位

Intellij IDEA 的編輯器有個好用的功能， `Ctrl`/`Command` + `B` 按下去，源碼可以直接抓出來給你看，從哪邊引用的都一清二楚。

## 拆 Java

首先我們看看 Java 版中的 `FirebaseFirestore` 來自哪裡
![](https://nmpsmw.bn.files.1drv.com/y4mg4WCDu6WF1YN9mnyjcOa6AiBih8nhtPfyLKp4aloZD-9jFokwyPVEKh3dlZqRZ6N5ba9xxLG0TV-YCORtktTMbWfjvV2G1Cmk7UPh9_4JgJ8X5FKQ2Om5Z75D4K4Go8YzP6H8RChT8RWX-GovSlf4yuSeQaiKjucz81I0NXfMDzfrKbDRgSXSczK2TKZXXH0XiXzKaXukzGpdiUgnsVArA)

可以看到他連結到了 package `com.google.firebase.firestore` 中的 `FirebaseFirestore` class
![](https://nm8ftq.bn.files.1drv.com/y4myU7ySzGPqYCM6qSct5E0nKnhoYDif4uwsAGsOl7Vw1WfUxZSXHzzmjyGKeo1Reyd7n9Q1nvp4v2LeG5-LGYzSzS9qArYE3atkxhVOJ_RvJfWvLSYTTTFir8Kl03AwfnpjqkIZp4UbI2sa4_08DZxDK6sDaPwhlyTBkethc5ivE0iN23NAXJlRp18ObfAsKeh6Uy3otkVQyj32LY78PS9bg)


## 拆 Kotlin

接著看看 Kotlin 版中的 `Firebase.firestore` 來自哪裡

![](https://nmq3vg.bn.files.1drv.com/y4m4D5wfGLxNubIJjwzkp5PmiimEIU6aogRbQufldE0qgdShnzONU9V5SNBN0Xbie4akFqqKYcJ3aybgdzzNbu8o8mBuT6XMcTOrVn1gVu_Nm8c5fjzsPit8_W13VTAYEqvdWYY1FVtEufgUrTDFCCB1PhaPDn2wSoxsabFZw844jnm_Rdqwujkf48cIxMvwV762uDRngH2qxWM_NOnwAk7ZA)

**Firebase**
拆下去結果是空的，下一位

**firestore**
這才是我們要看的，找到源碼後有沒有覺得很眼熟，就和我們在 Java 中寫的一樣
`FirebaseFirestore.getInstance()`
![](https://mqrcnq.bn.files.1drv.com/y4mMqUV2cTWrQufNzko3zgCNbbtbYfvPhEGwE6MOGgeK7g4_7x0nAstDTiBxqC5LwDxx4t5-BwQ8c2-TTaQggg0tks0zq_d1kPVu5j3_VC0Ty43RyF1FLti7T3E5viWDjCXysAFEHj-8wgjQmrW_-bJB2AIh7GifGczQKu0hXfEkjMljZ-ttM2UW6CekgB5apUybaNEsC89BfQ5CnZaAN636A)

那我們再拆拆看這裡的 `FirebaseFirestore` 來自哪裡

最後還是連結到了和 Java 中一樣的 class
![](https://nm8ftq.bn.files.1drv.com/y4myU7ySzGPqYCM6qSct5E0nKnhoYDif4uwsAGsOl7Vw1WfUxZSXHzzmjyGKeo1Reyd7n9Q1nvp4v2LeG5-LGYzSzS9qArYE3atkxhVOJ_RvJfWvLSYTTTFir8Kl03AwfnpjqkIZp4UbI2sa4_08DZxDK6sDaPwhlyTBkethc5ivE0iN23NAXJlRp18ObfAsKeh6Uy3otkVQyj32LY78PS9bg)

---

糖衣 到處都是糖衣.jpg