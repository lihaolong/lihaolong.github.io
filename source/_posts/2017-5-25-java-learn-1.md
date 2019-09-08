---
title: java学习点滴（一）：学习笔记汇总
date: 2017-5-25
categories:
- programming
tags:
- java
---
## 1.JVM相关

### （1）JVM内存模型和结构
* java堆：最大，被所有线程所共享，JVM启动时创建。目的：存放对象实例。
* 方法区：各线程共享，存储被JVM加载的类信息、常量、静态变量、即时编译器编译后的代码。
* 程序计数器：较小内存空间，当前线程所执行的字节码的行号指示器，字节码解释器通过改变这个计数器的值来选取下一条需要执行的字节码。多线程的每一条线程都需要一个独立的程序计数器。

* JVM栈：线程私有，生命周期与线程相同，方法对应栈帧。存放基本类型的变量和对象的引用变量。<br>
  stackoverflow:大量递归，不断压栈导致。
* 本地方法栈：对应JVM用到的native方法。
* java heap溢出：没有及时回收对象，达到最大容量限制。

### （2）GC原理
 * 程序计数器，JVM栈，native栈随线程生灭，无需GC。
 * GC算法：标记清除，先标记需要回收的对象，完成后统一回收。

### （3）实例创建过程
* 判断类是否加载
* 解析，初始化
* 为新对象分配内存
* 解决并发安全问题
* 初始化分配到的内存
* 设置实例对象
* 初始化对象方法

### （4）OutOfMemoryerror
没有内存完成实例分配时会发生OutOfMemoryerror。可能会发生在java堆，方法区，JVM栈。

## 2.数据类型

### （1）基本数据类型
short，int，long,float,double,byte,char,boolean
### （2）包装类
每个基本类型都有一个包装类，在java.lang包中。

* 用途：作为基本数据类型的类类型，方便涉及对象的操作。包含基本数据类型的相关属性，如最大值，最小值和相关操作。

相互转化：<br>
String转int:

    Integer.parseInt(Str)

int转String（三种）:

```

    String.valueOf(i);
    Integer.toString(i);
    String str = ""+i;
```
String转Integer:

    Integer.valueOf(Str)

String转int：

    String.valueOf(i)

int和integer的相互转化由于JDK1.5的自动装箱和拆箱，可以自动相互转化。
### （3）String
String是特殊的包装类。<br>

    String str = "abc";

的创建过程：

* 栈中创建引用变量str
* 常量池中找是否存在"abc"
* 如果存在，找到这个对象并把引用指向这个对象
* 如果不存在，创建新对象并放入常量池，指向这个对象

所以：

    String str1 = "abc";
    String str2 = "abc";
    str1==str2;

返回true。

“==”和“equals()”：String重写了Object的equals()方法，所以比较的是内容，“==”比较的是地址。（Math类也重写了equals()方法）。

StringBuffer：可变，效率比String高（String不可变，每次改变都要创建一个新对象），线程安全。
StringBuilder：可变，效率比StringBuffer高，适合单线程。

String常用方法：

* length()
* charAt(index)
* subString(beginIndex,endIndex)
* compareTo(anotherString)
* concat()
* indexOf(str)
* toUpperCase()
* reolace()
* contains(str)
* split(str)