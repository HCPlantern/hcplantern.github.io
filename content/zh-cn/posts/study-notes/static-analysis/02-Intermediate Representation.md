---
title: Static Analysis 02 IR
date: 2022-09-22T13:46:00+08:00
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->
{{<admonition quote "本文参考">}}
- [『南京大学-软件分析』课程笔记 - Theoyu](https://theoyu.top/2022/03/10/StaticProgramAnalysis.html)
- [【课程笔记】南大软件分析课程2——IR - 简书 (jianshu.com)](https://www.jianshu.com/p/acb73f72cf46)
{{</admonition>}}

## 重点

{{<admonition summary 重点>}}
- The relation between compilers and static analyzers 
- Understand 3AC and its common forms 
- How to build basic blocks on top of IR 
- How to construct control flow graphs on top of BBs?
{{</admonition>}}

## Compiler and Static Analysis

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-135848.png)

编译器将源代码 Source code   转换为机器代码 Machine Code  。其中的流程框架是：

1.  词法分析器 Scanner 结合正则表达式,  通过词法分析 Lexical Analysis 将 source code 翻译为 token. 
2.  语法分析器 Parser ,  结合上下文无关文法 Context-Free Grammar ,  通过语法分析 Syntax Analysis ,  将 token 解析为抽象语法树 Abstract Syntax Tree,  AST  
3.  语义分析器 Type Checker  , 结合属性文法 Attribute Grammar  , 通过语义分析 Semantic Analysis  , 将 AST 解析为 decorated AST 
4.  Translator, 将 decorated AST 翻译为生成三地址码这样的中间表示形式 Intermediate Representation,  IR  , 并**基于 IR 做静态分析** 例如代码优化这样的工作  , IR之前的部分我们称为前端, IR之后的部分称为后端。
5.  Code Generator, 将 IR 转换为机器代码。

几个问题：

1.  语法分析为什么是**Context-Free Grammar**？
    
    > 现在的编程语言完全足够, Context-Sensitive Grammar 更适合分析人讲的语言
    
2.  为什么不直接拿Source code做静态分析, 而是要经过到IR的这一系列步骤? 
    
    > 分析代码漏洞, 这是 non-trivial 的事情, 做 non-trivial 之前应该把 trivial 的部分交给各种分析器去做
    

## AST vs. IR
**为什么 IR 更适用于静态分析?**

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-140734.png)
## 3AC ( 三地址码 )

三地址码为什么叫做三地址码呢？因为每条 3AC 至多有三个地址。而一个「地址」可以是：

-   名称 Name: a, b
-   常量 Constant: 3
-   编译器生成的临时变量 Compiler-generated Temporary: t1, t2

常见的 3AC 包括：

-   $z=x bop y$：双目运算符并赋值，bop = binary operator
-   $x=uop y$ ：单目运算符并，unary operator
-   $x=y$：直接赋值
-   $goto L$：无条件跳转，L = label
-   $if x goto L$：条件跳转
-   $if x rop y goto L$：包含运算关系的跳转，rop = relational operator

## Soot: Real Static Analyzer

### Do-While Loop
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-143752.png)


### Method Call

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-143639.png)


{{<admonition info "Java IR (Jimple) basic knowledge:">}}
- invokespecal : call constructor, call superclass methods, call private methods
- invokevirtual: instance methods call (virtual dispatch)
- invokeinterface: cannot optimization, checking interface implementation
- invokestatic: call static methods
- Java 7: invokedynamic -> Java static typing, dynamic language runs on JVM

method signature: class name: return type, method name(para1 type, para2 type)

{{</admonition>}}

## SSA (Static Single Assignment)
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-145543.png)
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-145549.png)

**Why SSA?**
- flow-insensitive analysis 精度更好
- 容易做优化算法

**Why not SSA?**
- too many variables and phi-functions
- inefficiency problem due to copy operations
## Control Flow Analysis
### CFG

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-150055.png)

### Basic Block (BB)
**定义: 只有唯一一个入口和唯一一个出口的*最长* 3AC 连续序列**
- It can be entered only at the beginning
- It can be exited only at the end, i.e. the last instruction in the block

识别 BB 入口 (leader) 的方法: 
1. 第一条指令是 leader
2. goto 目标是 leader
3. goto 的下一条指令是 leader

由此可以划分出所有的 Basic Block

### Build CFG on top of BBs
1. Nodes of CFG are BBs
2. Block A -> Block B has an edge if and only if
	1. There is a **conditional or unconditional jump** from the end of A to the beginning of B
	2. B immediately follows A in the original order of instructions **and A doesn't end in a jump**
3. Add two nodes: **Entry** and **Exit**
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-151807.png)

**Example:**
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-152008.png)

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-151910.png)

