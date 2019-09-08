---
title: java学习点滴（四）：多线程总结
date: 2017-8-2
categories:
- programming
tags:
- java
---

## 1.相关概念

* 进程：每个进程都有独立的代码和数据空间，一个进程可以包含1-n个线程
* 线程：每个线程都有独立的程序计数器和运行栈
* 线程和进程的五个阶段：创建、就绪、运行、阻塞、终止
* 并行：多个CPU或机器同时执行一段代码，是真正的同时
* 并发：通过调度算法使用户看上去是在同时执行，其实在CPU操作层面并不是同时执行
* 线程安全：使用多线程后，程序的运行结果和单线程一样没有改变
* 同步：使用人为的方法使得程序共享资源的同时实现线程安全，保证结果准确


* synchronize：用于线程中加同步锁
* volatile：修饰变量，避免多线程中对变量的修改后取值混乱的问题


## 2.线程状态和转换

![thread](http://45.78.34.36/images/thread.png)

调用wait()后，线程释放锁的同时进入blocked状态，再调用notify()或notifyAll()会恢复同步锁，当锁被释放后进入runnable状态。也可以在running状态时直接加同步锁，当锁释放后进入runnable状态。

## 3.多线程的使用

### （1）继承Thread类

```

    class Thread1 extends Thread{
	private String name;
    public Thread1(String name) {
       this.name=name;
    }
	public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(name + "运行  :  " + i);
            try {
                sleep((int) Math.random() * 10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
       
	}
    }
    public class Main {
	public static void main(String[] args) {
		Thread1 mTh1=new Thread1("A");
		Thread1 mTh2=new Thread1("B");
		mTh1.start();
		mTh2.start();

	}
    }
```
### （2）实现runnable接口

```

    class Thread2 implements Runnable{
	private String name;

	public Thread2(String name) {
		this.name=name;
	}
	@Override
	public void run() {
		  for (int i = 0; i < 5; i++) {
	            System.out.println(name + "运行  :  " + i);
	            try {
	            	Thread.sleep((int) Math.random() * 10);
	            } catch (InterruptedException e) {
	                e.printStackTrace();
	            }
	        }
		
	}	
    }
    public class Main {
	public static void main(String[] args) {
		new Thread(new Thread2("C")).start();
		new Thread(new Thread2("D")).start();
	}
    }
```

### （3）两种方式的比较

使用接口符合面向接口编程的设计思想。同时可以避免java单继承的局限。

## 4.常用方法

* sleep()和wait()的区别：调用sleep 方法时保持同步锁，仍然占有该锁。调用wait方法会释放同步锁
* yield()：使线程由running状态进入runnable状态