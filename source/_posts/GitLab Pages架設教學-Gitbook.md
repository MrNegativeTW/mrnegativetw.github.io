---
title: GitLab Pages 架設教學 - Gitbook
date: 2018-07-31 20:23:57
tags:
categories: 架設教學
---
Gitbook，一個用來寫電子書的平台，在 V1 時非常好用，但近期改版為 V2 後，不但很多功能被拿掉，而且很多地方也改為付費制，V2 中還加入了 Space 機制，一本書就是一個 Space，免費使用者只能擁有一個公開 Space 和一個私人 Space，換言之就是使用者付費啦<br>
可我還是很迷戀 V1，而且剛好我在 Gitbook V2 Beta 時開了兩個space，GitBook 提示我說：「欸你這樣不行」
就因為這兩個原因讓我爬著爬著發現 GitLab 官方文件裡面有提供架設 Gitbook 的範本，經過多方嘗試之後終於成功將V2的文章轉移過來啦！
<!--more-->
# 準備
- **<a href="https://gitlab.com/users/sign_in">註冊一個 GitLab </a>帳號**
這 不解釋
- **GitBook Editor (<a href="https://github.com/GitbookIO/editor-legacy/releases">傳統板</a> 或 <a href="https://legacy.gitbook.com/editor">新<a href="">版</a>)**
用來編輯電子書，畢竟以前的 Gitbook 都是用這個工具編寫，而現在我們要架設的就是舊版的 Gitbook，所以用這套工具理所當然
- **<a href="https://desktop.github.com">GitHub Desktop</a>**
用來與 GitLab 上的資料同步。 對，用 Github 來同步 GitLab。

