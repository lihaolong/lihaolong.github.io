---
title: 数据结构和算法基础之查找算法
date: 2018-6-23
categories:
- programming
tags:
- datastructure
---

查找算法可分为无序查找和有序查找算法，无序查找算法的代表是散列对应jdk中的HashMap的底层算法，它的效率可以达到常数时间。有序查找算法的代表是二分法，通过分治的思想将问题的规模不断缩小，使查找效率达到了logN。
### 一、二分查找
#### 1.非递归的二分查找
下面copy了一个jdk的Collections类的二分查找算法
```
private static <T> int indexedBinarySearch(List<? extends T> l, T key, Comparator<? super T> c) {
    int low = 0;
    int high = l.size()-1;

    while (low <= high) {
        int mid = (low + high) >>> 1;
        T midVal = l.get(mid);
        int cmp = c.compare(midVal, key);

        if (cmp < 0)
            low = mid + 1;
        else if (cmp > 0)
            high = mid - 1;
        else
 return mid; // key found
  }
    return -(low + 1);  // key not found 
}
```
#### 2.递归版本的二分查找

```
private static int binarySearch(int[] a,int k,int low,int high,int mid){
    if(low==high)
        return a[mid]==k?mid:-1;
    if(a[mid]>k){
        high=mid-1;
        mid=(low+high)/2;
        return binarySearch(a,k,low,high,mid);
    }else if(a[mid]<k){
        low=mid+1;
        mid=(low+high)/2;
        return binarySearch(a,k,low,high,mid);
    }else if(a[mid]==k)
        return mid;
    return -1;
}
```
### 二、散列
散列是一种高效的数据结构，可以认为是一个大小固定的数组，数组的查找时间复杂度为常数时间。要想达到理想的查找效率，数据项在数组中要均匀分布，并且要能很好地处理冲突问题。
#### 1.散列函数
有些数据项的键值为整数，可以直接使用简单的散列函数对tablesize求余作为数组下标，tablesize一般选择素数来避免冲突。大部分情况下键值为字符串，这时候散列函数的选择就很重要了。这里copy一个jdk1.8中的HashMap的散列函数

```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
这个hash函数将key的hashcode的高16位和低16位进行异或运算，保留的hashcode的所有特征。
#### 2.冲突解决
冲突解决办法一般有“拉链法”和“线性探测法”。
拉链发就是将冲突的元素作为链表的下一个元素添加到冲突的位置，查找时再线性查找，jdk的早期HashMap版本就采用的这种办法，但是当冲突元素较多时，线性查找的效率就会很慢，因此jdk1.8采用了红黑树来存储冲突的元素。
线性探测发就是在发生冲突时线性地查找下一个空位置，这样可以使空位置得到很好的利用，但是很可能出现元素分布不均的情况，当某个位置冲突较多时这里附近会堆积大量元素使得再次插入的效率变低。
### 三、查找树
查找树和二分查找有很多相似之处，二叉查找树甚至可以转换为一个数组，它的查找过程和二分查找几乎一样。都是根据元素的有序性通过比较缩小问题的规模，提高查找效率。但是查找树存储的元素更加多样化，更加灵活和方便。
由于查找树在最坏的情况下会退化为线性查找，因此就需要维持左右子树的平衡来避免退化。红黑树是一个经典的平衡的树，这里copy一个jdk1.8的TreeMap在插入之后维持平衡的代码。

```
private void fixAfterInsertion(Entry<K,V> x) {
    x.color = RED;

    while (x != null && x != root && x.parent.color == RED) {
        if (parentOf(x) == leftOf(parentOf(parentOf(x)))) {
            Entry<K,V> y = rightOf(parentOf(parentOf(x)));
            if (colorOf(y) == RED) {
                setColor(parentOf(x), BLACK);
                setColor(y, BLACK);
                setColor(parentOf(parentOf(x)), RED);
                x = parentOf(parentOf(x));
            } else {
                if (x == rightOf(parentOf(x))) {
                    x = parentOf(x);
                    rotateLeft(x);
                }
                setColor(parentOf(x), BLACK);
                setColor(parentOf(parentOf(x)), RED);
                rotateRight(parentOf(parentOf(x)));
            }
        } else {
            Entry<K,V> y = leftOf(parentOf(parentOf(x)));
            if (colorOf(y) == RED) {
                setColor(parentOf(x), BLACK);
                setColor(y, BLACK);
                setColor(parentOf(parentOf(x)), RED);
                x = parentOf(parentOf(x));
            } else {
                if (x == leftOf(parentOf(x))) {
                    x = parentOf(x);
                    rotateRight(x);
                }
                setColor(parentOf(x), BLACK);
                setColor(parentOf(parentOf(x)), RED);
                rotateLeft(parentOf(parentOf(x)));
            }
        }
    }
    root.color = BLACK;
}
```
每次插入元素后，红黑树可能会不平衡，需要通过重新改变元素的颜色和左旋或右旋来维持树的平衡，旋转操作是通过左右子树的元素转移来调整左右子树的高度。