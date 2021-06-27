[TOC]

# 第 1 章 概述

## 1.1 Hello 简介

P2P (from program to process): `hello.c` 文件是通过文本编辑器编写, 存入文件系统中. `hello.c` 经过预处理器 `cpp` 预处理成 `hello.i`, 再经过编译器 `cc1` 变成 `hello.s`, 再经过汇编器 `as` 变成 `hello.o`, 最后经过连接器 `ld` 与 libc 进行链接, 最终变成 `hello` 可执行目标文件. Shell 通过键盘读取 `./hello` 命令, fork 子进程, 并使用 `execve` 加载 `./hello`, 最终 hello 变成了一个进程 (Process).

020 (i.e. from zero to zero): Shell 执行可执行目标文件 hello, 并创建一组新的代码, 数据, 堆和栈空间. 新的堆被初始化为 0. 执行 hello 的过程中, 堆和栈的大小发生变化. 在 hello 执行完成后, 父进程 shell 回收子进程 hello, IO 管理与信号也通过软硬结合, 输出到屏幕. 堆栈信息恢复到执行 hello 之前的状态, 也就是 020.

## 1.2 环境与工具

OS 环境:

```text
OS: Arch Linux x86_64
Host: VMware 16.1.0
Kernel: 5.12.12-arch1-1
Shell: zsh 5.8
CPU: Intel i7-8550U (4) @ 1.991GHz
```

软件/开发工具版本:

- gcc (GCC) 11.1.0
-

## 1.3 中间结果

| 文件          | 内容                                     |
| ------------- | ---------------------------------------- |
| `hello.i`     | 预处理过的源程序                         |
| `hello.s`     | 汇编程序                                 |
| `hello.o`     | 可重定位目标程序                         |
| `hello`       | 可执行程序                               |
| `hello_o.elf` | hello.o 通过 readelf 查看的 elf 结构文本 |
| `hello.elf`   | hello 通过 readelf 查看的 elf 结构文本   |
| `hello_o.asm` | hello.o 通过 objdump 查看的反汇编代码    |
| `hello.asm`   | hello 通过 objdump 查看的反汇编代码      |

## 1.4 本章小节

# 第 2 章 预处理

## 2.1 预处理的概念及作用

预处理器根据 # 开头的预处理指令, 修改原式的 C 程序. 常见的预处理指令包括 `#include`, `#define`, `#ifdef`, `#ifndef`, `#else` 等. 在 `hello.c` 中, 三条预处理指令为三条 `#include` 指令:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
```

hello.c 经过预处理, 生成 hello.i 文件. 预处理的作用有:

1. `#include` 头文件的内容插入到程序文本中;
2. `#define` 定义 token, 并将 token 替换指定文本;
3. 删除所有注释;
4. 添加行号和文件标识符. 用于显示调试信息, 如编译错误和警告等;
5. 处理条件预编译指令, 如 `#ifdef` 等指令;

## 2.2 在 Linux 下预处理的命令

```zsh
gcc -E hello.c -o hello.i
```

## 2.3 Hello 的预处理结果解析

预处理文件结果共有 3065 行.

