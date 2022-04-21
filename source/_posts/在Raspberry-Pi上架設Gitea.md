---
title: 在 Raspberry Pi 2B 上架設 Gitea
date: 2020-09-29 10:00:00
tags:
categories: 
- 架設教學
---
Gitea 說明文件實在是寫得不太好，弄了一小段時間才成功裝上 Raspberry Pi。
<!--more-->
中文版文件爛，英文也不是很好，自己 try and error 比較實在。
# 下載 Gitea
本教學使用 [執行檔安裝](https://docs.gitea.io/en-us/install-from-binary/)，直接前往 [下載頁面](https://dl.gitea.io/gitea/) 找到適合自己平台的。

因樹莓派平台比較特殊，而且這塊樹莓派是 arm-v7 的版本，所以我試了幾個版本才找到能用的。

1. 開 Treminal，下載 `arm-6` 版本的執行檔，並重命名為 `gitea`，本文撰寫時為 `1.12.4` 版。
```
wget -O gitea https://dl.gitea.io/gitea/1.12.4/gitea-1.12.4-linux-arm-6
```

2. 給權限
```
chmod +x gitea
```

# 安裝 MySQL
Gitea 還需要 SQL 才能正常運作，所以就裝個很常見的 MySQL 給他用，然後因為都是在本機端執行，所以可以略過官方說明文件說的一大堆步驟。

1. 開 Terminal，更新 apt 後裝個 MySQL
```
sudo apt update
sudo apt install mariadb-server
```

2. 進入 SQL 的主控台看看，預設密碼為樹莓派的登入密碼。
```
sudo mysql -u root -p
```

3. 建立一個使用者給 gitea 用
```sql
CREATE USER 'gitea' IDENTIFIED BY 'gitea';
```

4. 開一個 `giteadb` 資料庫給 gitea 用
```sql
CREATE DATABASE giteadb CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';
```
需要注意的是說明文件提到：用 `utf8mb4` 以便支援奇怪的東西

5. 權限全開
```sql
GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea';
FLUSH PRIVILEGES;
```

6. 離開
```sql
exit;
```

# 安裝 Gitea
進入前面下載 gitea 所在的資料夾讓他跑起來
```
./gitea web
```

{% note info %} 
如果你有裝 ufw 這類的防火牆，記得將 3000 port 加入允許名單，不然會像我一樣踩到地雷
以 ufw 來說，指令為：
```
sudo ufw allow 3000
```
{% endnote %}

接著就可以使用 Web GUI 進行設定了，設定時需注意將資料庫名稱改為上述設定的 `giteadb`，不然會噴錯說沒權限。
