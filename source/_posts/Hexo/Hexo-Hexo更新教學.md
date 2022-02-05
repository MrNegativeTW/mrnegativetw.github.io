---
title: Hexo 架設教學 - 如何更新 Hexo 版本
date: 2020-06-15 10:00:00
tags:
categories:
- Hexo 架設相關
---
教你如何更新 Hexo 的版本
<!--more-->
# 確認新版本
打開 Terminal 輸入 `npm outdated`
```
Package                  Current  Wanted  Latest  Location
hexo-deployer-git          0.3.1   0.3.1   2.1.0  hexo-site
hexo-generator-archive     0.1.5   0.1.5   1.0.0  hexo-site
hexo-generator-category    0.1.3   0.1.3   1.0.0  hexo-site
hexo-generator-index       0.2.1   0.2.1   1.0.0  hexo-site
hexo-generator-sitemap     1.2.0   1.2.0   2.0.0  hexo-site
hexo-generator-tag         0.2.0   0.2.0   1.0.0  hexo-site
hexo-renderer-ejs          0.3.1   0.3.1   1.0.0  hexo-site
hexo-renderer-marked       0.3.2   0.3.2   2.0.0  hexo-site
hexo-renderer-stylus       0.3.3   0.3.3   1.1.0  hexo-site
hexo-server                0.2.2   0.2.2   1.0.0  hexo-site
```

找到 package.json 並將 dependencies 中版本更新，請注意這個不要動：
```json
"hexo": {
    "version": "4.2.1"
}
```

接著 terminal 執行 `npm update`

輸入 `hexo -version` 可以確認版本