```c
# 0 "hello.c"
# 0 "<built-in>"
# 0 "<命令行>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 0 "<命令行>" 2
# 1 "hello.c"

# 1 "/usr/include/stdio.h" 1 3 4
# 27 "/usr/include/stdio.h" 3 4
# 1 "/usr/include/bits/libc-header-start.h" 1 3 4
# 33 "/usr/include/bits/libc-header-start.h" 3 4
# 1 "/usr/include/features.h" 1 3 4
# 473 "/usr/include/features.h" 3 4
# 1 "/usr/include/sys/cdefs.h" 1 3 4
# 462 "/usr/include/sys/cdefs.h" 3 4
# 1 "/usr/include/bits/wordsize.h" 1 3 4
# 463 "/usr/include/sys/cdefs.h" 2 3 4
# 1 "/usr/include/bits/long-double.h" 1 3 4
# 464 "/usr/include/sys/cdefs.h" 2 3 4
# 474 "/usr/include/features.h" 2 3 4
# 497 "/usr/include/features.h" 3 4
# 1 "/usr/include/gnu/stubs.h" 1 3 4
# 10 "/usr/include/gnu/stubs.h" 3 4
# 1 "/usr/include/gnu/stubs-64.h" 1 3 4
# 11 "/usr/include/gnu/stubs.h" 2 3 4
# 498 "/usr/include/features.h" 2 3 4
# 34 "/usr/include/bits/libc-header-start.h" 2 3 4
# 28 "/usr/include/stdio.h" 2 3 4

/* omit some code */

typedef unsigned char __u_char;
typedef unsigned short int __u_short;
typedef unsigned int __u_int;
typedef unsigned long int __u_long;


typedef signed char __int8_t;
typedef unsigned char __uint8_t;
typedef signed short int __int16_t;
typedef unsigned short int __uint16_t;
typedef signed int __int32_t;
typedef unsigned int __uint32_t;

typedef signed long int __int64_t;
typedef unsigned long int __uint64_t;

/* omit some code */

struct _IO_FILE
{
  int _flags;


  char *_IO_read_ptr;
  char *_IO_read_end;
  char *_IO_read_base;
  char *_IO_write_base;
  char *_IO_write_ptr;
  char *_IO_write_end;
  char *_IO_buf_base;
  char *_IO_buf_end;


  char *_IO_save_base;
  char *_IO_backup_base;
  char *_IO_save_end;

  struct _IO_marker *_markers;
  /* omit some code */
}

/* omit some code */

extern int usleep (__useconds_t __useconds);
# 478 "/usr/include/unistd.h" 3 4
extern int pause (void);

/* omit some code */

# 10 "hello.c"
int main(int argc, char *argv[])
{
 int i;

 if (argc != 4)
 {
  printf("用法: Hello 学号 姓名 秒数！\n");
  exit(1);
 }
 for (i = 0; i < 8; i++)
 {
  printf("Hello %s %s\n", argv[1], argv[2]);
  sleep(atoi(argv[3]));
 }
 getchar();
 return 0;
}

```

我们发现:

1. 注释被删除
2. 头文件内容被插入到 hello.i 中.

gcc 打开 stdio.h, 递归处理 stdio.h 中的 #include 指令, 直到不存在 #include 指令.

## 2.4 本章小结

本阶段完成了 hello.c 的预处理过程. 预处理使程序在后续的操作中不受阻碍, 可以进行下一阶段的汇编处理.

# 第三章 编译

## 3.1 编译的概念与作用

编译是由编译器完成的第二步操作, 主要做词法分析, 语法分析和语义分析等, 在检查无错误后后, 把代码翻译成汇编语言, 生成 .i 文件即被汇编程序.

具体作用, 在于通过词法分析提取出基本字, 标识符, 常数, 运算符和界符, 然后通过语法分析分划出同类型作为整体, 再通过词义分析产生四元式的中间代码, 经过优化后, 开始存储空间分配, 寄存器的调度等复杂工作生成最终的目标代码.

## 3.2 在 Linux 下编译的命令

```zsh
gcc -S hello.i -o hello.s
```

## 3.3 Hello 的编译结果解析

### 3.3.1

```s
	.file	"hello.c"
	.text
	.section	.rodata
	.align 8
.LC0:
	.string	"\347\224\250\346\263\225: Hello \345\255\246\345\217\267 \345\247\223\345\220\215 \347\247\222\346\225\260\357\274\201"
.LC1:
	.string	"Hello %s %s\n"
	.text
	.globl	main
	.type	main, @function
main:
.LFB6:
	.cfi_startproc
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	subq	$32, %rsp
	movl	%edi, -20(%rbp)
	movq	%rsi, -32(%rbp)
	cmpl	$4, -20(%rbp)
	je	.L2
	leaq	.LC0(%rip), %rax
	movq	%rax, %rdi
	call	puts@PLT
	movl	$1, %edi
	call	exit@PLT
.L2:
	movl	$0, -4(%rbp)
	jmp	.L3
.L4:
	movq	-32(%rbp), %rax
	addq	$16, %rax
	movq	(%rax), %rdx
	movq	-32(%rbp), %rax
	addq	$8, %rax
	movq	(%rax), %rax
	movq	%rax, %rsi
	leaq	.LC1(%rip), %rax
	movq	%rax, %rdi
	movl	$0, %eax
	call	printf@PLT
	movq	-32(%rbp), %rax
	addq	$24, %rax
	movq	(%rax), %rax
	movq	%rax, %rdi
	call	atoi@PLT
	movl	%eax, %edi
	call	sleep@PLT
	addl	$1, -4(%rbp)
.L3:
	cmpl	$7, -4(%rbp)
	jle	.L4
	call	getchar@PLT
	movl	$0, %eax
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE6:
	.size	main, .-main
	.ident	"GCC: (GNU) 11.1.0"
	.section	.note.GNU-stack,"",@progbits
```
