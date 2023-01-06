---
title: 關於用 Zxing 產生發票的兩三件事
date: 2023-01-06 10:00:00
tags:
categories:
- Android 筆記
---
你要用 Android 產生發票上的 Barcode 和 QR Code 嗎？我也是。

這篇文章會簡單敘述 Barcode 和 QR Code 該有的格式、內容，然後使用 Zxing 產生這三個圖檔，並組合起來變成一張圖，讓你可以拿去列印
<!--more-->

# 內容和格式沒那麼簡單

發票上的 Barcode 和 QR Code 的格式和內容都有規範，詳情可以上 [電子發票整合平台 - 營業人常用文件](https://www.einvoice.nat.gov.tw/EINSM/ein_upload/html/ENV/1428905476324-1.html) 查看，此文章僅簡單敘述 (有誤請以財政部文件為準)。

## 格式

- Barcode: 使用 Code 39 編碼
- QR Code: 使用 Version 6 以上，容錯率 L (7%) 以上

> 更多 QR Code 的版本資訊 👉 [Information capacity and versions of the QR Code](https://www.qrcode.com/en/about/version.html)

> 更多 QR Code 的 ECC 資訊 👉 [Error Correction Feature](https://www.qrcode.com/en/about/error_correction.html)

## 內容

### Barcode

| 欄位             | 長度 | 範例       |
|------------------|------|------------|
| 民國年           | 3    | 111        |
| 發票期別偶數月份 | 2    | 08         |
| 發票號碼         | 10   | AA12345678 |
| 隨機碼           | 4    | 4567       |

### QR Code

真的很多，自己看文件啦QQ

# 產生 Code 39 與 QR Code

我們使用 [ZXing Android Embedded](https://github.com/journeyapps/zxing-android-embedded) 來產生 Barcode 和 QR Code，雖然這不是這個  Library 的主要目的。

把下面這些加進去你的 build.gradle 中

```gradle
repositories {
    mavenCentral()
}

dependencies {
    implementation 'com.journeyapps:zxing-android-embedded:4.3.0'
}
```

## Code 39

產生 Code 39 時記得將 CharacterSet 設定為 utf-8，不然掃不出英文字的部分。

`WIDTH` 這參數其實沒用，因為字串轉為 Code 後就是那麼長

```kotlin
val hintForCode39 = mapOf(EncodeHintType.CHARACTER_SET to "utf-8")

val contentBar = "11012AA112233449999"

val barcode = BarcodeEncoder().encodeBitmap(contentBar, BarcodeFormat.CODE_39, WIDTH, HEIGHT, hintForCode39)
```

## QR Code

一樣將 CharacterSet 設定為 utf-8，並設定財政部規定的 ECC Level 和 QR Version。

這邊額外設定 Margin 是為了讓產生的 QR Code 沒有外圍一圈白色的邊框，讓我們使用 Canvas 合成時方便操作

```kotlin
val hintForQrCode = mapOf(
    EncodeHintType.CHARACTER_SET to "utf-8",
    EncodeHintType.ERROR_CORRECTION to ErrorCorrectionLevel.L,
    EncodeHintType.QR_VERSION to 10,
    EncodeHintType.MARGIN to 0
)

val contentQr = "AA1122334411011129999000000000000000001234567qwertyuioplkjhgfdsazxc==:**********:3:3:1:乾電池:1:105:"

val qrcodeLeft = BarcodeEncoder().encodeBitmap(contentQr, BarcodeFormat.QR_CODE, WIDTH, HEIGHT, hintForQrCode)
val qrcodeRight = BarcodeEncoder().encodeBitmap(contentQr, BarcodeFormat.QR_CODE, WIDTH, HEIGHT, hintForQrCode)
```

# 用 Canvas 把圖片結合起來

首先用 Bitmap 建立一張空白的圖片，接著用 Canvas 將剛剛產生的 Code 39 和 QR Code 畫上去

特別注意的是：
1. 建立空白 Bitmap 後其實是黑色的，所以用 `drawColor()` 把整張變白
2. `drawBitmap()` 的座標是從空白畫布的「左上角」算起，單位為 `float`

```kotlin
// 我的需求長寬是 384 x 234，請依個人需求修改
val WIDTH = 384
val HEIGHT = 234
val empty = Bitmap.createBitmap(WIDTH, HEIGHT, barcode.config)
Canvas(empty).apply {
    drawColor(Color.WHITE)
    drawBitmap(barCode, 4f, 0f, null)
    drawBitmap(qrcodeLeft, 4f, 54f, null)
    drawBitmap(qrcodeRight, 209f, 54f, null)
}
```

最後可以把圖片丟到某個地方，或者直接拿來列印囉！

```kotlin
val out = FileOutputStream(File("/PATH_TO_SOMEWHERE/", "invoice.jpg"))
empty.compress(Bitmap.CompressFormat.JPEG, 95, out)
out.flush()
out.close()
```

# 關於縮放

如果需要縮放剛剛產生的圖片，resample 時記得使用 nearest-neighbor 而不是 bilinear，使用 bilinear 會造成 Code 39 的黑色線條出現灰邊，而不是超平滑的黑線條

在 `Bitmap#createScaledBitmap()` 這個 method 中，最後一個參數可以選擇 resample 用的方法，`true` 為 bilinear，`false` 為 nearest-neighbor

```
createScaledBitmap(@NonNull Bitmap src, int dstWidth, int dstHeight, boolean filter)
```