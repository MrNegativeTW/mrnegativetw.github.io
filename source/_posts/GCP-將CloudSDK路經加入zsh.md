---
title: 將 Cloud SDK 路徑加入 ZSH 中
date: 2020-06-16 10:00:00
tags:
categories:
- Google Cloud Platform
---
換到 ZSH 之後 `gcloud` 指令就不管用了嗎？這篇教你把指令找回來。
<!--more-->
# 編輯環境檔
編輯位於家目錄底下的 `.zshrc`，使用任何文字編輯器打開定加入以下片段：

```bash
# The next line updates PATH for the Google Cloud SDK.
source /Users/trevor/google-cloud-sdk/path.zsh.inc

# The next line enables zsh completion for gcloud.
source /Users/trevor/google-cloud-sdk/completion.zsh.inc
```