---
title: Android 筆記 - 導入 GeckoView 並啟用 Autoplay
date: 2022-06-24 10:00:00
tags:
categories:
- Android 筆記
---

最近公司指派了一項專案給我，就是要幫捷運看板寫個 Android App，然後在裡面放 WebView 讓他可以接到前端顯示資訊，一開始還在想：簡單！

結果 Android WebView 直接擺爛不播放 HTML5 中的 `<video>` tag，Stack Overflow 上方法試過一輪了，怎麼搞他就是不播放，最後只好導入  GeckoView，導入後 APK 暴增至 500 MB 又是另一個故事了

---

你可以看這裡 [Getting Started with GeckoView¶](https://firefox-source-docs.mozilla.org/mobile/android/geckoview/consumer/geckoview-quick-start.html)，看不懂再來看我文章

# 設定 Gradle

文件說可以到 [Maven Repository](https://maven.mozilla.org/?prefix=maven2/org/mozilla/geckoview/) 去查你要的 `channel` 和 `version`，`channel` 要選好，如果不選的話 APK 會直上 500MB

舉例來說：
我開發的目標機器有支援 ARM v8，而且我想要最新的穩定版，所以：

- `channel` 我選擇 `arm64-v8a`
- `version` 我選擇 `102.0.20220623063721`

這樣一來 APK 剩下 145MB，至於為什麼會暴漲？因為 GeckoView 等於把一個瀏覽器塞進去你的 APP 中。

```gradle
ext {
    geckoviewChannel = <channel>
    geckoviewVersion = <version>
}
```

```gradle
repositories {
    maven {
        url "https://maven.mozilla.org/maven2/"
    }
}
```

最新版貌似不支援使用 `JavaVersion.VERSION1_8` 編譯，所以使用 11

```
compileOptions {
    sourceCompatibility JavaVersion.VERSION_11
    targetCompatibility JavaVersion.VERSION_11
}
```

```
dependencies {
    // ...
  implementation "org.mozilla.geckoview:geckoview-${geckoviewChannel}:${geckoviewVersion}"
}
```

# 加入 GeckoView

在 layout 的 xml 中加入

```xml
<org.mozilla.geckoview.GeckoView
    android:id="@+id/geckoView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

接著因 GeckoRuntime 只能有一個實體，所以創立一個 companion object 給他，並指向 `null`

```kotlin
companion object {
    private var geckoRuntime: GeckoRuntime? = null
}
```

最後稍微設定一下就能用啦

因為我的專案有開啟 ViewBinding，所以設定 session 時使用 `binding.geckoView`

```kotlin
private fun setUpGeckoView() {
    if (geckoRuntime == null) {
        geckoRuntime = GeckoRuntime.create(requireContext())
    }
    geckoSession.contentDelegate = object : GeckoSession.ContentDelegate {}
    geckoSession.open(geckoRuntime!!)
    binding.geckoView.setSession(geckoSession)
}
```

# 啟用 Autoplay

GeckoView 預設是不會自動播放影片的，就算你的 HTML 中加入了 `autoplay` 或 `autoplay="autoplay"`，他都不會理你

所以除了 `contentDelegate` 之外，還需要加入 `permissionDelegate`，當收到 `PERMISSION_AUTOPLAY_AUDIBLE` 或 `PERMISSION_AUTOPLAY_INAUDIBLE` 時回傳 `ALLOW` 讓影片能正常播。

```kotlin
geckoSession.permissionDelegate = object : GeckoSession.PermissionDelegate {
    override fun onContentPermissionRequest(
        session: GeckoSession,
        perm: ContentPermission
    ): GeckoResult<Int>? {
        when (perm.permission) {
            GeckoSession.PermissionDelegate.PERMISSION_AUTOPLAY_AUDIBLE,
            GeckoSession.PermissionDelegate.PERMISSION_AUTOPLAY_INAUDIBLE -> {
                return GeckoResult.fromValue(ContentPermission.VALUE_ALLOW)
            }
        }
        return super.onContentPermissionRequest(session, perm)
    }
}
```