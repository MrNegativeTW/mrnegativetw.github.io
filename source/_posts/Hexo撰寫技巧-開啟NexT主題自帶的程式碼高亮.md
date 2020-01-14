---
title: Hexo 撰寫技巧 - 開啟 NexT 主題自帶的程式碼高亮
date: 2018-09-04 10:00:00
tags:
categories:
- Hexo 架設相關
- 撰寫技巧
---
程式碼區塊就是要高亮啊，不然要幹嘛？
<!--more-->
# Hexo 設定
找到 Hexo 配置文件 `_config.yml` 並打開
接著找到下面這坨，並且把前三項修改為 `true`
```yml
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
```
# NexT 設定
找到 NexT 配置文件 `_config.yml` 並打開
找到下面這行，並改成自己喜歡的高亮主題
```yml
highlight_theme: night
```
NexT 提供 5 種主題，隨意挑選 歡迎試用
normal | night | night eighties | night blue | night bright
# 撰寫設定
記得使用程式碼區塊，並於頂端加入所使用的語言，例如：
```Python
```Python
import wow

```