# 第一步 - 偷過來
首先到 GitLab 官方提供的<a href="https://gitlab.com/pages/gitbook"> Gitbook 範本</a>偷(Fork)一份過來
![gitbook_tutorial_1](https://okhbwa.bn.files.1drv.com/y4msU3XsAQPxkfZcgPgX5sP9obke0G8MuFBfQv1qf_5SQFBvdWcVdFGFGKE_gaj-EPhsCitu0NZo503ynaEMoTLTypECUDIMKDO6Hh8XSjUzc-KVo7G-hoETDg84kDm7UVH_u8SX3KvL1eNlpTgIK8j_c074Y51HU-SwjhbeZ3sZru2wy-05D4Ulad1B_gwAx8aX_BasF6vZdh0_3tmaE8BCg)
GitLab 接著會提示請選擇命名空間，選自己的頭像就對了
![gitbook_tutorial_2](https://okfata.bn.files.1drv.com/y4maJusq4SOvi52RqyHMmJV9ERwUOJ7BDRCHFnLRxvM9wyifYFQMTkw9DRXx7Xx4Ds5gT9xLg4oUxDLtewOvVRDvJqrt-mxoPOSeCcSx_ijo9_axF8yhej_tQ10G79ZKwYM1AnR36pQXS_fpGehlFK-zP5Xcoh7NPfcSxGJ2KAFDPm0CNTZBn7TmItA0hd77fza8dXhGXRjn-saBsmzEO44JQ)
緊接著進到自己剛剛 Fork 的專案中，於左側選擇 **設定 > 一般**，並展開 **Advanced**
![gitbook_tutorial_3](https://okgl1w.bn.files.1drv.com/y4mr7jkR1jM2moVBXdQeBMwq7NVwxrIGLHmpV4cbQw8nKrc_NEXOn5XVNopPyL-fRxm0ayBJTm4BcV3U_C1b-klw-be6AMNuud7ESbdk_SkDiDUr8tvTxE0cWwZ7jc29k7HueOwGrsay0aUDpokKW6sehpFdrMZqs9mVP4kuYIsPljzA6spO7lqGLeXFjx0OPicUoR-Ev3MlK8DoA_7PrpXbA)
然後選擇 **Remove fork relationship**，沒錯 我們要移除與主專案的連結，將這個專案變成自己的！
![gitbook_tutorial_4](https://okeyvg.bn.files.1drv.com/y4m3crUy0SvfL6O9l8WXBU1MYPoO6zIbH9hGQBaUmbiI6ot8mGdbKVqn2TXoXuG6LOCxYde0sMui_hwtHjVGzO2J8thQveF3dwMniQ-rYJ6dMkX7WgheyQaLmOcdyRY8u8rAr8DPv9ApgDdjk_tjJFf7WH0N0GZb0fU06CmRYaPiJOPjTbwlP-B5pYGJ9g8VelzYNUUXQfhQPpDPwMyMReETQ)
輸入 **gitbook** 解除連結
![gitbook_tutorial_5](https://okgxsq.bn.files.1drv.com/y4mB9lRkggph2WPSFS0ZKLnhGqYWZIMGf6s8TgHkU8zhhBYEMOfqFOAGx2zyWn8PdazR7I2hx-mDG5DTmBYWExpHS6qKKBqZpQfOWZ8rmfQ613o6vVOFhwpcd4U9D8oyvFeQzy0-c5nG6yNIb8coUsuGsX5NJaxGgyWjcmP1kycyd_739c57Y9SezPdd8kvYuEOhEAYvVfbqehvzw5sAgHZkg)
接著回到專案首頁，選擇 README.md，對他編輯並儲存，加個空白或者隨便打幾個字都可以，目的是要觸發 GitLab 產生頁面，完成後可以按左側的 CI/CD 確認是否已經開始建造
![gitbook_tutorial_6](https://b81iwa.bn.files.1drv.com/y4msgdSi-NuIrZVVyAi8obdcNehd1WjGZ2KUXuyTnTh6Gd8kuV5TdGt_8OXsYcZdO0Kq-uN3ALuP4cBp04QMx_BHgK5n2mpujKbn0p_lebOWjvqQ6Zk2LGGNphWex7S0oGp5kp-fyDvbbLwjs4Di2OsNeRI2rvoetNUPEN3ZWPz-k0m06bLJ3fMtwd5gHm3dUxxj3Mr0-RqvYptj1eNd036xA)
這時按 **執行中(上圖) > pages** 即可看到黑盒子
![gitbook_tutorial_7](https://b83hsw.bn.files.1drv.com/y4mTesH1qVSo3chBcPBBJkxRe4zn5Y4QWijOyOggOURdXF3yFz7VpJ2XTAGqkGAkB3VLNOn7PTKNGdFqbc1OUKix-PWD15ERp3tPbqxjIEfxyvnMcwMfpmOy-NqiHL9u3RIVbc23tnSGrEI83soE9N16_1yb6keiG_C9sJy3Rco4T3jnRRm0JMPLOE0OYeHnsmmUOF7jbR4AMAIWSw-dk9HTg)
待黑盒子底部出現 `Job succeeded` 就代表成功啦！
![gitbook_tutorial_8](https://b80s1w.bn.files.1drv.com/y4mmHw7ydwi3kUnd0mXAo18doL-pMw4GpRtxHJQeRE8zy7FG25ZN429SSLvR_zL19CuBpLVwtfMFmzVqp1XKnUGS2srJ-_AmU7zoldirnCF6yu4XKvSXFaKcIb5CD72tzjY_6O4EoZeB0UwQCvoli_uUj1aR1lVyIoPNgAUxis6JPyljzpVAITvq4NjRwQ_FNKaje5zru4NgW3ymdbSp5T6ew)
成功後就前往 **設定 > 頁面** 領取我們的網頁連結吧！
![gitbook_tutorial_9](https://b82ryg.bn.files.1drv.com/y4mqT6ZpW_UBDiGTEjvwS0NO7c7Roqi7V3z-R8igp0XNI0NJLRw3hO_fmQ29edSJg6iCcOFzKckLcL_TqPlwA9ApaE3eSBDo1uSiJVEQ1JINr8Mw_pDAffMVn_nPQ7YXg8Wd3HGRHD87LlJ7_RMP5cKVZEGmytE6a4VpjFjPkXu-u3vHvGjGVNa02CBUVbfkI95GQW2Y4lP3iN_uUY-deQbbw)
好啦，到這邊為止我們已經做好 GitLab 上的初始設定了，接著讓我們開始本地端的操作吧！

# 第二步 - 從 GitLab 搶過來
打開你的 GitHub Desktop ，然後選擇 `File > Clone Repository`
![gitbook_tutorial_10](https://b83vpg.bn.files.1drv.com/y4mQVqQYOc4_JFjlI1-GjgmXURi8cbWYPAw_YlGunUrOA8ylJAG21hn5QQIpQrxo1UqgibcsUWTgg3qZrcW0vcHhSkTmTKfxtcdZ1x7jtG2-fIu50TllcSdrC4NI4_fdm7d4WKNedJWDDdb5Joo7qi9rRGFOJ5IvZ_TgpwPiz781ZoqMS4xAO6m1zwOn9EjZswfT2isxShv8Nut4YTdlr6q6w)
接著從 GitLab 複製 Repo 的 git 網址，並貼到 GitHub Desktop 的 URL 頁面中
`Local Path` 請自行選擇一個合適存放的地點 ~~(例如垃圾桶)~~
![gitbook_tutorial_11](https://bfjqvw.bn.files.1drv.com/y4mzWpLi0dJCDlV8TBQ_c-VN0infqNBYYe-lYhvErnQE9-xOWrGf_2gCQH_MV4bh7fy3ziPuye-ryxPOxyYegLZI4H8pd2PohidCTBugKMJZcsdjIUq8KUQ1YOKI8MuYuH2ym6p-EEFYBzeQZa79IagQ4ZEEs4E4PtrsRGuIjRWiuQ2bAITZbV5tuYptb-3KAnMc6g44zk54c_e1AehdkrviA)
![gitbook_tutorial_12](https://bfifna.bn.files.1drv.com/y4mmQ6d4i6ufRt71RQ9nPDYt88WCiX-GiLSfKvcfG-x0ed8njHlXMAzpuE_gCi_N8-64Pc0ldycHxTN2nNiWuENjUhNJZ513GPIsI9IexD7Bg5k2XZviW-RSr9YZ3iZVFY2C4U-toyWS0llNU-xY1T-epc18TyCuRje5ce9EPLU8xARdy2S8eRbDISSwYszZEnTHj5lfEJejNJKhG-MKVqXGQ)

# 第三步 - 開始寫書
打開你的 GitBook Editor (這邊採用新版示範)，起始畫面選擇 Do thet later
就算選了 Login 也登入不進去，GitBook 把舊版的登入權限拿掉了 ~~(GitBook你到底在衝三小)~~
![gitbook_tutorial_13](https://bfjxbg.bn.files.1drv.com/y4mfnSCUfMnu-U-CwcOjlS671Wo2Rtv1RfTORmqT2UVYDsOijysJ4YsxkUfqHNhzp2FtOWHsLjTORfx9Orp6WAxYa0MVQ57dBpf1-2tBziBTAwu37ik-uI6d4A7MTB1v7OPXyzn9KTUYKKuHVcoT8_XeyjpIDWP736HrNKAD0WD7MUhopaIFv-S6-N5pDxQ2Xf8zGIiOlDRrrFcU2wZhJ1w4g)

接著選擇 `GitBook Editor > Open`，並選擇到你剛剛儲存 Repo 的位置
![gitbook_tutorial_14](https://bfgn8a.bn.files.1drv.com/y4mHjzVTc7MLTNY3X1C0CNI6nkC4y34gaPWRN_zLzLa-J973kuKVpz0pEPOrxqb2zhqcC3IV-if0NI5y8fk8-EXkNRNN2O73I2dAAFLPIqU8A_eP2l6Zw8ptg-Kw-RO-Jsy0lOpug2DYWMyMmhmJl9H0xIRVJ5CNIYnnRb_AezHNqL7bwBHW53MNlW6LQ_WKvOeWX7DqGn1NDb7W_E6YcBfEA)

然後就能看到書籍啦！ 興奮吧？
![gitbook_tutorial_15](https://bfimew.bn.files.1drv.com/y4mfO-w3_wp3QH75sbPN5LoSy7aHIvQe6TPZIQ3N-lOQJUijoSeA5oR_RNw_zGQoy_4L8TMHZFMwxCy5PVPY-p8t4C5MfKVEbtvlofcoOvkKg5QXjTi_LqJOH3_kGPlLdbU3c0H1wt58QliVtQ0UrFgrzhZzj1EZ1Xhns--tFv97hIgojDaT-ITthzNZOYM68pWrBkWR0W-kRaC2QCTrXOEOw)

# 第四步 - 還給 GitLab
GitBook Editor 這邊就不教學了，使用起來很簡單，總之完成後我們要把東西推上去 GitLab 讓他自動為我們產生電子書，這時候就是 GitHub Desktop 派上用場的時候啦！

打開你的 GitHub Desktop，並選到剛剛 Clone 下來的 Repo
1.`Summary (required)` 填入主要變更內容，`Description` 填入詳細變更內容（選填）
2.接著按下 `Commit to master`
3.最後一步，Push 上去 GitLab
![gitbook_tutorial_15](https://bfhwkg.bn.files.1drv.com/y4mBWJHsXuNH_m6N6h14CKodVFpkjLSo8q6IhqjbypK-7y9g7zg4RnPs4Q429_LymQ7g7SctGjXLiXjVh7Cj9IBUnicPs0d-Y4LP_1LQ_rdKtG9bFjOWuyJyqsUwbd2Brb3CaJb0clKsW5wivE5fzJ57cxtjX9dru3CNvBfki4PN7_7HvJMy4QXslJYiOwQxn0HVDvSYL5K3McfQJYl4eDZxQ)

接著就可到前面提到 GitLab 上的 CI/CD 查看是否開始自動產生囉！
