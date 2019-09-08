---
title: 数据结构和算法基础之树
date: 2018-6-18
categories:
- programming
tags:
- datastructure
---
树作为一种基础的数据结构，在查找算法中有很广泛的应用。其中比较有代表性的有二叉查找树、AVL树、B树、红黑树。AVL树和红黑树利用一些规则实现的树的平衡，使查找的最坏情况得到了控制，B树则通过控制树的高度使得查找的次数控制在一定范围内，在数据库的索引中得到了应用。
## 一、树的定义和实现
对于树的创建一般采用递归的方式。下面copy一个jdk中TreeMap的实现，这里用的是红黑树，不过和一般的树的创建方式大体相同。
```
private final Entry<K,V> buildFromSorted(int level, int lo, int hi,

 int redLevel,

 Iterator<?> it,

 java.io.ObjectInputStream str,

 V defaultVal)

 throws java.io.IOException, ClassNotFoundException {

 /**Strategy: The root is the middlemost element. To get to it, we have to first recursively construct the entire left subtree,so as to grab all of its elements. We can then proceed with right subtree.
The lo and hi arguments are the minimum and maximum indices to pull out of the iterator or stream for current subtree.They are not actually indexed, we just proceed sequentially,
ensuring that items are extracted in corresponding order.**/

 if (hi < lo) return null;

 int mid = (lo + hi) >>> 1;

 Entry<K,V> left = null;

 if (lo < mid)

 left = buildFromSorted(level+1, lo, mid - 1, redLevel,

 it, str, defaultVal);

  // extract key and/or value from iterator or stream

 K key;

 V value;

 if (it != null) {

 if (defaultVal==null) {

 Map.Entry<?,?> entry = (Map.Entry<?,?>)it.next();

 key = (K)entry.getKey();

 value = (V)entry.getValue();

 } else {

 key = (K)it.next();

 value = defaultVal;

 }

 } else { // use stream

 key = (K) str.readObject();

 value = (defaultVal != null ? defaultVal : (V) str.readObject());

 }

 Entry<K,V> middle = new Entry<>(key, value, null);

  // color nodes in non-full bottommost level red

 if (level == redLevel)

 middle.color = RED;

 if (left != null) {

 middle.left = left;

 left.parent = middle;

 }

 if (mid < hi) {

 Entry<K,V> right = buildFromSorted(level+1, mid+1, hi, redLevel,

 it, str, defaultVal);

 middle.right = right;

 right.parent = middle;

 }

 return middle;

 }
```
创建过程大致如下：
* （1）定义左儿子为下一次递归调用的返回值（即下一次递归的middle节点）
* （2）定义middel节点为（1）中的parent，同时定义middle节点的儿子为（1）
* （3）同样的方式定义右儿子以及其和middle的关系
## 二、二叉查找树
二叉查找树是所有节点的左子树的所有节点都比该节点的值小，右子树的所有节点都比该节点的值大。
### 1、常规操作
* 插入：通过递归查找到一个该值不存在的位置
* 删除：如果是树叶直接删除，如果有儿子则需要调整位置，将右子树的最小值调整到该节点
### 2、效率和问题分析
最好的情况下即平衡的二叉查找树查找效率可以达到㏒N，但是经过大量的插入和删除操作后树会变得不平衡，这就引出了可以通过调整平衡来避免二叉查找树出现最坏情况的AVL树和红黑树。
### 3、平衡的树
AVL树是一种古老的平衡查找树，它需要满足左右两棵子树的高度差不超过1的二叉查找树。AVL树通过旋转来达到平衡，旋转是通过调整节点的位置来使左右子树的高度发生变化。
红黑树具有以下性质：
* 每一个节点必须着成红色或黑色
* 根是黑色的
* 如果某节点是红色则其子节点必须是黑色
* 从一个节点到null的引用链必须包含相同数目的黑色节点
最后两个规则保证了红黑树的子树的高度差在一定的范围内
## 三、B树和索引
由于数据存储在磁盘上，而对磁盘的查询非常耗时，就需要一种减少磁盘访问的数据结构来做索引。B树其实就是一棵M阶的二叉查找树，它的节点可以有多个值，也可以有多个子树，这样就降低了树的高度，从而降低的查找次数。