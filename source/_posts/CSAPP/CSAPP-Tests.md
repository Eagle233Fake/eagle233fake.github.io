---
title: '[CSAPP] Tests'
date: 2025-03-12 23:56:23
tags:
categories: 
- CSAPP
---

## Week 1

### 问题 1  

在 C 语言编译过程中，汇编程序是由哪个工具产生？  

- A. 预处理器  
- B. 编译器  
- C. 汇编器  
- D. 链接器  

### 问题 2  

在 C 程序的编译过程中，以下哪一个工具的输出是二进制文件？  

- A. 预处理器  
- B. 编译器  
- C. 链接器  
- D. 编辑器  

### 问题 3  

高速缓存位于以下哪个部件之内？  

- A. 寄存器  
- B. 主存储器  
- C. CPU  
- D. 磁盘  

### 问题 4  

在计算机系统中，以下哪一项不属于 CPU 组成部分？  

- A. 主存  
- B. 高速缓存  
- C. 寄存器  
- D. 程序计数器  

### 问题 5  

在计算机系统的抽象层次中，对于一个在 CPU 中正在运行程序的抽象是？  

- A. 源代码  
- B. 虚拟内存  
- C. 文件  
- D. 进程  

### 问题 6  

十进制数 172 的二进制是：  

- A. 10101110  
- B. 11001110  
- C. 10101100  
- D. 10101010  

### 问题 7  

二进制 `100100100` 转换为十六进制是：  

- A. 0x111  
- B. 0x100100100  
- C. 0x124  
- D. 以上都不对  

### 问题 8  

在“六进制”表示中，13 x 12 等于？  

- A. 156  
- B. 200  
- C. 1312  
- D. 以上都不是  

### 问题 9  

`0x5C5D - 0x2E9A = ?`  

- A. 0x2EF2  
- B. 0x2EC3  
- C. 0x2EC2  
- D. 0x2DC3  

**答案：**  
B C C A D C C B D  

---

## Week 2

### 问题 1  

在 64 位机器中，类型 `int **` 所占的字节数为：  

- A. 1  
- B. 4  
- C. 8  
- D. 16  

### 问题 2  

X86 机型中，`int x = 0xAC5A2EF3`，假设 `&x` 为 0x2100，则字节存储单元 0x2102 中存储的数据为：  

- A. AC  
- B. 5A  
- C. 2E  
- D. F3  

### 问题 3  

大端法机器中，字符串 `char *s = "hello world..."`，假设 `s` 为 0x2300，则 0x2305 中存储的字符为：  

- A. o  
- B. （空格）  
- C. r  
- D. l  

### 问题 4  

对于变量 `x`，以下哪个表达式的结果为：`x` 的最低有效字节不变，其它位均置为 1？  

- A. `x & 0xFF`  
- B. `x & ~0xFF`  
- C. `x | 0xFF`  
- D. `x | ~0xFF`  

### 问题 5  

C 表达式 `x == y` 等价于：  

- A. `!(x & y)`  
- B. `!(x | y)`  
- C. `!(x ^ y)`  
- D. 以上都不是  

### 问题 6  

`!!0x56` 的结果是（！表示逻辑运算符）：  

- A. 0x56  
- B. 0x01  
- C. 0x65  
- D. 0x00  

### 问题 7  

`0x96 & 0x33` 和 `0x96 && 0x33` 的结果分别为：  

- A. 0x12，0x01  
- B. 0x01，0x01  
- C. 0x01，0x12  
- D. 0x12，0x12  

### 问题 8  

变量 `x` 的二进制表示为 `[10100011]`，则 `x << 3` 的结果是：  

- A. `[00011000]`  
- B. `[00011111]`  
- C. `[10100011]`  
- D. 不确定  

### 问题 9  

`char` 类型变量 `x` 的二进制表示为 `[10100011]`，则 `x >> 3`（算术移位）的结果是：  

- A. `[00001100]`  
- B. `[00010100]`  
- C. `[11110100]`  
- D. `[00010100]`  

### 问题 10  

对长度为 4 位的整数数据，-3 的补码编码为：  

- A. 1011  
- B. 1101  
- C. 0101  
- D. 1010  

### 问题 11  

假设有一个 10 位的补码整数类型，它的 `TMin` 数值为：  

- A. 0  
- B. -128  
- C. -512  
- D. -1024  

**答案：**  
C B B D C B A A C B C  

---

## Week 3

### 问题 1  

对长度为 8 的整数类型，`U2T(180)` =  

- A. -76  
- B. -180  
- C. 76  
- D. 180  

### 问题 2  

`char x = -100`；`unsigned char ux = x`；则 `ux` 的值为：  

- A. 100  
- B. 256  
- C. 156  
- D. 非法数  

### 问题 3  

`unsigned i = -1`；则关系式 `i >= 0` 为：  

- A. false  
- B. true  
- C. 任意值  
- D. 非法操作  

### 问题 4  

表达式 `-1 < 1U` 的结果值是：  

- A. 0  
- B. 1  
- C. -1  
- D. 不确定  

### 问题 5  

设 8 位补码整数 `a` 的十进制值是 33，`a` 被截断成 6 位后的十进制值为：  

