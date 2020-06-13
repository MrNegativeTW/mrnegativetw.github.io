---
title: Android 筆記 - 客製化 RecyclerView 使用 Kotlin
date: 2020-04-07 10:00:00
tags:
categories: 
- Android 筆記
---
這篇文章使用教你如何自訂 RecyclerView 內的元件，使用 Kotlin。
<!-- more -->
# 成果圖
![]()

# 如何自訂 RecyclerView 的 Layout
## 官方指南
基本上來說都是參考 Android 官方開發指南：[Create a List with RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview)
大約把前面部分完成後就可以接著下一步了

## 新增自訂的 Layout
在 `res > layout` 資料夾右鍵，選擇 `` 建立給 RecyclerView 用的新 Layout

## 將自訂的 Layout 與 Adapter 串接
接著修改從官方指南建立的 Adapter，找到以下這段程式碼
```kotlin
class MyViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView)
```

並依照你自訂的 Layout 修改 Adapter，例如：
```kotlin
class MyViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        var situationType = itemView.textView_accidentCard_situationType as TextView
        var location = itemView.textView_accidentCard_location as TextView
        var situation = itemView.textView_accidentCard_situation as TextView
        var time = itemView.textView_accidentCard_time as TextView
        var userName = itemView.textView_accidentCard_userName as TextView
    }
```

## Subtitle Here
修改 `onCreateViewHolder`，移除句尾的 `as TextView`，像這樣：
```kotlin
val textView = LayoutInflater.from(parent.context)
                .inflate(R.layout.my_text_view, parent, false)
```

## 顯示資料
修改 `onBindViewHolder` 讓資料得以顯示在自訂 Layout 中
找到
```kotlin

```

並依個人需求修改，例如：
```kotlin
override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.textView999.text = mutableList[position].fieldOne.toString()
}
```

## 測試資料

```kotlin

```