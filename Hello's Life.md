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
2. 头文件内容被插入到 `hello.i` 中.

gcc 打开 `stdio.h`, 递归处理 `stdio.h` 中的 `#include` 指令, 直到不存在 `#include` 指令.

## 2.4 本章小结

本阶段完成了 `hello.c` 的预处理过程. 预处理使程序在后续的操作中不受阻碍, 可以进行下一阶段的汇编处理.

# 第三章 编译

## 3.1 编译的概念与作用

编译是由编译器完成的第二步操作, 主要做词法分析, 语法分析和语义分析等, 在检查无错误后后, 把代码翻译成汇编语言, 生成 `.i` 文件即被汇编程序.

具体作用, 在于通过词法分析提取出基本字, 标识符, 常数, 运算符和界符, 然后通过语法分析分划出同类型作为整体, 再通过词义分析产生四元式的中间代码, 经过优化后, 开始存储空间分配, 寄存器的调度等复杂工作生成最终的目标代码.

## 3.2 在 Linux 下编译的命令

```zsh
gcc -S hello.i -o hello.s -fomit-frame-pointer -Og
```

## 3.3 Hello 的编译结果解析

### 3.3.1

```s
	.file	"hello.c"
	.text
	.section	.rodata.str1.8,"aMS",@progbits,1
	.align 8
.LC0:
	.string	"\347\224\250\346\263\225: Hello \345\255\246\345\217\267 \345\247\223\345\220\215 \347\247\222\346\225\260\357\274\201"
	.section	.rodata.str1.1,"aMS",@progbits,1
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
	pushq	%rbx
	.cfi_def_cfa_offset 24
	.cfi_offset 3, -24
	subq	$8, %rsp
	.cfi_def_cfa_offset 32
	cmpl	$4, %edi
	jne	.L6
	movq	%rsi, %rbx
	movl	$0, %ebp
	jmp	.L2
.L6:
	leaq	.LC0(%rip), %rdi
	call	puts@PLT
	movl	$1, %edi
	call	exit@PLT
.L3:
	movq	16(%rbx), %rdx
	movq	8(%rbx), %rsi
	leaq	.LC1(%rip), %rdi
	movl	$0, %eax
	call	printf@PLT
	movq	24(%rbx), %rdi
	call	atoi@PLT
	movl	%eax, %edi
	call	sleep@PLT
	addl	$1, %ebp
.L2:
	cmpl	$7, %ebp
	jle	.L3
	call	getchar@PLT
	movl	$0, %eax
	addq	$8, %rsp
	.cfi_def_cfa_offset 24
	popq	%rbx
	.cfi_def_cfa_offset 16
	popq	%rbp
	.cfi_def_cfa_offset 8
	ret
	.cfi_endproc
.LFE6:
	.size	main, .-main
	.ident	"GCC: (GNU) 11.1.0"
	.section	.note.GNU-stack,"",@progbits
```

### 3.3.1 数据

#### 3.3.1.1 常量

对于字符串常量, 数据存储在 `.rodata` 段中. 在 `hello.c` 中, `"用法: Hello 学号 姓名 秒数！"` 和 `"Hello %s %s\n"` 存储在 `.rodata` 段中.

在函数中的数值字面常量被存储在 `.text`, 以指令的一部分存储. 常见的情况有立即数 (`movl $1, %edi`) 和地址计算.

#### 3.3.1.2 变量

已初始化为非零的全局变量和静态局部变量存储在 .data 段中, 未初始化和初始化为 0 的全局变量和静态局部变量存储在 `.bss` 段中. 在 `hello.c` 中, 并不存在这两类变量.

局部变量存储在寄存器或栈上. 比如, 循环变量 `i` 就存储在 `%ebp` 寄存器中.

### 3.3.2 表达式

表达式的值存储在寄存器或栈上, 具体存储位置取决于指令类型和指令参数.

### 3.3.3 类型

有符号整数在编译后表示为补码形式, 在 `hello.c` 中没有体现. 不同大小的整数在同一个表达式里也需要转换, `hello.c` 中没有体现.

### 3.3.4 赋值

全局变量或静态变量赋初值, 表示将其放在 .data 段中. 若不赋初值, 则放在 `.bss` 段中. 在 `hello.c` 中没有体现.

剩下的赋值语句等效于 `mov` 指令, 也可能是某些运算指令的写回/访存阶段. 如 `i = 0` 对应 `movl $0, %ebp`.

### 3.3.5 算术操作

`++` / `--` 符号等价于 `+= 1` / `-= 1`. 此处的算术操作仅有 `i++`, 对应 `addl $1, %rbp`.

### 3.3.6 关系操作

`cmpl %rA, %rB` 表示计算 `%rB - %rA`, 设置 `CC`, 但不保存结果.

