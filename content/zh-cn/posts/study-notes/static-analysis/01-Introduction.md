---
title: Static Analysis 01 Introduction
date: 2022-09-22T13:40:00+08:00
tags: 
  - static-analysis
categories:
  - study-notes
---

<!--more-->

{{<admonition quote "本文参考">}}
- [『南京大学-软件分析』课程笔记 - Theoyu](https://theoyu.top/2022/03/10/StaticProgramAnalysis.html)
{{</admonition>}}

## 重点
- What are the differences between static analysis and (dynamic) testing? 
- Understand soundness, completeness, false negatives, and false positives. 
- Why soundness is usually required by static analysis? 
- How to understand abstraction and over-approximation?

## 为什么我们需要静态分析

![image-20220310205025687](https://cdn.hcplantern.cn/img/2022/09/25/20220925-202824.png-default)

-   程序可靠性
    
    空指针引用、内存泄漏…
    
-   程序安全性
    
    信息泄漏、注入攻击…
    
-   编译优化
    
    死代码消除、代码移动(比如在循环里定义不会改变的值，可以把初始化放在循环外面)
    
-   理解程序
    
    idea自动提示、分析结构…
    

## 什么是静态分析
- Static analysis analyzes a program P to reason about its behaviors and determines whether it satisfies some properties **before running P.**

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-154757.png)

### Rice's Theorm
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-154832.png)
- non-trivial properties 简单理解为: 和运行时行为有关的特性
- Rice's Theorm 告诉我们程序分析不存在 **exact answer**.
- 但是可以有两种选择: Compromise soundness or Compeomise completeness

## Sound & Complete
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-155338.png)

-   妥协 soundness -> 漏报 **False Negatives** 
-   妥协 completeness -> 误报 **False Positives** 

> 注意这里「妥协」是指无法满足而倾向于另一种

### 为什么通常选择 Soundness
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-155544.png)

- 准则：宁可误报不能漏报，妥协 completeness，保证soundness. 
- 确保 soundness 的基础上，在**精度**和**速度**上作出权衡。


## Abstraction
{{<admonition example Example>}}
Example: Determine the sign (+, -, or 0) of all the variables of a given program.
{{</admonition>}}

![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-153203.png)

## Over-approximation
### Transfer Function

- In static analysis, transfer functions define how to evaluate different program statements on abstract values.
- Transfer functions are defined according to "analysis problem" and the "semantics"of different program statements.
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-153253.png)
### Control Flows
![](https://cdn.hcplantern.cn/img/2022/09/22/20220922-153556.png)
