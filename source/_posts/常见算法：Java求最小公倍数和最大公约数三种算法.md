---
title: 常见算法：Java求最小公倍数和最大公约数三种算法
keywords: [算法,Java,java求最大公约最小公倍数]
date: 2017/10/21 11:23
tags: [算法,java]
categories: 算法
---
最小公倍数：
数论中的一种概念，两个整数公有的倍数成为他们的公倍数

其中一个最小的公倍数是他们的最小公倍数

同样地，若干个整数公有的倍数中最小的正整数称为它们的最小公倍数

---

求最小公倍数算法：

最小公倍数=两整数的乘积÷最大公约数

---

求最大公约数算法：
### (1) 辗转相除法
有两整数a和b：
1. a%b得余数c
2. 若c=0，则b即为两数的最大公约数
3. 若c≠0，则a=b，b=c，再回去执行**1**

例如:求27和15的最大公约数过程为
1. 27÷15余12
2. 5÷12余3
3. 12÷3余0

因此，3即为最大公约数

---

代码实现：
```java
  /**
   * 最大公约数
   */
  public int getGCD(int m, int n) {
    if (n == 0) {
      return m;
    }
    return getGCD(n, m % n);
  }

  /**
   * 最小公倍数
   * @param m
   * @param n
   * @return
   */
  public int getLCM(int m, int n) {
    int mn = m * n;
    return mn / getGCD(m, n);
  }

  /**
     * 辗转相除求最大公约数
     * 有两整数a和b：
     * ① a%b得余数c
     * ② 若c=0，则b即为两数的最大公约数
     * ③ 若c≠0，则a=b，b=c，再回去执行①
     */
    public int divisionGCD(int m, int n) {
      int a;
      while (n != 0) {
        a = m % n;
        m = n;
        n = a;
      }
      return m;
    }
    /**
     * 相减法求最大公约数
     * 有两整数a和b：
     * ① 若a>b，则a=a-b
     * ② 若a<b，则b=b-a
     * ③ 若a=b，则a（或b）即为两数的最大公约数
     * ④ 若a≠b，则再回去执行①
     */
    public int subtractionGCD(int m,int n){
      while(m != n){
        if (m>n){
          m = m-n;
        }else {
          n = n - m;
        }
      }
      return m;
    }
```
