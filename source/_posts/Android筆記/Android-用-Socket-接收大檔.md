---
title: Android 筆記 - 用 Socket 接收大檔
date: 2022-08-10 10:00:00
tags:
categories:
- Android 筆記
---

這個案例是把圖片轉為 Base64 塞在 JSON 裡面用 Socket 傳送，結果 JSON 大到靠北導致讀取出問題。照網路上其他人用 length 判斷來讓讀取迴圈停下來的方法又會導致其他 API 壞掉，簡單來說就是修 A 壞 B，修 B 壞 A，無法共存。

但為了讓所有 API 都能正常運作只好用了點不是很好看的手法。

<!--more-->

使用 TCP 傳送的資料有時會因讀取太快導致內容不完整，結果後面就是發生 JSON parse error，
雖然去爬文有很多解，但都會造成「長度小於 1024」和「長度大於 1024」的資料擇一正常運作。

以各種教學常見的 buffer 大小 `byte[1024]` 來說，如果用 Log 把 read 後的 length 印出來看會發現「有時候讀 buffer 資料時不會是 1024」，類似這樣：

```java
// Example
in = new BufferedInputStream(socket.getInputStream());
byte[] buffer = new byte[1024];
int length = in.read(buffer);
Log.i(TAG, "length: " + length);
```
```
...
1024
1024
1024
388
1024
1024
768
...
```

## 修改前

原本出問題的 code 長這樣，因為以前傳過來的資料都小於 1024 所以沒事。但這次傳過來的資料動輒 10M，就算把 `byte[1024]` 提升也無法解決問題。

```java
in = new BufferedInputStream(socket.getInputStream());
out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())), true);

byte[] buffer = new byte[1024];
StringBuilder data = new StringBuilder();

int length = in.read(buffer);
data.append(new String(buffer, 0, length));

if (data.length() > 0) {
    // Do something
}

```

## 修改後

發現問題後跑去問了公司做後端的大神，聽他指點完才知道，透過 TCP 的話不會一次傳完，所以要用 loop 下去讀取，這時爬文就會發現類似這樣的 code：

```java
// ...

in = new BufferedInputStream(socket.getInputStream());
out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())), true);

byte[] buffer = new byte[1024];
StringBuilder data = new StringBuilder();

int length = 0;
do {
    length = in.read(buffer);
    if (length != -1) {
        data.append(new String(buffer, 0, length));
    }
} while (length > 0);

if (data.length() > 0) {
    // Do something
}

// ...
```

你複製了，按了 build and run，測試到一半發現了另一個問題，也就是文章開頭說的：「長度小於 1024」和「長度大於 1024」的資料擇一正常運作。

這時最簡單直觀的方式就是直接把資料丟進去 parse，正確 parse 的話表示資料正確，也就是傳輸完畢。
```java
// ...

in = new BufferedInputStream(socket.getInputStream());
out = new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())), true);

byte[] buffer = new byte[1024];
StringBuilder data = new StringBuilder();

int length = 0;
do {
    length = in.read(buffer);
    if (length != -1) {
        data.append(new String(buffer, 0, length));
    }
    if (isReceivedJsonIntact(data.toString())) {
        break;
    }
} while (length > 0);

if (data.length() > 0) {
    // Do something
}

// ...
```

## 檢查 JSON

開一個新的 function 來檢查 JSON 到底讀完了沒吧

使用 `JSONObject` 的話若傳入空字串也不會導致 Exception，所以在 parse 前先檢查傳入的 `String` 是不是 empty，不是的話再塞進去 `JSONObject` 試試看 parse 會不會導致 Exception。

### JSONObject

```java
/**
 * Parse JSON each loop to check receiving is the end or not.
 *
 * @param message Received JSON in TCP transfer.
 * @return true if parse not fucked up, aka transfer finished.
 */
private boolean isReceivedJsonIntact(String message) {
    if (message.isEmpty()) {
        return false;
    }
    try {
        JSONObject jsonObject = new JSONObject(message);
    } catch (Exception exception) {
        return false;
    }
    return true;
}

```

### GSON

```kotlin
private fun isReceiverJsonIntact(message: String) {
    // TOOD
}
```

## 大檔傳送很慢

由於這個產品是用 Socket 來實作後端資料同步，從後端過來的資料又是 base64 編碼的圖片，所以 QA 那邊常常反應：「傳輸很慢，很容易失敗」

不如我們在 `data.append` 後面印出 `data.length()` 來看看到底有沒有收到資料

```java
do {
    length = in.read(buffer);
    if (length != -1) {
        data.append(new String(buffer, 0, length));
    }

    // Print data.length each loop
    Log.i(TAG, "data.length(): " + data.length());

    if (isReceivedJsonIntact(data.toString())) {
        break;
    }
} while (length > 0);
```

印出每次 loop 的 `data.length()` 後會發現，「哇，這收資料速度也太慢」，這時矛頭就指向了 buffer 和 `StringBuilder` ，由於 buffer 只有 1KB 的大小，導致 `StringBuilder.append()` 用了很多時間，不如一口氣將 buffer 提升至 64K，直接讓效能提高不知道幾百 %

```java
byte[] buffer = new byte[65536]; // 65536 Bytes == 64KB
```

就這樣，覆蓋一張華麗的 64K 結束被 QA 煩三週的 Issue！


> 我知道這解法很醜，但不小心跑到接案廠就是這樣，只求能動 : (