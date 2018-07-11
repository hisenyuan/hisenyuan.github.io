---
title: Java静态代码块
tags: [java,练习]
date: 2017-02-11 10:14:26
categories: java
---
有时候重新回味一下以前的知识也很美妙

总能发现以前自己没有怎么在意的细节

---

静态代码块是在类中独立于类成员的static语句块，可以有多个。

如果要初始化静态变量，可以声明一个静态块。

格式如下：
```
static {
	//块执行代码
}
```

静态块存在单独的内存中，仅在该类被加载时执行，示例如下：
```
package com.hisen.javaGaiShu.page91test20;

public class JingTaiDaiMaKuai {
	private static String a;
	private String b;
	static {
		JingTaiDaiMaKuai.a = "我学习了很多语言";
		System.out.println(a);
		JingTaiDaiMaKuai t = new JingTaiDaiMaKuai();
		t.fina();
		t.b="Java语言";
		System.out.println(t.b);
	}

	static {
		JingTaiDaiMaKuai.a = "I Love Java";
		System.out.println(a);
	}

	public static void main(String[] args) {

	}

	static {
		JingTaiDaiMaKuai.a = "我还将继续学习下去";
		System.out.println(a);
	}

	private void fina() {
		System.out.println("但是我最喜欢的是：");
	}
}
```

---

输出如下：
<!--more-->
```
我学习了很多语言
但是我最喜欢的是：
Java语言
I Love Java
我还将继续学习下去
```

静态代码块在运行main方法时可以直接调用而不用创建实例。

静态代码块直接是按顺序执行的。