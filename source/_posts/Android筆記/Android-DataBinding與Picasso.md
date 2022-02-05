---
title: Android 筆記 - Data Binding 搭配 picasso 使用
date: 2020-06-20 10:00:00
tags:
categories:
- Android 筆記
---
找了很多教學都只有貼程式碼，沒看到比較詳細的流程說明，所以這篇就來說明一下流程吧。
<!--more-->
不廢話直接上 Code
# Code
## CustomViewModel
1. 依照你的狀況照常設定變數
2. Uri 為空時將變數設定為 `Uri.EMPTY`
```kotlin
var photoUrl = MutableLiveData<Uri>()

// ...
if (firebaseUser != null) {
    this.photoUrl.value = firebaseUser.photoUrl
} else {
    this.photoUrl.value = Uri.EMPTY
}

// ...
```

## ImageBindingAdapter
1. 須為 object，非 class
2. 使用 `@JvmStatic` 及 `@BindingAdapter("")`
```kotlin
object ImageBindingAdapter {
    @JvmStatic
    @BindingAdapter("imageUrl", "error")
    fun setImageUrl(view: ImageView, url: Uri, notSignedIn: Drawable) {
        Picasso.get()
                .load(url)
                .error(notSignedIn)
                .into(view)
    }
}
```

## fragment.xml
1. `<data>` 標籤中僅需 ViewModel 即可，不用 `ImageBindingAdapter`
2. 使用 `app:CUSTOM_ATTR_HERE` 來使用自訂的 Setter
```xml
<ImageView
    ...
    app:imageUrl="@{viewModel.photoUrl}"
    app:error="@{@drawable/ic_launcher}">
```


# 運作流程
因為在 `ImageBindingAdapter` 裡面使用 `BindingAdapter` 指定了 xml 中的屬性，所以
1. xml 中監聽到 `ViewModel` 中的變化時
2. 呼叫 `ImageBindingAdapter` 中定義 `BindingAdapter` 的 Function
3. 傳入 xml 中自定義的 `@{viewModel.photoUrl}` 數值
4. 執行 Function 內容，也就是使用 Picasso 將傳過來的 `@{viewModel.photoUrl}` 載入 ImageView

好了，希望可以幫助到也搞不懂原理的你。