- A. -33  
- B. -31  
- C. 1  
- D. 33  

### 问题 6  

在无符号数加法中发生了溢出的是：  

- A. `x + y`（无符号加法和） = `x`  
- B. `x + y`（无符号加法和） = `y`  
- C. `x + y`（无符号加法和） > `x`  
- D. `x + y`（无符号加法和） < `x`  

### 问题 7  

对于长度为 4 位的无符号乘法，`5 x 7` =  

- A. 3  
- B. 5  
- C. 7  
- D. 35  

### 问题 8  

`TMax` 的加法逆元是：  

- A. `TMax`  
- B. `TMin`  
- C. `TMin + 1`  
- D. 无  

### 问题 9  

对长度为 4 的整数数据，`x=[1010]`，`y=[1100]`，`x + y` 补码加法的结果为：  

- A. 1010  
- B. 0110  
- C. 1100  
- D. 10110  

### 问题 10  

整数变量 `x` 乘以 52，只使用 `+`、`-`、`<<` 运算，其 C 表达式为：  

- A. `(x << 5) + (x << 4)`  
- B. `(x << 6) - (x << 4) + (x << 2)`  
- C. `(x << 6) - (x << 2)`  
- D. `(x << 6) + (x << 4) - (x << 5)`  

### 问题 11  

采用 8 位补码，`-100` 的二进制表示是：  

- A. 01100100  
- B. 11100100  
- C. 10011100  
- D. 10011000  

### 问题 12  

对于长度为 6 位的补码乘法，`30 x 4` =  

- A. -120  
- B. 120  
- C. 56  
- D. -8  

**答案：**  
A C B A B D A C B B C D  

---

## Week 4

### 问题 1  

对一个负整数进行除以 128 的补码除法，在执行算术右移之前需要加上的偏置值为：  

- A. 1  
- B. 7  
- C. 63  
- D. 127  

### 问题 2  

补码整型数据的运算满足：  

- A. 交换律  
- B. 结合律  
- C. 分配律  
- D. 以上都满足  

### 问题 3  

二进制小数 `101.101` 的十进制值是：  

- A. 101.101  
- B. 5.5  
- C. 5.101  
- D. 5.625  

### 问题 4  

二进制循环小数 `10.0110110 [110]` 的数值是：  

- A. 9/7  
- B. 17/7  
- C. 2.3  
- D. 2.5  

### 问题 5  

十进制数字 23 用浮点表示时的小数字段 `frac` 为：  

- A. 1101  
- B. 1011  
- C. 0111  
- D. 1110  

### 问题 6  

数字 11 用浮点表示，采用 K 位阶码，则 `exp` 字段为：  

- A. `2^(K-1) + 2`  
- B. `2^K + 1`  
- C. 3  
- D. `4 - 2^(K-1)`  

### 问题 7  

假设一个参照 IEEE 浮点格式的 8 位浮点格式，有 1 个符号位、3 个阶码位和 4 个小数位，在此编码下 `01010101` 的十进制值为：  

- A. 85.0  
- B. 42.0  
- C. 5/4  
- D. 21/4  

### 问题 8  

假设一个参照 IEEE 浮点格式的 8 位浮点格式，有 1 个符号位、4 个阶码位和 3 个小数位，其编码表示为 `10000101`，则对应的阶码值为：  

- A. -6  
- B. -7  
- C. -14  
- D. -15  

### 问题 9  

假设一个参照 IEEE 浮点格式的 11 位浮点表示，有 1 个符号位、6 个阶码位和 4 个小数位。其最大非规格数是：  

- A. 2 的 -26 次方 × 15  
- B. 2 的 -34 次方 × 15  
- C. 2 的 -18 次方 × 15  
- D. 2 的 -18 次方 × 16  

### 问题 10  

假设一个参照 IEEE 浮点格式的 6 位浮点表示，有 1 个符号位、2 个阶码位和 3 个小数位。其最小的正数是：  

- A. 1/64  
- B. 1/16  
- C. 1/8  
- D. 1  

### 问题 11  

二进制小数 `10.1011` 向偶数舍入到小数点右边 2 位后的数值是：  

- A. 10.10  
- B. 11.00  
- C. 10.11  
- D. 以上都不是  

### 问题 12  

二进制小数 `11.011` 向偶数舍入到小数点右边 2 位后的数值是：  

- A. 11.01  
- B. 11.10  
- C. 11.00  
- D. 以上都不是  

### 问题 13  

`1e20 + (-1e20 + 1.25)` 在计算机中的运行结果是：  

- A. 0.0  
- B. 5.1  
- C. N/A  
- D. 以上都不对  

### 问题 14  

关于浮点运算，以下说法正确的是：  

- A. 浮点加法具有交换律  
- B. 浮点加法具有结合律  
- C. 说法 A 和 B 都正确  
- D. 说法 A 和 B 都错误  

### 问题 15  

在计算机系统中，以下两个命题为真的是：  

(1) 对任何 `int i`，表达式 `( i * i ) >= 0`  
(2) 对任何非 inf 及非 NaN 的 `float f`，表达式 `( f * f ) >= 0.0`  

