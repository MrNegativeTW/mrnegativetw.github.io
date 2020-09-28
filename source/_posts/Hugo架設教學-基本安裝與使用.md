---
title: Hugo 架設教學 - 基本安裝與使用
date: 2020-03-22 10:00:00
tags:
categories:
- Hugo 筆記
---
這篇文章會教你如何把 Hugo 從無到有架設起來。
<!--more-->
# 安裝
## Windows
{% note info %}
本篇文章 Windows 部分皆使用 PowerShell 操作。
{% endnote %}

Windows 下分為「使用套件包管理員」及「手動下載 Release」兩種安裝方式，本文使用套件包管理員的安裝方式，省事快速方便。

首先安裝 Chocolatey：[官網](https://chocolatey.org/install#individual)
基本上就是用**管理員**權限打開 PowerShell 並輸入
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

安裝完成後重新開啟 PowerShell 並輸入
```
choco install hugo -confirm
```
安裝完成後一樣重啟 PowerShell 即可使用 `hugo` 指令確認安裝是否成功。

## macOS
未安裝，未來會更新

# 建立網站
因為我打算使用 Gitlab 來發布 Hugo，所以我選擇在 Gitlab 的資料夾建立網站，不過先不用急著新增資料夾，先 cd 進去就好，例如 `cd ~/Documents/GitLab`，接著請 Hugo 建立新網站
```
hugo new site username.gitlab.io
```
最後一個 `username.gitlab.io` 為新網站資料夾名稱，指令完成後 cd 進入。

# 尋找主題
首先至 [Hugo Themes](https://themes.gohugo.io/) 找個中意的主題使用
{% note info %}
個人使用：[Dev Resume](https://themes.gohugo.io/hugo-devresume-theme)
還有一個看起來也不錯：[Hugo Coder](https://themes.gohugo.io/theme/hugo-coder/)
{% endnote %}

找到主題後可以使用如下 git 指令將主題安裝進來，或者照主題說明檔安裝。
```
git clone https://github.com/cowboysmall-tools/hugo-devresume-theme.git themes/devresume
```

## 基本設定
使用編輯器開啟網站根目錄的 `config.tmol`
前三行修改為自己的設定，再外加一項 `theme` 指定使用的主題
主題名稱為稍早的 git 指令第四項參數(`themes/devresume`) 的資料夾名稱 

```yml
baseURL = "https://covid19resume.gitlab.io/"
languageCode = "en-us"
title = "武漢肺炎簡歷"
theme = "devresume"
```


最後輸入 `hugo server` 即可在本地端 `localhost:1313` 預覽網站的樣貌了，而且每當存檔，網頁都會自己重新整理！超好用！

# 產生內容
於網站根目錄輸入 `hugo` 即可產生靜態內容，並依據個人所需找地方部署囉！