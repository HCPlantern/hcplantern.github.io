---
title: Static Analysis 0304 Data Flow Analysis Applications
date: 2022-10-07T19:38:00
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}
- [【课程笔记】南大软件分析课程3——数据流分析应用（课时3/4） - 简书 (jianshu.com)](https://www.jianshu.com/p/45eb5e5565d5)
{{</admonition>}}

## 重点
- Understand the three data flow analyses: 
	- reaching definitions
	- live variables
	- available expressions 
- Can tell the differences and similarities of the three data flow analyses
- Understand the iterative algorithm and can tell why it is able to terminate

## 概述
什么是数据流分析：
> How application-specific **Data**
> - **Flows** through the
> - Nodes (BBs/statements) and
> - Edges (control flows) of
> - CFG (a program)? 

其中 **Data** 就对应第一节课中所讲的静态分析中 **Abstraction** 的部分，而关于 Data 是如何流动的则是 **Over-approximation** 部分。对于大部分使用 may analysis 的分析来说是 Over-approximation，如果是 must anaysis 则对应 Under-approximation，这两者目的都为实现 **Safe-approximation** 的近似。

{{<admonition question "什么是 may & must analysis ？">}}
- may analysis:
	- outputs information that may be true (over-approximation)
	- 对应 over-approximation
	- 可以有误报
- must analysis:
	- outputs information that must be true (under-approximation)
	-  对应 under-approximation
	- 可以有漏报
- Over- and under-approximations are both for safety of analysis

其中的部分又可有：

- application-specific data <- `abstraction` (+/-/0)
- Nodes <- `Transfer function`
- Edges <- `Control-flow handling`
{{</admonition>}}


不同的数据流分析有不同的数据**抽象表达**和不同的 **safe-approximation 策略**（may or must），比如不同的**转换规则**和**控制流**处理。

![](https://cdn.hcplantern.cn/img/2022/10/08/20221008-221540.png-default)

## 预备知识
### Input and Output States
程序的执行改变输入和输出状态，其状态与程序点（program point）相关联。
![](https://cdn.hcplantern.cn/img/2022/10/08/20221008-224932.png-default)

在每个数据流分析应用中，我们将每个程序点与一个数据流值关联起来，该值代表了从该点可以观察到的所有可能的程序状态的集合的抽象。

以第一节课中所讲的例子来说：
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-094453.png-default)


### 数据流分析的结果
**数据流分析** 是为所有语句 s 的 $IN[s]'s$ 和 $OUT[s]'s$ 找到 **一套 safe-approximation 为导向的约束的解决方案**。
- 基于语句语义的约束(转移函数) 
- 基于控制流的约束

### 控制流约束
分为正向和反向的控制流分析

![](https://cdn.hcplantern.cn/img/2022/10/08/20221008-225612.png-default)

## 到达定值分析
### 定义
到达定值（Reaching Definitions）：
- 定义：给变量v一个定义d（赋值），存在一条路径使得程序点 p 能够到达 q ，且在这个过程中不能改变 v 的赋值。
	- 是 may analysis: 只要有可能 undefined 就报出来，因此可能会误报。
- 应用：检测未定义的变量，若 v 可达 p 且 v 没有被定义，则为未定义的变量。
	- 比如，在 CFG 入口处为所有的 v 引入一个 dummy def. 如果这个 dummy def 到达了某一点, 则说明中间 v 未被定义
- 抽象表示：设程序有 n 条定义，用 n 位向量来表示该定义能否 reach 。
- 方向：Forward
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-094948.png-default)
### 公式
- $D: v = x\ op\ y$
生成了定义 D，同时杀死了之前对 v 的定义。

- Transfer Function
$OUT[B] = gen_B \cup (IN[B] - kill_B)$

- Control Flow
$IN[B] = \cup_{p\ a\ predecessor\ of\ B}\ OUT[P]$

### 算法
输入为 CFG，输出每个基本块的 IN 和 OUT
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-105502.png-default)
为什么算法会停下：$gen_B$ 和 $kill_B$ 是不会变的， $IN[B]$ 中新增加的元素可能会被杀死，也可能不会被杀死，从而进入 $OUT[B]$，且进入的元素永远也不会被杀死了。因此 $OUT[B]$ 不会缩小，最终算法会停止，到达一个不动点。

### 例子
黑色-初始化；红色-第1次遍历；蓝色-第2次遍历；绿色-第3次遍历。

 ![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-111402.png-default)



## 活跃变量分析
### 定义
活跃变量（Live Variable）：
- 定义：活跃变量分析指出变量 v 在程序点 p 的值是否可以沿着 CFG 中从 p 开始的某个路径使用。如果是，v 在 p 处 live ；否则，v 在 p 处 dead 。
	- 也是 may analysis，只要后继节点中存在 live，那么该节点就算 live。
- 应用：目标代码生成时，需要用到活跃变量分析来分配寄存器。我们需要知道寄存器中所存放的变量哪一个使用时间最远，从而替换掉该变量。
- 抽象表示：程序中有 n 个变量，用 n 位向量来表示变量是否 live。
- 方向：Backward

### 公式
- Transfer Function
$IN[B] = use_B \cup (OUT[B] - def_B)$
- Control Flow: 
$OUT[B] = \cup_{S\ a\ successor\ of\ B} IN[S]$
- 对不同表达式的理解：
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-113912.png-default)
在一个基本块中，如果在定义之前使用，那么就算在 $use_B$ 中，否则不算。
具体来说，假设基本块 B 中包含语句 $s_1, s_2, \dots, s_n$ ，那么
- $use_B = use_1 \cup (use_2 - def_1) \cup (use_3 - def_1 - def_2)\cup \dots \cup (use_n - def_1 - def_2 \dots - def_{n-1})$
- $def_B = def_1 \cup def_2 \cup def_3 \cup \dots \cup def_n$

### 算法
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-115103.png-default)

一个初始化的规律：一般情况下，may analysis 全部初始化为空，must analysis 全部初始化为 all。

### 例子
黑色-初始化；红色-第1次；蓝色-第2次；绿色-第3次。
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-115516.png-default)


## 可用表达式分析
### 定义

可用表达式（Available Expression）:
- 定义：一个表达式 $x\ op\ y$ 在程序点p是可用的，如果 (1) 从入口到p的所有路径都必须对 $x\ op\ y$ 求过值，并且 (2) 在  $x\ op\ y$ 的最后一次求值之后，没有对 x 或 y 进行重新定义。
	- 是 must analysis，因为允许有漏报，不能有误报。
- 应用：检测全局公共子表达式，优化运行效率
- 抽象表示：程序中的 n 个表达式，用 n 位向量表示是否可用。
- 方向：Forward
### 公式
- Transfer Function
$OUT[B] = gen_B \cup (IN[B] - kill_B)$
- Control Flow
$IN[B] = \cap_{P\ a\ predecessor\ of\ B} OUT[P]$
（从入口到 p 的所有路径都必须经过对 $x\ op\ y$ 的求值）
- 理解
为什么是 must analysis？比如下图中虽然 $e^{16} \times x$ 在经过 $x=3$ 之后被识别为不
可用，但事实上是可用的，这里的漏报是可接受的，不影响正确性。
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-121202.png-default)


### 算法
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-121616.png-default)

和到达定值比较相似，不同在于是 must analysis，所以初始化为 all，且 Control Flow 用 $\cap$ 交

### 例子
黑色-初始化；红色-第1次；蓝色-第2次；

![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-122141.png-default)

## 总结
![](https://cdn.hcplantern.cn/img/2022/10/09/20221009-122354.png-default)