- A. 仅 1  
- B. 仅 2  
- C. 1 和 2  
- D. 都不是  

**答案：**  
D D D B C A D A B C C B A A B

---

## Week 5

### 问题 1

假设制造口罩的过程包括了缝制、消毒两个步骤，而且其耗时分别占 25% 和 75%，按照 Amdahl 定律，为了将生产速度加快 1/3 倍，消毒步骤所需要的加速比是多少？

- A. 1.33
- B. 1.50
- C. 1.67
- D. 2.00

### 问题 2

在汇编语言中，函数的返回值最后是存放在哪个寄存器？

- A. `%rdi`
- B. `%rax`
- C. `%rsp`
- D. 不确定

### 问题 3

已知 `%rcx` 值为 `0x100`，`%rdi` 值为 `0x18`，则操作数 `0x36(%rcx, %rdi, 2)` 表示的地址值为？

- A. `0x166`
- B. `0x172`
- C. `0x254`
- D. `0x25E`

### 问题 4

下列 `MOV` 指令中有错误的是？

- A. `movw %si, %di`
- B. `movl $0x1234, %edi`
- C. `movq %edx, (%rsp)`
- D. `movb $22, -23(%rsi)`

### 问题 5

若当前栈顶的地址为 `0x0f0100`，`%rbx` 存的值为 `0x126`，当执行完指令 `pushq %rbx` 后，栈顶的地址为？

- A. `0x0f00f8`
- B. `0x0f0092`
- C. `0x0f0108`
- D. `0x0f0126`

### 问题 6

假设 `%rax` 存的值为 `0x108`, `%rsp` 存的值为 `0x158`, 执行 `popq %rax` 指令后，`%rsp` 的值为？

- A. `0x108`
- B. `0x150`
- C. `0x158`
- D. `0x160`

**答案：**  
B B A C A D

---

## Week 6

### 问题 1  

`%rdi = 0x310，%rsi = 0x06，%rdx = 0x20`，执行 `leaq 0x51(%rdi, %rsi, 4), %rdx` ，则 `%rdx` 值为：  

- A. `0x31A`
- B. `0x371`
- C. `0x379`  
- D. `0x395`  

### 问题 2  

`%rci = 0x363`， 执行 `xorq %rci, %rci` ， 则 `%rci` 值为  

- A. `1 ` 
- B. 0  
- C. 0x363  
- D. 以上都不是  

### 问题 3  

以下哪条指令是将 `%rdx` 的内容乘以 2  

- A. `imulq $2`  
- B. `imulq %rdx, $2`  
- C. `salq $1, %rdx`  
- D. `sarq $1, %rdx`  

### 问题 4  

条件码 `ZF` 是  

- A. 进位标志  
- B. 零标志  
- C. 溢出标志  
- D. 符号标志  

### 问题 5  

与下列汇编代码相对应的 C 代码是  
`cmpl %edi, %esi`  
`setbe %al`  

- A. `int comp(int a, int b) { return a >= b; }`  
- B. `int comp(int a, int b) { return a <= b; }`  
- C. `int comp(unsigned a, unsigned b) { return a >= b; }`  
- D. `int comp(unsigned a, unsigned b) { return a <= b; }`  

### 问题 6  

与下列汇编代码最直接对应的 C 代码是  
`cmp %rdi, (%rdx)`  
`jge .L3`  

- A. `if (a <= *p) goto done;`  
- B. `if (a == p) goto done;`  
- C. `if (a != 0) goto done;`  
- D. `if (a >= *p) goto done;`  

### 问题 7  

已知变量 `a` 在 `%rdx`， `b` 在 `%rsi`，与以下汇编代码对应的 C 代码是  
`leaq   (%rdx, %rsi, 16), %rdx`  
`testq   %rdx, %rdx`  
`jne    .L3`  

- A. `if (a + b + 16 == 0) goto .L3;`  
- B. `if (a + 16 * b == 0) goto .L3;`  
- C. `if (a + 16 + b != 0) goto .L3;`  
- D. `if (a + 16 * b != 0) goto .L3;`

**答案：**  
C B C B C A D

---

## Week 7

### 问题 1  

```asm
cmpq  $12, %rcx  
jl   .L3  
salq  $4, %rbx  
.L3  
incq  %rbx  
```

所对应 C 代码的划线处应填：  

```c
if (_________)
    b <<= 4;
else
    b++;
```

- A. k < 12  
- B. k > 12  
- C. k <= 12  
- D. k >= 12  

### 问题 2  

C 语言的 while 循环语句转换到汇编语言，常用的两种翻译方法之一是  

- A. do while  
- B. jump to end  
- C. guarded-do  
- D. conditional move  

### 问题 3  

```asm
310040:    mov 0x5(%rdx), %rax  
310045:    callq 310020 <some_function>  
310052:    add (%rcx), %rax  
```

汇编代码如上所示，在执行 `call` 指令前，`%rip` 为  

- A. 310040  
- B. 310045  
- C. 310020  
- D. 310052  

### 问题 4  

汇编代码如上所示，`call` 指令执行之后，哪个值被压入栈中  

- A. 310040  
- B. 310045  
- C. 310020  
- D. 310052  

