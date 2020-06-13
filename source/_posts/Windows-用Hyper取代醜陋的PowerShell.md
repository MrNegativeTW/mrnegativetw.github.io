---
title: Windows - 用 Hyper 取代醜陋的 PowerShell
date: 2020-03-21 10:00:00
tags:
categories:
- Windows
- 技巧
---
PowerShell 缺點多到數不盡，例如：殘破的新細明體、詭異的藍色背景，這篇文章教你用 Hyper 取代 PowerShell。
![](https://b80q1w.bn.files.1drv.com/y4mM_1661KnGljH_BO3Szh9rja4f-roX1L-pswYAX65bdOx8onkEl_D_2W_irCbq-xOpHBLUyFuTu-ezB8bKXeyLeUFtXNtzGAimJhj_rZVzoSnIrYErb7wi_Z7Wwak0R2sOC7L3tEg2uzCRM3Ga_uE1ssTlZVPkRXiompBccYAGxxldUurl2Z8Y10OG6qjLgyjqc1nYJHiMabgyu_Sb6LI-w)
<!--more-->
# 下載
Hyper 官網：[Hyper.is](https://hyper.is/)
下載安裝沒難度，而且安裝程式超好看。

# 設定
Hyper 預設使用 Windows 原本的命令提示字元，要讓 Hyper 使用 PowerShell 還需要做一點小修改，按下 Hyper 左上角的選單並選擇 `Edit > Preferences`
![](https://b82pyg.bn.files.1drv.com/y4msT_BtHM6sLunRc7-W3pkTF1GT8ouioP-5vz7eJk6kwtQ9jpfVfoxO25x79HV3aD5ZdeI4ZyAXp7pj8yNuuCm-0MpVhfrkxf7fkqcyPxn4zwD4XFbARMo6vUhAanWtf0Qa_Ch1kEiW7EaFp90b4BimzCZVq54aJdnaelJf2x-g5Jdmyu7lSoo7kRPtJlnfoGXpNRX4QeCDeTxWl61_NF5hA)

接著在跳出來的 Notepad 中找到 
```
shell: ''
shellArgs: ['--login']
```

並修改為
```
shell: 'C:\\WINDOWS\\System32\\WindowsPowerShell\\v1.0\\powershell.exe'
shellArgs: []
```

修改完成後就可以享受漂亮點的 PowerShell 了！