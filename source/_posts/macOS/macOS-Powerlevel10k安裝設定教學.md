---
title: Powerlevel10k 安裝設定教學
date: 2020-06-13 18:56:20
tags:
categories:
- macOS
---
教你安裝與設定比 Powerlevel9k 更強大好看的 ZSH 主題。
<!--more-->
# 完工圖
![](https://imgur.com/Y3q6ui6.png)

# 安裝
先上 Repo 連結： [romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k)

官方 README 有教很多安裝方案，這邊因為我用 iTerm 2 配上 Oh My Zsh，所以本文安裝方法是透過 Oh My Zsh，首先打開你的 iTerm 2，並直接輸入：
```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
```

之後到 `~/.zshrc` 設定一下主題：
```
ZSH_THEME="powerlevel10k/powerlevel10k"
```

完成後你可以重開 iTerm 2 或輸入 `exec $SHELL` 來重新讀取。


# 設定l
接著在安裝後第一次開啟時應該會跳出這次 10k 更新的重點功能：**設定精靈**，引用一下原作的圖：
![](https://raw.githubusercontent.com/romkatv/powerlevel10k-media/master/configuration-wizard.gif)

如果沒有跳出設定精靈的話也可以手動輸入 `p10k configure`，設定精靈大致長這樣，每一步驟會問你不同的喜好設定，只要照畫面提示輸入 Y/N 或 1234 就可以了。
![](https://imgur.com/D5uwcop.gif)
