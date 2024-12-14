---
title: Hexo 撰寫技巧 - 增加醒目提醒區塊增加可讀性
date: 2018-08-18 10:00:00
tags:
categories:
- Hexo 架設相關
---
這東西叫做 Bootstrap Callout
{% note default %} 範例 {% endnote %}
<!--more-->
# 使用教學
在 [NexT 官方文檔](https://theme-next.iissnan.com/tag-plugins.html#bootstrap-callout) 中也有提到這個使用方式，而且在 `note` 當中是支援 md 寫法的，也就是說可以加入字級、粗體等等，不過旁邊得導航會亂掉就是了...

```
{% note default %} 內容 {% endnote %}
{% note primary %} 內容 {% endnote %}
{% note success %} 內容 {% endnote %}
{% note info %} 內容 {% endnote %}
{% note warning %} 內容 {% endnote %}
{% note danger %} 內容 {% endnote %}
```

{% note default %} 預設 {% endnote %}
{% note primary %} **主要** {% endnote %}
{% note success %} `成功` {% endnote %}
{% note info %} *資訊* {% endnote %}
{% note warning %} # 警告 {% endnote %}
{% note danger %} 危險 {% endnote %}
