---
title: OnTouchListener 與 MediaController 同時使用的坑
date: 2023-10-17 10:00:00
tags:
categories:
- Android 筆記
---

因公司的開發需求，VideoView 的 setOnTouchListener 有做 `ACTION_DOWN` `ACTION_UP` 的動作處理，這時加入 MediaController 若沒弄好 return true/false 就容易發生點擊 VideoView 時 MediaController 會不正常跳動的問題

<!--more-->

加入 MediaController 的 Code 是這樣，沒有多做處理

```java
MediaController mediaController = new MediaController(this);
mediaController.setAnchorView(videoView);
if (isShowMediaController) {
    videoView.setMediaController(mediaController);
} else {
    videoView.setMediaController(null);
}
```

以我們公司產品為例，發生 `ACTION_DOWN` `ACTION_UP` 時 return true, 其他狀況 return false

```java
videoView.setOnTouchListener(new OnSwipeTouchListener(getBaseContext()) {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if (event.getAction() == MotionEvent.ACTION_DOWN) {
                    // do something
                    return true;
                } else if (event.getAction() == MotionEvent.ACTION_UP) {
                    // do something
                    return true;
                }
                return false;
            }
        });
```

但是在 OnTouchLintener 這邊 
- return 為 false 時 -> 繼續往下觸發 onTouchEvent
- return 為 true 時 -> 不會往下觸發 onTouchEvent

我們來翻翻 Android 的源碼，VideoView 在 onTouchEvent 也有處理 `ACTION_DOWN` 顯示 MediaController 的邏輯，

```java
package android.widget;

public class VideoView extends SurfaceView implements MediaPlayerControl, SubtitleController.Anchor {
  
    // ...
  
    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        if (ev.getAction() == MotionEvent.ACTION_DOWN
                && isInPlaybackState() && mMediaController != null) {
            toggleMediaControlsVisiblity();
        }
        return super.onTouchEvent(ev);
    }
}
```

既然 VideoView 自己有處理 MediaController 的顯示隱藏，那就全部交給它就好啦！

```java
// 為兼容 MediaController 顯示隱藏，onTouch 一律 return true，
// 由 ACTION_DOWN 觸發 onTouchEvent
videoView.setOnTouchListener(new OnSwipeTouchListener(getBaseContext()) {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if (event.getAction() == MotionEvent.ACTION_DOWN) {
                    // do something
                    videoView.onTouchEvent(event);
                } else if (event.getAction() == MotionEvent.ACTION_UP) {
                    // do something
                }
                return true;
            }
        });
```

雖然這樣感覺很怪，但實際使用的感覺就是好很多，MediaController 不會在手碰到 VideoView 後一直亂閃

---

**Refer:**

[Android onTouchEvent和setOnTouchListener中onTouch的区别](https://blog.csdn.net/shineflowers/article/details/41080435)