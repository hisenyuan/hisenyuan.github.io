---
title: String 和 StringBuilder、StringBuffer的区别
date: 2017-02-08 21:07:59
tags: [java,StringBuilder,StringBuffer的区别]
---
String和StringBuilder、StringBuffer的区别？ 
---
答：Java平台提供了两种类型的字符串：String和StringBuffer/StringBuilder，

它们可以储存和操作字符串。其中String是只读字符串，

也就意味着String引用的字符串内容是不能被改变的。

而StringBuffer/StringBuilder类表示的字符串对象可以直接进行修改。

StringBuilder是Java 5中引入的，它和StringBuffer的方法完全相同，

区别在于它是在单线程环境下使用的，因为它的所有方面都没有被synchronized修饰，

因此它的效率也比StringBuffer要高。

**简而言之：**

String：不能被修改

StringBuffer：可以随意修改，有synchronized修饰，是线程安全的，效率略低

StringBuilder：可以随意修改，无synchronized修饰，不是线程安全的，效率高

<!--more-->

**面试题1：说出程序的输出结果**
---
```
classStringEqualTest {
 
	publicstaticvoidmain(String[] args) {
		String s1 = "Programming";
		String s2 = new String("Programming");
		String s3 = "Program" + "ming";
		System.out.println(s1 == s2);//false
		System.out.println(s1 == s3);//true
		System.out.println(s1 == s1.intern());//true
	}
}
```
存在于.class文件中的常量池，在运行期被JVM装载，并且可以扩充。

String的intern()方法就是扩充常量池的一个方法；

当一个String实例str调用intern()方法时，

Java查找常量池中是否有相同Unicode的字符串常量，

如果有，则返回其的引用， 

如果没有，则在常量池中增加一个Unicode等于str的字符串并返回它的引用

**面试题2**
---
什么情况下用+运算符进行字符串连接比调用StringBuffer/StringBuilder对象的append方法连接字符串性能更好？

答：

如果使用少量的字符串操作，使用 (+运算符)连接字符串； 

如果频繁的对大量字符串进行操作，则使用 

1：全局变量或者需要多线程支持则使用StringBuffer； 

2：局部变量或者单线程不涉及线程安全则使有StringBuilder。