`!=` 对应的是 `cmpl` 指令, 检测结果是否为零. `argc != 4` 对应汇编语句 `cmpl $4, %edi` 并用不相等跳转.

`<` 对应的也是 `cmpl` 指令, 检测结果是否有符号. `i < 8` 对应汇编语句 `cmpl $7, %ebp` 并用小于等于跳转.

### 3.3.7 数组操作

数组操作包括取 `argv` 中的元素, 通过 `movq` 实现, 如取 `argv[3]`, 汇编指令为 `movq 24(%rbx), %rdi`, 其中 `%rbx` 为 `argv` 的值. 前面的 `24` 是 `3 * sizeof(char *)`.

### 3.3.8 控制转移

通常是先执行 `cmpx`, 然后执行 `jxx` 实现条件跳转. 在与之相反的分支, 执行无条件跳转.

对于 `if (argc != 4)`, 编译生成

```s
	cmpl	$4, %edi
	jne	.L6
	<body code>
	jmp	.L2
```

对于 `for (i = 0; i < 8; i++)`, 编译生成

```s
  cmpl	$7, %ebp
	jle	.L3
```

### 3.3.9 函数调用

函数调用的前三个参数通过 `%rdi`, `%rsi`, `%rdx` 传递. 函数调用的返回值存储在 %rax 中. 调用函数时, 先设置参数, 随后执行 `callq` 指令. 该指令将下一条指令地址压栈, 并将 `%rip` 设置为要调用的函数的地址.

如, `printf("Hello %s %s\n", argv[1], argv[2]);`, 对应汇编指令为

```s
	movq	16(%rbx), %rdx
	movq	8(%rbx), %rsi
	leaq	.LC1(%rip), %rdi
	movl	$0, %eax
	call	printf@PLT
```

## 3.4 本章小节

编译器通过编译将修改了的源程序编译成汇编程序. 本章对比 C 语言的语句和汇编语句, 理解了汇编语言不同语句的具体含义, 以及不同数据类型的操作与存储.

# 第 4 章 汇编

## 4.1 汇编的概念与作用

把汇编语言翻译成机器语言的过程称为汇编.

作用: 汇编器是将汇编代码 (.s) 转变成机器可以识别的机器指令, 并将这些指令打包成可
重定位目标程序 (.o). .o 文件是一个二进制文件.

## 4.2 在 Linux 下汇编的命令

```zsh
gcc -c hello.s -o hello.o
```

## 4.3 可重定位目标 ELF 格式

| 段        | 功能                                          |
| --------- | --------------------------------------------- |
| ELF 头    | 描述生成该文件的系统字的大小和字节顺序        |
| .text     | 已编译程序的机器代码                          |
| .rodata   | 只读数据                                      |
| .data     | 已初始化的全局和静态 C 变量                   |
| .bss      | 未初始化或初始化为 0 的全局和静态 C 变量      |
| .symtab   | 存放程序中定义和引用的函数和全局变量信息      |
| .rel.text | 一个 .text 节中位置的列表                     |
| .rel.data | 被模块引用或定义的所有全局变量的重定位信息    |
| .debug    | 条目是局部变量, 类型定义, 全局变量及 C 源文件 |
| .line     | C 源程序中行号和 .text 节机器指令的映射       |
| .strtab   | .symtab 和 .debug 中符号表及节头部中节的名字  |
| 节头部表  | 描述目标文件的节                              |

readelf 命令格式

```zsh
readelf --all hello.o > hello_o.elf
```

### 4.3.1 ELF header

```text
ELF 头：
  Magic：  7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00
  类别:                              ELF64
  数据:                              2 补码，小端序 (little endian)
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI 版本:                          0
  类型:                              REL (可重定位文件)
  系统架构:                          Advanced Micro Devices X86-64
  版本:                              0x1
  入口点地址：              0x0
  程序头起点：              0 (bytes into file)
  Start of section headers:          1088 (bytes into file)
  标志：             0x0
  Size of this header:               64 (bytes)
  Size of program headers:           0 (bytes)
  Number of program headers:         0
  Size of section headers:           64 (bytes)
  Number of section headers:         14
  Section header string table index: 13
```

- Magic: 供操作系统辨认使用
- Type: 文件类型 (REL/EXE)
- OS: 操作系统
- Machine: 架构
- Version: ELF 版本, 目前均为 1
- Entry: 程序的入口地址. .o 文件没有入口, 故为 0
- Size / number of section headers: 节头部表中条目的大小和数量

### 4.3.2 Section Headers

节头部表, 包含了文件中出现的各个节的语义, 节的类型, 位置, 偏移量和大小等信息.

