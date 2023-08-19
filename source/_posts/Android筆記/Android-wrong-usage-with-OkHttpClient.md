---
title: 錯誤使用 OkHttpClient 導致的 Too many open files
date: 2023-02-15 10:00:00
tags:
categories:
- Android 筆記
---

這問題困擾我們業務一段時間了，因為業務用的資料量比較大 (檔案數 1800 多個)，每次更新都無法正確下載完成，
但實際客戶因檔案數較少沒遇過這問題。

<!--more-->

# 重現

這 APP 之前 Code 不知道怎寫的 (ninja-code * god class)，在下載檔案時會有超級爆幹頻繁的網路活動，當到了一定數量後就爆炸了。

# 分析

首先看一下崩潰時的 Log， 在 try-catch 中接到 IOException 印出 okHttpClient IOException 前，
可以看到 `AppData::create pipe(2) failed: Too many open files`

```
2023-02-15 11:29:23.511  BufferQueueProducer  E [com.package.name.debug/com.package.name.MainActivity] queueBuffer: fence is NULL
2023-02-15 11:29:23.512  Surface              E  queueBuffer: error queuing buffer to SurfaceTexture, -22
--------- beginning of crash
2023-02-15 11:29:23.512  OpenGLRenderer       A  Encountered EGL error 12299 EGL_BAD_NATIVE_WINDOW during rendering
2023-02-15 11:29:23.512  libc                 A  Fatal signal 6 (SIGABRT), code -6 in tid 3225 (RenderThread)
2023-02-15 11:29:23.553  NativeCrypto         E  AppData::create pipe(2) failed: Too many open files
2023-02-15 11:29:23.554  SomeService          E  okHttpClient IOException 
2023-02-15 11:29:23.554  System.err           W  javax.net.ssl.SSLException: Unable to create application data
```

找到[一篇文章](https://blog.csdn.net/u010687761/article/details/100098138)是這樣寫的：

问题所在为retrofit和okhttp每一次请求都创建新的实例，并且在请求的头部都拿本地存储在SharePreferences的token和其他信息，因此导致频繁请求的时候实例太多文件打开太多而崩溃。


# 解決

仔細看了下原本的 Code

恩，不知道是哪個天才，每次都要 new OkHttpClient()

```java
protected boolean post() {

    // ...
    
    OkHttpClient okHttpClient = new OkHttpClient()
          .newBuilder()
          .connectTimeout(m_nTimeOut,TimeUnit.SECONDS)
          .writeTimeout(m_nTimeOut,TimeUnit.SECONDS)
          .readTimeout(m_nTimeOut,TimeUnit.SECONDS)
          .build();
    }
    
    // ...
}
```

稍微改一下，把 OkHttpClient 拿出來就行囉

```java
private OkHttpClient okHttpClient;

protected boolean post() {
    
    // ...
    
    if (okHttpClient == null) {
        okHttpClient = new OkHttpClient()
            .newBuilder()
            .connectTimeout(m_nTimeOut,TimeUnit.SECONDS)
            .writeTimeout(m_nTimeOut,TimeUnit.SECONDS)
            .readTimeout(m_nTimeOut,TimeUnit.SECONDS)
            .build();
    }
    
    // ...
}
```