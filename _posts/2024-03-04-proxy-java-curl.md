---
layout: post
title:  "How to use proxy with CURL and HTTP requests from Java code"
date:   2024-03-04
categories: java curl http proxy
---

## CURL is easy pizy :
```bash
# export corresponding env vars:
export http_proxy=http://HOST:PORT
export https_proxy=http://HOST:PORT

# and execute request to host that requires proxy:
curl https://your-host-dot-com-slash-something

# or you can specify proxy directly in curl:
curl --proxy http://proxy:port https://your-host-dot-something
```

## Java is trickier :
```bash
# pass the following arguments to your program:
java \
-Dhttp.proxySet=true \
-Dhttp.proxyHost=host.com \
-Dhttp.proxyPort=port-number \
-Dhttps.proxySet=true \
-Dhttps.proxyHost=host.com \
-Dhttps.proxyPort=port-number \
...
your-app.jar
```

Here is an example of an app for testing:
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Arrays;

public class TestIP {

    public static void main(String[] args) throws IOException {
        System.out.println("Args = " + Arrays.toString(args));
        String host = args[0];
        System.out.println("URL = " + host);

        URL url = new URL(host);
        HttpURLConnection connection = (HttpURLConnection)url.openConnection();
        connection.setRequestMethod("GET");
        int status = connection.getResponseCode();

        System.out.println("Response Status = " + status);
        BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
        StringBuffer sb = new StringBuffer();

        String line;
        while((line = br.readLine()) != null) {
            sb.append(line);
        }

        br.close();
        connection.disconnect();
        System.out.println("Response = " + sb);
    }
}
```
