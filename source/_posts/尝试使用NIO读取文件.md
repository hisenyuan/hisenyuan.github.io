---
title: 尝试使用NIO读取文件
keywords: [nio,java,文件读取]
date: 2017/6/1 14:45
tags: [java]
categories: java
---

具体代码如下，这里获取出来的文件大小是准确的

FileInputStream方式获取出来的大小会有差异

NIO与IO的对比详见：<a href="https://github.com/hisen-yuan/IDEAPractice/blob/9b237de2874042b896c134a911c5965b46c4ce25/src/main/java/com/hisen/nio/copyFile/NIOCopy.java" target="_blank">NIOCopy.java</a>
```
/**
   * 利用NIO进行读写文件
   *
   * @param oldFileName 原文件的路径
   * @param newFileName 新文件的路径
   */
  public static void nioCopy(String oldFileName, String newFileName) {
    try {
      FileChannel fileChannelIn = new FileInputStream(new File(oldFileName)).getChannel();
      FileChannel fileChannelOut = new FileOutputStream(new File(newFileName)).getChannel();
      //获取文件大小
      long size = fileChannelIn.size();
      System.out.printf("文件大小为：%s byte \n",size);
      //缓冲
      ByteBuffer byteBuffer = ByteBuffer.allocate(1024);

      long start = System.currentTimeMillis();
      while (fileChannelIn.read(byteBuffer) != -1) {
        //准备写
        byteBuffer.flip();
        fileChannelOut.write(byteBuffer);
        //准备读
        byteBuffer.clear();
      }
      long end = System.currentTimeMillis();
      System.out.printf("NIO方式复制完成，耗时 %s 秒\n",(end-start)/1000);
      //关闭
      fileChannelIn.close();
      fileChannelOut.close();
    } catch (FileNotFoundException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
```