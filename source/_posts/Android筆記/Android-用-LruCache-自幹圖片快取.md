---
title: Android 筆記 - 用 LruCache 自幹圖片快取
date: 2022-05-10 10:00:00
tags:
categories:
- Android 筆記
---

前陣子投了某公司的 Junior Android Developer，過了莫約一週原以為沒消息了，沒想到竟然發了作業給我，其中一項要求是要將讀取過的圖片快取，且不能使用 Picasso 或 Glide 等 Lib。

自幹圖片快取對我來說最有挑戰，畢竟以前都無腦用 Glide，所以這篇文章就紀錄一下如何用 LruCache 重新造輪子。

<!-- more -->

# 關於 LruCache

在 [Caching Bitmaps | Android Developers](https://developer.android.com/topic/performance/graphics/cache-bitmap) 文件中提到兩種快取：

- LruCache：快取在 Memory 中
- DiskLruCache：快取在磁碟中

這個小作業我只用了 LruCache。

真的實作上有很多顯示圖片的方法，像我在 Fragment 與 RecyclerView 中用的方法就不同了。

# 關於這個 App

- Single Activity
- 架構是 MVVM
- 使用 Data Binding
- 使用 XML layout，不是 Jetpack Compose
- 有一個 `Photo` data class 大概長這樣：

    ```kotlin
    @Parcelize
    data class Photo(
        @SerializedName("albumId") var albumId: Int? = null,
        @SerializedName("id") var id: Int? = null,
        @SerializedName("title") var title: String? = null,
        @SerializedName("url") var url: String? = null,
        @SerializedName("thumbnailUrl") var thumbnailUrl: String? = null
    ): Parcelable
    ```
-  有一個 `DownloadImageTask` class 長這樣，用來從網路下載圖片的：

    ```kotlin
    class DownloadImageTask {
        fun getBitmapFromUrl(url: String): Bitmap? {
            var bitmap: Bitmap? = null
            try {
                val conn = URL(url).openConnection()
                conn.setRequestProperty("User-Agent", "FILL_YOUR_AGENT_HERE")
                bitmap = BitmapFactory.decodeStream(conn.getInputStream())
            } catch (e: IOException) {
                e.printStackTrace()
            }
            return bitmap
        }
    }
    ```

# 建立 LruCache

首先需要開一個新檔 `ImageCache.kt`，用來存放 LruCache 的邏輯。

```kotlin
package com.your.app

import android.graphics.Bitmap
import android.util.LruCache

class ImageCache {

    companion object {
        val newInstance = ImageCache()
    }

    private lateinit var memoryCache: LruCache<String, Bitmap>

    fun initCache() {
        val maxMemory = (Runtime.getRuntime().maxMemory() / 1024).toInt()
        val cacheSize = maxMemory / 8
        memoryCache = object : LruCache<String, Bitmap>(cacheSize) {
            override fun sizeOf(key: String, bitmap: Bitmap): Int {
                return bitmap.byteCount / 1024
            }
        }
    }

    fun addImageToMemoryCache(key: String, bitmap: Bitmap) {
        if (getImageFromMemoryCache(key) == null) {
            memoryCache.put(key, bitmap)
        }
    }

    fun getImageFromMemoryCache(key: String): Bitmap? {
        return memoryCache.get(key)
    }

    fun removeImageFromMemoryCache(key: String) {
        memoryCache.remove(key)
    }

    fun clearCache() {
        if (memoryCache != null) {
            memoryCache.evictAll()
        }
    }

}
```

需要特注意的是，由於 LruCache 是存放在 Memory 中，因此若在 Fragment 中初始化，退出 Fragment 時 instance 就會被銷毀，因此建議在 Activity 或 Activity ViewModel 中初始化，這樣就可以在整個 app 的生命週期中存活。

```kotlin
// MainActivity
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // ...

    imageCache = ImageCache.newInstance
    imageCache.initCache()
}
```

之後需要使用 LruCache 時只要呼叫 `ImageCache.newInstance` 就可以取得已初始化的 LruCache。


# 在 Fragment 實作

這邊設計是：前一個 Fragment 會傳一個 Parcelable 的 `Photo` 進來，這個 Fragment 在初始化時會使用 ViewModelFactory 將 `Photo` 傳進去。

1. 使用 `someFragmentViewModel.initImageCache()` 將 ViewModel 中的 `ImageCache` 初始化，也就是拿一個 instance

2. 使用 `someFragmentViewModel.loadImage()` 叫 ViewModel 開始讀圖片。首先會從 `ImageCache` 中找是否有一樣 key 值的檔案，若有則取出並設為圖片；若無則啟動 Coroutine 抓取圖片，並存至快取才設為圖片

3. Fragment 會 observe ViewModel 中的數值，一有風吹草動就把圖片設定到 `ImageView` 上

```kotlin
// SomeFragment.kt

override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
    // ...

    someFragmentViewModel.initImageCache()
    someFragmentViewModel.loadImage()

    subscribeUi()

    // ...
}

private fun subscribeUi() {
    thirdViewModel.image.observe(viewLifecycleOwner) {
        binding.imageViewSomeFragPhoto.setImageBitmap(it)
    }
}
```

```kotlin
// SomeViewModel.kt

class SomeViewModel(var photo: Photo) : ViewModel() {
    
    private lateinit var imageCache: ImageCache

    val photos = MutableLiveData(photo)
    val image = MutableLiveData<Bitmap>()

    fun initImageCache() {
        imageCache = ImageCache.newInstance
    }

    fun loadImage() {
        val url = photos.value?.thumbnailUrl!!
        val cachedBitmap = imageCache.getImageFromMemoryCache(url)
        
        if (cachedBitmap != null) {
            image.value = cachedBitmap!!
        } else {
            viewModelScope.launch(Dispatchers.Main) {
                withContext(Dispatchers.IO) {
                    val bitmap = DownloadImageTask().getBitmapFromUrl(url)
                    if (bitmap != null) {
                        imageCache.addImageToMemoryCache(url, bitmap)
                    }
                    bitmap
                }.run {
                    image.postValue(this)
                }
            }
        }
    }
}

class SomeViewModelFactory(private val photo: Photo) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(SomeViewModel::class.java)) {
            @Suppress("UNCHECKED_CAST")
            return SomeViewModel(photo) as T
        }
        throw IllegalArgumentException("Unknown viewModel class")
    }
}
```

至於 xml 沒啥特別的，就是一個基本的 `ImageView` tag。

# 在 RecyclerView 實作

RecyclerView MVVM 的架構可以參考官方的 [Sunflower](https://github.com/android/sunflower) 專案，因為在 Adapter 中已經將資料綁定到 list_item_photo 中的 photo，所以在 Adapter 中我們不用寫任何設定圖片的步驟，反之是將其移到 Binding Adapter 中。

```kotlin
// PhotoAdapter.kt
class PhotoAdapter : 
    ListAdapter<Photo, RecyclerView.ViewHolder>(PhotoDiffCallback()) {

        // ...

    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        val photo = getItem(position)
        (holder as PhotoViewHolder).bind(photo)
    }

    class PhotoViewHolder(private val binding: ListItemPhotoBinding) :
        RecyclerView.ViewHolder(binding.root) {

        fun bind(item: Photo) {
            binding.apply {
                photo = item
                executePendingBindings()
            }
        }
    }

}
```

```xml
// list_item_photo.xml

// Data Binding 的必需品，將 Photo data class 放進來
<data>

    <variable
        name="photo"
        type="com.txwstudio.app.cloudinteractive_trevor.data.Photo" />
</data>

// 一個簡單的 ImageView，會將 photo.thumbnailUrl 傳入 Binding Adapter
<ImageView
    android:id="@+id/imageView_listItemPhoto_photo"
    android:layout_width="0dp"
    android:layout_height="0dp"
    android:scaleType="fitCenter"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintDimensionRatio="H,1:1"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent"
    app:loadImageFromUri="@{photo.thumbnailUrl}" />
```

```kotlin
// ImageBindingAdapter.kt

@BindingAdapter("loadImageFromUri")
fun loadImageFromUri(view: ImageView, uri: String) {
    val imageCache = ImageCache.newInstance
    val cachedBitmap = imageCache.getImageFromMemoryCache(uri)

    if (cachedBitmap != null) {
        view.setImageBitmap(cachedBitmap)
    } else {
        GlobalScope.launch(Dispatchers.Main) {
            view.setImageResource(R.drawable.ic_baseline_photo_24)
            withContext(Dispatchers.IO) {
                val bitmap = DownloadImageTask().getBitmapFromUrl(uri)
                if (bitmap != null) imageCache.addImageToMemoryCache(uri, bitmap)
                bitmap
            }.run {
                view.setImageBitmap(this)
            }
        }
    }
}
```