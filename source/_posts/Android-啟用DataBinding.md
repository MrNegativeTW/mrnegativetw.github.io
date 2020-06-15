---
title: Android 筆記 - MVVM、LiveData、DataBinding
date: 2020-06-15 10:00:00
tags:
categories:
- Android 筆記
---
很亂嗎？沒關係，把它們混起來做撒尿牛丸吧。
<!--more-->

# MVVM

# Live Data

# Data Binding
## 啟用
在 build.gradle(Module: app) 中加入
```
apply plugin: 'kotlin-kapt'

android {
    // ...
    buildFeatures {
        dataBinding = true
    }
}
```

## 改寫 Layout
將現有 Layout 使用 `<layout>` 標籤包起來，並加入 `<data>` 標籤，如下所示：
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

## 用法
接著在 Layout 中的 text 欄位就可以使用 `@{user.firstName}` 這種格式取代了
```xml
<TextView 
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@{user.firstName}"
    tools:text="PlaceHolder" />
```
順帶一提，使用 `@{}` 後 Layout 預覽中的文字會直接不見，所以可以使用 `tools:text` 屬性指定 Place holder，其他 tools 屬性請參考 [Tools attributes reference](https://developer.android.com/studio/write/tool-attributes#tools_instead_of_android)


# 注意事項
LiveData 和 DataBinding 擇一使用即可，也就是下列選一：
- 在 Activity / Fragment 用 observe 的更新 UI
```
viewModel.name.observe(this, object : Observer<String> {
            override fun onChanged(name: String) {
                viewModel.updateName(name)
            }
})```

- 在 layout.xml 用 @{} 的更新 UI
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