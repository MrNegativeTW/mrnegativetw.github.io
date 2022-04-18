---
title: 如何在 Gitbook 中加入 Google Analytics 進行流量追蹤
date: 2018-10-18 10:00:00
tags:
categories:
- 其他
---
這幾天回去寫我放在 Gitbook 上的「[Ryzen 安裝黑蘋果教學](https://mtwstudio.gitbook.io/ryzentosh/)」，一時興起想說找找看有沒有 Gitbook 提供的流量分析，結果不得了，觀看次數竟然破千了，但是流量分析看起來不是很專業，所以乾脆把 Google (Google：閃開，讓專業的來！) 分析加進來。

<!--more-->
# Google Analytics 設定
首先前往 [Google Analytics](https://analytics.google.com/analytics/web/#/)，接著選擇左下角的管理，並 **建立帳戶**
![gga_1](https://pbz6ha.bn.files.1drv.com/y4m8cjM1YlpxzHeS7O6IrikWxQYnXZroWDXN0MAPNA0L1XmWGhKHO7fo7TzrXIBZ2OwuwVGRFAZBosTpyfSK5hBVz5oFD_QD8cX2BqSMCTr3PqCDToQ-5gFjdsXY9JdA8BYpZxs1c5Vo4vI1tiyB8947GC2sODqdSo8XPuk15guMG-lqdp-EUd4HEX2p9beISRAL_SFD3PQkI8skvJhE2C7BA)

接著輸入網站的基本資料，並同意所有使用規則
![gga_2](https://pbbfpw.bn.files.1drv.com/y4mnrwyiSZq7R-OfmnLE6lK9KmNKrzGDOvXBLNJeEHjcARxtMuN4yuLIkVj63vmaGNatFcdOMM0xweWohyfnpBPd7Yc2D1Fjt3dfm-G_Hb7o1GGjBHI2ull-zTWGFxrKF95tZG4nSePuisOSXmLy9tdItDSGFJWSZJez3nBWxf8UYii0Rxo1YSsM1Eg0vwxMc77gKANGzNJ0NUIxfuwtbvviA)

完成後通常會跳出 **追蹤ID**，如果不小心關掉的話，還可以從 **追蹤資訊** 中的 **追蹤程式碼** 找到
![gga_3](https://pbyc2a.bn.files.1drv.com/y4mG_7PBIuTTVyyYG1rO8LcXsm42BzvjMEisteJNgD9EyV6TcyJXS1iEKLV25ctoWUSaSxIQzLAWuJQqplRBceaO90GmfmIRA3tMrrJbx-AZgXhWUcb6e9NABydp8Wc0upxOAU3MtVdyg__pfhAkhEhtK5S70hmLJD1bBsKt4woiHFx8PGdj6tOxlLShwUAjGTE_IqH8Q5aOT8V1YIRqIjUlg)

![gga_4](https://pbabyw.bn.files.1drv.com/y4mxEvomV_XJ6adyt-jaR2Ylmn46ALqC-9YBROHD1178s0b3mKWln7_WUhXKCzpc1GFKxQR8huEz-N_N8r6esulPcz8ehLZhlgMfJ6zB_4M0t_BCZeaWUescdBx8ERs3-MpCoLRO9-ewieDCo42g2AMEFb1PYl76AXiDOOIw9UiTTQV9MzM1dEQwKchNqk_qX7RBE0XspV-v1IT1gleYXErgg)

# Gitbook 設定
完成「專業的」設定後，就可以進到 [Gitbook.com](https://www.gitbook.com)開始設定啦，沒意外的話會自動導到自己的 Space，這時找到自己的公開書本並按右上角的三個點選擇 **Integrations**
![gga_5](https://pbbm7g.bn.files.1drv.com/y4mwT2Tin0W8OvTjUyqOEtabtO3yUrvxBCRXpKU3mmNNaf_JZGcMu2M1QfWj-va-q85yZX03zK4mxsuM_Mls8pdlI-VVyblHLXDQzbMb2oqubNTMMfX2xyfc8LcI5Q-J7KiWLSeH5j6oghAszUyWpRSj8fwkeQOPaqmeCaWpmr2XrfSquUrMAU7blt6PoWYQc02qlcEd49cfOtgyTWSpP9w8g)

接著將剛剛拿到的 **追蹤ID** 填入後，就可以馬上去 Google Analytics 看結果囉
![gga_6](https://pbzleg.bn.files.1drv.com/y4mmv6lgKpFUqzatpxQrZypKDwzyTjPEz2xxQK51cn2r_r1F5OfkHT6wx1o7MSAURffT2uAZ3hfB1Qm537dPaR3SjchFUpauYcRRGkz__6nJgcuqTKVcZPOab0vx6pthcq0ORSosnsGfA6jlvEAph5GpHZxhU9M4sGa853-FkuFtfSnA-fN4QsfR2zwSyhMPbsU37prvUtUagvTHhw7ZRxEwA)
