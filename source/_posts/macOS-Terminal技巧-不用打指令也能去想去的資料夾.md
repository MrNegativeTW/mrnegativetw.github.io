---
title: Terminal 技巧 - 不用打指令也能去想去的資料夾
date: 2018-11-02 10:00:00
tags:
categories:
- macOS
- 技巧
---
每次打開 Terminal 要做事，總是會先用 `cd` 指令導航到我們要去的資料夾，但每次都要打一樣的指令真的很煩，所以這篇文章就來告訴你我個人常用的三種 **不打指令懶人法**。
<!--more-->
# 方法一 - Command + Q
第一種方法還是要打一點點的指令，不過只要一次就能撐很久，那就是「結束」大法。
macOS 不同於 Windows 的地方就是「**關閉程式**」與「**關閉視窗**」是截然不同的東西
在 Windows 當中，關閉視窗等於關閉程式 (`WinKey + W` = `ALT + F4`)；
在 macOS 當中，你可以關閉「視窗」`Command + W` 或「程式」`Command + Q`。

說了這麼多，這方法的作法就是：
1. 確認系統偏好設定中的「結束 App 時關閉視窗」沒有勾選
![terminal_no_command_5](https://mqrrka.bn.files.1drv.com/y4mFHeMP1ZpU-zD-w3NzFZn_RW7MPIMiUheo8rS1S5DldXcZLP4xoZ87h9c-alMRDWt_EeQ9n_5EX8KGvCj19F_9YbUP0sbPR4uLdFKaG811E9JdSfU5bW047oMkW-7pPKItxeQtOHnMt_TE3_1Gih3g24Dy7hg5N6HajNP4GsGw2Q9zlNwvwCYQFDbdCgpQ_B14iCXPznwaIAVZjIR4Fr74g)
2. 照往常方法導航到要去的資料夾，或者本文下方的方法
3. 要結束工作時，請直接了當的按下 `Command + Q`，這樣一來下次開啟時就會回復上一次的工作階段囉。
下圖就是重新打開後的狀況，還會寫著 `[已回復 2018年11月 2 日 下午8:57:09]`
![terminal_no_command_1](https://pby97a.bn.files.1drv.com/y4mJfwoL2Zj9MCqUxGlsFSaP57Yu1leeoyE89vrjZk6ABruIGgQkojgIjvBzpCM7tD4X4uBhKwBApGCS5IbMPD49s8ST5JCDUmeU1aE4E9zW8nOa5Kw_rdBgLvLUQMCGuOC84X2Pti7KgZl-MCC0fC9uK-LTZX5oiHARJ_30ISuXGbYJh31MLucsyQ1GrabPD9Q4FZ6Gn6WipevO5GJ-5V51A)

# 方法二 - 拖曳資料夾
這方法就是真懶人用法了，可以配合上面的使用，或者每次開每次用
首先，打開你要去的資料夾與 Terminal，
先在 Terminal 裡輸入 `cd(+空格)`，
按著視窗上方路徑列的藍色小小小資料夾，並拖曳到 Terminal 當中，然後按下 Enter，搞定！
![terminal_no_command_2](https://okejyw.bn.files.1drv.com/y4mOY9kuDl5gxbEoifM4bIZtxyhFQhH-re2opVG6fq3LvcdWYXlMwCX8N9AtmiWXN-9eiWqjhtxpPstD16uuTy6UaTahB9gemUzVaws7nSSX6IHegtcOXmu-INMvo-o3hUoN_A9fNCWsxfE4y-eN7H9KTXFD-VD1BkyIgnkew84pp1auSnri4OuiYbcDT-KqmS904m0mrUz459XRZzG41pKXQ)
# 方法三 - 右鍵
這是超懶人用法，連 `cd` 都不用打
首先，到 系統偏好設定 > 鍵盤 > 快速鍵 > 服務
接著把 `新增位於檔案夾位置的終端機邊視窗` 和 `新增位於檔案夾位置的終端機邊標籤頁` 打勾
![terminal_no_command_3](https://okfu7g.bn.files.1drv.com/y4m3OAFLL7QOMJmKrPkVzNLSpWWnde9vCj7b7LMem-f0uYOW0kjCjx9Stt3Sap-q96D2XhFap6o6XzOu2GtJAGLFnyXg0Alq2JMD51qRXxdhP0aD_ztitSRk4aXykBcKEtbqhQ8YrV2frevn9lEriMWX8CwFzhDDttCP-6teger1YvvbP8L1AON_S4k65txoJLDg7y0wHeY4cg9-dCRTGSaDg)

之後就可以在要去的資料夾上按右鍵 > 服務，並選擇 `新增位於檔案夾位置的終端機邊視窗` 或 `新增位於檔案夾位置的終端機邊標籤頁`
![terminal_no_command_4](https://okhteq.bn.files.1drv.com/y4mdBTmrMXoB0dTFtN_FJ9667BWX-EaLBWirQZxkt2BAYYiJpwnbldnP_92_MrcoPrUJBcy-YpVN-4GSoibJlLoO1N_rTpV5r1oRyJ31eewTBKQzhBi-wnptyylFyen1O-uKwvdtJhBozf7Jee4-kEyr4-xG-vjQElB7hPfsIjGcvJPh5-qTmd9X2tySK75gkd1y8hoh3WHQJvICEAIo_3HHg)
