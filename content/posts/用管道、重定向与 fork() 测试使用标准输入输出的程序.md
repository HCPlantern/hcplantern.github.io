---
title: 用管道、重定向与 fork() 测试使用标准输入输出的程序
date: 2022-10-04
tags: 
  - c
categories:
  - study-notes
---

如何测试在 OS Lab1 中实现的汇编程序

<!--more-->

## 背景

在 OS Lab1 中，我们实现了一个汇编程序，它使用标准输入输出，能够计算大数加法和乘法，并且输出结果后等待下一次输入，如：

```shell
❯ ./cal
Usage: <input_x><operator><input_y>
Limits: -10^20 ≤ x, y ≤ 10^20
Supported Operator: +, *
To quit, input q
Please input: 
1+444
445
Please input: 
3*666
1998
Please input: 
q
```

那我们该如何测试这个程序呢？

一个想法是，将该程序改造成函数的形式，接受参数并返回值，再配合 c 程序调用该函数。

我们也可以利用重定向来完成这件事。

声明：本文所提及的方法存在缺陷，可能存在更好的测试方法，请谨慎参考。

## 实现方法

文件目录：
```text
lab1
├── build.sh		// build shell
├── cal				// calculation program
├── code
│   ├── cal.asm		// asm file
│   ├── main.c		// test file
│   └── main.h
├── README.md
└── test			// test data
    ├── test.in
    └── test.out
```
### 改造 nasm 程序

实验要求是能够循环接受输入直到输入 'q' 。为了测试的方便，我们进行一些小改造
```nasm
section .text
global cal_main
cal_main:
    mov eax, [esp + 4]                   ; enable_test argument
    cmp eax, 0
    je  disable_test
    jmp enable_test
enable_test:
    mov byte[test_enable_bool], 1
    read input, MAX_INPUT_LEN            ; Read input
    call parse_input                     ; Parse input
    call calculate                       ; Calculate
    call print_result                    ; Print result
    jmp exit
disable_test:
......                                   ; your original program
```

首先，在 nasm 程序中接受一个参数 `enable_test`，若为 1 则关闭循环接受输入，以及关闭所有的提示文字。在收到一次输入并输出之后，程序停止。若参数为 0 则程序保持原状。

同时，我们将程序入口设置在 .c 文件中，由 c 程序决定是否打开测试模式，故这里的 nasm 程序入口改名为 `cal_main` 

### 测试程序

接下来编写 .c 文件，关键的测试函数 `test_once` 如下：
```c
int test_once(char input[], char output[]) {
    int p[2];  // pipe
    pipe(p);   // create pipe
    int pid = fork();
    if (pid < 0) {
        fprintf(stderr, "fork error\n");
        return -1;
    } else if (pid == 0) {         // child process
        dup2(p[0], STDIN_FILENO);  // redirect child process stdin and stdout to pipe
        dup2(p[1], STDOUT_FILENO);
        cal_main(1);  // run child process in test mode

        fprintf(stderr, "child process run error\n");  // should not reach here
        return -1;
    } else {  // parent process
        write(p[1], input, strlen(input));
        write(p[1], "\n", 1);
        int status;
        wait(&status);             // wait for child process to finish
        if (!WIFEXITED(status)) {  // if process exit normally, WIFEXITED(status) will be true
            fprintf(stderr, "process exit unexpectedly, status code: %d\n", status);
            return -1;
        }
        if (read(p[0], buf, sizeof(buf)) == -1) {
            fprintf(stderr, "read error\n");
            return -1;
        } else {
            trim(buf);              // buf may have new line
            return strcmp(buf, output) == 0 ? 0 : 1;
        }
    }
}
```
该函数参数为字符串 `input` 和预期的输出 `output` 。首先创建管道 `p[2]` ，并 `fork()` 创建子进程。在子进程中，使用 `dup2()` 关闭 stdin stdout，并将其重定向至管道，最后调用 nasm 程序，开启测试模式。这样，父进程就可以通过管道与子进程进行交互。

在父进程中，向管道写入 `input` 并等待子进程结束。这里简单判断一下子进程的退出状态；随后，从管道获取子进程的输出，并与 `output` 进行比较，将比较的结果返回。

这样，我们通过一次 `fork()` 就完成了对 nasm 程序的一次测试。 

### 完整程序
`build.sh`

```bash
nasm -gdwarf -felf32 code/cal.asm 
gcc -m32 -g code/main.c code/cal.o -o cal
rm code/cal.o
```

`main.h`

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>

/**
 * @brief calculation program entry point written by nasm
 * 
 * @param enable_test 1 to enable test so loop will be disabled, 0 to disable test
 */
extern void cal_main(int enable_test);

/**
 * @brief syscall function.
 * wait for child process to exit
 * 
 */
extern void wait(int* status);

/**
 * @brief test function
 * 
 * @param testfile test file name which refers to file.in and file.out, two files in the same directory
 *
 *
 */
void test(char* testfile);

/**
 * @brief Create a pipe and run cal_main in child process.
 * Using pipe to write input and read output from child process.
 * 
 * @param input input expression
 * @param output expected output
 * @return 0 if output is expected, 1 if output is not expected, -1 if error occurs
 */
int test_once(char input[], char output[]);

