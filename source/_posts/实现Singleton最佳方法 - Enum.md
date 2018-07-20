---
title: Java实现Singleton最佳方法 - Enum
keywords: [java,singleton,enum]
date: 2018/1/27 23:42
tags: [java]
categories: java
---
effective java：
```
单元素的枚举类型已经成为实现Singleton的最佳方法
```
理由：
1. 因为枚举单例有序列化和线程安全的保证
2. 避免反射和并发困扰

单例模式模式：<a href="https://github.com/hisenyuan/IDEAPractice/tree/master/src/main/java/com/hisen/interview/effective/no3enumsingleton" target="_blank">完整代码+测试</a>

主要代码：

```java
public class EnumSingleton {

  private EnumSingleton() {
  }

  public static EnumSingleton getInstance() {
    return Singleton.INSTANCE.getInstance();
  }

  private enum Singleton {
    INSTANCE;
    private EnumSingleton singleton;

    Singleton() {
      singleton = new EnumSingleton();
    }

    public EnumSingleton getInstance() {
      return singleton;
    }
  }
}
```
