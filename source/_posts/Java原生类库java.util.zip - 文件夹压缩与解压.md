---
title: Java原生类库java.util.zip - 文件夹压缩与解压
keywords: [java压缩文件夹,java解压文件,java压缩文件,java文件压缩,java文件解压]
date: 2017/4/26 14:49
tags: [java,解压]
categories: java
---
到处搜了一下也没有看到专门做好的jar包

真实的目录结构如下：
```
C:\1\hisenyuan\build.png
C:\1\hisenyuan\DSCN6812.JPG
C:\1\hisenyuan\test\test\hello.zip
C:\1\hisenyuan\test\hisenyuan.zip
C:\1\hisenyuan\test\test.txt
C:\1\hisenyuan\test\test\hello\hello.txt
C:\1\hisenyuan\tomcat.png
```
压缩包的目录结构如下：
```
build.png
DSCN6812.JPG
test\hello.zip
test\hisenyuan.zip
test\test.txt
test\hello\hello.txt
tomcat.png
```

全部代码如下：
<!--more-->
```
package com.hisen.utils;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.zip.ZipEntry;
import java.util.zip.ZipFile;
import java.util.zip.ZipInputStream;
import java.util.zip.ZipOutputStream;
import org.apache.commons.io.IOUtils;
import org.junit.Test;

/**
 * 文件夹压缩解压工具
 * Created by hisenyuan on 2017/4/20 at 17:27.
 */
public class ZipOrUnZipFileUtil {

  private InputStream is;
  private ZipOutputStream zos;
  private int lastIndexOf;

  @Test
  public void testZipOrUnZipFile() {
    //分隔符，windows linux下有所不同
    String separator = File.separator;
    //想要压缩的文件所在目录 C:\1\hisenyuan
    String folderPath = "c:" + separator + "1" + separator + "hisenyuan";
    //压缩文件路径：C:\1\hisenyuan\hisenyuan.zip
    String zipFilePath = "c:" + separator + "1" + separator + "hisenyuan" + ".zip";
    //解压文件所在的目录 E:\file\hisenyuan
    String newPath = "e:" + separator + "file" + separator + "hisenyuan";

    unZipFile(zipFilePath, newPath);
    zipFile(folderPath);
  }


  /**
   * 压缩文件
   *
   * @param filePath 压缩文件夹的路径
   */
  private void zipFile(String filePath) {
    File file = new File(filePath);
    File zipFile = new File(filePath + ".zip");
    lastIndexOf = file.getAbsolutePath().length() + 1;
    try {
      zos = new ZipOutputStream(new FileOutputStream(zipFile));
      zos.setComment("log");
      long start = System.currentTimeMillis();
      listAllFile(filePath);
      long stop = System.currentTimeMillis();
      System.out.println("zip done，time：" + (stop - start) / 1000 + "s");
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } finally {
      IOUtils.closeQuietly(zos, is);
    }
  }

  /**
   * 循环遍历当前文件夹下的所有文件，使用递归
   */
  private void listAllFile(String filePath) {
    File file = new File(filePath);
    if (file.exists()) {
      File[] files = file.listFiles();
      if (files == null) {
        System.out.println("folder is null");
      } else {
        for (File file2 : files) {
          if (file2.isDirectory()) {
            listAllFile(file2.getAbsolutePath());
          } else {
            String file3 = file2.getAbsolutePath();
            try {
              is = new FileInputStream(file3);
              //在zip压缩包当中出现的文件名
              String name = file3.substring(lastIndexOf, file3.length());
              System.out.println("name:" + name);
              zos.putNextEntry(new ZipEntry(name));
              int temp;
              int bufferSize = 1024 * 5;
              byte[] buffer = new byte[bufferSize];
              while ((temp = is.read(buffer, 0, bufferSize)) != -1) {
                zos.write(buffer, 0, temp);
                zos.flush();
              }
            } catch (IOException e) {
              e.printStackTrace();
            }
          }
        }
      }
    }
  }

  /**
   * 解压文件
   *
   * @param filePath 压缩文件所在目录
   * @param newPath 想解压到那个目录
   */
  private void unZipFile(String filePath, String newPath) {
    //压缩文件所在的父目录
    String oldPath = new File(filePath).getParentFile().toString();
    File outFile;
    ZipInputStream zipInputStream = null;
    OutputStream outputStream = null;
    InputStream inputStream = null;
    ZipEntry zipEntry;
    try {
      ZipFile zipFile = new ZipFile(filePath);

      zipInputStream = new ZipInputStream(new FileInputStream(filePath));
      while (null != (zipEntry = zipInputStream.getNextEntry())) {
        System.out.println("解压缩" + zipEntry.getName() + "文件。");
        //newPath为空就代表解压在当前目录
        if ("".equals(newPath) || newPath.isEmpty()) {
          outFile = new File(oldPath + zipEntry.getName());
        } else {
          //防止传入的目录不存在
          File file = new File(newPath);
          if (!file.exists()) {
            file.mkdir();
          }
          outFile = new File(newPath + File.separator + zipEntry.getName());
        }
        //判断当前文件路径是否存在，不存在就创建
        buildFile(outFile);
        inputStream = zipFile.getInputStream(zipEntry);
        outputStream = new FileOutputStream(outFile);
        int temp;
        int bufferSize = 1024 * 5;
        byte[] buffer = new byte[bufferSize];
        while ((temp = inputStream.read(buffer, 0, bufferSize)) != -1) {
          outputStream.write(buffer, 0, temp);
        }
      }
    } catch (IOException e) {
      e.printStackTrace();
    } finally {
      IOUtils.closeQuietly(inputStream, outputStream, zipInputStream);
    }
  }

  /**
   * 判断文件是否存在，不存在创建
   */
  private static void buildFile(File file) {
    if (!file.exists()) {
      File parent = file.getParentFile();
      if (parent != null && !parent.exists()) {
        parent.mkdirs();
      }
      try {
        file.createNewFile();
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```