### 问题 5  

汇编代码如上所示，`call` 指令执行之后（即进入了被调用函数），`%rip` 值为  

- A. 310040  
- B. 310045  
- C. 310020  
- D. 310052  

### 问题 6  

```c
int fun1()  
{  
  int a1 = 123;  
  int a2 = 456;  
  int a3 = 789;  
  int value = change(a1, &a2, &a3);  
  value >>= 3;  
  return value;  
}
```

在对应的汇编程序中  

- A. `a1` 必须存放在内存中  
- B. `a2` 可以存放在寄存器中  
- C. `a3` 必须存放在内存中  
- D. `value` 必须存放在内存中

**答案：**  
D C B D C C

---

## Week 8

### 问题 1

`int  A[6];`  
已知 A 的起始地址为 a，其第 i 个元素的地址是：

- A. `a + 2i`
- B. `a + 4i`  
- C. `a + 6i`  
- D. `a + 8i`  

### 问题 2

`char **C[12];`  
已知 C 起始地址为 c，则 C 的第 k 个元素的地址是：

- A. `c + 2k`  
- B. `c + 8k`  
- C. `c + 12k`  
- D. `c + 24k`  

### 问题 3

```c
int rect[32][32];
int i, j;
int result;
```

假设 `rect、i、 j、result` 分别在寄存器 `%rdi、%rsi、%rdx、%rax` 中，  
则 `movl 0x60(%rdi, %rsi, 256), %eax` 相对应的 C 代码是：

- A. `result = rect[2*i][15];`  
- B. `result = rect[2*i][24];`  
- C. `result = rect[8*i][15];`  
- D. `result = rect[8*i][24];`  

### 问题 4

```c
struct TCP {
   short dest_port;
   char offset;
   int sn;
   struct {
      char ACK;
      char SYN;
      char FIN;
   } flag;
   struct TCP *next;
};
struct TCP *msg;
```

则 `msg->flag.SYN` 的偏移量是多少？

- A. 7  
- B. 8  
- C. 9  
- D. 10  

### 问题 5

如上，此结构体的总大小为多少？

- A. 18  
- B. 19  
- C. 24  
- D. 32  

### 问题 6

```c
struct MEMO {
   int index;
   char flag;
   int ref;
   char title[21];
   char *content;
   char isPrivate;
};
```

为了节省空间，重新排列这个结构体中的字段，则最优的字段次序是：

- A. `index`, `flag`, `ref`, `title0~title20`, `content`, `isPrivate`  
- B. `title0~title20`, `index`, `flag`, `ref`, `content`, `isPrivate`  
- C. `content`, `index`, `ref`, `flag`, `title0~title20`, `isPrivate`  
- D. `flag`, `content`, `isPrivate`, `index`, `title0~title20`, `ref`  

### 问题 7

```c
union {
  unsigned char c[8];
  unsigned int i[2];
} dw;

for (int j = 0; j < 8; j++)
    dw.c[j] = 0xd0 + j;
```

在小端法机器中，`i[0]` 的值为：

- A. `0xd0d1d2d3`  
- B. `0xd3d2d1d0`  
- C. `0xd7d6d5d4`  
- D. `0xd4d5d6d7`  

### 问题 8

使用教材练习题 3.43 的内容，与下列汇编程序相对应的 C 代码是：

```asm
movq (%rdi), %rax
movq 8(%rdi), %rbx
addq (%rbx), %rax
movq %rax, (%rsi)
```

- A. `*dest = up->t1.u + up->t1.w;`  
- B. `*dest = up->t1.u + up->t1.v;`  
- C. `*dest = up->t1.u + *up->t2.p;`  
- D. `*dest = up->t2.a[0] + *up->t2.p;`  

### 问题 9

下列存储器中存取速度最快的是：

- A. Flash-Memory U盘  
- B. SDRAM  
- C. SSD 固态硬盘  
- D. SRAM  

### 问题 10

我们平时所说的内存条是：

- A. 固态硬盘 SSD  
- B. 非易失性存储器  
- C. 主存 main memory  
- D. 一级缓存  

**答案：**  
B B B C C C B C D C

---

## Week 9

### 问题 1  

已知一个磁盘的转速 = 10000 RPM，平均寻道时间 T_avg_seek = 9ms，磁盘的平均访问时间 T_access = 12.02ms，则每条磁道的平均扇区数为：

- A. 200  
- B. 300  
- C. 400  
- D. 500  

### 问题 2  

扫描读取一个二维数组中的所有元素值，按行读取和按列读取的差别在于：

- A. 按行读的空间局部性更好  
- B. 按列读的空间局部性更好  
- C. 按行读的时间局部性更好  
- D. 按列读的时间局部性更好  

### 问题 3  

上网时，点击昨天看过的一条链接，很可能不用去远程访问 Web 网站就可以直接本地打开这个网页的缓存。技术上这属于：

- A. 缓存替换  
- B. 缓存好运  
- C. 缓存冲突  
- D. 缓存命中  

### 问题 4  

以下关于程序局部性和缓存命中率的说法，错误的是：

