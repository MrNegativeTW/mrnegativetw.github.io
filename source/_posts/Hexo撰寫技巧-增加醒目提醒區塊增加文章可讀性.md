---
title: Hexo 撰寫技巧 - 增加醒目提醒區塊增加文章可讀性
date: 2018-08-18 10:00:00
tags:
categories:
- Hexo 架設相關
- 文章類
---
純 Markdown 有時候真的會感到不夠用，有些想要特別註解、提醒的地方，常常會讓我陷入猶豫，到底要用 `****` 還是 `>` ，然後就是這麼剛好，在爬 Python 問題時，看到有人的 Hexo 部落格放了各種彩色醒目大框框，這才驚覺，原來這種框框不只是 GitBook v2 特有的，繼續爬文之下，原來這東西叫做 Bootstrap Callout ，至於如何使用，就繼續看下去吧。
<!--more-->
# 使用教學
直接複製下面就可以使用啦，其實在 [NexT 官方文檔](https://theme-next.iissnan.com/tag-plugins.html#bootstrap-callout) 中也有提到這個使用方式，而且在 `note` 當中是支援 md 寫法的，也就是說可以加入字級、粗體等等，不過旁邊得導航會亂掉就是了...

```
{% note default %} 內容 {% endnote %}
{% note primary %} 內容 {% endnote %}
{% note success %} 內容 {% endnote %}
{% note info %} 內容 {% endnote %}
{% note warning %} 內容 {% endnote %}
{% note danger %} 內容 {% endnote %}
```
# 範例
{% note default %} 預設 {% endnote %}
{% note primary %} **主要** {% endnote %}
{% note success %} `成功` {% endnote %}
{% note info %} *資訊* {% endnote %}
{% note warning %} # 警告 {% endnote %}
{% note danger %} 危險 {% endnote %}
