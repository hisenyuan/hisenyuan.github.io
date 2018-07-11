---
title: HashMap和Hashtable的区别
keywords: [HashMap和Hashtable的区别]
tags: [HashMap,Hashtable]
date: 2017-02-17 16:34:17
categories: java
---
在Java 2以前，一般使用Hashtable来映射键值和元素。为了使用Java集合框架，Java对Hashtable进行了重新设计，但是，为了向后兼容保留了所有的方法。Hashtable实现了Map接口，除了Hashtable具有同步功能之外，它与HashMap的用法是一样的。·在使用时一般是用ArrayList代替Vector，LinkedList代替Stack，HashMap代替HashTable，即使在多线程中需要同步，也是用同步包装类。

另外在使用上还有一些小的差异，比如：
1. HashTable的key和value都不允许为null值，而HashMap的key和value则都是允许null值的。这个其实没有好坏之分，只是Sun为了统一Collection的操作特性而改进的。
2. HashTable有一个contains(Object value)方法，功能上与containsValue(Object value)一样，但是在实现上花销更大，现在已不推荐使用。而HashMap只有containsValue(Object value)方法。
3. HashTable使用Enumeration，HashMap使用Iterator。Iterator其实与Enmeration功能上很相似，只是多了删除的功能。用Iterator不过是在名字上变得更为贴切一些。模式的另外一个很重要的功用，就是能够形成一种交流的语言（或者说文化）。有时候，你说Enumeration大家都不明白，说Iterator就都明白了。


在实现上两者已有一些差异，这里简单说明一下：
```
public Hashtable(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal Load: "+loadFactor);
        if (initialCapacity==0)
            initialCapacity = 1;
        this.loadFactor = loadFactor;
        table = new Entry[initialCapacity];
        threshold = (int)Math.min(initialCapacity * loadFactor, MAX_ARRAY_SIZE + 1);
        initHashSeedAsNeeded(initialCapacity);
    }
public Hashtable(int initialCapacity) {
        this(initialCapacity, 0.75f);
    }
public Hashtable() {
        this(11, 0.75f);
    }


public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        threshold = initialCapacity;
        init();
    }
public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }
public HashMap() {
        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR);
    }
```
HashTable中构造hash数组时initialCapacity默认大小是11，增加的方式是 old*2+1。HashMap中构造hash数组时initialCapacity默认大小是16，而且一定是2的指数。对于哈希值的使用也有所不同，HashTable直接使用对象的hashCode，代码是这样的：
```
int hash = key.hashCode();
int index = (hash & 0x7FFFFFFF) % tab.length;
```
而HashMap重新计算hash值，而且用与代替求模：
```
int hash = hash(k);
int i = indexFor(hash, table.length);
 
static int hash(Object x) {
　　int h = x.hashCode();
 
　　h += ~(h << 9);
　　h ^= (h >>> 14);
　　h += (h << 4);
　　h ^= (h >>> 10);
　　return h;
}
static int indexFor(int h, int length) {
　　return h & (length-1);
}
```

仅供参考，内容来源于互联网