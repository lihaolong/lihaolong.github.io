---
title: MySQL 学习总结（一）：基础知识
date: 2017-8-22
categories: 
- programming
tags:
  - mysql
---

## 1.基本增删改查

    insert into table values (value1,value2,value3...);

    delete from table where 删除条件;

    update table set col1 = value1,col2 = value2 where 条件;

    select col1,col2 from table where 条件;


## 2.查询语法

    SELECT selection_list /*要查询的列名称*/
    FROM table_list /*要查询的表名称*/
    WHERE condition /*行条件*/
    GROUP BY grouping_columns /*对结果分组*/
    HAVING condition /*分组后的行条件*/
    ORDER BY sorting_columns /*对结果分组*/
    LIMIT offset_start, row_count /*结果限定*/

* 条件查询：and、or、in、not in、is null、between and...
* 模糊查询：where like '_a%'
* distinct：去除重复记录
* desc：order by后降序排列，默认升序排列
* 聚合函数：count、max、min、sum、avg
* where 和 having：where是分组前的约束条件，having是分组后的约束条件。不符合where条件的数据不会参加分组
* limit：起始行从0开始

## 3.多表查询

* 内连接：得到两个表相匹配的所有数据
* 左外连接：A left join B 以A表为主表在右侧列出所有符合连接条件的数据，没有数据显示为null
* 右外连接：A right join B 以A表为主表

外链接三种情况：

* 对于table1中的每一条记录对应的记录如果在table2中也恰好存在而且刚好只有一条，那么就会在返回的结果中形成一条新的记录。
* 对于table1中的每一条记录对应的记录如果在table2中也恰好存在而且有N条，那么就会在返回的结果中形成 N条新的记录。
* 对于table1中的每一条记录对应的记录如果在table2中不存在，那么就会在返回的结果中形成一条新的记录，且该记录的右边全部NULL。

### where和on的区别

on是连接的条件，筛选出的是中间结果。where是对查询结果的筛选，会影响到查询结果。

### 子查询
查询结果作为另一个查询的条件的查询。

    select col1,col2 from table1 where col3 in (select col3 from table2 where 条件)；