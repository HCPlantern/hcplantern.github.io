---
title: Static Analysis 12 Pointer Analysis Context Sensitivity II
date: 2022-11-28
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}
- [上下文敏感分析（下） - Static Program Analysis Book (pblo.gq)](https://spa-book.pblo.gq/ch3/context-sensitivity/03-05-cs2)
- [【课程笔记】南大软件分析课程8——指针分析-上下文敏感（课时11/12） - 简书 (jianshu.com)](https://www.jianshu.com/p/5ab79839f686)
{{</admonition>}}

## 重点
*   上下文敏感指针分析的完整算法（一般其他教程中很少涉及到）。
*   上下文敏感概念，堆对象的上下文敏感表示，上下文敏感指针分析的规则。
*   上下文的三种选择，以及效率、准确度的对比。

## Context Sensitive Pointer Analysis：Algorithms

**区别**：和过程间指针分析相比，仍然分为两个过程，分别是构造 PFG 和根据 PFG 传递指向信息。主要区别是添加了上下文。

**PFG 构造**：边添加规则和之前一样，Assign、Store、Load、Call，Call 需要加参数传递、返回值传递的边。  

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182657.png-default)

**符号**：
*   S：可达语句的集合（就是 RM 中的语句）
*   Sm：函数 m 中的语句
*   RM：可达函数的集合
*   CG：调用图的边

**算法**：被调用函数的上下文暂时用 ct 表示，之后会解释 `Select()` 函数。

1.  先处理 New、Assign 指令。`AddReachable(c:m)` 只多了上下文。
2.  遍历 WL，`Propagate()` 和原来相同。
3.  处理 Store、Load 指令，`AddEdge()` 只多了上下文。
4.  处理 Call 指令，`ProcessCall()`，多了一行 **`ct=Select (c, l, c': oi, m)`**，在找到调用目标函数之后，需选择被调用的函数的上下文。

## Context Sensitivity Variants —上下文的选取

**上下文的选取主要采用 3 类**：

*   Call-Site Sensitivity
*   Object Sensitivity
*   Type Sensitivity
*   ...

**说明**：Select (c, l, c': oi, m)，c—调用者上下文，l—调用者，c':oi—接收对象（含堆的上下文信息）。

### （1）Call-Site Sensitivity

**原理**：又称为 k-call-site sensitivity / k-CFA，不断添加调用行号。1991 年 [Olin Shivers](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.ccs.neu.edu%2Fhome%2Fshivers%2Fpapers%2Fdiss.pdf) 提出。

> Select (**c, l**, c': oi, m) = (l',..., l'', l)

**问题**：如果函数调用自身，导致无限递归，如何限制上下文长度？

**解决**：k-limiting Context Abstraction。只取最后 k 个上下文，通常取 k<=3。例如，函数的上下文通常取 2，堆上下文通常取 1。

**示例**：采用 1-Call-Site。

```java
interface Number { int get(); }
class One implements Number { public int get() { return 1; }}
class Two implements Number { public int get() { return 2; }}
1   class C {
2       static void main() {
3           C c = new C();
4           c.m();
5       }
7       Number id(Number n) {
8           return n;
9       }
10      void m() {
11          Number n1,n2,x,y;
12          n1 = new One();
13          n2 = new Two();
14          x = this.id(n1);
15          y = this.id(n2);
16          x.get();
17      }
18  }
```

**上下文不敏感 vs 上下文敏感（1-Call-Site）**：  

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182705.png-default)

### （2）Object Sensitivity

**原理**：针对面向对象语言，用 `receiver object` 来表示上下文。对比 1 层的调用点敏感和对象敏感，时间和准确性上对象敏感显然更优，这是由面向对象语言的特点所确定的。

> Select (c, l,**c':oi**, m) = [oj, ... , ok, oi] （c' = [oj, ... , ok]）

**示例**：选取 1-object，最终 pt (x)=o3。  

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182712.png-default) 

**对比**：对比 `1-Call-Site` 和 `1-object` 上下文，在这个示例中 `1-object` 明显更准确。原因是面向对象语言的特性，多态性产生很多继承链，一层一层调用子对象，其中最关键的是 `receiver object`，`receiver object` 决定了调用者的根源。本例有多层调用，若采用 2-Call-Site 就不会出错。  

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182714.png-default) 

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182717.png-default) 

**示例 2**：在本示例中，`1-Call-Site` 明显更准确。因为同一个 `receiver object` 用不同参数多次调用了子函数，导致局部变量无法区分。  

![](https://cdn.hcplantern.cn/img/2023/02/18/20230218-182719.png-default) 

**结论**：所以理论上，对象敏感与 callsite 敏感的准确度无法比较。但是对于面向对象语言，对象敏感的准确度要优于 callsite 敏感。

### （3）Type Sensitivity

**原理**：牺牲精度，提高速度。基于创建点所在的类型，是基于对象敏感粗粒度的抽象，精度较低。

> Select (c, l,**c':oi**, m) = [𝑡′,...,𝑡′′, InType (𝑜𝑖)] 其中𝑐′ = [𝑡′, ... , 𝑡′′]  
> 
> ![](https://r2.hcplantern.top/2024/01/9/1704785819.png)

### （4）总体对比

**精度**：object > type > call-site

**效率**：type > object > call-site

本课老师提出选择上下文的方法，对代码的特点有针对性的选择上下文方法，见 [A Principled Approach to Selective Context Sensitivity for Pointer Analysis](https://links.jianshu.com/go?to=https%3A%2F%2Fcs.au.dk%2F%7Eamoeller%2Fpapers%2Fzipper%2F)。厉害了！

## 课后问题

**问题 1**：流敏感和上下文敏感对变量 / 堆抽象的表示有什么区别？

*   上下文敏感：某个变量在不同上下文的指向，以调用点位置作为区分度。
*   流敏感：比如说程序运行到第 4 行，在这个位置变量的指向是什么，第 20 行又指向哪些，以控制流的位置来作为区分度。

对于面向对象语言，Steensgaard 算法不实用。

**问题 2**：如果循环中同一行多次调用同一函数，会区分每次调用吗？循环展开还是不展开？

本课程分析的是流不敏感，所以不会展开循环。Java 分析不需要流敏感，开销太大了，效果不明显。
