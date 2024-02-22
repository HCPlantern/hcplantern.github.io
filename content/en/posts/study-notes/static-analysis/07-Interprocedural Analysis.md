---
title: Static Analysis 07 Interprocedual Analysis
date: 2022-10-27
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}

- [【课程笔记】南大软件分析课程5——过程间分析（课时7） - 简书 (jianshu.com)](https://www.jianshu.com/p/2d14c0ae41cd)
- [软件分析笔记：3.过程分析(Interprocedural Analysis) - crossain - 博客园 (cnblogs.com)](https://www.cnblogs.com/crossain/p/12720612.html)

{{</admonition>}}

## 重点

- 学习如何利用类层级分析来构建调用图
- 过程间控制流
- 过程间数据流分析
- 过程间的常量传播

## Motivation

**问题**：过程内的分析未考虑函数调用，导致分析不精确。

两者的区别如下：

**过程内分析 Intra-procedural Analysis**
-   只考虑过程内部语句，不考虑过程调用
-   目前的所有分析都是过程内的

**过程间分析 Inter-procedural Analysis**
-   考虑过程调用的分析
-   有时又称为全程序分析 (Whole Program Analysis)
-   增加 Call edges 和 Return edges
-   需要 call graph

## 调用图构建

### 调用图

定义：本质可以看作是调用边的集合，从调用点（call-sites）到目标函数（target methods / callees）的边。

示例：  

![image-20221027220554583](https://cdn.hcplantern.cn/img/2022/10/27/20221027-220555.png-default)

应用：

- 所有过程间分析（跨函数分析）的基础
- 程序优化
- 程序理解
- 程序调试
- 程序测试

### 面向对象语言的调用图构造（Java）

#### 代表性算法

从上往下精度变高，速度变慢。本节课将重点分析第 1、4 个算法。

*   Class hierarchy analysis (CHA)
*   Rapid type analysis (RTA)
*   Variable type analysis (VTA)
*   Pointer analysis (k-CFA)

#### Java 调用分类

![image-20221027220801880](https://cdn.hcplantern.cn/img/2022/10/27/20221027-220803.png-default)

其中 Virtual call 的关键步骤是 Method Dispatch，就是**找到最终调用的实际函数**

该过程涉及到 2 个要素：

1.  receiver object 的具体类型：$c$
2.  调用点的函数签名 （method signature）：$m$（通过 signature 可以唯一确定一个函数）
    1.  signature = class type （方法具体所在的类） + method name（方法名） + descriptor
    2.  Descriptor = return type + parameter types

简记为 $C.foo(P, Q, R)$

![image-20221027221224719](https://cdn.hcplantern.cn/img/2022/10/27/20221027-221225.png-default)


### Method Dispatch（virtual call)

**定义**：用 $Dispatch(c, m)$ 来模拟动态 Method Dispatch 过程

![image-20221027221445385](https://cdn.hcplantern.cn/img/2022/10/27/20221027-221446.png-default)

**解释**：若该类的非抽象方法（实际可执行的函数）中包含和 $m$ 相同 descriptor 的 $m'$，则直接返回；否则到 $c$ 的父类中去寻找。

**示例**：  

![](https://cdn.hcplantern.cn/img/2022/10/27/20221027-225018.png-default)

### 类层级分析 (CHA)

类层级分析 Class Hierarchy Analysis

**目的**：根据每个 virtual call 的 receiver variable 的**声明类型**来求解所有可能调用的目标函数。如 

```java
A a = ... ;
a.foo();
```

这个 a 就是 receiver variable，声明类型就是 A。

假定 a 可以指向 A 以及 A 所有子类对象，CHA 的实现过程就是查询 A 的继承结构，从 A 和子类继承结构去找目标方法。

算法：$Resolve(cs)$ ：利用 CHA 算法找到调用点所有可能的调用方法。  

![](https://cdn.hcplantern.cn/img/2022/10/27/20221027-225023.png-default) 

算法示例：  

![](https://r2.hcplantern.top/2024/01/9/1704785434.png)

算法应用：  

![](https://cdn.hcplantern.cn/img/2022/10/27/20221027-225035.png-default) 

错误：因为**已经指定了是 B 类型**，所以 `b.foo()` 根本不会调用 C、D 的 `foo()`。CHA **只考虑声明类型**B，从而导致不准确

优点：CHA 优点是速度快，只考虑声明类型，忽略数据流和控制流
缺点：准确度低。

虽然精度低，但是在 IDEA 中却有相应应用，如：

![image-20221027223058967](https://cdn.hcplantern.cn/img/2022/10/27/20221027-223100.png-default)

总结：本类中有同名函数就在本类和子类找，没有就从父类找，接着找父类的子类中的同名函数（CHA 分析）。

### 利用 CHA 构造调用图

**算法**：遍历每个函数中的每个调用指令，调用 CHA 的 `Resolve()`找到对应的目标函数和调用边

![调用图构造算法](https://cdn.hcplantern.cn/img/2022/10/27/20221027-223405.png-default)

**示例**：  

![image-20221027223857743](https://cdn.hcplantern.cn/img/2022/10/27/20221027-223859.png-default) 

## 过程间控制流分析 (ICFG)

**定义**：过程间控制流图 ICFG = CFG + (Call edges + Return edges)。

*   Call edges：连接调用点和目标函数入口
*   Return edges：从一个 return 语句连回到紧跟着调用点下面的语句

**示例**： 

![image-20221027223950856](https://cdn.hcplantern.cn/img/2022/10/27/20221027-223952.png-default)

## 过程间数据流分析

相较于过程内的数据流分析，过程间的数据流分析的转换函数多了一个 edge transfer 的部分（包含 Call edge transfer 和 Return edge transfer)，这也跟前面说的 ICFG 相较于 CFG 多的那两种边相对应。

![image-20221027224117760](https://cdn.hcplantern.cn/img/2022/10/27/20221027-224118.png-default)

以常量传播数据流分析为例：

*   Node transfer：与过程内分析相同，对每个调用点，将等号左边部分去掉。
*   Call edge transfer：传参
*   Return edge transfer：传返回值

常量传播示例：  

![image-20221027224312434](https://cdn.hcplantern.cn/img/2022/10/27/20221027-224313.png-default)两条黑色虚线表示的边必须要有。从 `b = addOne(a)` 到 `c=b-3` ，a 通过此边传递，b 通过 `addOne()` 传递。若 a 也通过 `addOne()` 传递，会额外消耗太多资源。