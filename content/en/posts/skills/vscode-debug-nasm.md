---
title: 如何使用 VSCode + SSH 调试 NASM 程序
date: 2022-09-28
tags: 
  - vscode
  - debug
categories:
  - skills
---

使用 VSCode 调试 NASM 程序
<!--more-->

## 前言
最近在做软院 OS 的~~祖传~~实验，课程要求编写 nasm 程序。我的开发环境是远程 Linux 主机，系统为 Ubuntu 20.04 64bit，并在 Windows 上使用 VSCode SSH 连接主机。然而，当我试图使用 VSCode 调试的时候，发现网络上几乎没有相关的教程或资料~~毕竟是祖传实验~~。无奈本人也不擅长 gdb，用 gdb 调试真的折磨……所以在此分享一个可行方案。

## 准备工作
- Linux 主机：
	- 配置好 nasm gcc gdb 环境，确保能够使用命令行编译运行和 gdb 调试。
	- 必须为 64 bit 系统，否则没法使用 VS Code SSH 连接。
	- WSL 理论上也可行，我没试过
- 本地 VS Code：安装 **C/C++** 插件，其他插件不影响
	- 如果你知道更好的 gdb 调试插件，欢迎评论告诉我

## 配置文件

工作目录如下：

```text
hello
├── build.sh
├── hello.asm
└── .vscode
    ├── launch.json
    └── tasks.json
```

{{<admonition warning "注意">}}
开始之前，把 hello 文件夹添加到工作区，或者选择直接将该文件夹在 vscode 中打开为根目录
{{</admonition>}}

- 主程序 `hello.asm`
```nasm
; ----------------------------------------------------------------------------------------
; 仅使用系统调用来输出 "Hello, World" 到控制台。 这个程序仅在 64 位的 Linux 下运行。
; ----------------------------------------------------------------------------------------

        global  main

        section .text
main:
        ; write(1, message, 13)
        mov     rax, 1                  ; 1 号系统调用是写操作 
        mov     rdi, 1                  ; 1 号文件系统调用是标准输出 
        mov     rsi, message            ; 输出字符串的地址 
        mov     rdx, 13                 ; 字符串的长度 
syscall                         ; 调用系统执行写操作 

        ; exit(0)
mov     rax, 60                 ; 60 号系统调用是退出 
        xor     rdi, rdi                ; 0 号系统调用作为退出 
        syscall                         ; 调用系统执行退出 
message:
        db      "Hello, World", 10      ; 注意到最后的换行 
```

- 编译脚本 `build.sh`
```bash
#/bin/bash
nasm -gdwarf -felf64 hello.asm 
gcc -m64 hello.o -o target
```
开启 `-gdwarf` 选项附加 gdb 调试信息，运行脚本之前执行 `chmod +x build.sh` 增加运行权限。

如果是 32 位 nasm 程序，那么脚本写成：
```bash
#/bin/bash
nasm -gdwarf -felf32 hello.asm 
gcc -m32 hello.o -o target
```


- `tasks.json` 
```json
{
    "tasks": [
        {
            "label": "nasm build",
            "type": "shell",
            "command": "./build.sh"
        }
    ],
    "version": "2.0.0"
}
```

 - `launch.json`
```json
{
  "version": "0.2.0",
  "configurations": [
    {
            "name": "nasm debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/target", // specify your generated program
            "args": [], // arguments
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [{ "name": "config", "value": "Debug" }],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "nasm build",
            "miDebuggerPath": "/usr/bin/gdb"
        }
  ]
}
```

## 开始调试
配置完毕后，F5 启动调试

![](https://cdn.hcplantern.cn/img/2022/09/28/20220928-112345.png-default)

成功！开始快乐 debug 吧！

PS. 使用该插件调试 nasm 程序时功能比较受限，若要查看内存或进行其他操作，还是需要使用 gdb 命令。查看内存可以考虑这个命令：[GDB x command](https://visualgdb.com/gdbreference/commands/x)

PPS. 调试控制台执行 gdb 命令需要加上前缀 `-exec` 