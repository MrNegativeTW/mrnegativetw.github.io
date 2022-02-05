---
title: ffmpeg 超快速轉檔指令教學
date: 2018-09-05 10:00:00
tags:
category: 無法分類
---
ffmpeg 指令貌似複雜，每次都忘，所以乾脆寫一篇給我自己看

<!--more-->

# 一句入魂
```
ffmpeg -i input.mkv ouput.mp4
```

將要轉的檔案複製到 `bin` 資料夾下，也就是 ffmpeg 主程式的所在地，然後將檔案重新命名成超短的名稱，例如：`1.mkv`、`87.mp4` 等等
接著輸入如上指令，CPU 就會開始火力全開火爆轉檔，看著 Ryzen 滿載心裡有一種莫名的爽感啊~

# 實際範例
我有時會從 Youtube 下載 4K 影片，可是 Premiere 不吃 mkv 檔案，所以只好透過 ffmpeg 轉成 mp4

```
ffmpeg -i 1.mkv 1.mp4
```
