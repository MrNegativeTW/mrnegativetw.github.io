---
title: Get all file access after Android 11
date: 2022-06-29 10:00:00
tags:
categories:
- Android 筆記
---

In this article, I will show you how to check and ask for storage permission.

You usually don't need this permission if your app goes on Play Store.

<!--more-->
---

Declear permission

```xml
<uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE" />
```

Check and ask for permission

`Environment.isExternalStorageManager()` only available after `VERSION_CODES.R`

```kotlin
class YourActivity {
    
    private fun checkPlatformPermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
            if (Environment.isExternalStorageManager()) {
                setupAfterPermissionChecked()
            } else {
                check30AndAfter()
            }
        } else {
            setupAfterPermissionChecked()
        }
    }

    @RequiresApi(api = Build.VERSION_CODES.R)
    private fun check30AndAfter() {
        try {
            Intent(Settings.ACTION_MANAGE_APP_ALL_FILES_ACCESS_PERMISSION).apply {
                addCategory("android.intent.category.DEFAULT")
                data = Uri.parse("package:${applicationContext.packageName}")
                startActivityForResult(this, REQUEST_CODE_ALL_FILE_ACCESS)
            }
        } catch (e: Exception) {
            Intent().apply {
                action = Settings.ACTION_MANAGE_ALL_FILES_ACCESS_PERMISSION
                startActivityForResult(this, REQUEST_CODE_ALL_FILE_ACCESS)
            }
        }
    }

    protected override fun onActivityResult(
        requestCode: Int, resultCode: Int, @Nullable data: Intent?
    ) {
        super.onActivityResult(requestCode, resultCode, data)
        if (requestCode == REQUEST_CODE_ALL_FILE_ACCESS) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.R) {
                if (Environment.isExternalStorageManager()) {
                    // Storage access GRADNTED
                    setupAfterPermissionChecked()
                } else {
                    // Storage access DENIED
                    checkPlatformPermission()
                }
            }
        }
    }

    fun setupAfterPermissionChecked() {
        // Do something
    }

    companion object {
        private const val REQUEST_CODE_ALL_FILE_ACCESS = 404
    }
}
```


# Reference

[Manage all files on a storage device](https://developer.android.com/training/data-storage/manage-all-files)

[File.listFiles() is returning null in android 11](https://stackoverflow.com/questions/68032841/file-listfiles-is-returning-null-in-android-11)