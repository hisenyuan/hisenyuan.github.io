---
title: Data URI scheme 利用base64字符串通过image标签显示图片
keywords: [ata:image/png;base64,image]
date: 2017/5/12 9:15
tags: [java,jsp]
categories: java
---
目前，Data URI scheme支持的类型有：

1. data:,文本数据
2. data:text/plain,文本数据
3. data:text/html,HTML代码
4. data:text/html;base64,base64编码的HTML代码
5. data:text/css,CSS代码
6. data:text/css;base64,base64编码的CSS代码
7. data:text/javascript,Javascript代码
8. data:text/javascript;base64,base64编码的Javascript代码
9. data:image/gif;base64,base64编码的gif图片数据
10. data:image/png;base64,base64编码的png图片数据
11. data:image/jpeg;base64,base64编码的jpeg图片数据
12. data:image/x-icon;base64,base64编码的icon图片数据

Data URL是在本地直接绘制图片，不是从服务器加载，所以节省了HTTP连接，起到加速网页的作用。

也无法获取到图片在服务器上的真实地址

注意：本方法适合于小图片，大图片就不要考虑了，另外IE8以下浏览器不支持这种方法。

用这种方法会加重客户端的CPU和内存负担，总之有利有弊。

前台代码：
```
<%@ page import="com.hisen.image.ShowImageByBase64" %><%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2017/5/11
  Time: 18:55
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<% String imageStr = ShowImageByBase64.showimage();%>
<html>
<head>
    <title>Title</title>
</head>
<body>
<img src="data:image/png;base64,<%=imageStr%>" alt="base64图片"/>
</body>
</html>
```

后台代码：
```
package com.hisen.image;

import com.hisen.utils.Base64Util;
import com.hisen.utils.File2ByteArraysUtil;
import java.io.ByteArrayOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import sun.misc.BASE64Encoder;

/**
 * <img src="data:image/png;base64,<%=imageStr%>" alt="base64image"/>
 * Created by hisenyuan on 2017/5/11 at 18:44.
 */
public class ShowImageByBase64 {

  public static String showimage() {
    //写相对路径会报错，暂时不知道如何解决
    String imagePath = "C:\\1\\830.jpg";
    byte[] bytes = File2ByteArraysUtil.file2Bytes(imagePath);
    String s = Base64Util.encodeBase64(bytes);
    return s;
  }

  /**
   * sun.misc.BASE64Encoder
   */
  public static String encodeBase64(byte[] str) {
    if (str == null) {
      return null;
    } else {
      BASE64Encoder encoder = new BASE64Encoder();
      try {
        return encoder.encode(str);
      } catch (Exception var3) {
        return null;
      }
    }
  }

  /***
   * file2byte[]
   * @param path
   * @return
   */
  public static byte[] file2Bytes(String path) {
    byte[] buffer = null;
    File file = new File(path);
    try {
      FileInputStream fis = new FileInputStream(file);
      ByteArrayOutputStream bos = new ByteArrayOutputStream(1000);
      byte[] b = new byte[1000];
      int n;
      while ((n = fis.read(b)) != -1) {
        bos.write(b, 0, n);
      }
      fis.close();
      bos.close();
      buffer = bos.toByteArray();
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
    return buffer;
  }
}
```