---
title: Android下载文件
tags: [Android]
categories: [Android]
date: 2018-01-25 17:46:19
---



### Android下载文件
>使用 Android 的 HttpURLConnection 下载文件时，要警惕下载路径的重定向问题( httpUrlConnection.getResponseCode() == 302 ),否则会出现 下载的文件的 length =0 的情况 !
>
>

<!-- more -->

#### 贴上主要的代码:

```
 public static void downLoadPdfFile(String urlStr, String savePAth, String fileName) throws Exception {

        URL url = new URL(urlStr);
        HttpURLConnection httpUrlConnection = (HttpURLConnection) url.openConnection();

        httpUrlConnection.setInstanceFollowRedirects(true);

        Log.e(TAG, "httpUrlConnection.getResponseCode() :" + httpUrlConnection.getResponseCode());

//============
        if(httpUrlConnection.getResponseCode() == 302) {
            String location = httpUrlConnection.getHeaderField("Location");
            Log.e(TAG, "location:" + location);
            downLoadPdfFile(location,savePAth,fileName);
            return;
        }
//============
        
        InputStream inputstream = httpUrlConnection.getInputStream();


        File file = new File(savePAth, fileName);
        FileOutputStream outputstream = new FileOutputStream(file);

        byte buffer[] = new byte[512];
        int byteCount = 0;
        //将输入流中的内容先输入到buffer中缓存，然后用输出流写到文件中
        while ((byteCount = inputstream.read(buffer)) != -1) {
            outputstream.write(buffer, 0, byteCount);
        }

        Log.i("HttpUtil", "file length:" + file.length());

    }
```


* 相关的 URL

```
// downLoadUrl2 会重定向到 downLoadUrl5 .
    private String downLoadUrl2 = "https://api2.keylink.cn:443/v2/xfile/download";
    private String downLoadUrl3 = "https://avatars1.githubusercontent.com/u/";
    private String downLoadUrl4 = "https://github.com/keylink-corp/android-sdk/raw/master/docs/%E9%80%8F%E4%BC%A0Demo%E4%BD%BF%E7%94%A8%E6%96%87%E6%A1%A3.pdf";

    private String downLoadUrl5="http://keylink.oss-cn-hangzhou.aliyuncs.com/";

```
