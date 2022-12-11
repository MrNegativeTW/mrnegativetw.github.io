---
title: Android 筆記 - OutOfMemory 解決經驗談
date: 2022-12-10 10:00:00
tags:
categories:
- Android 筆記
---

神奇的 OutOfMemory，以前一直以為只有記憶體被吃爆會遇到，沒想到 threads 太多也會噴這個錯。

<!--more-->

# 記憶體爆炸

先分享最直覺的：記憶體爆炸就真的是記憶體爆炸了。

物件沒有好好回收，或者傳了不該傳的物件進去，各種情況都可能造成，導致 GC 時無法回收，久而久之就爆炸了。

透過 Android Studio 中的 Profiler 觀察記憶體的話，應該可以看到每個一段時間或是特定操作後，記憶體會往上漲，並且一去不復返。

## 案例分享

這是公司的主力產品，症頭是在內容一樣的狀況下，發生 OutOfMemory 的間隔幾乎是精準到秒，花了 1.5 天做各種測試改各種 code 後，最後產出 3 行解決這個大麻煩。

這個 OOM 總是發生在 PagerAdapter 的 instantiateItem 中，而且每次增加記憶體用量時都是 16MB。
因產品需求，沒有使用 FragmentPagerAdapter 或是 FragmentStatePagerAdapter，而是繼承 PagerAdapter 實作自己的。

繼承 PagerAdapter 的話有幾個必須要 Override 的 function

- `instantiateItem`
- `destroyItem`
- `getCount`
- `isViewFromObject`

然而某些原因，每次在 `new PagerAdapter()` 時並不會呼叫 `destroyItem`，導致舊的 item 就這樣被棄用，解決方法是除了 `removeAllViews()` 之外，記得把 adapter 設定成 null，到此為止解決了 16MB 中的 8MB。

```java
listTmp.removeAllViews();
listTmp.setAdapter(null);
```

而剩下的 8MB 呢？就在我將客製化的 PagerAdapter 看過一遍時，發現當初竟然將 ViewPager 傳進了 PagerAdapter，而且還沒有用到，這操作我真的看得一頭霧水，總之將他從 PagerAdapter 的 Constructor 中移除後，OOM 問題就被解決了。

# Threads 太多


這個狀況真的比較罕見，出現的錯誤訊息通常是 `pThread_create failed`，

```log
pthread_create (1040KB stack) failed: Try again
...
```

透過 Android Studio 中的 Profiler 觀察 CPU 的 Threads 數量，可以看到每個一段時間或是特定操作後，都會有新的 Thread 跑出來，而且不會結束，會呈現 Sleeping 的狀態，久而久之 Threads 數量就會達到上百上千，然後就 Crash 了。

而我遇到的狀況是這樣的：

參考連結：[不可思议的OOM](https://www.jianshu.com/p/e574f0ffdb42)

## 案例分享

一樣是公司的主力產品，這次有用到各種 SoundPool 之類的東西，然而每次 new 新物件時，舊物件中的 SoundPool 都沒有被正確結束掉，久而久之就產生了一大堆在 Sleeping 狀態的 SoundPool Thread。

找到正確的點，在物件銷毀前將 SoundPool release 就解決問題了！
