---
title: Static Analysis 09 Pointer Analysis Foundations I
date: 2022-11-03T22:56:50
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}
- [指针分析理论（上） - Static Program Analysis Book (pblo.gq)](https://spa-book.pblo.gq/ch3/pointer-analysis/03-02-pointer2-analysis-spa)
- [【课程笔记】南大软件分析课程7——指针分析基础（课时9/10） - 简书 (jianshu.com)](https://www.jianshu.com/p/5cbc5bb5c4da)
{{</admonition>}}

## 重点

-  Understand **Rules** for pointer analysis
-   Understand **PFG** (Pointer flow graph)
-   Understand **Algorithm** for pointer analysis

## Notations

首先关注前四种语句，`Call` 语句下节课讲

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-225819.png-default)

介绍一些数学表示

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-225858.png-default)

## Pointer Analysis: Rules

主要解释 Rule 一列中的内容。**横线上的内容是前提 (Premises) ，横线下的内容是结论 (Conclusion) 。**

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-230420.png-default)

New 语句：将新建的对象加入 $pt(x)$
Assign 语句：将 $x$ 指向 $y$ 指向的对象。
Store 和 Load: 同上 

![](https://cdn.hcplantern.cn/img/2022/11/03/20221103-231403.png-default)

## How to Implement Pointer Analysis

指针分析是在指针间**传递**指向关系。最开始的指向关系都来自于 `new` 语句。

指针分析的关键：当 $pt(x)$ 变化时，也要更新 $x$ 相关的其他指针

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-210153.png-default)

### Pointer Flow Graph

> Pointer Flow Graph (PFG) of a program is a directed graph that expresses how objects flow among the pointers in the program.

指针流图的两大要素是 Node 和 Edge

-   `Node: Pointer = V ⋃ (O × F)`
    -   A node n represents **a variable** or **a field of an abstract object**
-   `Edges: Pointer × Pointer`
    -   **An edge** $x \to y$ means that the objects pointed by pointer $x$ **may flow to (and also be pointed to by)** pointer $y$

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-210608.png-default)

### Example

假设 c 和 d 一开始都指向 $o_i$，根据上述规则，我们能够从左侧的程序语句从上到下构建出右侧的指针流图。

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-210720.png-default)

所有 b 所指向的对象更新时，都要传递到 e 中。这是一个求传递闭包 (transitive closure) 的过程。

假如我们考虑 j 位置的一条新语句`b = new T();`

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-211147.png-default)

PFG 的构建和传递指针关系这两个步骤是相互依赖的。指针分析过程中需要不断在这两个步骤之间循环往复。

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-211330.png-default)

## Pointer Analysis: Algorithms

### Introduction

- 由于做流不敏感分析。输入为Set，丢失了语句的顺序关系也没关系。
- WorkList：保存接下来要处理的指向信息，与BFS中的队列作用类似。
- Each worklist entry $\left \langle n, pts \right \rangle$ is a pair of pointer $n$ and points-to set $pts$, which means that $pts$ should be propagated to $pt(n)$
	- $E.g.,  [\langle x, \{o_i\} \rangle, \langle y, \{o_j, o_k\} \rangle , \langle o_j.f, \{o_l\} \rangle \dots]$

四个红框部分对应之前提到的四种基本语句—— New, Assign, Store 和 Load

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-211604.png-default)

### Handling of New and Assign

#### Init and adding edges

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-212800.png-default)

首先考虑两种简单的语句：New和Assign。

- 前三行代码做初始化的工作，并针对所有的 **New** 语句，将所有的初始指向关系加入WorkList。注意 $pt(n)$ 初始化后为空集$\{\}$，随着算法的迭代会增加元素。
- 之后的两行代码处理 **Assign** 语句，添加`y->x`的边到PFG中。添加边的具体算法如下

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-213107.png-default)

#### Propagate

在 `Propage()` 之前要先去重

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-213336.png-default)

解释 `Propagate()` 具体做的事情

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-213357.png-default)

### Detial-Differential Propagation

为什么要去重？
- 去重与否不影响正确性
- 主要是为了避免冗余的操作

在真实的指针分析中，对象的数量非常巨大（上亿），我们通过Differential Propagation来消除冗余。

```java
Solve(𝑆)
    ...
    while WL is not empty do
        remove 𝑛, 𝑝𝑡𝑠 from WL
        Δ = pts – pt(n) // Differential Propagation
        Propagate(n, Δ) // Differential Propagation
```

Example: 

首先是a->c->d的传递路线

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-221218.png-default)

如果不考虑 Differential Propagation，在 b->c->d 的传递路线中，$\{o_1, o_3\}$
之前已经在c所指向的集合中了，但依然需要参与传播，这是冗余的。

如果去重，只需要传播  $\{O_5\}$ 这一项即可。

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-221640.png-default)

### Handling Store and Load

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-221841.png-default)

### Example

以这个程序为例讲解算法。

```java
1 b = new C(); 
2 a = b;
3 c = new C(); 
4 c.f = a;
5 d = c;
6 c.f = d; 
7 e = d.f;
```

结果如下图，过程看视频

![](https://cdn.hcplantern.cn/img/2022/11/11/20221111-225537.png-default)
