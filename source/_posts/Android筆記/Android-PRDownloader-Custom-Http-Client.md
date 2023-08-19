---
title: PRDownloader Custom HTTP Client
date: 2022-09-16 10:00:00
tags:
categories:
- Android 筆記
---

In this article, I will show you how to customize a HttpClient for PrDownloader

<!--more-->

Create a new class called `EvilHostnameVerifier`

```kotlin
import javax.net.ssl.HostnameVerifier
import javax.net.ssl.SSLSession

class EvilHostnameVerifier: HostnameVerifier {

    override fun verify(hostname: String?, sslString: SSLSession?): Boolean {
        return hostname == "example.com"
    }
}
```

Copy `DefaultHttpClient` from PRDownloader library, I name it the `EvilHttpClient`!

```
URLConnection
  ⎿ HttpURLConnection
    ⎿ HttpsURLConnection
```

```java
public class EvilHttpClient implements HttpClient {

    // The original one:
    private URLConnection connection;
    
    // Change to this:
    private HttpsURLConnection connection;

    // Other implementations...
    
    @Override
    public void connect(DownloadRequest request) throws IOException {
        connection = (HttpsURLConnection) new URL(request.getUrl()).openConnection();
        connection.setReadTimeout(request.getReadTimeout());
        connection.setConnectTimeout(request.getConnectTimeout());
        final String range = String.format(Locale.ENGLISH,
                "bytes=%d-", request.getDownloadedBytes());
        connection.addRequestProperty(Constants.RANGE, range);
        connection.addRequestProperty(Constants.USER_AGENT, request.getUserAgent());
        addHeaders(request);
        
        // Add hostname verifier before connect
        connection.setHostnameVerifier(new EvilHostnameVerifier());
        
        connection.connect();
    }
    
    // Other implementations...

}
```

Then add the custom HttpClient when initialize PRDownloader.

```kotlin
class MyApplication : Application() {

    // ...
    
    private fun setupPrDownloadManger() {
        val config = PRDownloaderConfig.newBuilder()
            .setHttpClient(EvilHttpClient())
            .setDatabaseEnabled(true)
            .setReadTimeout(30000)
            .setConnectTimeout(30000)
            .build()
        PRDownloader.initialize(applicationContext, config)
    }
}
```

There you go, no more "Hostname example.com was not verified"