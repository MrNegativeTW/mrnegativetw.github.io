---
title: Hadoop 雙機架設教學
date: 2020-06-10 10:00:00
tags:
categories:
- 架設教學
---
剛好這學期上課有教如何在兩台虛擬機當中架設 Hadoop 並讓他們可以互相連動，
<!--more-->
# 建立虛擬機
安裝過程不贅述，照下列需求安裝即可：
- Virtual Box
- Ubuntu 18.04

安裝過程不贅述

# 基本設定
## 建立專用使用者
建立新使用者並授與權限， **往後步驟皆使用此使用者**。
```
~$
~$
~$
~$ su hadoop
```

## 安裝 Java
```
~$ 
```

## 修改 .bashrc


# Hadoop 安裝與測試


# 第二台機器



# 為虛擬機建立區網

分別進入兩台虛擬機設定頁面，將介面卡二設定為

完成後開機輸入 `ifconfig` 確認是否有出現 `192.168.xxx.xxx` 的 IP 出現，並使用 `ping` 測試是否能相通。

# 執行 MapReduce 任務
```
hdfs dfs -cat /demo/FILE.txt
```