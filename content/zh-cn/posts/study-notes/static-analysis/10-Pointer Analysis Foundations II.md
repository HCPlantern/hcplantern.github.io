---
title: Static Analysis 10 Pointer Analysis Foundations II
date: 2022-11-20T15:11:15
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->

{{<admonition quote "本文参考">}}
- [指针分析理论（下） - Static Program Analysis Book (pblo.gq)](https://spa-book.pblo.gq/ch3/pointer-analysis/03-03-pointer3-analysis-spa)
{{</admonition>}}

## 重点
- Understand pointer analysis rule for method call 
- Understand inter-procedural pointer analysis algorithm 
- Understand on-the-fly call graph construction

## Pointer Analysis with Method Calls

首先回顾一下在上一篇文章中列出的大纲。

- 1.Pointer Analysis: Rules
- 2.How to Implement Pointer Analysis
- 3.Pointer Analysis: Algorithms
- 4.Pointer Analysis with Method Calls

承接上一节课程，本文谈谈包含指针分析**如何处理函数调用**。接下来用指针分析的方式来构建 Call graph，先来对比一下 CHA 和指针分析两种方法：

-   CHA: imprecise, introduce spurious call graph edges and points-to relations
-   Pointer analysis: more precise than CHA, both for call graph and points-to relations (a.k.a on-the-fly call graph construction)

本课将给出一个包含函数间分析的适用于全程序的算法。
考虑下面这样一小段代码，显然，我们必须要有过程间的分析，才能有更准确的分析结果。

```java
void foo(A a) {
	…
	// 𝑝𝑡(𝑎) = ?
	b = a.bar();
	// 𝑝𝑡(𝑏) = ?
	…
}
```

### Rule for Call

过程调用发生了什么：
> 一个参考答案：保存现场，构造调用栈帧，传递参数，跳转到目标函数开始执行……目标函数执行完毕跳转回来，后从预定的位置取返回值（若需要），恢复现场，继续往下执行……

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-152249.png-default)

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-152253.png-default)

在静态分析中，我们更多地关心数据流，而非控制流。而针对Java，处理函数调用的数据流可以分为以下四个部分：

- 确定目标方法。用第7课介绍过的Dispatch函数完成。
- 传receiver object，也就是传递 this 变量
	- ![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153312.png-default)
- 传参数
	- ![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153321.png-default)
- 传返回值
	- ![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153329.png-default)

完整的规则：

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153632.png-default)

### Detail 1

- 为什么不增加一条边 $x \to m_{this}$ ？
- 如果连上，可能会导致 this 指针指向多个类型，影响准确度。

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153947.png-default)

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-153953.png-default)

### Detail 2

像之前用 CHA 做过程间分析时一样，我们需要将分析的过程和 Call graph 构建的过程结合起来。

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-154640.png-default)


不同的是，这次我们只分析从 main 方法（或者一般性地说，程序入口）开始可达的部分。原因有二：
- 提升分析速度。因为我们能够避免分析不会被执行到的死代码。
- 提升分析精度。避免了 unreachable 部分的代码调用 reachable 部分方法时可能引起的精度下降。

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-154633.png-default)

## Algorithm: PA with Method Calls

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-154916.png-default)

算法整体上来说和上一节课所介绍的大框架相似，黄色标记的部分是这次新加入的部分。绿色部分是对新的全局变量的说明：
- S 里的 statements 就是 Reachable Methods (RM) 里 methods 的 statements（语句）
- Call Graph 和指针集作为最后的输出。

### Add Reachable 函数

-   **输入参数** m 是最新的可达方法。
-   函数修改维护全局的 $RM$、$S$ 和 $S_m$ ，并处理新的方法 m 中的 New 和 Assign 语句。

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-155424.png-default)

- 怎么不处理 Store 和 Load 语句？
	- 比如对于 `x.f = y` ，处理这个语句需要 x 的指针指向信息，而一开始执行算法的时候并没有。 
	- New 和 Assign 语句不需要额外的信息，可以一开始就处理。
	- Load 和 Store 语句将会在算法后面的 while 循环中处理。

### Process Call 函数

![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-163652.png-default)

ProcessCall的作用是：
- 输入的 $o_i$ 是 x 新指向的目标。
- 函数在可达的语句集合 S 中，选择所有与 x 有关的过程调用，做之前提到的数据流相关四步处理（确定被调用方法、传对象、传参数，传返回值）。

- 为什么要检查 $l \to m$ 是否在CG中，即为什么同样的 $l \to m$ 可能不止一次地被处理？
	- *l 代表 call site。可以用行号作为 call site 的 label *
	- 答案：$o_j, o_k$ 同样可能通过 Dispatch 返回同一个 m 

## Example

```java
class A {
    static void main() {
        A a = new A();
        A b = new B();
        A c = b.foo(a);
    }
    A foo(A x) { … }  
}
class B extends A {
    A foo(A y) {
        A r = new A();
        return r;
    }
}
```

结果：
![](https://cdn.hcplantern.cn/img/2022/11/20/20221120-171601.png-default)
