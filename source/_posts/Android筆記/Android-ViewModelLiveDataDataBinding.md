---
title: Android 筆記 - ViewModel、LiveData、DataBinding
date: 2020-06-17 10:00:00
tags:
categories:
- Android 筆記
---

一、兩個還好，但三個就很亂，但沒關係，我們把它們混起來做撒尿牛丸吧。

<!--more-->

本文章使用 `UserFragment` 和 `UserViewModel` 作為範例。

# ViewModel

如何新增 ViewModel 呢？手動太麻煩了，直接右鍵 New 一個 Activity/Fragment with ViewModel，讓 Android Studio 幫我們建立 `Fragment` 和他的 `ViewModel` 吧。

建立完成之後需要修改一些已經被棄用的東西，進入 `Fragment` 將 `ViewModelProviders.of(this)` 改成 `ViewModelProvider(this)`，像這樣：

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    // ...
    val userViewModel = ViewModelProvider(this).get(UserViewModel::class.java)
    // ...
}
```

# Live Data

進入剛建立的 UserViewModel，將數值使用 `MutableLiveData` 包起來。

關於 `LiveData` 和 `MutableLiveData` 的差異，請見：[StackOverflow](https://stackoverflow.com/a/46814399/9412238)

```kotlin
class UserViewModel : ViewModel() {
    val user = MutableLiveData<String>()
}
```

然後就可以在 `Fragment` 裡面 observe 這筆資料了。 

```kotlin
userViewModel.user.observe(this, Observer {
    textView.text = it?.name
})
```

# Data Binding

## 啟用
在 build.gradle(Module: app) 中加入
```
apply plugin: 'kotlin-kapt'
```

與

```gradle
android {
    // ...
    buildFeatures {
        dataBinding = true
    }
}
```

## 改寫 Layout

將現有 layout XML 檔的最外層使用 `<layout>` 標籤包起來，並加入 `<data>` 標籤，如下所示：
```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>
       <variable 
            name="user" 
            type="com.example.User" />
   </data>

   // Original Layout

</layout>
```

`<variable>` 中的：
- `name`：此 ViewModel 在此 Layout 中的別名
- `type`：你要引入的 ViewModel

接著在 Layout 中的 text 欄位就可以使用 `@{user.firstName}` 這種格式取代了

```xml
<TextView 
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{user.firstName}"
    tools:text="PlaceHolder" />
```

順帶一提，使用 `@{}` 後 Layout 預覽中的文字會直接不見，所以可以使用 `tools:text` 屬性指定 Place holder，其他 tools 屬性請參考 [Tools attributes reference](https://developer.android.com/studio/write/tool-attributes#tools_instead_of_android)

## Binding 檔案到底在哪裡

很多教學都說：「設定好 Data Binding之後會自動產生 Bidning 的 Class」，但這個檔案不管如何在列表中都沒看到，實際打 Code 好像也不會自動引入？
實際上此檔案會位於後方帶有 `(generate)` 的資料夾中，所以平常在檔案列表是找不到的，至於引入方法可以在要引入的檔案最上面輸入 
```kotlin
import com.YOUR.PACKAGE
```

接著在 code 中就能引入了

```kotlin
val binding = DataBindingUtil.setContentView<ActivityViewModelBinding>(this, R.layout.activity_view_model)
binding.viewModel = userViewModel
binding.setLifecycleOwner(this)
```

# 注意事項

LiveData 和 DataBinding 擇一使用即可，也就是下列選一：

1. 在 Activity / Fragment 用 observe 更新 UI

```kotlin
viewModel.name.observe(this, object : Observer<String> {
            override fun onChanged(name: String) {
                viewModel.updateName(name)
            }
})
```

2.  在 layout.xml 用 `@{}` 更新 UI

```xml
<TextView 
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{user.firstName}" />
```

# 撒尿牛丸
這三者都是屬於 [Android JetPack](https://developer.android.com/jetpack) 中的 Architecture，借張圖表示：
![jetpack_donut](https://raw.githubusercontent.com/android/sunflower/master/screenshots/jetpack_donut.png)

Android 官方有個 App 完美展現了 Jetpack 的應用：
**[android](https://github.com/android)** / **[sunflower](https://github.com/android/sunflower/)**

# 參考資料

- [Day 15 LiveData 介紹與使用](https://ithelp.ithome.com.tw/articles/10222799)
- [Android MVVM探索系列](https://juejin.im/post/5bd6acd1e51d457a976637c3#heading-1)