- A. 程序的空间局部性越好，缓存命中率就倾向于越高  
- B. 程序的时间局部性越好，缓存命中率就倾向于越高  
- C. 数组引用时的步长越大，缓存命中率就倾向于越高  
- D. 对于读取指令来说，循环体越小，缓存命中率就倾向于越高  

### 问题 5  

高速缓存分为直接映射、组相联、全相联高速缓存等类型，根据以下哪一项能够区分直接映射和其它类型：

- A. 高速缓存的总容量  
- B. 高速缓存的标记位数  
- C. 每个数据块的字节数  
- D. 每个组的高速缓存行数  

### 问题 6  

已知一个系统的物理地址宽 16 位 (m = 16)，其高速缓存大小为 1024 字节，是四路组相联的 (E = 4)，块大小为 4 字节 (B = 4)，这个高速缓存的组数是多少？

- A. 16  
- B. 32  
- C. 64  
- D. 128  

### 问题 7  

如上，高速缓存的标记位即 tag 的位数是多少？

- A. 1  
- B. 2  
- C. 6  
- D. 8  

**答案：**  
B A D C D C D

---

## Week 10

### 问题 1

采用练习题 6.12 的高速缓存（p.436），引用地址 `0x123a`，则返回的字节是：

- A. 3F  
- B. 30  
- C. C0  
- D. 26  

### 问题 2

如上，这个高速缓存的标志位数 (t) 是多少？

- A. 4  
- B. 6  
- C. 8  
- D. 12  

### 问题 3

在作业题 6.34（p.454）基础上，对于一个总大小为 128 字节的高速缓存，src 数组的行 1（第二行）的命中 (h) 及不命中 (m) 分布值为：

- A. m m m m  
- B. m m h m  
- C. m m h h  
- D. m h h h  

### 问题 4

在作业题 6.34（p.454）基础上，对于一个总大小为 96 字节的高速缓存，src 数组的行 2（第三行）的命中 (h) 及不命中 (m) 分布值为：

- A. m h m h  
- B. m h h h  
- C. m m h h  
- D. m m m h  

### 问题 5

对于存储器山而言，步长 stride 值越大表明：

- A. 时间局部性越好  
- B. 时间局部性越差  
- C. 空间局部性越好  
- D. 空间局部性越差  

### 问题 6

```c
int example(int a, int b)
{
   int r1 = fun(a, 0);
   int r2 = fun(b, 1);
   int r = r1 - r2;
   return r << 2;
}
```

在对应的汇编程序中，需要采用被调用者保存寄存器来保存的值有几个？

- A. 1
- B. 2
- C. 3
- D. 4

**答案：**  
D C D B D B

---

## Week 12

### 问题 1

采用 ELF 格式的目标文件中， `.text` 节存放的是

- A. 程序代码
- B. 程序中用到的字符串
- C. 程序中的已初始化的全局变量
- D. 文件类型信息

### 问题 2

在可重定位目标文件中，符号表是存放在哪个节里

- A. `.text`
- B. `.data`
- C. `.bss`
- D. `.symtab`

### 问题 3

以下哪一个工具可以查看目标文件的结构和内容

- A. `vim`
- B. `gcc`
- C. `readelf`
- D. `make`

### 问题 4

静态链接器的主要任务之一是：

- A. 符号定义
- B. 加载目标文件
- C. 执行目标文件
- D. 重定位

### 问题 5

```c
/* Module 1 */
#include <stdio.h>

int a;

int main() {
    a = 0;
    p2();
    a++;
    printf("%d\n", a);
    return 0;
}

/* Module 2 */
int a = 20;

int p2() {
   a = 10;
}
```

编译链接后，运行输出的结果是：

- A. 1
- B. 11
- C. 20
- D. 21

### 问题 6

符号解析所涉及到的强弱符号，其中弱符号被分配到可重定位目标文件的哪个节？

- A. `.text`
- B. `.data`
- C. `.bss`
- D. `COMMON`

### 问题 7

已知：

`g.o --> libx.a --> liby.a --> g.o`
`g.o --> liby.a --> libz.a --> libx.a`

使得静态链接器能够解析所有符号引用的最小命令行是：

- A. `gcc g.o libx.a liby.a libz.a`
- B. `gcc g.o libx.a liby.a libz.a libx.a`
- C. `gcc g.o libx.a liby.a g.o liby.a libz.a libx.a`
- D. `gcc g.o libx.a liby.a libz.a libx.a liby.a`

### 问题 8

下列汇编代码中，第一行指令的地址是多少？
（由于格式原因，以虚线代表空格）

```asm
XXXXXX: 77 02 - - - - ja 4004d5
XXXXXX: 5d  - - - - - pop %rbp
```

- A. `4004d0`
- B. `4004d1`
- C. `4004d3`
- D. `4004d5`

### 问题 9

`net.o` 文件中的一个重定位条目 `5e: R_X86_64_PC32 search-0x4`， 该条目位于文件的哪个节

- A. `.text`
- B. `.data`
- C. `.symtab`
- D. `.rel.text`

### 问题 10

在一个重定位条目 `3a: R_X86_64_32 address` 中， `3a` 的含义是：

