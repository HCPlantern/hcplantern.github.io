---
title: Static Analysis 11 Pointer Analysis Context Sensitivity I
date: 2022-11-27
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}
-  [上下文敏感分析（上） - Static Program Analysis Book (pblo.gq)](https://spa-book.pblo.gq/ch3/context-sensitivity/03-04-context-sensitivity)
{{</admonition>}}

## 重点

- Concept of context sensitivity (**C.S.**)
- Concept of context-sensitive heap (**C.S. heap**)
- **Why** C.S. **and** C.S. heap improve **precision**
	- 单独使用C.S.或C.S. heap依然会损失精度
- Context-sensitive pointer analysis **rules**

## Introduction

### Example

```java
void main() {
Number n1, n2, x, y;
    n1 = new One(); // 𝑜1
    n2 = new Two(); // 𝑜2
    x = id(n1);
    y = id(n2);
    int i = x.get();
    //假设使用我们之前所讲述的上下文不敏感分析
    //这里i的常量分析结果是什么？
}
Number id(Number n) {
    return n;
}
interface Number {
    int get(); 
}
class One implements Number {
    public int get() { return 1; }
}
class Two implements Number {
    public int get() { return 2; }
}
```

使用上节课所讲的分析方法，由于上下文不敏感，会得到如下结果
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-205031.png-default)

也就因此造成了分析结果的低精度，即在动态运行时 $i$ 一定是 1，而分析的结果认为 $i$ 为 $NAC$ (Not a constant)：
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-205119.png-default)


如果我们使用**上下文敏感**的分析方式，区分不同时间对的 id 调用，则会得到这样的 PFG：
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-205126.png-default)

对应地也就能得到更为精确的分析结果：
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-205135.png-default)

## Theory

### Context Insensitive

什么原因导致了上下文不敏感分析的低精度：
- 1. 在动态执行时，对同一个函数的不同调用，往往有着不同的调用上下文（calling contexts），如上一小节的例子中两次对 id 的调用。
- 2. 不同的调用上下文会被混合并传播，进而形成假的数据流。如上一小节的例子中指针 x 和 y 指向两个目标。

### Context Sensitive

上下文敏感分析通过区分不同调用上下文的数据流，对调用上下文建模。举个例子，在这一段代码中，对id调用的上下文就是两行call-site，记为[1]和[2]：

```java
x = id(n1); //context 1 for id()
y = id(n2); //context 2 for id()
int i = x.get();
Number id(Number n) {
	return n;
}
```

进而，我们可以**通过对同一函数的不同调用添加标号进行区分**，而得到更精确的PFG：
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-211703.png-default)

### heap
**对于Java中被动态分配到heap上的对象，我们也需要对他们做相应的标记以提高分析精度。**

例子：
- 不做标记时，第八行new出来的对象无法区分，只能同一记作 $o_8$
- 做标记后，可以分别记为 $3:o_8$ 和 $4:o_8$

![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-215117.png-default)

但是，采取C.I.+ C.S. heap进行分析时，C.S. heap就不能提高精度了。

![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-215136.png-default)

## Rules

### Domains and Notations

适用于上下文敏感的规则：

methods, variable 和 objects 都变成了有上下文标识的。

![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-220440.png-default)

### Rules

首先我们考虑不包含调用（Call）语句的四类语句所对应的规则。

规则对比，左边为上下文敏感指针分析
![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-221007.png-default)

其次是函数调用的规则，增加了 `Select` 函数，其作用是为 object 添加上下文标识

![](https://cdn.hcplantern.cn/img/2022/11/27/20221127-221401.png-default)

`Select()` 可以为参数和返回值添加上下文标识，上下文的信息是在处理调用时添加的。

![](https://cdn.hcplantern.cn/img/2022/11/28/20221128-171042.png-default)
