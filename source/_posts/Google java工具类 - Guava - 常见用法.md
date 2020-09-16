---
title: Google java工具类 - Guava - 常见用法
keywords: [Google,Guava]
date: 2017/8/1 10:58
tags: [java,google,jar]
categories: java
---

概述

工具类 就是封装平常用的方法，不需要你重复造轮子，节省开发人员时间，提高工作效率。谷歌作为大公司，当然会从日常的工作中提取中很多高效率的方法出来。所以就诞生了Guava。

高效设计良好的API，被Google的开发者设计，实现和使用

遵循高效的java语法实践

使代码更刻度，简洁，简单

节约时间，资源，提高生产力 Guava工程

包含了若干被Google的 Java项目广泛依赖 的核心库，例如：
1. 集合 [collections]
2. 缓存 [caching]
3. 原生类型支持 [primitives support]
4. 并发库 [concurrency libraries]
5. 通用注解 [common annotations]
6. 字符串处理 [string processing]
7. I/O 等等。

部分用法如下：
<!--more-->
```java
public class CommonUsage {

  /**
   * 2.集合转换为特定格式的字符串
   */
  @Test
  public void collection2Srt() {
    //List转
    List<String> list = new ArrayList<String>();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");
    list.add("e");
    list.add("f");
    String result = Joiner.on("-").join(list);
    System.out.println(result);//a-b-c-d-e-f

    //Map 转
    HashMap<String, Integer> map = Maps.newHashMap();
    map.put("a", 1);
    map.put("b", 2);
    map.put("c", 3);
    //on 分隔符，withKeyValueSeparator key value之间的分隔符
    String s = Joiner.on(",").withKeyValueSeparator("=").join(map);
    System.out.println(s);//a=1,b=2,c=3
  }

  /**
   * 3.将String 转为特定的集合
   */
  @Test
  public void str2Collection() {
    String s = "1,2,3,4,5,6,7,8,9";
    List<String> list = Splitter.on(",").splitToList(s);
    System.out.println(list);

    s = "1-2-3-4- 5-  6  ";
    //去除空串与空格
    List<String> list1 = Splitter.on("-").omitEmptyStrings().trimResults().splitToList(s);
    System.out.println(list1);

    //将String转换为map
    String str = "a=1,b=2";
    Map<String, String> map = Splitter.on(",").withKeyValueSeparator("=").split(str);
  }

  /**
   * 4.多个字符切割，或者特定的正则分隔
   */
  @Test
  public void strSplit() {
    String input = "aa.dd,,ff,,.";
    List<String> list = Splitter.onPattern("[.|,]").omitEmptyStrings().trimResults()
        .splitToList(input);
    System.out.println(list);
    // 判断匹配结果
    boolean result = CharMatcher.inRange('a', 'z').or(CharMatcher.inRange('A', 'Z'))
        .matches('K');//true
    // 保留数字文本
    String s1 = CharMatcher.digit().retainFrom("abc 123 efg");//123
    // 删除数字文本
    String s2 = CharMatcher.digit().removeFrom("abc 123 efg");//abc  efg
  }
  /**
   * 9.计算程序开始结束用了多少时间
   */
  @Test
  public void countTime() {
    Stopwatch stopwatch = Stopwatch.createStarted();
    for (int i = 0; i < 100000; i++) {
    }
    //TimeUnit可以指定时间精度
    long nanos = stopwatch.elapsed(TimeUnit.MILLISECONDS);
    System.out.println(nanos);
  }
  /**
   * 10.Files 文件操作
   * 方便，
   */
  @Test
  public void filesOperation() {
    String fileName = "./src/main/java/com/hisen/jars/guava/test.txt";
    for (int i = 0; i < 1000; i++) {
      //写
      filesWrite(fileName, String.valueOf(i) + "\n");
    }
    //读
    List<String> list = filesRead(fileName);
    System.out.println("读取到的内容：" + list.toString());
//    Files.copy(sourceFile, targetFile); //复制文件
//    Files.equal(File,File);//比较文件内容是否完全一致
//    touch方法创建或者更新文件的时间戳。
//    createTempDir()方法创建临时目录
//    Files.createParentDirs(File) 创建父级目录
//    getChecksum(File)获得文件的checksum
//    hash(File)获得文件的hashmap系列方法获得文件的内存映射
//    getFileExtension(String)获得文件的扩展名
//    getNameWithoutExtension(String file)获得不带扩展名的文件名
  }

  /**
   * Files写文件
   *
   * @param fileName 文件名（包括路径）
   * @param contents 写入的单行内容
   */
  public void filesWrite(final String fileName, final String contents) {
    checkNotNull(fileName, "文件名称不能为空");
    checkNotNull(contents, "写入内容不能为空");
    final File file = new File(fileName);
    try {
      //Files.write(contents.getBytes(), file);//覆盖
      Files.append(contents, file, Charsets.UTF_8);//追加
    } catch (IOException e) {
      System.out.println("ERROR trying to write to file '" + fileName + "' - "
          + e.toString());
    }
  }

  /**
   * Files 文件单行读取
   *
   * @param filePath 文件名(包括路径)
   */
  public List<String> filesRead(String filePath) {
    checkNotNull(filePath, "文件名称不能为空");
    File file = new File(filePath);
    List<String> list = null;
    try {
      list = Files.readLines(file, Charsets.UTF_8);
    } catch (IOException e) {
      e.printStackTrace();
    }
    return list;
  }
}
```

部分摘自：<a href="http://blog.csdn.net/yyychyzzzz/article/details/54983574" target="_blank">点击查看</a>