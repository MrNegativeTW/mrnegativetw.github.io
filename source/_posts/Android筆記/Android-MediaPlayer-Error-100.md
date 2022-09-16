---
title: Android 筆記 - MediaPlayer Error 100,0
date: 2022-09-16 10:00:00
tags:
categories:
- Android 筆記
---

在系統廠與接案廠的綜合體之中，總能遇到很多正規 Android 手機不會遇到的問題，像這次 MediaPlayer 掛掉的原因連 MediaPlayer 自己都不知道

<!--more-->

## 前言

這個案子是某醫院的自助報到機，他使用 WebView 的 JavascriptInterface 來觸發 Android App 中的 TTS，而且不是使用 Google TTS，而是使用 Cyberon TTS。

這 Bug 行為也很妙，他會在播放語音的 n 次之後出現爆音，然後就再也沒聲音了。至於這個 n 是幾次呢？有可能是 30 也有可能是 800。

還記得我那天出差到醫院去，從中午坐在那邊搞到晚上 6 點。

## Log

這問題也很冷門，Google 一下之後找到的文章寥寥無幾，而且苦主是 Nexus。

就讓我們來看一下出問題時的 Log 長怎樣吧

首先會有 media server died，接著是 error(100, 0)，然後就沒聲音了。

```
...
2022-08-22 14:12:59.270 21001-21034 AudioSystem             com.package.release  W  AudioFlinger server died!
2022-08-22 14:12:59.271 21001-21018 IMediaDeathNotifier     com.package.release  W  media server died
2022-08-22 14:12:59.271 21001-21018 MediaPlayer             com.package.release  E  error (100, 0)
2022-08-22 14:12:59.271 21001-21018 MediaPlayer             com.package.release  E  error (100, 0)
2022-08-22 14:12:59.271 21001-21021 AudioSystem             com.package.release  W  AudioPolicyService server died!
2022-08-22 14:12:59.274 21001-21001 ServiceManager          com.package.release  I  Waiting for service media.audio_flinger...
...
```

## 99% 完美的解決辦法

重設 MediaPlayer。對。


MediaPlayer 有一個 onErrorListener，在這邊可以完美收到 `error (100, 0)` 這個錯誤，所以在錯誤發生時將 MediaPlayer 殺掉就可以了。

Code 大概會長這樣：

```java
private class PlayBellTask extends AsyncTask<String, String, Void> {

    // ...

    public void playBellSound() {

        // ...

        mp.setOnErrorListener(new MediaPlayer.OnErrorListener() {
            @Override
            public boolean onError(MediaPlayer mediaPlayer, int what, int extra) {
                Log.e(TAG, "PlayBellTask() playBellSound() onError(), what: " + what + ", extra: " + extra);
                isPlaying = false;
                mediaPlayer.stop();
                mediaPlayer.reset();
                mp.release();
                mp = null;
                return false;
            }
        });

        // ...
        
    }

    // ...

}
```