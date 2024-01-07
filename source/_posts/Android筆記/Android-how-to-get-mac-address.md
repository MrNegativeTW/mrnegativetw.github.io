---
title: Android 系統的行業主板取得 MAC 位置
date: 2023-09-18 10:00:00
tags:
categories:
- Android 筆記
---

注意：此文章適用行業主板，一般手機不保證能做到

眾所皆知，較新版本的 Android 因為隱私需求，拿到的 MAC 位置都會是 `02:00:00:00:00:00`，但在我們產品中還是需要 MAC，甚至會有客戶拿自己的機器過來想裝我們的系統

# 查看 MAC

一般來說有兩種方法可以查看 MAC 位置

兩種都是透過 `adb shell`，但也能透過 Code 做到

1. 透過SystemProperties

    在 Terminal 執行
    ```
    getprop
    ```

    接著列出的資訊中有一項 ro.boot.mac，這個就是以太網的 MAC 位置了
    ```
    [ro.boot.mac]: ...
    ```

2. 讀取特定檔案

    在 Terminal 執行
    ```
    cat /sys/class/net/eth0/address
    ```

    接著畫面上就會噴出 MAC 位置了

# 寫入 MAC

以這次合作的客戶舉例，他們有很多電視盒要裝在房間中給顧客使用，順便播放一些文宣；我們的系統是使用 MAC 做註冊，但偏偏客戶提供的電視盒又沒有將乙太網的 MAC 寫在系統中

這時就需要自己將東西寫進去

```
echo 1 > /sys/class/unifykeys/attach 
echo "mac" > /sys/class/unifykeys/name 
cat /sys/class/unifykeys/name 
echo "B1:28:2D:23:03:21" > /sys/class/unifykeys/write 
cat /sys/class/unifykeys/read
```

# 萬能讀取 MAC 的方法

順便提供一個在行業主板上還沒失敗過的取 MAC 方法

以下是我試過的主板，皆不用主板商的 SDK 就能成功取得乙太網 MAC：
- 亮鑽科技 (Android 7, 11, 12)
- 雍慧電子 (Android 11, 12)
- 欣威視通 (Android 11)
- 眾雲世紀 (Android 11)

```kotlin
class MacRetriever {

    /**
     * Get ethernet mac address by common method (some may not work)
     *
     * @return 乙太網的 MAC 位置，大寫。無法取得時回傳 [DEFAULT_MAC_ADDRESS]
     */
    fun getEthMacAddress(): String {
        // 1. 透過反射拿 getprop 裡面的 ro.boot.mac
        try {
            Log.i(TAG, "嘗試反射取得 ro.boot.mac")
            val systemPropertiesClass = Class.forName("android.os.SystemProperties")
            val getMethod = systemPropertiesClass.getDeclaredMethod("get", String::class.java)

            // Pass the property name for the MAC address
            // val propertyName = "wifi.interface";
            val propertyName = "ro.boot.mac"
            val interfaceName = getMethod.invoke(null, propertyName) as String

            if (interfaceName != null && interfaceName.isNotEmpty()) {
                return interfaceName.uppercase()
            }
        } catch (e: Exception) {
            Log.e(TAG, "嘗試反射取得 ro.boot.mac 時失敗")
            e.printStackTrace()
        }
        Log.w(TAG, "無法反射取得 ro.boot.mac")

        // 2. 讀取 /sys/class/net/eth0/address 取得 MAC 位置
        try {
            Log.i(TAG, "嘗試讀取 /sys/class/net/eth0/address")
            val addressFile = loadFileAsString("/sys/class/net/eth0/address")

            if (addressFile != null && addressFile.isNotEmpty()) {
                return addressFile.uppercase(Locale.getDefault()).substring(0, 17)
            }
        } catch (e: Exception) {
            Log.e(TAG, "嘗試讀取 /sys/class/net/eth0/address 時失敗")
            e.printStackTrace()
        }
        Log.w(TAG, "無法讀取 /sys/class/net/eth0/address")

        // 都拿不到，回傳預設值，沒救了
        return DEFAULT_MAC_ADDRESS
    }

    @Throws(IOException::class)
    private fun loadFileAsString(filePath: String): String {
        val fileData = StringBuffer(1000)
        val reader = BufferedReader(FileReader(filePath))
        val buf = CharArray(1024)
        var numRead = 0
        while (reader.read(buf).also { numRead = it } != -1) {
            val readData = String(buf, 0, numRead)
            fileData.append(readData)
        }
        reader.close()
        return fileData.toString()
    }

    companion object {
        private const val TAG = "MacRetriever"
        const val DEFAULT_MAC_ADDRESS = "02:00:00:00:00:00"
    }
}
```