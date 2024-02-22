---
title: Static Analysis 08 Pointer Analysis
date: 2022-11-03T15:26:49
tags: 
  - static-analysis
categories:
  - study-notes
---
 
<!--more-->
{{<admonition quote "本文参考">}}
- [指针分析简介 - Static Program Analysis Book (pblo.gq)](https://spa-book.pblo.gq/ch3/pointer-analysis/03-01-pointer-analysis-spa)
- [【课程笔记】南大软件分析课程6——指针分析介绍（课时8） - 简书 (jianshu.com)](https://www.jianshu.com/p/9d15edf2604e)
{{</admonition>}}

## 重点

- What is pointer analysis? 
- Understand the key factors of pointer analysis
- Understand what we analyze in pointer analysis

## Motivation

回顾上节课的 CHA，举个例子：

```java
Number n = new One();
int x = n.get();

interface Number {
	int get();
}
class Zero implements Number{
	public int get() { return 0; }
}
class One implements Number{
	public int get() { return 1; }
}
class Two implements Number{
	public int get() { return 2; }
}
```

由于我们在 CHA 中只会分析到 `Number n = ...`，因此我们会认为 x 的 call target 不止一个，因此 x 是 NAC。但这是不准确的，我们可以通过分析 n 所指向的对象来获得确实的常量。

## 指针分析简介

**目标**：分析程序指针可以指向哪些对象

**说明**：指针分析属于 may analysis，分析的结果是某指针所有可能指向哪些对象，是个 over-approximation 集合。

**举例**：

对于`setB()`函数，this 指向`new A()`，因为是调用者是`a.setB()`；`setB()`中的 b 是 x 传过来的，所以 b 指向`new B()`，`A.b`指向 `new B()`

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-163349.png-default)

### 指针分析与别名分析的区别

两者相近但解决的问题不同

- 指针分析：一个指针可以指向哪些对象？
- 别名分析：两个指针可以指向同一个对象吗？

如下例，p 和 q 是别名

```java
// p and q are aliases
p = new C();
q = p;
// x and y are not aliases
x = new X();
y = new Y();
```

**别名分析可以通过指针分析推导得到**

### 重要性

> 业界大佬都说很重要

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-164056.png-default)

## 指针分析中的关键要素

- 指针分析是一个复杂系统
- 许多因素都会影响到其精确度和效率

*红色标出了本课程的选择，也是目前的主流做法*

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-223310.png-default)


### Heap abstraction

在动态执行中，由于循环和递归的结构，堆上的对象数量可以是无限的。如果不做抽象，面对无限的对象，分析算法可能根本停不下来。所以为保证指针分析可以终止，我们采用**堆抽象技术**，将无穷的具体对象**抽象成有限的抽象对象**。也即，将有共性的对象抽象成1个静态对象，从而限制静态分析对象的个数。

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-164835.png-default)

相关的技术有很多，本课程只讲一个最常用的 **Allocation-Site Abstraction**。基本思想是使用创建点来抽象在该处创建的所有对象。

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-165035.png-default)

### Context Sensitivity

**调用上下文（calling contexts）** 记录的是函数调用前后相关变量的值

-   Context-sensitive：根据某函数调用上下文的不同，多次分析同一函数。
-   Context-insensitive：每个函数只分析一次。

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-165600.png-default)

### Flow Sensitivity

流敏感分析重视语句的执行顺序。虽然精度更高，但是开销也更大，且优势不明显，故一般使用流不敏感分析。

例如，对于下面一段代码，将会有如下分析结果（左敏感，右不敏感）

```java
c = new C();
c.f = "x";
s = c.f;
c.f = "y";
```

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-185508.png-default)

之前课程中的所有数据流分析技术都是流敏感的。

### Analysis Scope

需要考虑分析程序的哪一部分

-   Whole-program 全程序：分析全程序的指向关系。
-   Demand-driven 需求驱动：只分析影响特定域的指针的指向关系。（按需分析）

该课程选择全程序分析，更全面且更加容易掌握。

## Concerned Statements

在指针分析中，我们只关注会影响到指针的语句（pointer-affecting statements）。而对于 if/switch/loop/break/continue 等等语句则可以直接忽略。

### Java 中的指针

- Local variable: `x`
- Static field: `C.f`
- Instance field: `x.f`
- Array element: `array[i]`
	- 涉及数组的分析中，我们**忽略下标**，代之以一个域（a single field）。例如，在下图中我们用 arr 表示。
	- 数组下标为变量时难以分析
![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-224343.png-default)

### 关注的语句类型

```c
// New
x = new T()
// Assign
x = y
// Store
x.f = y
// Load
y = x.f
// Call
r = x.k(a, …)
```

Java 中有三类方法调用，其中 **Static call** 和 **Special call** 较为简单，本课程关注 **Virtual call** 

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-225115.png-default)