- A. 待修改的引用的节内偏移
- B. 这是一个 32 位 PC 相对地址的引用
- C. 被修改引用应该指向的符号
- D. 待修改的全局符号的定义地址

### 问题 11

可重定位目标文件、可执行目标文件的格式具有相似之处，它们有着相同类型 的节，但不包括以下哪一项

- A. `.text`
- B. `.rel.data`
- C. `.bss`
- D. `.symtab`

### 问题 12

关于静态库和动态库，以下错误的是：

- A. 使用静态库的可执行文件所占用的磁盘/内存空间，相对比使用动态库的可执行文件要大
- B. 静态链接的可执行文件必须依赖静态库的存在才能运行
- C. 动态链接的可执行文件必须依赖动态库的存在才能运行
- D. 使用动态库的程序更加方便升级和部署

### 问题 13

对于动态链接所形成的可执行目标文件的 `.data` 节，以下描述正确的是：

- A. 其中包含有局部变量。
- B. 其中包含有符号表。
- C. 在运行前，有些部分需要进行重定位。
- D. 在加载时，加载器保持原样地复制其内容到内存、并执行其中的指令。

### 问题 14

在动态链接过程中，链接器 `ld` 输出的是

- A. 可重定位目标文件
- B. 部分链接的可执行目标文件
- C. 完全链接的可执行目标文件
- D. 重定位和符号表文件

### 问题 15

```bash
linux> gcc -shared –fpic -o libgraph.so node_model.c edge_model.c
```

其中 `-fpic` 选项的用途为

- A. 创建静态目标文件
- B. 编译时进行动态链接
- C. 生成与位置无关的代码
- D. 创建可执行目标文件

### 问题 16

进行链接时打桩的通常条件是：

- A. 能够访问程序的源代码文件
- B. 能够访问程序的可重定位目标文件
- C. 能够访问程序的可执行目标文件
- D. 能够访问程序的汇编代码

**答案：**
A D C D B D B B D A B B C B C B

---

## Week 13

### 问题 1

在能够改变程序控制流的各种机制中，不包含以下哪一项

- A. 函数调用
- B. 强制类型转换
- C. 进程上下文切换
- D. 中断

### 问题 2

以下有关各类异常的正确描述是：

- A. 中断是同步的
- B. 陷阱异常即 `syscall` 意味着发生了某些致命错误
- C. 当故障处理完成并且恢复后，返回到下一条指令
- D. 如果一个应用程序发生了终止异常，终止处理程序从来不将控制返回给该应用程序

### 问题 3

四个进程的运行时间段为：A( 1 -- 4 ), B( 3 -- 6 ), C( 5 -- 9 ), D( 7 -- 11 )
在两两组合的所有进程对中，有多少对是并发运行的？

- A. 2
- B. 3
- C. 4
- D. 5

### 问题 4

对于进程表述错误的是：

- A. 进程提供的抽象之一是：在时间上提供了一个独立的逻辑控制流
- B. 普通用户程序的进程，运行初始时是在用户模式中
- C. 当异常发生时，控制从应用程序转到异常处理程序，处理器将模式从用户模式变为内核模式
- D. 在进行两个进程的上下文切换时，一般是处于用户模式

### 问题 5

以下程序会输出多少行“2”？

```c
int main()
{
   fork();
   if (fork() == 0) {
      fork();
      printf("2\n");
      exit(0);
   }
   printf("2\n");
   exit(0);
}
```

- A. 3
- B. 4
- C. 6
- D. 7

### 问题 6

```c
int main()
{
   int status;
   int k = 1;

   if (fork() == 0) {
      printf("a"); fflush(stdout);
      k++;
   } else {
      printf("b"); fflush(stdout);
      if (waitpid(-1, &status, 0) > 0) {
         if (WIFEXITED(status) != 0) {
           printf("%d", WEXITSTATUS(status) * 2);
           fflush(stdout);
         }
      }
   }
   printf("c"); fflush(stdout);
   exit(k);
}
```

以上程序的一种可能的输出是什么？

- A. abc2c
- B. acb2c
- C. bca4c
- D. bac4c

### 问题 7

如果进程 A 和子进程 A1、 进程 B 和子进程 B1、 进程 C 和子进程 C1 都被挂起，它们后续被调度的先后次序是

- A. 不能确定，与调度器实现有关
- B. A 先于 A1， B 先于 B1， C 先于 C1
- C. A1 先于 A， B1 先于 B， C1 先于 C
- D. 先 A、B、C，再 A1、B1、C1

### 问题 8

在程序中调用 `wait(status)`，以下错误的是

- A. 它相当于 `waitpid(-1, status, 0)`
- B. 调用进程继续运行，并等待着子进程终止的消息通知
- C. 当有一个子进程终止后， `wait(status)` 返回
- D. 通过 `wait(status)` ，父进程得以回收已终止的子进程

### 问题 9

```c
void fork9() {
  if (fork() == 0) {
 printf("Running Child, PID = %d\n", getpid());
 while (1); /* Infinite loop */
   } else {
 printf("Running Parent, PID = %d\n", getpid());
 while (1); /* Infinite loop */
   }
}
```

如果 父进程号为 6639，子进程号为 6640，以下正确的陈述是

