---
title: Windows 隱藏密技 - 右鍵開啟命令提示字元及 PowerShell
date: 2018-09-07 10:00:00
tags:
category:
- Windows
- 技巧
---
每次開命令提示字元都要 cd 來 cd 去好麻煩，有沒有簡單點的方法呢？
<!--more-->
# 開啟命令提示字元
1. 首先用 `WinKey + R` 並輸入 `regedit` 打開登錄編輯程式
2. 接著導航到 `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Folder\shell\`
3. 在這邊右鍵 > 新增 >　機碼，這邊的名字會出現在右鍵選單中，所以取一個喜歡的名字吧，例如：`在這邊「開啟命令提示字元」`
4. 緊接著繼續在剛剛新增的機碼底下，再新增一個機碼，這次我們叫他 `command`
5. 接著於 `command` 底下雙擊打開 `(預設值)` ，並於 **數值資料** 填入
```
"C:\Windows\System32\cmd.exe" /k "cd %L"
```
![rightclick_1](https://fvtspg.bn.files.1drv.com/y4mwzLe0b-iTgznKdx5ebZR6wwHyDqQWYwy8lShfIcW6J0DyGMLiN61DHia-kGXwkgl3n3uWo-lCtI1ID6-1zwf7DKKSlol92jJS8Ve04WcjKZFWuFY5JPV-W_CPshI6mKPYHXf-078yzn1i_9sQ_YF69UEVZWSBgr21zf9_uheakCWkfE1I4seDcDfGnFHi2g86n05NAol_FGJDeFL0TG1Tw)

大功告成，以後按右鍵就會有 `在這邊「開啟命令提示字元」` 囉
![rightclick_2](https://sm8hpa.bn.files.1drv.com/y4m2MPcrFs762MJ98g950IdXt8X7JIFULnyfdOE93oTZbze58JOKQvTv-TRaHOBfM0nMaVAfApEB4S18bgxz9mwyINHKFkeVaDw92F-XP0VrznHauqFWyHuCzlHAPy18ZeSWHhkckHHh6Gfe-rpr-6Gno3KZEwmPEHgDEyl0p0M56FiOrdd2MP3lMsUDzPdr8heqIIKFAQry7gbp0cDUPviUA)
# 開啟 PowerShell
只要按下 Shift 在按下右鍵，就會出現「在這裡開啟 PowerShell 視窗」
![rightclick_3](https://fvvtgg.bn.files.1drv.com/y4m3p0dgge_q10MpfKgBS-B_TrmNFG2yDZWxlTn9u3qtZ1b4we8vhmaLuD8yO__qeNZ5C6MlZlW_YPou4VvLUhEuSjHbVUfxFipdMdtBP6Tzof4uUro229bAXzjdqO-VKcIxpDqjzqV6F2i71Yvjuw4GArf7UU95i-6heJgiPcu95vtThsdoMQ5fZcODMfCYRBlBiBCEQjzGMDOGn-aMOvFPA)
