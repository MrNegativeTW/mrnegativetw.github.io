---
title: Android 如何與原生 TTL Serial Port 溝通
date: 2024-03-28 10:00:00
tags:
categories:
- Android 筆記
---

手把手教你在 Android 上使用 TTL 接口與 RS485 溝通，並送出帶有 Hi Lo 的 CRC 到裝置上!

本文應該不適用你手上的 Android 手機，僅適用於「開發版」，一般的 Android 手機可參考 
[mik3y/usb-serial-for-android](https://github.com/mik3y/usb-serial-for-android)

<!-- more -->

# 整合第三方資源

首先你會需要大神的 [cepr/android-serialport-api](https://github.com/cepr/android-serialport-api)

## jni 檔案

請前往 `android-serialport-api/project/jni` 資料夾，我們會需要裡面的檔案：
- SerialPort.c
- SerialPort.h
- gen_SerialPort_h.sh

將上述檔案放入 `jni` 資料夾

## Java 檔案

請前往 `android-serialport-api/project/src/android_serialport_api` 資料夾，
我們會需要裡面的檔案：
- SerialPort.java
- SerialPortFinder.java

將上述檔案放入 `src/android_serialport_api` 資料夾

(使用右鍵 New > module)

## Migrate from ndk-build to CMake

由於此專案較舊 (Commits on Oct 30, 2011)，可以看到使用的是  `Application.mk` 與 
`Android.mk`，我們就將他順手升級為 `CMakeList.txt`

CMakeList.txt 加入如下資訊

```cmake

#...

add_library(serial_port SHARED SerialPort.c)

#...

target_link_libraries(
        serial_port
        -llog)
```

sync gradle and you're good to go

## build.gradle

如果你的專案是第一次導入 jni 的話，可以參考這份 build.gradle (app)

```gradle
anroid {
    defaultConfig {
        ndk {
            ...

            //noinspection ChromeOsAbiSupport
            // locked to arm64-v8a, add armeabi-v7a if you need
            abiFilters "arm64-v8a"
        }
    }

    externalNativeBuild {
        cmake {
            path "src/main/jni/CMakeLists.txt"
        }
    }

}
```

# 使用範例

## Extensions

先從網路上找一些 extensions 讓我們更方便將資料轉為 ByteArray 送進去

```kotlin
/**
 * Turn byte array to readable string
 *
 * Sample usage: "123".toByteArray().toHexString(),
 * Output will be: "31 32 33"
 */
fun ByteArray.toHexString() = joinToString(" ") { "%02x".format(it) }

/**
 * Turn HEX string into bytearray
 */
fun String.decodeHex(): ByteArray {
    check(length % 2 == 0) { "Must have an even length" }

    return chunked(2)
        .map { it.toInt(16).toByte() }
        .toByteArray()
}

/**
 * Turn hex string into binary
 * Sample usage: "78".hexToBinary(),
 * Output will be: "1111000"
 */
fun String.hexToBinary() = toInt(16).toString(radix = 2)
```

## Modbus CRC Lo / Hi

此次設備走 RS485 且使用 Modbus，最後的 CRC 有兩位 Lo/Hi，所以上 GitHub 找大神寫好的計算

Refer:  [fbiego/CRC16Modbus](https://github.com/fbiego/CRC16Modbus)

```kotlin
class CRC16Modbus: Checksum {

    private var sum = 0xFFFF

    val crcBytes: ByteArray
        get() {
            val crc = this.value.toInt().toLong()
            val byteStr = ByteArray(2)
            byteStr[0] = ((crc and 0x000000ff)).toByte()
            byteStr[1] = ((crc and 0x0000ff00).ushr(8)).toByte()
            return byteStr
        }

    companion object {
        private val TABLE = intArrayOf(
            0x0000, 0xc0c1, 0xc181, 0x0140, 0xc301, 0x03c0, 0x0280, 0xc241, 0xc601, 0x06c0, 0x0780, 0xc741, 0x0500, 0xc5c1, 0xc481, 0x0440,
            0xcc01, 0x0cc0, 0x0d80, 0xcd41, 0x0f00, 0xcfc1, 0xce81, 0x0e40, 0x0a00, 0xcac1, 0xcb81, 0x0b40, 0xc901, 0x09c0, 0x0880, 0xc841,
            0xd801, 0x18c0, 0x1980, 0xd941, 0x1b00, 0xdbc1, 0xda81, 0x1a40, 0x1e00, 0xdec1, 0xdf81, 0x1f40, 0xdd01, 0x1dc0, 0x1c80, 0xdc41,
            0x1400, 0xd4c1, 0xd581, 0x1540, 0xd701, 0x17c0, 0x1680, 0xd641, 0xd201, 0x12c0, 0x1380, 0xd341, 0x1100, 0xd1c1, 0xd081, 0x1040,
            0xf001, 0x30c0, 0x3180, 0xf141, 0x3300, 0xf3c1, 0xf281, 0x3240, 0x3600, 0xf6c1, 0xf781, 0x3740, 0xf501, 0x35c0, 0x3480, 0xf441,
            0x3c00, 0xfcc1, 0xfd81, 0x3d40, 0xff01, 0x3fc0, 0x3e80, 0xfe41, 0xfa01, 0x3ac0, 0x3b80, 0xfb41, 0x3900, 0xf9c1, 0xf881, 0x3840,
            0x2800, 0xe8c1, 0xe981, 0x2940, 0xeb01, 0x2bc0, 0x2a80, 0xea41, 0xee01, 0x2ec0, 0x2f80, 0xef41, 0x2d00, 0xedc1, 0xec81, 0x2c40,
            0xe401, 0x24c0, 0x2580, 0xe541, 0x2700, 0xe7c1, 0xe681, 0x2640, 0x2200, 0xe2c1, 0xe381, 0x2340, 0xe101, 0x21c0, 0x2080, 0xe041,
            0xa001, 0x60c0, 0x6180, 0xa141, 0x6300, 0xa3c1, 0xa281, 0x6240, 0x6600, 0xa6c1, 0xa781, 0x6740, 0xa501, 0x65c0, 0x6480, 0xa441,
            0x6c00, 0xacc1, 0xad81, 0x6d40, 0xaf01, 0x6fc0, 0x6e80, 0xae41, 0xaa01, 0x6ac0, 0x6b80, 0xab41, 0x6900, 0xa9c1, 0xa881, 0x6840,
            0x7800, 0xb8c1, 0xb981, 0x7940, 0xbb01, 0x7bc0, 0x7a80, 0xba41, 0xbe01, 0x7ec0, 0x7f80, 0xbf41, 0x7d00, 0xbdc1, 0xbc81, 0x7c40,
            0xb401, 0x74c0, 0x7580, 0xb541, 0x7700, 0xb7c1, 0xb681, 0x7640, 0x7200, 0xb2c1, 0xb381, 0x7340, 0xb101, 0x71c0, 0x7080, 0xb041,
            0x5000, 0x90c1, 0x9181, 0x5140, 0x9301, 0x53c0, 0x5280, 0x9241, 0x9601, 0x56c0, 0x5780, 0x9741, 0x5500, 0x95c1, 0x9481, 0x5440,
            0x9c01, 0x5cc0, 0x5d80, 0x9d41, 0x5f00, 0x9fc1, 0x9e81, 0x5e40, 0x5a00, 0x9ac1, 0x9b81, 0x5b40, 0x9901, 0x59c0, 0x5880, 0x9841,
            0x8801, 0x48c0, 0x4980, 0x8941, 0x4b00, 0x8bc1, 0x8a81, 0x4a40, 0x4e00, 0x8ec1, 0x8f81, 0x4f40, 0x8d01, 0x4dc0, 0x4c80, 0x8c41,
            0x4400, 0x84c1, 0x8581, 0x4540, 0x8701, 0x47c0, 0x4680, 0x8641, 0x8201, 0x42c0, 0x4380, 0x8341, 0x4100, 0x81c1, 0x8081, 0x4040
        )
    }

    override fun update(b: Int) {
        sum = (sum shr 8) xor TABLE[((sum) xor (b and 0xff)) and 0xff]
    }

    override fun update(b: ByteArray, off: Int, len: Int) {
        for (i in off until off + len) {
            update(b[i].toInt())
        }
    }

    override fun getValue(): Long {
        return sum.toLong()
    }

    override fun reset() {
        sum = 0xFFFF
    }

    fun update(byteArray: ByteArray) {
        for (byte in byteArray) {
            update(byte.toInt())
        }
    }
}
```

### Basic usage

```kotlin
val crc = CRC16Modbus()
crc.update(your_bytearray)
val checkSum = crc.crcBytes
```

## Send it!

GitHub 上作者是把 get / release 等操作放在 application，但此功能並不是常開，
所以我把它放到了獨立的 class 來方便進階操作

```kotlin
class NativeTTL {

    private var serialPort: SerialPort? = null

    @Throws(SecurityException::class, IOException::class, InvalidParameterException::class)
    fun getSerialPort(path: String, baudRate: Int): SerialPort {
        if (serialPort == null) {

            // do more things...

            if (path.isEmpty()) {
                throw InvalidParameterException("Incorrect tty path: $path")
            }

            serialPort = SerialPort(File(path), baudRate, 0)
        }

        return serialPort!!
    }

    fun closeSerialPort() {
        if (serialPort != null) {
            serialPort!!.close()
            serialPort = null
        }
    }
}
```

接下來就可以打開 port，並寫資料與讀資料了

```kotlin
class MagicDeviceController {

    private val nativeTTL = NativeTTL()

    private var mSerialPort: SerialPort? = null
    private var mOutputStream: OutputStream? = null
    private var mInputStream: InputStream? = null
    private var mReadThread: ReadThread? = null

    /**
     * 用來從 TTL 接口讀資料，讀取後資料回調至 [onDataReceived]
     */
    private inner class ReadThread : Thread() {
        override fun run() {
            super.run()
            while (!isInterrupted) {
                var size: Int
                try {
                    val buffer = ByteArray(64)
                    if (mInputStream == null) return
                    size = mInputStream!!.read(buffer)
                    if (size > 0) {
                        onDataReceived(buffer, size)
                    }
                } catch (e: IOException) {
                    e.printStackTrace()
                    return
                }
            }
        }
    }

    init {
        try {
            mSerialPort = nativeTTL.getSerialPort(TTL_PATH, BAUD_RATE)
            mOutputStream = mSerialPort?.outputStream
            mInputStream = mSerialPort?.inputStream

            mReadThread = ReadThread()
            mReadThread?.start()
        } catch (e: Exception) {
            Log.e(TAG, "$e")
        }
    }

    /**
     * TTL 接口讀取到的資料
     */
    private fun onDataReceived(buffer: ByteArray, size: Int) {
        val receiveRaw = buffer.toHexString()
        Log.i(TAG, "onDataReceived: $receiveRaw}")

        // do something
    }

    fun release() {
        mReadThread?.interrupt()
        mInputStream?.close()
        mOutputStream?.close()
        nativeTTL.closeSerialPort()
    }

    /**
     * 送出資料到 TTL 接口
     */
    private fun sendDataToSerialPort(commandRaw: String) {
        val commandWithOutSpace = commandRaw.replace(" ", "").decodeHex()

        // Calc for CRC (Lo and Hi)
        val crc = CRC16Modbus()
        crc.update(commandWithOutSpace)
        val checkSum = crc.crcBytes

        val command = commandWithOutSpace + checkSum

        try {
            Log.i(TAG, "sendDataToSerialPort: ${command.toHexString()}")
            mOutputStream?.write(command)
            Thread.sleep(SEND_DATA_INTERVAL)
        } catch (e: Exception) {
            Log.e(TAG, "$e")
        }
    }

    companion object {
        private const val TAG = "MagicDeviceController"
        private const val TTL_PATH = "/dev/ttyS3" // 對應硬體位置
        private const val BAUD_RATE = 9600
        private const val SEND_DATA_INTERVAL = 250L
    }
}
```

例如：

```kotlin
class MagicDeviceController {
    
    // ...

    fun runTests() {
        listOf<String>(
            "01 06 00 0D 02 58"
        ).forEach { commandRaw ->
            sendDataToSerialPort(commandRaw)
        }
    }
}
```