- A. 此程序在命令行中运行时，先输出 6639，再输出 6640
- B. 在命令行中运行此程序，执行 `kill 6640` 后，子进程终止并被回收
- C. 在命令行中 运行此程序， 执行 `kill 6640` 后，子进程成为孤儿进程
- D. 在命令行中运行此程序，执行 `kill 6639`、 `kill 6640` 后，由 `init` 进程回收子进程 6640

**答案：**
B D B D C D A B D

---

## Week 14

### 问题 1

已知：

```bash
linux> ./myexample
Child1: pid=3196 pgrp=3195
Child2: pid=3197 pgrp=3195

linux> ps
 PID TTY          TIME CMD
3175 pts/2    00:00:00 tcsh
3196 pts/2    00:00:02 myexample
3197 pts/2    00:00:02 myexample
3201 pts/2    00:00:00 ps
```

请问执行什么命令，然后可以产生以下效果？

```bash
linux> ps
 PID TTY          TIME CMD
3175 pts/2    00:00:00 tcsh
3201 pts/2    00:00:00 ps
```

- A. `/bin/kill 3195`
- B. `/bin/kill -9 3195`
- C. `/bin/kill -9 -3195`
- D. `/bin/kill 3196 3197`

### 问题 2

在 shell 窗口中，用户输入了一个可执行文件名并运行它。此时 shell 程序中的主要处理流程是：

- A. `execve()`，然后调用 `fork()`
- B. `fork()`，然后调用 `execve()`
- C. `fork()`，然后在子进程中调用 `execve()`
- D. `execve()`，然后调用 `fork()`，然后在父进程中调用 `execve()`

### 问题 3

在 Linux shell 中运行一个程序时，按 `Ctrl-Z` 可以将它停止，此处内核发送的信号是：

- A. `SIGINT`
- B. `SIGQUIT`
- C. `SIGALRM`
- D. `SIGTSTP`

### 问题 4

```c
void sig_handler(int sig) {
   printf("Caught\n");
   exit(0);
}

int main() {
   signal(SIGINT, sig_handler);
   printf("Wait\n");
   pause();
   printf("End\n");
   return 0;
}
```

在 shell 中运行以上程序，然后按 `Ctrl-C`，请问输出是什么？

- A. Wait
- B. Caught
- C. Wait
     Caught
     End
- D. Wait
     Caught

### 问题 5

```c
int beeps = 0;
void handler(int sig) {
  if (sig == SIGALRM)
    safe_printf("BEEP\n");
  if (++beeps < 5)
    alarm(1);
  else {
    safe_printf("BOOM!\n");
    exit(0);
  }
}

main() {
  signal(SIGALRM, handler); 
  signal(SIGINT, handler);
  alarm(1);
  while (1) {
  }
}
```

执行以上程序，在 2.5 秒时按 `Ctrl-C`，当程序运行结束的全部输出是什么？

- A. BEEP
     BEEP
- B. BEEP
     BEEP
     BOOM!
- C. BEEP
     BEEP
     BEEP
     BEEP
     BOOM!
- D. BEEP
     BEEP
     BEEP
     BEEP
     BEEP
     BOOM!

### 问题 6

已知 `SIGUSR1` 是用户定义的信号，默认行为是终止进程。

以下这个程序的所有输出的 `counter` 值之和是多少？

```c
pid_t pid;
int counter = 101;

void handler1(int sig) {
   counter = counter * 2;
   printf("counter = %d\n", counter);
   fflush(stdout); /* Flushes the printed string to stdout */
   kill(pid, SIGUSR1);
}

void handler2(int sig) {
   counter ++;
   printf("counter = %d\n", counter);
   exit(0);
}

main() {
   signal(SIGUSR1, handler1);
   if ((pid = fork()) == 0) {
      signal(SIGUSR1, handler2);
      kill(getppid(), SIGUSR1);
      while(1) {};
   } else {
      pid_t p; int status;
      if ((p = wait(&status)) > 0) {
         counter += 100;
         printf("counter = %d\n", counter);
      }
   }
}
```

- A. 101
- B. 406
- C. 606
- D. 706

### 问题 7

`setjmp` 函数的特点是：

- A. 调用一次，从不返回
- B. 调用一次，返回一次
- C. 调用一次，返回两次
- D. 调用一次，返回一次或者多次

### 问题 8

在程序正常运行时，以下哪些函数的行为是 "调用一次，从不返回"？ （多选）

- A. `execve`
- B. `fork`
- C. `longjmp`
- D. `setjmp`

### 问题 9

虚拟内存的位置通常在哪里

- A. 高速缓存
- B. 物理内存
- C. 硬盘
- D. 服务器

### 问题 10

CPU / 进程如果对内存中的数据进行读写操作，使用的地址是：

- A. 物理地址
- B. 虚拟地址
- C. 高速缓存地址
- D. 磁盘逻辑块地址

**答案：**
B D B D C D A B D

---

## Week 15

### 问题 1

在地址翻译时，使用 TLB 的直接好处是

- A. 节省页表的存储空间  
- B. 节省数据页的存储空间  
- C. 节省页表的查找时间  
- D. 节省数据页的查找时间  

