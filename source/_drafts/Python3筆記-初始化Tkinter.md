---
title: Python 3 筆記 - 初始化 Tkinter
date: 2018-08-29 10:00:00
tags:
categories:
- Python 3 筆記
---
Python 做 GUI 好像沒什麼資源，也沒什麼人在做的感覺，可能大家都寫 Flask 或 Django 去了，反正這篇就是用來紀錄如何用 Python 自帶的 Tkinter 框架產生一個 **空白** 視窗出來。
<!--more-->
# 重點程式碼
這些大概是初始化一個空白 Tkinter 視窗的必備程式碼了，全部複製貼上就可以開始作業啦
```Python
import tkinter as tk
window=tk.Tk()
window.title('聽說這是視窗標題')
window.resizable(0,0)
window.geometry('87x87')

# 要放的內容

window.mainloop()
```
![20180829_py3_tkinter](https://smpuvq.bn.files.1drv.com/y4maqARgudN-W1YWmbpI45QsgMdRAkVEOHJljjdznsDkvLx6_rdWNiFgmTIcA2TNrO16n1K-ff3qUwH8vGpXvoyuKKlqHOWxY_G8byQm-vXidHC7AJJ8-umPLflLNaeZAPrmC7VgDQOrT5b4qfw8SuMHxRCKsM1bBV8vhwxvoz-PvYfWLQqVaDwo40QbOSadnUtXdcK0HChBC45HEMHFLb_hQ?width=412)

# 解釋
`import tkinter as tk`
載入 Tkineter 模組

`window.title('聽說這是視窗標題')`
設定視窗標題

`window.resizable(0,0)`
設定視窗不可縮放，要可縮放拿掉這行即可

`window.geometry('87x87')`
設定視窗大小，單位為 `寬` x `高`

`window.mainloop()`
保持視窗迴圈，也就是不斷刷新的意思，不然視窗只會顯示初始狀態，後續做的變更都不會顯示