```text
节头：
  [号] 名称              类型             地址              偏移量
       大小              全体大小          旗标   链接   信息   对齐
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [ 1] .text             PROGBITS         0000000000000000  00000040
       0000000000000094  0000000000000000  AX       0     0     1
  [ 2] .rela.text        RELA             0000000000000000  000002f0
       00000000000000c0  0000000000000018   I      11     1     8
  [ 3] .data             PROGBITS         0000000000000000  000000d4
       0000000000000000  0000000000000000  WA       0     0     1
  [ 4] .bss              NOBITS           0000000000000000  000000d4
       0000000000000000  0000000000000000  WA       0     0     1
  [ 5] .rodata           PROGBITS         0000000000000000  000000d8
       0000000000000033  0000000000000000   A       0     0     8
  [ 6] .comment          PROGBITS         0000000000000000  0000010b
       0000000000000013  0000000000000001  MS       0     0     1
  [ 7] .note.GNU-stack   PROGBITS         0000000000000000  0000011e
       0000000000000000  0000000000000000           0     0     1
  [ 8] .note.gnu.pr[...] NOTE             0000000000000000  00000120
       0000000000000030  0000000000000000   A       0     0     8
  [ 9] .eh_frame         PROGBITS         0000000000000000  00000150
       0000000000000038  0000000000000000   A       0     0     8
  [10] .rela.eh_frame    RELA             0000000000000000  000003b0
       0000000000000018  0000000000000018   I      11     9     8
  [11] .symtab           SYMTAB           0000000000000000  00000188
       0000000000000120  0000000000000018          12     4     8
  [12] .strtab           STRTAB           0000000000000000  000002a8
       0000000000000048  0000000000000000           0     0     1
  [13] .shstrtab         STRTAB           0000000000000000  000003c8
       0000000000000074  0000000000000000           0     0     1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  l (large), p (processor specific)
```

### 4.3.3 重定位节 .rela.text

```text
重定位节 '.rela.text' at offset 0x2f0 contains 8 entries:
  偏移量          信息           类型           符号值        符号名称 + 加数
000000000018  000300000002 R_X86_64_PC32     0000000000000000 .rodata - 4
000000000020  000600000004 R_X86_64_PLT32    0000000000000000 puts - 4
00000000002a  000700000004 R_X86_64_PLT32    0000000000000000 exit - 4
000000000053  000300000002 R_X86_64_PC32     0000000000000000 .rodata + 22
000000000060  000800000004 R_X86_64_PLT32    0000000000000000 printf - 4
000000000073  000900000004 R_X86_64_PLT32    0000000000000000 atoi - 4
00000000007a  000a00000004 R_X86_64_PLT32    0000000000000000 sleep - 4
000000000089  000b00000004 R_X86_64_PLT32    0000000000000000 getchar - 4
```

这一步生成的可重定向目标文件由于未和标准 C library 链接, 因此有些信息需要修改, 如代码节, 数据节中的对每个符号的引用. 我们在 .rela.text 节中记录这些需要修改的地址. .rela.text 中每个条目维护这些信息:

| 信息   | 含义                                                                                                                                            |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Offset | 要修改的引用相对于 .text 或 .data 节头的偏移量                                                                                                  |
| Info   | 8 Byte. 前 4 Byte 是 symbol, 代表重定位到的目标在 .symtab 中的偏移量. 后 4 Byte 是 type, 是重定位类型 (有 R_X86_64_PC32 和 R_X86_64_PLT32 两种) |
| Addend | 计算重定位位置的辅助信息, 共占 8 个字节                                                                                                         |
| Name   | 重定向到的目标的名称                                                                                                                            |

hello.o 的 .rela.text 节中维护了这些条目: .LC0 (第一个 printf 中的字符串), puts 函
数, exit 函数, .LC1 (第二个 printf 中的字符串), printf 函数, atoi 函数, sleep 函
数, getchar 函数.

### 4.3.4 .symtab

符号表, 用来存放程序中定义和引用的函数和全局变量的信息. 重定位需要引用的符号都在其中声明.

```text
Symbol table '.symtab' contains 12 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS hello.c
     2: 0000000000000000     0 SECTION LOCAL  DEFAULT    1
     3: 0000000000000000     0 SECTION LOCAL  DEFAULT    5
     4: 0000000000000000   148 FUNC    GLOBAL DEFAULT    1 main
     5: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND _GLOBAL_OFFSET_TABLE_
     6: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND puts
     7: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND exit
     8: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND printf
     9: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND atoi
    10: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND sleep
    11: 0000000000000000     0 NOTYPE  GLOBAL DEFAULT  UND getchar
```

## 4.4 Hello.o 的结果解析

执行

```zsh
objdump -d -r hello.o > hello_o.asm
```

将 hello.o 进行反汇编