```

`main.c`

- `main` 函数通过命令行参数来开启或关闭测试模式。
- 测试存放在 `test.in` `test.out` 两个文件中。`test` 函数读取这两个文件中的数据，并将其传递给 `test_once` ，最后输出测试信息。
- `trim` 函数是为了去除数据首尾的空格和换行，我参考~~照抄~~了[这个](https://stackoverflow.com/questions/122616/how-do-i-trim-leading-trailing-whitespace-in-a-standard-way)

```c
#include "main.h"

char buf[64];  // store output

/**
 * @brief Test program for cal.nasm.
 * To disable test, simply run "cal".
 * To enable test, run "cal -t [testfile]".
 * You must put file.in and file.out in the same place.
 * Test pairs are one line to one line correspondence in .in and .out files
 *
 * @param argc argument numbers
 * @param argv
 * @return int
 */
int main(int argc, char** argv) {
    if (argc == 1) {  // no argument, disable test mode
        cal_main(0);
    } else if (argc == 2 || argc >= 4) {
        printf("Usage: cal [-t] [testfile]\n");
    } else {
        if (strcmp("-t", argv[1]) == 0) {  // test mode enabled
            test(argv[2]);
        } else {
            printf("Usage: cal [-t] [testfile]\n");
        }
    }
    return 0;
}

char* ltrim(char* s) {
    while (isspace(*s)) s++;
    return s;
}

char* rtrim(char* s) {
    char* back = s + strlen(s);
    while (isspace(*--back))
        ;
    *(back + 1) = '\0';
    return s;
}

char* trim(char* s) {
    return rtrim(ltrim(s));
}

void test(char* testfile) {
    char in_path[256];
    char out_path[256];
    char in[64];
    char out[64];
    sprintf(in_path, "%s.in", testfile);
    sprintf(out_path, "%s.out", testfile);
    FILE* in_fd = fopen(in_path, "r");
    FILE* out_fd = fopen(out_path, "r");

    for (int i = 0;; i++) {
        char* get1 = fgets(in, sizeof(in), in_fd);
        char* get2 = fgets(out, sizeof(out), out_fd);
        if (get1 == NULL && get2 == NULL) {
            break;
        } else if (get1 == NULL || get2 == NULL) {
            fprintf(stderr, "Error, test pair cannot match\n");
        } else {
            memset(buf, 0, sizeof(buf));  // clear buf
            trim(in);                   // remove all blanks and newlines
            trim(out);
            int res = test_once(in, out);
            if (res == 0) {
                printf("Test %d passed\n", i);
            } else if (res == 1) {
                printf("Test %d failed\n", i);
                printf("    Input: %s\n", in);
                printf("    Expected: %s\n", out);
                printf("    Actual: %s\n", buf);
            } else if (res == -1) {
                printf("Test %d error\n", i);
            }
        }
    }
    return;
}

int test_once(char input[], char output[]) {
	// ...... see above
}
```

### 测试数据
可自行补充测试数据

`test.in`
```text
1234+567
0+0
100000000000000000000+100000000000000000000
99999999999999999999+99999999999999999999
-100000000000000000000+-100000000000000000000
-99999999999999999999+-99999999999999999999
-1234--567
1234/567
-1234+-567
1234+-567
10090001+-98172305814
-100000000000000000000+-100000000000000000000
-100000000000000000000+100000000000000000000
-99999999999999999999+-99999999999999999999
0+-3
1234*567
1234*-567
-1234*-567
0*-567
0*1
-100000000000000000000*-100000000000000000000
-99999999999999999999*99999999999999999999
999+-1000
```

`test.out`
```text
1801
0
200000000000000000000
199999999999999999998
-200000000000000000000
-199999999999999999998
Invalid
Invalid
-1801
667
-98162215813
-200000000000000000000
0
-199999999999999999998
-3
699678
-699678
699678
0
0
10000000000000000000000000000000000000000
-9999999999999999999800000000000000000001
-1
```

一个可能的测试结果：
```shell
❯ ./build.sh
❯ ./cal -t test/test
Test 0 passed
Test 1 passed
Test 2 passed
Test 3 passed
Test 4 passed
Test 5 passed
Test 6 passed
Test 7 passed
Test 8 passed
process exit unexpectedly, status code: 139
Test 9 error
process exit unexpectedly, status code: 139
Test 10 error
Test 11 passed
Test 12 failed
    Input: -100000000000000000000+100000000000000000000
    Expected: 0
    Actual: -0
Test 13 passed
process exit unexpectedly, status code: 139
Test 14 passed
Test 15 passed
Test 16 passed
Test 17 passed
Test 18 passed
Test 19 passed
Test 20 passed
Test 21 passed
Test 22 passed
```

## 总结

笔者也尝试过保留 nasm 程序的循环，但最大的问题在于无法控制父进程和子进程的交互顺序。 `read()` 虽然会在没有内容可读的时候阻塞，但只要至少有一个字节可读，进程就可能会被调度执行，所以可能会出现子进程未输出所有内容，父进程就被调度的情况，从而无法读取所有数据。最后笔者决定还是把 nasm 程序改造成“开启测试时只跑一次”，这样利用 `wait()` 可以确保子进程退出之后父进程再从管道读取数据。

本文的测试可能会存在一些问题。
- 比如在之后的程序中，如果遇到 "Invalid" 的输入，开启测试时程序需要停止，而不开启测试时程序需要跳转到下一次循环，因此在这个地方还需要再判断一次是否开启了测试，从而执行不同的操作，否则会出现 bug。
- 又比如每次循环可能需要进行内存清空与初始化，使用本文的方法无法测试初始化是否正确。