### 问题 2

对于一个地址翻译的案例，我们假设：

1) 内存是按字节寻址的。  
2) 页面大小是 32 字节。  
3) 虚拟地址的长度是 12 位。  
4) 物理地址的长度是 10 位。  
5) TLB 是二路组相联的，一共有 16 个条目。  

已知虚拟地址 `0x3D2`。求 PPO  

- A. `0x12`  
- B. `0x1E`  
- C. `0x02`  
- D. `0x3D`  

### 问题 3

如上，已知虚拟地址 `0x3D2`。试求 TLBT  

- A. `0x01`  
- B. `0x03`  
- C. `0x06`  
- D. `0x07`  

### 问题 4

在一个 k 级页表体系中，虚拟地址中的 VPN 被分隔成 `VPN 1`、`VPN 2`、... `VPN k`。其中，`VPN i`（1 <= i <= k-1）的用途是：  

- A. 用于查找页内的一个数据块  
- B. 用于查找页的物理地址 PPN  
- C. 用于查找第 i 级页表中的一个 PTE 条目  
- D. 用于查找第 i 级页表的基址  

### 问题 5

以下关于页表的描述中，错误的是：  

- A. 利用页表，可实现虚实地址变换。  
- B. 页表由页表条目（页表项）组成，页表项反映了对应虚页的存储状态。  
- C. 页表的大小由实页数及每个页表条目所需字节数决定。  
- D. 在多级页表中，第一级页表是常驻主存。  

### 问题 6

一个程序的主体代码如下：

```c
int fd;
char *mmaped;

fd = open("hello.txt", O_RDWR);   
mmaped = (char *)mmap(NULL, 10, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);   
close(fd);   
mmaped[5] = 'e';
munmap(mmaped, 10);
```

`hello.txt` 原来内容为 `"Hello, world!"`，运行以上程序之后，现在内容为：  

- A. `Hello, world!`  
- B. `Helle, world!`  
- C. `Helloe, world!`  
- D. `Helloe world!`  

### 问题 7

以下哪一项体现了动态内存隐式分配器的特点：  

- A. Java语言中的 `new`  
- B. C语言中的 `free`  
- C. Python语言中的垃圾收集  
- D. C++语言中的多重继承  

### 问题 8

动态内存分配器的两个性能指标，即吞吐率和内存利用率之间的关系是  

- A. 内存和磁盘的关系  
- B. 时间和空间的关系  
- C. 网络和本地的关系  
- D. 全局和局部的关系  

### 问题 9

在隐式空闲链表（implicit list）中，块与块之间的链接主要是依靠什么来完成  

- A. 块的长度信息  
- B. 块的分配/空闲标记  
- C. 块的单向链表指针  
- D. 块的双向链表指针  

### 问题 10

在采用隐式空闲链表的动态存储分配过程中，搜索一个空闲块进行存储分配的放置策略不包括  

- A. Mark and Sweep  
- B. 首次适配法  
- C. next fit  
- D. 最佳适配法  

### 问题 11

在隐式空闲链表中，增加脚部作为边界标记的主要原因是  

- A. 便于知道邻居块的对齐情况  
- B. 便于增加碎片的大小  
- C. 便于一个已被释放的块和前一个空闲块合并  
- D. 便于一个已被释放的块和后一个空闲块合并  

### 问题 12

假设一个显式空闲链表，每个空闲块中有四字节的 `pred` 和 `succ` 指针，头部和脚部都存放在四字节的字中，不允许有效载荷的大小为零。对齐要求为双字对齐。符合要求的最小块大小是  

- A. 16 字节  
- B. 20 字节  
- C. 24 字节  
- D. 32 字节  

### 问题 13

在实际应用中，`malloc` 动态内存管理包通常采用的分配器方案是  

- A. 隐式空闲列表  
- B. 显式空闲列表  
- C. 简单分离存储的空闲链表  
- D. 分离适配的空闲链表  

### 问题 14

为了支持采用隐式空闲链表的动态存储器分配，使用教材图 9-39 所示的内存块布局。请在以下程序的空白处选择合适语句：

```c
/* given a pointer p to an allocated block, i.e., p is a pointer returned by some previous malloc()/realloc() call; returns the pointer to the header of the block */
void *header(void* p) {
    void *ptr;
    _____________;
    return ptr;
}
```

- A. `ptr = p - 1`  
- B. `ptr = (void *)((int *)p - 1)`  
- C. `ptr = (void *)((int *)p - 4)`  
- D. 以上都不对  

### 问题 15

为了支持采用隐式空闲链表的动态存储器分配，使用教材图 9-39 所示的内存块布局。请在以下程序的空白处选择合适语句：

```c
/* given a pointer to a valid block header or footer, returns the usage of the current block, 1 for allocated, 0 for free */
int allocated(void *hp) {
    int result;
    ____________;
    return result;
}
```

- A. `result = (*(int *)hp) & 1`  
- B. `result = (*(int *)hp) & 0`  
- C. `result = (*(int *)hp) | 1`  
- D. 以上都不对  

**答案：**  
C A B C C D C B A A C C D B A

---
