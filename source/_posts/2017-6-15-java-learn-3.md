---
title: java学习点滴（三）：学习笔记汇总
date: 2017-6-15
categories:
- programming
tags:
- java
---
## 1.java集合框架

### （1）Collection

list：有序可重复

* ArratList，底层数组，访问快，增删慢（要移动元素）
* LinkedList，底层链表访问慢，增删块

set：无序不可重复

* HashSet，顺序有HashCode决定，查询速度快
* TreeSet，基于TreeMap，有序的set

### （2）map

HashMap：通过HashCode快速查找。<br>
TreeMap：有序的map。

### （3）Collection和Collections的区别

Collection是集合框架的一个接口，是list和set的父接口。<br>
Collections是一个工具类服务于Collection框架，不能实例化，包含搜索，排序等方法。

## 2.异常机制

定义：阻止当前程序方法或作用域继续执行的问题。<br>
好处：提高程序的健壮性和可维护性。<br>
throws：用于方法抛出异常。<br>
throw：用于语句抛出异常。<br>

## 3.抽象类和接口

抽象类：只要包含一个抽象方法就必须定义为抽象类。不能实例化，子类必须实现抽象方法。<br>
接口：所有数据成员都是public static final，所有方法都是抽象方法。即只定义了声明，没有实现。<br>

区别：

* 抽象层次：抽象类是对类的抽象，接口是对行为的抽象。
* 跨域：抽象类体现的是“is a”的继承关系，父类和子类在概念本质上是一样的，接口只需满足具有相同的行为即可。
* 设计层次：抽象类是自底向上，现有子类再设计父类。接口是自顶向下，先抽象出具体的行为，再有具体实现。