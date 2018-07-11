---
title: Java判断全角半角字符以及相互转换
keywords: [Java判断全角半角字符以及相互转换,java全角半角,java正则判断全角半角]
date: 2017/4/1 11:01
tags: [java]
categories: java
---
在计算机屏幕上，一个汉字要占两个英文字符的位置，人们把一个英文字符所占的位置称为"半角"，

相对地把一个汉字所占的位置称为"全角"。在汉字输入时，系统提供"半角"和"全角"两种不同的输入状态，

但是对于英文字母、符号和数字这些通用字符就不同于汉字，在半角状态它们被作为英文字符处理；

而在全角状态，它们又可作为中文字符处理。

半角和全角切换方法：单击输入法工具条上的按钮或按键盘上的Shift+Space键来切换。

1、全角：指一个字符占用两个标准字符位置。

汉字字符和规定了全角的英文字符及国标GB2312-80中的图形符号和特殊字符都是全角字符。一般的系统命令是不用全角字符的，只是在作文字处理时才会使用全角字符。

---

2、半角：指一字符占用一个标准的字符位置。

通常的英文字母、数字键、符号键都是半角的，半角的显示内码都是一个字节。在系统内部，以上三种字符是作为基本代码处理的，所以用户输入命令和参数时一般都使用半角。

---

3、全角与半角各在什么情况下使用？

全角占两个字节，半角占一个字节。

半角全角主要是针对标点符号来说的，全角标点占两个字节，半角占一个字节，而不管是半角还是全角，汉字都还是要占两个字节。

在编程序的源代码中只能使用半角标点（不包括字符串内部的数据）

在不支持汉字等语言的计算机上只能使用半角标点（其实这种情况根本就不存在半角全角的概念）

对于大多数字体来说，全角看起来比半角大，当然这不是本质区别了。

---

4、全角和半角的区别
全角就是字母和数字等与汉字占等宽位置的字。半角就是ASCII方式的字符，

在没有汉字输入法起做用的时候输入的字母数字和字符都是半角的。

在汉字输入法出现的时候，输入的字母数字默认为半角，但是标点则是默认为全角，

可以通过鼠标点击输入法工具条上的相应按钮来改变。

---

5、关于“全角”和“半角”：

全角：是指中GB2312-80（《信息交换用汉字编码字符集·基本集》）中的各种符号。

半角：是指英文件ASCII码中的各种符号。

全角状态下字母、数字符号等都会占两个字节的位置，也就是一个汉字那么宽，半角状态下，

字母数字符号一般会占一个字节，也就是半个汉字的位置，全角半角对汉字没有影响。

有两种方式可以判断:

1:通过正则表达式来进行判断  [^\\x00-\\xff]

2: 通过字符编码的范围进行判断.

通过打印所有的字符发现：

1. 半角字符是从33开始到126结束
2. 与半角字符对应的全角字符是从65281开始到65374结束
3. 其中半角的空格是32.对应的全角空格是12288
4. 半角和全角的关系很明显,除空格外的字符偏移量是65248(65281-33 = 65248)

具体的代码如下：
<!--more-->
```
package com.hisen.String;

/**
 * 半角字符和全角字符的转换 以及 判断
 * Created by hisenyuan on 2017/4/1 at 10:37.
 */
public class FullHalf {
    /**
     * ASCII表中可见字符从!开始，偏移位值为33(Decimal)
     */
    static final char DBC_CHAR_START = 33; // 半角!

    /**
     * ASCII表中可见字符到~结束，偏移位值为126(Decimal)
     */
    static final char DBC_CHAR_END = 126; // 半角~

    /**
     * 全角对应于ASCII表的可见字符从！开始，偏移值为65281
     */
    static final char SBC_CHAR_START = 65281; // 全角！

    /**
     * 全角对应于ASCII表的可见字符到～结束，偏移值为65374
     */
    static final char SBC_CHAR_END = 65374; // 全角～

    /**
     * ASCII表中除空格外的可见字符与对应的全角字符的相对偏移
     */
    static final int CONVERT_STEP = 65248; // 全角半角转换间隔

    /**
     * 全角空格的值，它没有遵从与ASCII的相对偏移，必须单独处理
     */
    static final char SBC_SPACE = 12288; // 全角空格 12288

    /**
     * 半角空格的值，在ASCII中为32(Decimal)
     */
    static final char DBC_SPACE = ' '; // 半角空格
    public static void main(String[] args) {
        String s = "123456";
        //半角转换成全角字符
        String s1 = bj2qj(s);
        //全角转换成半角
        String s2 = qj2bj(s1);
        System.out.println("全角："+s1 +" -> 半角："+s2);
        System.out.println("--------------------------");
        String fh = s1+s2;
        //判断全角还是半角
        fullOrHalf(fh);
        //打印ASCII表中所有字符
        printAllChar();
    }
    /**
     * <PRE>
     * 半角字符->全角字符转换
     * 只处理空格，!到˜之间的字符，忽略其他
     * </PRE>
     */
    private static String bj2qj(String src) {
        if (src == null) {
            return src;
        }
        StringBuilder buf = new StringBuilder(src.length());
        char[] ca = src.toCharArray();
        for (int i = 0; i < ca.length; i++) {
            if (ca[i] == DBC_SPACE) { // 如果是半角空格，直接用全角空格替代
                buf.append(SBC_SPACE);
            } else if ((ca[i] >= DBC_CHAR_START) && (ca[i] <= DBC_CHAR_END)) { // 字符是!到~之间的可见字符
                buf.append((char) (ca[i] + CONVERT_STEP));
            } else { // 不对空格以及ascii表中其他可见字符之外的字符做任何处理
                buf.append(ca[i]);
            }
        }
        return buf.toString();
    }
    /**
     * <PRE>
     * 全角字符->半角字符转换
     * 只处理全角的空格，全角！到全角～之间的字符，忽略其他
     * </PRE>
     */
    public static String qj2bj(String src) {
        if (src == null) {
            return src;
        }
        StringBuilder buf = new StringBuilder(src.length());
        char[] ca = src.toCharArray();
        for (int i = 0; i < src.length(); i++) {
            if (ca[i] >= SBC_CHAR_START && ca[i] <= SBC_CHAR_END) { // 如果位于全角！到全角～区间内
                buf.append((char) (ca[i] - CONVERT_STEP));
            } else if (ca[i] == SBC_SPACE) { // 如果是全角空格
                buf.append(DBC_SPACE);
            } else { // 不处理全角空格，全角！到全角～区间外的字符
                buf.append(ca[i]);
            }
        }
        return buf.toString();
    }

    /**
     * 使用正则表达式判断字符是否为全角
     * @param str
     */
    public static void fullOrHalf(String str){
        char[] chars = str.toCharArray();
        for (int i = 0; i < chars.length; i++) {
            String temp = String.valueOf(chars[i]);
            // 正则判断是全角字符
            if (temp.matches("[^\\x00-\\xff]")) {
                System.out.println("全角 -> " + temp);
            }
            // 判断是半角字符
            else {
                System.out.println("半角 -> " + temp);
            }
        }
    }

    /**
     * 打印所有字符
     */
    public static void printAllChar(){
        for (int i = Character.MIN_VALUE; i <= Character.MAX_VALUE; ++i) {
            System.out.println("ASCII："+i + " -> " + "字符："+(char)i);
        }
    }
}
```