---
title: '[CSAPP] Lab2 - Bomb Lab Report'
date: 2025-04-14 21:38:55
tags:
categories: 
- CSAPP
---

- **整体思路：利用汇编语言知识结合 gdb 调试一起推导过程，仅调试和仅观察都无法解开问题，必须一边调试一边理解汇编代码，找出答案规律。**

## Phase 1

### 汇编代码

```nasm
0000000000400ee0 <phase_1>:
  400ee0:   48 83 ec 08             sub    $0x8,%rsp 
  400ee4:   be 00 24 40 00          mov    $0x402400,%esi 
  COMMENT ; 将 0x402400 中的内容作为第二个参数传入 strings_not_equal 函数
  400ee9:   e8 4a 04 00 00          call   401338 <strings_not_equal>
  400eee:   85 c0                   test   %eax,%eax
  400ef0:   74 05                   je     400ef7 <phase_1+0x17>
  COMMENT ; 如果两个参数相等，跳过炸弹
  400ef2:   e8 43 05 00 00          call   40143a <explode_bomb>
  400ef7:   48 83 c4 08             add    $0x8,%rsp
  400efb:   c3                      ret
```

### 分析过程

`400ee0`中，`0x402400`内存中的内容存到`%esi`寄存器中，作为第二个参数传入`strings_not_equal`函数中（第一个是`%edi`，是用户传入的）。

之后在`400ee9`调用了函数比较两个字符串的内容，倘若相等，返回`0`。

接下来`400eee`和`400ef0`判断返回值是多少，如果是`0`，即字符串相等，则跳过炸弹到`400ef7`，反之到`400ef2`触发炸弹。

于是，我们现在明白了，只需要知道`0x402400`里面存放的是什么，然后在gdb中输入这个字符串，就可以解开炸弹了。

最终答案为：

**`Border relations with Canada have never been better.`**

### 伪代码

我们可以得到如下C伪代码便于理解：

```cpp
int strings_not_equal(string &a, string &b) {
    if (a == b) {
        return 0;
    } 
    return 1;
}

void phase_1(userInput, *(0x402400)) {
    if (string_not_equal(userInput, *(0x402400)) == 0) {
        The bomb is defused;
    } else {
        The bomb explodes;
    }
}
```

### 运行截图

![phase_1.png](https://s2.loli.net/2025/04/13/ilBF8UfAdHs7JTg.png)

---

## Phase 2

### 汇编代码

```nasm
0000000000400efc <phase_2>:
  400efc:   55                      push   %rbp
  400efd:   53                      push   %rbx
  400efe:   48 83 ec 28             sub    $0x28,%rsp
  400f02:   48 89 e6                mov    %rsp,%rsi
  COMMENT ; 栈底现在往下 0x28 个单位
  400f05:   e8 52 05 00 00          call   40145c <read_six_numbers>
  COMMENT ; 输入6个数字
  400f0a:   83 3c 24 01             cmpl   $0x1,(%rsp)
  COMMENT ; 第一个数字是1
  400f0e:   74 20                   je     400f30 <phase_2+0x34>
  400f10:   e8 25 05 00 00          call   40143a <explode_bomb>
  400f15:   eb 19                   jmp    400f30 <phase_2+0x34>
  400f17:   8b 43 fc                mov    -0x4(%rbx),%eax
  400f1a:   01 c0                   add    %eax,%eax
  400f1c:   39 03                   cmp    %eax,(%rbx)
  400f1e:   74 05                   je     400f25 <phase_2+0x29>
  400f20:   e8 15 05 00 00          call   40143a <explode_bomb>
  400f25:   48 83 c3 04             add    $0x4,%rbx
  400f29:   48 39 eb                cmp    %rbp,%rbx
  400f2c:   75 e9                   jne    400f17 <phase_2+0x1b>
  400f2e:   eb 0c                   jmp    400f3c <phase_2+0x40>
  400f30:   48 8d 5c 24 04          lea    0x4(%rsp),%rbx
  400f35:   48 8d 6c 24 18          lea    0x18(%rsp),%rbp
  COMMENT ; 边界，循环结束的标志
  400f3a:   eb db                   jmp    400f17 <phase_2+0x1b>
  400f3c:   48 83 c4 28             add    $0x28,%rsp
  400f40:   5b                      pop    %rbx
  400f41:   5d                      pop    %rbp
  400f42:   c3                      ret
```

### 分析过程

`400f05`中，调用了一个获得6个数字的函数`read_six_numbers`，之后将对这六个数字进行操作。

`400f0a`后，比较了此时在栈底的`%rsp`里面的值和`1`，如果相等就跳到`400f30`，防止炸弹爆炸。显然，`%rsp`的值就是`1`。

`400f30`后，`%rbx`指向的是`%rsp`往上4个位置，`%rbp`指向的是`%rsp`往上18个位置，然后跳转到`400f17`。

在`400f17`，`%rbx`向下移动4个位置的值存储在`%eax`当中，此时就是`%rsp`的值，为`1`。之后`%eax`的值翻倍，并且判断其值是否就是`%rbx`.由于翻倍后的`%eax`的值是`2`，因此`%rbx`的值也应该是`2`。

接着两个值的确相同的话，跳转到`400f25`，`%rbx`向上移动4个单位，比较此时`%rbx`和`%rbq`。这里分为两个分支：如果相同跳转到`400f3c`，反之跳转到`400f17`。我们不妨先来分析不相同的情况。

`400f17`其实已经在上文分析过了，通过刚刚的规律可以知道，这个时候`%rbx`里面的值总是`%rbx`往下4个单位里面的值的两倍，因此从栈底到栈顶的方向，我们就知道有这样一串等比数列：

`1 2 4 8 16 32 ...`

那到底什么时候结束呢？很显然通过`read_six_numbers`可以知道，我们只要6个数字。这时候又有一个问题：是递增的序列输入还是递减的序列输入呢？我们知道每个新的数字都要是**前一个**数字的两倍，因此一定是递增数列。投机取巧的方法就是都试一遍，哪个不会炸那个就是对的了。

因此答案是：

**`1 2 4 8 16 32`**

### 伪代码

```c
void phase_2(void) {
    int a[6];
    read_six_numbers(a);
    int n = 6;
    for (int i = 1; i < 6; i++) {
        if (a[i] != a[i - 1] * 2) {
            The bomb explodes;
        }
    }
    The bomb is defused;
}
```

### 运行截图

![phase_2.png](https://s2.loli.net/2025/04/13/4rfZhIgwNKpRmTv.png)

---

## Phase 3

### 汇编代码

```nasm
0000000000400f43 <phase_3>:
  400f43:   48 83 ec 18             sub    $0x18,%rsp
  400f47:   48 8d 4c 24 0c          lea    0xc(%rsp),%rcx
  400f4c:   48 8d 54 24 08          lea    0x8(%rsp),%rdx
  400f51:   be cf 25 40 00          mov    $0x4025cf,%esi
  400f56:   b8 00 00 00 00          mov    $0x0,%eax
  400f5b:   e8 90 fc ff ff          call   400bf0 <__isoc99_sscanf@plt>
  400f60:   83 f8 01                cmp    $0x1,%eax
  400f63:   7f 05                   jg     400f6a <phase_3+0x27>
  400f65:   e8 d0 04 00 00          call   40143a <explode_bomb>
  400f6a:   83 7c 24 08 07          cmpl   $0x7,0x8(%rsp)
  400f6f:   77 3c                   ja     400fad <phase_3+0x6a>
  400f71:   8b 44 24 08             mov    0x8(%rsp),%eax
  400f75:   ff 24 c5 70 24 40 00    jmp    *0x402470(,%rax,8)
  400f7c:   b8 cf 00 00 00          mov    $0xcf,%eax
  400f81:   eb 3b                   jmp    400fbe <phase_3+0x7b>
  400f83:   b8 c3 02 00 00          mov    $0x2c3,%eax
  400f88:   eb 34                   jmp    400fbe <phase_3+0x7b>
  400f8a:   b8 00 01 00 00          mov    $0x100,%eax
  400f8f:   eb 2d                   jmp    400fbe <phase_3+0x7b>
  400f91:   b8 85 01 00 00          mov    $0x185,%eax
  400f96:   eb 26                   jmp    400fbe <phase_3+0x7b>
  400f98:   b8 ce 00 00 00          mov    $0xce,%eax
  400f9d:   eb 1f                   jmp    400fbe <phase_3+0x7b>
  400f9f:   b8 aa 02 00 00          mov    $0x2aa,%eax
  400fa4:   eb 18                   jmp    400fbe <phase_3+0x7b>
  400fa6:   b8 47 01 00 00          mov    $0x147,%eax
  400fab:   eb 11                   jmp    400fbe <phase_3+0x7b>
  400fad:   e8 88 04 00 00          call   40143a <explode_bomb>
  400fb2:   b8 00 00 00 00          mov    $0x0,%eax
  400fb7:   eb 05                   jmp    400fbe <phase_3+0x7b>
  400fb9:   b8 37 01 00 00          mov    $0x137,%eax
  400fbe:   3b 44 24 0c             cmp    0xc(%rsp),%eax
  400fc2:   74 05                   je     400fc9 <phase_3+0x86>
  400fc4:   e8 71 04 00 00          call   40143a <explode_bomb>
  400fc9:   48 83 c4 18             add    $0x18,%rsp
  400fcd:   c3                      ret
```

### 分析过程

前面两个炸弹都可以干看看出答案，但是第三个往后明显就困难多了。往后的炸弹我们很难逐条分析汇编代码来分析功能，现在开始我们使用gdb断点来帮助我们了解程序功能。

我们总体上要关注每次出现`explode_bomb`这个函数前程序做了什么，有助于理解我们传入的参数需要满足什么条件。

`400f5b`的`__isoc99_sscanf@plt`函数长得非常奇怪，我们可以使用如下指令去看看其到底是做什么的。

```bash
gdb bomb
b 89 # 在 bomb.c 中，这是进入 phase_3 的入口
run ans.txt # 第一和第二个炸弹的答案已经保存了
test
stepi
... # 跳过好多条汇编代码
```

最终展现如下信息：

```bash
__GI___isoc99_sscanf (s=0x603820 <input_strings+160> "test", format=0x4025cf "%d %d") at ./stdio-common/isoc99_sscanf.c:24
```

说明第三个炸弹需要我们传入两个整型。

我们将`ans.txt`随意传入两个整数`22 23`。

使用`stepi` `nexti` `finish` `disas`等指令，当跳转到：

```nasm
0x0000000000400f63 <+32>:    jg     0x400f6a <phase_3+39>
```

我们很好奇这上一行`400f60`中的`%eax`的值是多少，因为只有这个值比1大，炸弹才不会爆炸。那么我们用`info r`或者`p $eax`查看一下寄存器的值，发现是`2`。大胆猜测，这个值实际上是`sscanf`的返回值，意味着我们传入了两个参数。验证的方法也很简单，只需要在拆除第三个炸弹时只传入一个参数，这时候确实爆炸了，而且`$eax`值为`1`。

让我们仍然传入两个参数`22 23`。

当运行到：

```nasm
0x0000000000400f6a <+39>:    cmpl   $0x7,0x8(%rsp)
```

我们可以检查一下`0x8(%rsp)`是什么，因为接下来就要进行函数跳转了。运用命令，得：

```bash
(gdb) x/d ($rsp + 8)
0x7fffffffd678: 22
```

这个`22`就是我们传入的第一个参数！这个数值一旦比`7`来得大，就会跳转到`400fad`，炸弹爆炸。因此我们第一个参数必须小于等于`7`！我们修改第一个参数为`5`重新实验。运行到：

```nasm
0x0000000000400fb9 <+118>:   mov    $0x137,%eax
0x0000000000400fbe <+123>:   cmp    0xc(%rsp),%eax
0x0000000000400fc2 <+127>:   je     0x400fc9 <phase_3+134>
```

我们又不由自主关心起`0xc(%rsp)`和`%eax`是什么，因为`0xc(%rsp)`的值如果不是`0x137`就会引爆炸弹。我们发现：

```bash
(gdb) p $eax
$3 = 206
(gdb) x /d ($rsp + 0xc)
0x7fffffffd67c: 2
```

这说明炸弹会引爆。我们需要回去逐行分析代码找出规律了。运行到：

```nasm
0x0000000000400f71 <+46>:    mov    0x8(%rsp),%eax
```

`%eax`存放了`0x8(%rsp)`的值，即上文提到的第一个参数值。接下来运行到：

```nasm
0x0000000000400f98 <+85>:    mov    $0xce,%eax
```

这个时候把`0xce` `(206)`赋值给`%eax`，和我们那次即将引爆炸弹的`%eax`值相等。那我刚刚的分析好像显得有些多余了？？？

再看这条指令：

```bash
(gdb) x /d ($rsp + 0xc)
0x7fffffffd67c: 2
```

我们似乎并不清楚这里的`2`是什么含义，但我瞬间有一种很不好的预感：在第二个炸弹的时候，我直接用 **Windows 下的 VS Code** 编辑我的`ans.txt`，结果答案正确输入了，炸弹还是爆炸。于是我换成了 **Vim** 重新编辑了我的答案，炸弹就正常拆除了。我第二个参数传入的是23，我很合理的怀疑这里的`2`实际上应该是`23`。于是我抛弃 VS Code ，再重新编辑，这一次我再查看`($rsp + 0xc)`，结果就符合我的意图了。

```bash
(gdb) x /d ($rsp + 0xc)
0x7fffffffd67c: 23
```

我很难排查到究竟是什么原因导致了 Linux 和 Windows 上的格式差距，但这个例子提醒我：**尽量在 Linux 环境下进行编辑修改，以免产生不必要的格式方面的麻烦**。

那么我这次使用 **Vim** 修改了答案为`5 206`，炸弹就被拆除了。

实际上，第二个参数不总是`206`。当我把第一个参数换成别的值的时候，如果不改变第二个值，炸弹会爆炸。因此两个参数存在某种神奇的关系。但是我们只要能拆除炸弹就好了，所以关系是什么并不重要。

其实这么多的`jmp`，让我感觉到这个程序实际上实现了`switch`的功能。

最终答案是：

**`5 206`**

### 伪代码

仅仅是伪代码，忽略语法错误。

```c
void phase_3(int argc, char *argv[]) {
    if (argc != 2) {
        The bomb explodes;
    }
    // 当 argv[0] == 5 的时候，argv[1] == 206，不会爆炸。
    // 但是我并不知道这两个参数之间有什么关系，总之这样子可以拆除炸弹。
    if (stoi(argv[0]) <= 7 && stoi(argv[1]) == ???) { 
        The bomb is defused；
    }
}
```

### 运行截图

![phase_3.png](https://s2.loli.net/2025/04/13/eAidJQslZ71PSgF.png)

---

## Phase 4

### 汇编代码

```nasm
0000000000400fce <func4>:
  400fce:   48 83 ec 08             sub    $0x8,%rsp
  400fd2:   89 d0                   mov    %edx,%eax
  400fd4:   29 f0                   sub    %esi,%eax
  400fd6:   89 c1                   mov    %eax,%ecx
  400fd8:   c1 e9 1f                shr    $0x1f,%ecx
  400fdb:   01 c8                   add    %ecx,%eax
  400fdd:   d1 f8                   sar    $1,%eax
  400fdf:   8d 0c 30                lea    (%rax,%rsi,1),%ecx
  400fe2:   39 f9                   cmp    %edi,%ecx
  400fe4:   7e 0c                   jle    400ff2 <func4+0x24>
  400fe6:   8d 51 ff                lea    -0x1(%rcx),%edx
  400fe9:   e8 e0 ff ff ff          call   400fce <func4>
  400fee:   01 c0                   add    %eax,%eax
  400ff0:   eb 15                   jmp    401007 <func4+0x39>
  400ff2:   b8 00 00 00 00          mov    $0x0,%eax
  400ff7:   39 f9                   cmp    %edi,%ecx
  400ff9:   7d 0c                   jge    401007 <func4+0x39>
  400ffb:   8d 71 01                lea    0x1(%rcx),%esi
  400ffe:   e8 cb ff ff ff          call   400fce <func4>
  401003:   8d 44 00 01             lea    0x1(%rax,%rax,1),%eax
  401007:   48 83 c4 08             add    $0x8,%rsp
  40100b:   c3                      ret

000000000040100c <phase_4>:
  40100c:   48 83 ec 18             sub    $0x18,%rsp
  401010:   48 8d 4c 24 0c          lea    0xc(%rsp),%rcx
  401015:   48 8d 54 24 08          lea    0x8(%rsp),%rdx
  40101a:   be cf 25 40 00          mov    $0x4025cf,%esi
  40101f:   b8 00 00 00 00          mov    $0x0,%eax
  401024:   e8 c7 fb ff ff          call   400bf0 <__isoc99_sscanf@plt>
  401029:   83 f8 02                cmp    $0x2,%eax
  40102c:   75 07                   jne    401035 <phase_4+0x29>
  40102e:   83 7c 24 08 0e          cmpl   $0xe,0x8(%rsp)
  401033:   76 05                   jbe    40103a <phase_4+0x2e>
  401035:   e8 00 04 00 00          call   40143a <explode_bomb>
  40103a:   ba 0e 00 00 00          mov    $0xe,%edx
  40103f:   be 00 00 00 00          mov    $0x0,%esi
  401044:   8b 7c 24 08             mov    0x8(%rsp),%edi
  401048:   e8 81 ff ff ff          call   400fce <func4>
  40104d:   85 c0                   test   %eax,%eax
  40104f:   75 07                   jne    401058 <phase_4+0x4c>
  401051:   83 7c 24 0c 00          cmpl   $0x0,0xc(%rsp)
  401056:   74 05                   je     40105d <phase_4+0x51>
  401058:   e8 dd 03 00 00          call   40143a <explode_bomb>
  40105d:   48 83 c4 18             add    $0x18,%rsp
  401061:   c3                      ret
```

### 分析过程

使用和上题一样的方法，发现：

```nasm
__GI___isoc99_sscanf (s=0x603870 <input_strings+240> "3", format=0x4025cf "%d %d") at ./stdio-common/isoc99_sscanf.c:24
```

传入的是两个整型。那我们先输入`22 23`。再次运行，观察如下代码：

```bash
0x0000000000401029 <+29>:    cmp    $0x2,%eax
0x000000000040102c <+32>:    jne    0x401035 <phase_4+41>
```

这里`%eax`一定要和`0x2`相等，否则就爆炸了。因此查看一下`%eax`的值：

```bash
(gdb) p $eax
$1 = 2
```

再次大胆猜测这里`%eax`就是传入的参数数量。当输入一个参数时，`%eax`为`1`，猜想大概率是正确的。再往下看：

```nasm
0x000000000040102e <+34>:    cmpl   $0xe,0x8(%rsp)
0x0000000000401033 <+39>:    jbe    0x40103a <phase_4+46>
0x0000000000401035 <+41>:    call   0x40143a <explode_bomb>
```

我们要保证`0x8(%rsp)`的值要小于等于`0xe`。因此查看一下`0x8(%rsp)`。

```nasm
(gdb) p ($rsp + 8)
$1 = (void *) 0x7fffffffd698
(gdb) p *(int *)($rsp + 8)
$2 = 22
```

就是我们的第一个参数，那么我们把第一个参数改小一点，改成`10`。然后再往下，不妨把断点打在`*0x000000000040103a`：

```nasm
0x0000000000401048 <+60>:    call   0x400fce <func4>
```

要进入`func4`了，我们`stepi`一下，往里看：

```nasm
0x0000000000400fe2 <+20>:    cmp    %edi,%ecx
0x0000000000400fe4 <+22>:    jle    0x400ff2 <func4+36>
```

`%ecx`要小于等于`%edi`，查看一下这两个：

```bash
(gdb) p $edi
$3 = 10
(gdb) p $ecx
$4 = 7
```

`%edi`我们显然知道是第一个参数，那`%ecx`呢？但是此时是满足条件的，我们接着往下走。走多一点吧，中间并没有引爆炸弹的代码，我们放心往下走。这个时候我们跳回到了`phase_4`，看看这几行：

```nasm
0x000000000040104d <+65>:    test   %eax,%eax
0x000000000040104f <+67>:    jne    0x401058 <phase_4+76>
```

这个时候`%eax`一定要是0。查看一下`%eax`：

```nasm
(gdb) p $eax
$5 = 5
```

很明显不符合我们的要求。这个时候我们得去看看我们这之前的代码了。可不可以不管`func4`，从前面发现一点规律呢？比如会不会有哪个寄存器的值就是5？看这几行：

```nasm
40103a: ba 0e 00 00 00          mov    $0xe,%edx
40103f: be 00 00 00 00          mov    $0x0,%esi
401044: 8b 7c 24 08             mov    0x8(%rsp),%edi
```

查看一下这几个寄存器的值：`%edx`肯定是`0xe`，`%esi`肯定是`0`。

```nasm
(gdb) p $edi
$1 = 10
```

说明把第一个参数给了`0x8(%rsp)`。不妨试一下把第二个参数改成`0`或者是`0xe`？把断点打在`*0x40104d`。先试试`0`，失败了。再试试`0xe`，也失败了。代码都是卡在之前一样的跳转的位置，说明和第二个参数没有关系。那么再把第一个参数改成`0`或者`e`？先试试`0 23`。这一次通过了原来的比较，查看一下`%eax`：

```nasm
(gdb) p $eax
$1 = 0
```

嗯，确实符合预期了。接下来看：

```nasm
401051: 83 7c 24 0c 00          cmpl   $0x0,0xc(%rsp)
401056: 74 05                   je     40105d <phase_4+0x51>
```

查看一下`0xc(%rsp)`，确保其就是`0`，炸弹才不会爆炸。

```nasm
(gdb) x/d $rsp + 12
0x7fffffffd69c: 23
```

正好是我们第二个参数！我们只要把第二个参数改成`0`，炸弹就拆除了。

这种解法其实又有点投机取巧，因为我们根本没有分析`func4`的功能，只是有些盲目地给第一个或者第二个参数赋值为`0`或者`14`。

最终答案是：

**`0 0`**

### 运行截图

![phase_4.png](https://s2.loli.net/2025/04/13/BskvmIge45TuS1f.png)

---

## Phase 5

### 汇编代码

```nasm
0000000000401062 <phase_5>:
  401062:   53                      push   %rbx
  401063:   48 83 ec 20             sub    $0x20,%rsp
  401067:   48 89 fb                mov    %rdi,%rbx
  40106a:   64 48 8b 04 25 28 00    mov    %fs:0x28,%rax
  401071:   00 00 
  401073:   48 89 44 24 18          mov    %rax,0x18(%rsp)
  401078:   31 c0                   xor    %eax,%eax
  40107a:   e8 9c 02 00 00          call   40131b <string_length>
  40107f:   83 f8 06                cmp    $0x6,%eax
  401082:   74 4e                   je     4010d2 <phase_5+0x70>
  401084:   e8 b1 03 00 00          call   40143a <explode_bomb>
  401089:   eb 47                   jmp    4010d2 <phase_5+0x70>
  40108b:   0f b6 0c 03             movzbl (%rbx,%rax,1),%ecx
  40108f:   88 0c 24                mov    %cl,(%rsp)
  401092:   48 8b 14 24             mov    (%rsp),%rdx
  401096:   83 e2 0f                and    $0xf,%edx
  401099:   0f b6 92 b0 24 40 00    movzbl 0x4024b0(%rdx),%edx
  4010a0:   88 54 04 10             mov    %dl,0x10(%rsp,%rax,1)
  4010a4:   48 83 c0 01             add    $0x1,%rax
  4010a8:   48 83 f8 06             cmp    $0x6,%rax
  4010ac:   75 dd                   jne    40108b <phase_5+0x29>
  4010ae:   c6 44 24 16 00          movb   $0x0,0x16(%rsp)
  4010b3:   be 5e 24 40 00          mov    $0x40245e,%esi
  4010b8:   48 8d 7c 24 10          lea    0x10(%rsp),%rdi
  4010bd:   e8 76 02 00 00          call   401338 <strings_not_equal>
  4010c2:   85 c0                   test   %eax,%eax
  4010c4:   74 13                   je     4010d9 <phase_5+0x77>
  4010c6:   e8 6f 03 00 00          call   40143a <explode_bomb>
  4010cb:   0f 1f 44 00 00          nopl   0x0(%rax,%rax,1)
  4010d0:   eb 07                   jmp    4010d9 <phase_5+0x77>
  4010d2:   b8 00 00 00 00          mov    $0x0,%eax
  4010d7:   eb b2                   jmp    40108b <phase_5+0x29>
  4010d9:   48 8b 44 24 18          mov    0x18(%rsp),%rax
  4010de:   64 48 33 04 25 28 00    xor    %fs:0x28,%rax
  4010e5:   00 00 
  4010e7:   74 05                   je     4010ee <phase_5+0x8c>
  4010e9:   e8 42 fa ff ff          call   400b30 <__stack_chk_fail@plt>
  4010ee:   48 83 c4 20             add    $0x20,%rsp
  4010f2:   5b                      pop    %rbx
  4010f3:   c3                      ret
```

### 分析过程

先在`line 101`打断点，进入程序，随便输入个`2asdsads`。看到：

```nasm
0x000000000040107a <+24>:    call   0x40131b <string_length>
0x000000000040107f <+29>:    cmp    $0x6,%eax
0x0000000000401082 <+32>:    je     0x4010d2 <phase_5+112>
```

这里说明`%eax`要和`6`相等，我们看看`%eax`：

```bash
(gdb) p $eax
$1 = 8
```

这刚好是`2asdsads`的长度，可以推测函数`string_length`修改了`%eax`，我们需要确保输入的字符串长度是`6`。于是改成输入`asdfgh`。来到：

```nasm
0x00000000004010a8 <+70>:    cmp    $0x6,%rax
0x00000000004010ac <+74>:    jne    0x40108b <phase_5+41>
```

如果`%rax`的值不是`6`，就会跳回`phase_5+41`。我们看看`%rax`。

```bash
(gdb) p $rax
$1 = 1
```

的确不是。那让程序继续运行。运行了很久很久之后，我们再检查一下`%rax`：

```bash
(gdb) p $rax
$2 = 1
```

来到如下代码区域：

```nasm
0x00000000004010bd <+91>:    call   0x401338 <strings_not_equal>
0x00000000004010c2 <+96>:    test   %eax,%eax
0x00000000004010c4 <+98>:    je     0x4010d9 <phase_5+119>
```

说明`%eax`必须是`0`，否则爆炸。因此查看一下：

```bash
(gdb) p $eax
$3 = 1
```

会爆炸，我们要找寻一下原因。我们是否应该回去想想`strings_not_equal`是拿来干什么的？重新运行一下试试：

```nasm
0x00000000004010ae <+76>:    movb   $0x0,0x16(%rsp)
0x00000000004010b3 <+81>:    mov    $0x40245e,%esi
0x00000000004010b8 <+86>:    lea    0x10(%rsp),%rdi
0x00000000004010bd <+91>:    call   0x401338 <strings_not_equal>
```

运行到`<+91>`了，我们很有必要查看一下`0x10(%rsp)`和`0x40245e`都有些什么。

```bash
(gdb) x/s 0x40245e
0x40245e:       "flyers"
(gdb) x/s $rsp + 16
0x7fffffffd9b0: "auirsn"
```

这两个字符串都是长度为`6`，我们是不是该把我们的输入改成它们呢？先试试`"flyers"`。断点可以打在`*0x4010b8`了。然后查看一下`%eax`是多少：

```bash
(gdb) p $eax
$1 = 1
```

那我们试试`"auirsn"`：

```bash
(gdb) p $eax
$3 = 1
```

还是不行。那我们就很有必要对再往前的代码进行分析了。把断点打在`*0x4010d2`，然后分析如下代码：

```nasm
401089: eb 47                 jmp    4010d2 <phase_5+0x70>
40108b: 0f b6 0c 03           movzbl (%rbx,%rax,1),%ecx
40108f: 88 0c 24              mov    %cl,(%rsp)
401092: 48 8b 14 24           mov    (%rsp),%rdx
401096: 83 e2 0f              and    $0xf,%edx
401099: 0f b6 92 b0 24 40 00  movzbl 0x4024b0(%rdx),%edx
4010a0: 88 54 04 10           mov    %dl,0x10(%rsp,%rax,1)
4010a4: 48 83 c0 01           add    $0x1,%rax
4010a8: 48 83 f8 06           cmp    $0x6,%rax
4010ac: 75 dd                 jne    40108b <phase_5+0x29>
4010ae: c6 44 24 16 00        movb   $0x0,0x16(%rsp)
4010b3: be 5e 24 40 00        mov    $0x40245e,%esi
4010b8: 48 8d 7c 24 10        lea    0x10(%rsp),%rdi
4010bd: e8 76 02 00 00        call   401338 <strings_not_equal>
4010c2: 85 c0                 test   %eax,%eax
4010c4: 74 13                 je     4010d9 <phase_5+0x77>
4010c6: e8 6f 03 00 00        call   40143a <explode_bomb>

4010d2: b8 00 00 00 00        mov    $0x0,%eax
4010d7: eb b2                 jmp    40108b <phase_5+0x29>
```

观察可以知道，程序刚开始跳到了`4010d2`，之后又跳回上面的代码。我们有必要逐行分析一下。在`40108b`执行前查看一下`%rbx` `%rax` `%ecx`。

```bash
(gdb) p $ecx
$1 = 6
(gdb) x/s ($rax + $rbx)
0x6038c0 <input_strings+320>:   "asdfgh"
(gdb) x $rbx
0x6038c0 <input_strings+320>:   97 'a'
(gdb) x $rbx + 1
0x6038c1 <input_strings+321>:   115 's'
(gdb) x $rbx + 2
0x6038c2 <input_strings+322>:   100 'd'
(gdb) x $rbx + 3
0x6038c3 <input_strings+323>:   102 'f'
(gdb) x $rbx + 4
0x6038c4 <input_strings+324>:   103 'g'
(gdb) x $rbx + 5
0x6038c5 <input_strings+325>:   104 'h'
(gdb) x $rbx + 6
0x6038c6 <input_strings+326>:   0 '\000'
```

寄存器含义不言而喻，`$rbx`指向了输入的字符串的最低一位。执行后：

```bash
(gdb) p $ecx
$4 = 97
```

`97`是什么？显然就是`a`的ASCII码，此时`%ecx`存放的是输入字符串的首位。其实也很好理解，我们把`(%rbx,%rax,1)`传给`%eax`。查看一下`%cl`（其实就是`%ecx`）：

```bash
(gdb) p $cl
$5 = 97
```

再看到如下代码：

```nasm
0x000000000040108b <+41>:    movzbl (%rbx,%rax,1),%ecx
0x000000000040108f <+45>:    mov    %cl,(%rsp)
0x0000000000401092 <+48>:    mov    (%rsp),%rdx
0x0000000000401096 <+52>:    and    $0xf,%edx
```

这里还是`a`的ASCII码。我们发现这个值最终传给了`%rdx`，并且让其与`1111`作按位与运算。由于`'a'`的二进制表示是`01100001`，按位与的结果应该就是`1`。我们验证一下：

```bash
(gdb) p $rdx
$12 = 1
```

果然如此！因此这里的作用就是取`%rdx`的低4位。我们再往下看：

```nasm
0x0000000000401099 <+55>:    movzbl 0x4024b0(%rdx),%edx
0x00000000004010a0 <+62>:    mov    %dl,0x10(%rsp,%rax,1)
0x00000000004010a4 <+66>:    add    $0x1,%rax
0x00000000004010a8 <+70>:    cmp    $0x6,%rax
0x00000000004010ac <+74>:    jne    0x40108b <phase_5+41>
```

首先好奇`0x4024b0(%rdx)`是什么。我们知道`%dl`实际上是`%edx`的低八位寄存器，所以此时里面的值应该和进行按位与运算后的`%edx`一样，它给到了`0x10(%rsp,%rax,1)`。

```nasm
(gdb) x 0x4024b0
0x4024b0 <array.3449>:  "maduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"
(gdb) x 0x4024b0 + $rdx
0x4024b1 <array.3449+1>:        "aduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"
```

为什么要把`*0x4024b0`加上`%rdx`的值呢？我们知道`%rdx`实际上是我们刚刚输入的第一个字符的低4位，也就是要从`"maduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"`这个字符串的第（输入的字符串的第一个字符的低4位）位，也就是第一位开始取，因此`0x4024b0 + $rdx`结果符合我们预期。我们可以推断，我们输入的字符串有取字符的作用。我们把取出来的字符存到`0x10(%rsp,%rax,1)`当中，这是一个全新的内存区域。能取到刚好是一个字符的原因是，`%dl`是`%edx`的低八位寄存器，刚好取到一个`char`类型的长度。这里的`$rax`有特定的用途，我们知道在之前：

```nasm
0x00000000004010d2 <+112>:   mov    $0x0,%eax
```

这里`%eax`被置为0，因此可以知道如下的汇编代码实现了特定的功能，保证执行了六次：

```nasm
0x00000000004010a4 <+66>:    add    $0x1,%rax
0x00000000004010a8 <+70>:    cmp    $0x6,%rax
0x00000000004010ac <+74>:    jne    0x40108b <phase_5+41>
```

伪代码如下：

```c
for (int i = 0; i < 6; i++) {
    goto: 0x40108b;
}
```

第二次循环，我们也可以继续观察。

```bash
(gdb) p $rax
$37 = 1
(gdb) x $rbx
0x6038c0 <input_strings+320>:   "asdfgh"
```

之后，多亏了八位寄存器`%cl`，通过`%rax`的一次偏置，我们可以把字符串的第二个元素存入`%rdx`当中。

```nasm
(gdb) p/c $dl
$48 = 115 's'
```

`'s'`的低四位是`0011`，我们推测这一次能取到神秘字符串的第`3`位。我们试一试：

```bash
(gdb) p/c $dl
$50 = 117 'u'
```

没错！`"maduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"`第三位就是`u`！那我们继续看看运行到第六次之后，新开辟的内存空间`0x10(%rsp)`里面是不是存放着我们取出来的字符。

```bash
(gdb) x/s 16 + $rsp
0x7fffffffd9b0: "auirsn"
```

那么我们现在很明白我们输入的字符串会被怎么处理了：**取出输入的字符串的每个字母的ASCII码的第八位，作为神秘字符串的索引，分别取出字符，存入一个新的内存区域当中**。

接下来我们看看新的字符串要有什么用。

```nasm
0x00000000004010b3 <+81>:    mov    $0x40245e,%esi
0x00000000004010b8 <+86>:    lea    0x10(%rsp),%rdi
0x00000000004010bd <+91>:    call   0x401338 <strings_not_equal>
0x00000000004010c2 <+96>:    test   %eax,%eax
0x00000000004010c4 <+98>:    je     0x4010d9 <phase_5+119>
```

这里调用了一个`strings_not_equal`函数，很可能就是把我们新的字符串和某个字符串比较是否相等，如果相等就返回`0`，不相等返回`1`。我们在最开始已经分析过了，如果是`1`就爆炸了，因此两个字符串必须是相等的。我们知道：

```bash
(gdb) x/s 0x40245e
0x40245e:       "flyers"
(gdb) x/s $rsp + 16
0x7fffffffd9b0: "auirsn"
```

那我们只要能取出`"flyers"`就能拆除炸弹了吧！我们来试试。对于`"maduiersnfotvbylSo you think you can stop the bomb with ctrl-c, do you?"`，我们要取出第`9 15 14 5 6 7` (`1001 1111 1110 0101 0110 0111`)位即可。通过查询ASCII码表，我们可以试试输入字符串`"yONEFG"`，炸弹确实拆除了。最终答案就是：

**`"yONEFG"`**

### 运行截图

![phase_5.png](https://s2.loli.net/2025/04/14/AwjyF7LZvIgHQ43.png)

---

## Phase 6

### 汇编代码

```nasm
00000000004010f4 <phase_6>:
  4010f4: 41 56                 push   %r14
  4010f6: 41 55                 push   %r13
  4010f8: 41 54                 push   %r12
  4010fa: 55                    push   %rbp
  4010fb: 53                    push   %rbx
  4010fc: 48 83 ec 50           sub    $0x50,%rsp
  401100: 49 89 e5              mov    %rsp,%r13
  401103: 48 89 e6              mov    %rsp,%rsi
  401106: e8 51 03 00 00        call   40145c <read_six_numbers>
  40110b: 49 89 e6              mov    %rsp,%r14
  40110e: 41 bc 00 00 00 00     mov    $0x0,%r12d
  401114: 4c 89 ed              mov    %r13,%rbp
  401117: 41 8b 45 00           mov    0x0(%r13),%eax
  40111b: 83 e8 01              sub    $0x1,%eax
  40111e: 83 f8 05              cmp    $0x5,%eax
  401121: 76 05                 jbe    401128 <phase_6+0x34>
  401123: e8 12 03 00 00        call   40143a <explode_bomb>
  401128: 41 83 c4 01           add    $0x1,%r12d
  40112c: 41 83 fc 06           cmp    $0x6,%r12d
  401130: 74 21                 je     401153 <phase_6+0x5f>
  401132: 44 89 e3              mov    %r12d,%ebx
  401135: 48 63 c3              movslq %ebx,%rax
  401138: 8b 04 84              mov    (%rsp,%rax,4),%eax
  40113b: 39 45 00              cmp    %eax,0x0(%rbp)
  40113e: 75 05                 jne    401145 <phase_6+0x51>
  401140: e8 f5 02 00 00        call   40143a <explode_bomb>
  401145: 83 c3 01              add    $0x1,%ebx
  401148: 83 fb 05              cmp    $0x5,%ebx
  40114b: 7e e8                 jle    401135 <phase_6+0x41>
  40114d: 49 83 c5 04           add    $0x4,%r13
  401151: eb c1                 jmp    401114 <phase_6+0x20>
  401153: 48 8d 74 24 18        lea    0x18(%rsp),%rsi
  401158: 4c 89 f0              mov    %r14,%rax
  40115b: b9 07 00 00 00        mov    $0x7,%ecx
  401160: 89 ca                 mov    %ecx,%edx
  401162: 2b 10                 sub    (%rax),%edx
  401164: 89 10                 mov    %edx,(%rax)
  401166: 48 83 c0 04           add    $0x4,%rax
  40116a: 48 39 f0              cmp    %rsi,%rax
  40116d: 75 f1                 jne    401160 <phase_6+0x6c>
  40116f: be 00 00 00 00        mov    $0x0,%esi
  401174: eb 21                 jmp    401197 <phase_6+0xa3>
  401176: 48 8b 52 08           mov    0x8(%rdx),%rdx
  40117a: 83 c0 01              add    $0x1,%eax
  40117d: 39 c8                 cmp    %ecx,%eax
  40117f: 75 f5                 jne    401176 <phase_6+0x82>
  401181: eb 05                 jmp    401188 <phase_6+0x94>
  401183: ba d0 32 60 00        mov    $0x6032d0,%edx
  401188: 48 89 54 74 20        mov    %rdx,0x20(%rsp,%rsi,2)
  40118d: 48 83 c6 04           add    $0x4,%rsi
  401191: 48 83 fe 18           cmp    $0x18,%rsi
  401195: 74 14                 je     4011ab <phase_6+0xb7>
  401197: 8b 0c 34              mov    (%rsp,%rsi,1),%ecx
  40119a: 83 f9 01              cmp    $0x1,%ecx
  40119d: 7e e4                 jle    401183 <phase_6+0x8f>
  40119f: b8 01 00 00 00        mov    $0x1,%eax
  4011a4: ba d0 32 60 00        mov    $0x6032d0,%edx
  4011a9: eb cb                 jmp    401176 <phase_6+0x82>
  4011ab: 48 8b 5c 24 20        mov    0x20(%rsp),%rbx
  4011b0: 48 8d 44 24 28        lea    0x28(%rsp),%rax
  4011b5: 48 8d 74 24 50        lea    0x50(%rsp),%rsi
  4011ba: 48 89 d9              mov    %rbx,%rcx
  4011bd: 48 8b 10              mov    (%rax),%rdx
  4011c0: 48 89 51 08           mov    %rdx,0x8(%rcx)
  4011c4: 48 83 c0 08           add    $0x8,%rax
  4011c8: 48 39 f0              cmp    %rsi,%rax
  4011cb: 74 05                 je     4011d2 <phase_6+0xde>
  4011cd: 48 89 d1              mov    %rdx,%rcx
  4011d0: eb eb                 jmp    4011bd <phase_6+0xc9>
  4011d2: 48 c7 42 08 00 00 00  movq   $0x0,0x8(%rdx)
  4011d9: 00 
  4011da: bd 05 00 00 00        mov    $0x5,%ebp
  4011df: 48 8b 43 08           mov    0x8(%rbx),%rax
  4011e3: 8b 00                 mov    (%rax),%eax
  4011e5: 39 03                 cmp    %eax,(%rbx)
  4011e7: 7d 05                 jge    4011ee <phase_6+0xfa>
  4011e9: e8 4c 02 00 00        call   40143a <explode_bomb>
  4011ee: 48 8b 5b 08           mov    0x8(%rbx),%rbx
  4011f2: 83 ed 01              sub    $0x1,%ebp
  4011f5: 75 e8                 jne    4011df <phase_6+0xeb>
  4011f7: 48 83 c4 50           add    $0x50,%rsp
  4011fb: 5b                    pop    %rbx
  4011fc: 5d                    pop    %rbp
  4011fd: 41 5c                 pop    %r12
  4011ff: 41 5d                 pop    %r13
  401201: 41 5e                 pop    %r14
  401203: c3                    ret
```

### 分析过程

代码很长，如果一条一条分析会很费时费力。我们尝试浏览一下代码，捕获一点关键信息。有：

```nasm
401106: e8 51 03 00 00        call   40145c <read_six_numbers>
```

说明我们这里要读入6个数字，那么这个数字的特征尚不明确。我们先试着输入`1 3 5 7 9 11`，然后在`*0x40111b`处打断点。因为我们观察到，下面的代码似乎是在验证数字的合法性，否则炸弹就会爆炸。

```nasm
0x000000000040110b <+23>:    mov    %rsp,%r14
0x000000000040110e <+26>:    mov    $0x0,%r12d
0x0000000000401114 <+32>:    mov    %r13,%rbp
0x0000000000401117 <+35>:    mov    0x0(%r13),%eax
0x000000000040111b <+39>:    sub    $0x1,%eax
0x000000000040111e <+42>:    cmp    $0x5,%eax
0x0000000000401121 <+45>:    jbe    0x401128 <phase_6+52>
0x0000000000401123 <+47>:    call   0x40143a <explode_bomb>
```

我们在每次运行到：

```nasm
0x0000000000401121 <+45>:    jbe    0x401128 <phase_6+52>
```

查看一下`%eax`的值，揣测一下有什么规律，这样就不必逐行分析代码了。代码会跳到后面的部分，我们等待运行回到上面的时候记录。接下来我将记录六次的`%eax`的值。

```bash
(gdb) p $eax
$1 = 0

(gdb) p $r12d
$2 = 2

(gdb) p $r12d
$3 = 4

(gdb) p $r12d
$4 = 6
```

运行了四次炸弹就爆炸了。这意味着我们第四个值开始是有问题的。我们可以看到`%eax`实际上就是我们每次的`input - 1`，是否意味着`input - 1`一定要小于6？因为：

```nasm
0x0000000000401117 <+35>:    mov    0x0(%r13),%eax
0x000000000040111b <+39>:    sub    $0x1,%eax
0x000000000040111e <+42>:    cmp    $0x5,%eax
0x0000000000401121 <+45>:    jbe    0x401128 <phase_6+52>
0x0000000000401123 <+47>:    call   0x40143a <explode_bomb>
```

这里`%eax - 1`要小于等于`5`，否则就会爆炸，因此我们就知道`input - 1 <= 5`，则我们输入一定要小于等于`6`！

我们试一下`1 2 3 4 5 6`，然后这一个炸弹一定是避开了，断点打在`*0x40113b`，每当运行到：

```nasm
40113b: 39 45 00              cmp    %eax,0x0(%rbp)
40113e: 75 05                 jne    401145 <phase_6+0x51>
```

我们就查看一次`%eax`和`%rbp`：

```bash
(gdb) p $eax
$1 = 2
(gdb) x/6 $rbp
0x7fffffffd950: 1       2       3       4
0x7fffffffd960: 5       6
```

`$rbp`正好存放着我们输入的数字，我们在第二次循环再查看一下：

```bash
(gdb) p $eax
$12 = 3
(gdb) x/6 $rbp
0x7fffffffd950: 1       2       3       4
0x7fffffffd960: 5       6
```

显而易见，`$eax`是`input + 1`。我们看看第六次循环：

```bash
(gdb) p $eax
$13 = 3
(gdb) x/6 $rbp
0x7fffffffd954: 2       3       4       5
0x7fffffffd964: 6       -134230016
```

好像超过我们预期的六个数字的界限了？我们看看第五次循环：

```bash
(gdb) p $eax
$1 = 6
(gdb) x/6 $rbp
0x7fffffffd950: 1       2       3       4
0x7fffffffd960: 5       6
```

说明运行到第六次的时候，我们的`input`由于自增了，超出了`6`的界限，我们看看第六次跳转的时候这一行：

```nasm
0x0000000000401145 <+81>:    add    $0x1,%ebx
0x0000000000401148 <+84>:    cmp    $0x5,%ebx
0x000000000040114b <+87>:    jle    0x401135 <phase_6+65>
```

查看一下`%ebx`的值：

```bash
(gdb) p $ebx
$2 = 6
```

这个时候刚好到了`6`，刚好会跳转了。写下来达到断点的次数（包括最开始的一次）为`5 4 3 2 1`。

我们看到奇怪的一行：

```nasm
0x0000000000401183 <+143>:   mov    $0x6032d0,%edx
```

这里访问一下`0x6032d0`：

```nasm
(gdb) x 0x6032d0
0x6032d0 <node1>:       0x0000014c
```

我们发现了一个节点？根据节点的特征，节点应该有值和下一个结点的内存地址，我们查看大一点的内容：

```nasm
(gdb) x/100 0x6032d0
0x6032d0 <node1>:       0x0000014c      0x00000001      0x006032e0      0x00000000
0x6032e0 <node2>:       0x000000a8      0x00000002      0x006032d0      0x00000000
0x6032f0 <node3>:       0x0000039c      0x00000003      0x006032e0      0x00000000
0x603300 <node4>:       0x000002b3      0x00000004      0x006032f0      0x00000000
0x603310 <node5>:       0x000001dd      0x00000005      0x00603300      0x00000000
0x603320 <node6>:       0x000001bb      0x00000006      0x00603310      0x00000000
COMMENT ; 以下省略
```

可以看到，第二个值是我们输入的内容，第三个值就是下一个结点的内存。我们可以写如下的代码：

```c
typedef ListNode {
    int rootVal;
    int val;
    struct ListNode *next;
} ListNode;
```

这个结构有两个值和一个指针，其中这两个值一定有某种关系。最最最朴素的一种想法：会不会是**排序**呢？是从大到小还是从小到大呢？

那我们来尝试一下：从小到大应该输入的是`2 1 6 5 4 3`，从大到小是`3 4 5 6 1 2`。结果都不正确。

那会不会是排序的顺序进行了某种处理呢？我们已经限定了输入的数据必须小于等于`6`，按照排序的顺序应该输入的数值是`1 - 6`，那么加密后的数据也是`1 - 6`。这怎么做到的呢？比如加上一个数字模`7`？或者被`7`减去？那么我们肯定要找找汇编代码有没有数字`7`了，通过搜索果然发现：

```nasm
40115b: b9 07 00 00 00        mov    $0x7,%ecx
401160: 89 ca                 mov    %ecx,%edx
401162: 2b 10                 sub    (%rax),%edx
```

这个代码比较简单，我们只要查看一下运行到这里的时候`%rax`是谁就可以了，相信这个寄存器一定与我们的输入有关。我们在`*0x401162`打一个断点，还是输入`1 2 3 4 5 6`，每次经过的时候记录一下值。结果如下：

```bash
(gdb) x $rax
0x7fffffffd950: 0x00000001

(gdb) x $rax
0x7fffffffd954: 0x00000002

(gdb) x $rax
0x7fffffffd958: 0x00000003

(gdb) x $rax
0x7fffffffd95c: 0x00000004

(gdb) x $rax
0x7fffffffd960: 0x00000005

(gdb) x $rax
0x7fffffffd964: 0x00000006
```

说明`($rax)`就是我们的输入。由上面的代码分析，我们知道最终`%edx`的值是`7 - input`。我们处理一下我们刚刚的猜测：从小到大应该输入的是`5 6 1 2 3 4`，从大到小是`4 3 2 1 6 5`。最终答案就是：

**`4 3 2 1 6 5`**

### 运行截图

![phase_6.png](https://s2.loli.net/2025/04/14/QsUo7mROE1VBiKY.png)

---

## Secret Phase

### 汇编代码

```nasm
0000000000401204 <fun7>:
  401204: 48 83 ec 08           sub    $0x8,%rsp
  401208: 48 85 ff              test   %rdi,%rdi
  40120b: 74 2b                 je     401238 <fun7+0x34>
  40120d: 8b 17                 mov    (%rdi),%edx
  40120f: 39 f2                 cmp    %esi,%edx
  401211: 7e 0d                 jle    401220 <fun7+0x1c>
  401213: 48 8b 7f 08           mov    0x8(%rdi),%rdi
  401217: e8 e8 ff ff ff        call   401204 <fun7>
  40121c: 01 c0                 add    %eax,%eax
  40121e: eb 1d                 jmp    40123d <fun7+0x39>
  401220: b8 00 00 00 00        mov    $0x0,%eax
  401225: 39 f2                 cmp    %esi,%edx
  401227: 74 14                 je     40123d <fun7+0x39>
  401229: 48 8b 7f 10           mov    0x10(%rdi),%rdi
  40122d: e8 d2 ff ff ff        call   401204 <fun7>
  401232: 8d 44 00 01           lea    0x1(%rax,%rax,1),%eax
  401236: eb 05                 jmp    40123d <fun7+0x39>
  401238: b8 ff ff ff ff        mov    $0xffffffff,%eax
  40123d: 48 83 c4 08           add    $0x8,%rsp
  401241: c3                    ret

0000000000401242 <secret_phase>:
  401242: 53                    push   %rbx
  401243: e8 56 02 00 00        call   40149e <read_line>
  401248: ba 0a 00 00 00        mov    $0xa,%edx
  40124d: be 00 00 00 00        mov    $0x0,%esi
  401252: 48 89 c7              mov    %rax,%rdi
  401255: e8 76 f9 ff ff        call   400bd0 <strtol@plt>
  40125a: 48 89 c3              mov    %rax,%rbx
  40125d: 8d 40 ff              lea    -0x1(%rax),%eax
  401260: 3d e8 03 00 00        cmp    $0x3e8,%eax
  401265: 76 05                 jbe    40126c <secret_phase+0x2a>
  401267: e8 ce 01 00 00        call   40143a <explode_bomb>
  40126c: 89 de                 mov    %ebx,%esi
  40126e: bf f0 30 60 00        mov    $0x6030f0,%edi
  401273: e8 8c ff ff ff        call   401204 <fun7>
  401278: 83 f8 02              cmp    $0x2,%eax
  40127b: 74 05                 je     401282 <secret_phase+0x40>
  40127d: e8 b8 01 00 00        call   40143a <explode_bomb>
  401282: bf 38 24 40 00        mov    $0x402438,%edi
  401287: e8 84 f8 ff ff        call   400b10 <puts@plt>
  40128c: e8 33 03 00 00        call   4015c4 <phase_defused>
  401291: 5b                    pop    %rbx
  401292: c3                    ret
  401293: 90                    nop
  401294: 90                    nop
  401295: 90                    nop
  401296: 90                    nop
  401297: 90                    nop
  401298: 90                    nop
  401299: 90                    nop
  40129a: 90                    nop
  40129b: 90                    nop
  40129c: 90                    nop
  40129d: 90                    nop
  40129e: 90                    nop
  40129f: 90                    nop

00000000004015c4 <phase_defused>:
  4015c4: 48 83 ec 78           sub    $0x78,%rsp
  4015c8: 64 48 8b 04 25 28 00  mov    %fs:0x28,%rax
  4015cf: 00 00 
  4015d1: 48 89 44 24 68        mov    %rax,0x68(%rsp)
  4015d6: 31 c0                 xor    %eax,%eax
  4015d8: 83 3d 81 21 20 00 06  cmpl   $0x6,0x202181(%rip)        # 603760 <num_input_strings>
  4015df: 75 5e                 jne    40163f <phase_defused+0x7b>
  4015e1: 4c 8d 44 24 10        lea    0x10(%rsp),%r8
  4015e6: 48 8d 4c 24 0c        lea    0xc(%rsp),%rcx
  4015eb: 48 8d 54 24 08        lea    0x8(%rsp),%rdx
  4015f0: be 19 26 40 00        mov    $0x402619,%esi
  4015f5: bf 70 38 60 00        mov    $0x603870,%edi
  4015fa: e8 f1 f5 ff ff        call   400bf0 <__isoc99_sscanf@plt>
  4015ff: 83 f8 03              cmp    $0x3,%eax
  401602: 75 31                 jne    401635 <phase_defused+0x71>
  401604: be 22 26 40 00        mov    $0x402622,%esi
  401609: 48 8d 7c 24 10        lea    0x10(%rsp),%rdi
  40160e: e8 25 fd ff ff        call   401338 <strings_not_equal>
  401613: 85 c0                 test   %eax,%eax
  401615: 75 1e                 jne    401635 <phase_defused+0x71>
  401617: bf f8 24 40 00        mov    $0x4024f8,%edi
  40161c: e8 ef f4 ff ff        call   400b10 <puts@plt>
  401621: bf 20 25 40 00        mov    $0x402520,%edi
  401626: e8 e5 f4 ff ff        call   400b10 <puts@plt>
  40162b: b8 00 00 00 00        mov    $0x0,%eax
  401630: e8 0d fc ff ff        call   401242 <secret_phase>
  401635: bf 58 25 40 00        mov    $0x402558,%edi
  40163a: e8 d1 f4 ff ff        call   400b10 <puts@plt>
  40163f: 48 8b 44 24 68        mov    0x68(%rsp),%rax
  401644: 64 48 33 04 25 28 00  xor    %fs:0x28,%rax
  40164b: 00 00 
  40164d: 74 05                 je     401654 <phase_defused+0x90>
  40164f: e8 dc f4 ff ff        call   400b30 <__stack_chk_fail@plt>
  401654: 48 83 c4 78           add    $0x78,%rsp
  401658: c3                    ret
  401659: 90                    nop
  40165a: 90                    nop
  40165b: 90                    nop
  40165c: 90                    nop
  40165d: 90                    nop
  40165e: 90                    nop
  40165f: 90                    nop
```

### 分析过程

我们想要找到`secret_phase`什么时候被调用。通过搜索得到：

```nasm
00000000004015c4 <phase_defused>:
  COMMENT ; ...
  401630: e8 0d fc ff ff        call   401242 <secret_phase>
  COMMENT ; ...
```

在`phase_defused`中找到了我们函数的入口。我们看到：

```nasm
4015fa: e8 f1 f5 ff ff        call   400bf0 <__isoc99_sscanf@plt>
```

因此可以尝试看看`__isoc99_sscanf@plt`里面需要我们输入什么。我们给`phase_defused`和`*0x4015fa`各打一个断点，以便我们查看其内容。发现在第六个炸弹拆除后调用`phase_defused`时，出现了如下一行：

```bash
__GI___isoc99_sscanf (s=0x603870 <input_strings+240> "0 0", format=0x402619 "%d %d %s") at ./stdio-common/isoc99_sscanf.c:24
```

这里的`0 0`显然是我们在`phase_4`时输入的答案，此时这个函数允许我们输入两个整型和一个字符串，或许我们只要找到这个字符串就能找到入口了？观察到这一行：

```nasm
0x0000000000401604 <+64>:    mov    $0x402622,%esi
0x0000000000401609 <+69>:    lea    0x10(%rsp),%rdi
0x000000000040160e <+74>:    call   0x401338 <strings_not_equal>
0x0000000000401613 <+79>:    test   %eax,%eax
0x0000000000401615 <+81>:    jne    0x401635 <phase_defused+113>
```

我们不由地想到`strings_not_equal`函数的用法，如果我们在第三个参数输入了正确的字符串，就不会跳转到`0x0000000000401635 <+113>`，而是会进入`0x0000000000401630 <+108>`，进入隐藏炸弹。

```nasm
0x0000000000401630 <+108>:   call   0x401242 <secret_phase>
0x0000000000401635 <+113>:   mov    $0x402558,%edi
```

我们尝试获取`0x402622`所表示的字符串：

```bash
(gdb) x /s 0x402622
0x402622:       "DrEvil"
```

这时候我们修改一下我们的`ans.txt`，在第四个炸弹答案中加上`DrEvil`。果然显示了如下句子：

```bash
Curses, you've found the secret phase!
But finding it and solving it are quite different...
```

这样就进入了隐藏炸弹。来观察一下我们的`secret_phase`。先解决一下输入格式：

```nasm
401243: e8 56 02 00 00        call   40149e <read_line>
COMMENT ; ...
401255: e8 76 f9 ff ff        call   400bd0 <strtol@plt>
```

说明程序读入一个字符串并且转成`long int`。我们可以推测答案实际上就是一个整数。观察到：

```nasm
0x000000000040125a <+24>:    mov    %rax,%rbx
0x000000000040125d <+27>:    lea    -0x1(%rax),%eax
0x0000000000401260 <+30>:    cmp    $0x3e8,%eax
0x0000000000401265 <+35>:    jbe    0x40126c <secret_phase+42>
0x0000000000401267 <+37>:    call   0x40143a <explode_bomb>
```

我们要判断一下`%rax`一开始的值，在`*0x40125a`打断点，并且输入`23`：

```bash
(gdb) p $rbx
$1 = 23
(gdb) p $eax
$2 = 22
```

说明一开始`%rax`就存放我们输入的数字，然后自减一次，结果一定要小于等于`0x3e8`，说明我们输入的数一定要小于等于`1001`。再往下看：

```nasm
0x000000000040126c <+42>:    mov    %ebx,%esi
0x000000000040126e <+44>:    mov    $0x6030f0,%edi
0x0000000000401273 <+49>:    call   0x401204 <fun7>
0x0000000000401278 <+54>:    cmp    $0x2,%eax
0x000000000040127b <+57>:    je     0x401282 <secret_phase+64>
0x000000000040127d <+59>:    call   0x40143a <explode_bomb>
```

我们知道`%ebx == %rbx == 23`，就是我们输入的数值。查看一下`*0x6030f0`：

```bash
(gdb) p *0x6030f0
$7 = 36
```

看不出什么规律。那我们接着往下，等调用完`func7`后看看`%eax`的值：

```bash
(gdb) p $eax
$8 = -10
```

要让`%eax`等于`2`才不会爆炸。我们因此需要分析一下`func7`了。在`fun7`打断点，忽略栈空间分配，我们看到：

```nasm
0x0000000000401208 <+4>:     test   %rdi,%rdi
0x000000000040120b <+7>:     je     0x401238 <fun7+52>
0x000000000040120d <+9>:     mov    (%rdi),%edx
COMMENT ; ...
0x0000000000401238 <+52>:    mov    $0xffffffff,%eax
```

这里查看一下`%rdi`：

```bash
(gdb) p *$rdi
$2 = 36
```

刚好是我们上面那个不明所以的数字。如果`%rdi`为`0`，注意到`0x401238`，就会给返回值`%eax`赋值为`-1`。为什么有`-1`呢？大概率是这个函数不符合某个条件的时候会返回这个值。那么当`%rdi`为`0`就满足了这个条件。在看这个函数，我们可以发现其两次调用了自己本身，因此这是一个递归函数。我们观察一下什么时候会发生递归：

```nasm
0x0000000000401213 <+15>:    mov    0x8(%rdi),%rdi
0x0000000000401217 <+19>:    call   0x401204 <fun7>
COMMENT ; ...
0x0000000000401229 <+37>:    mov    0x10(%rdi),%rdi
0x000000000040122d <+41>:    call   0x401204 <fun7>
```

可以看出，每次递归都多开辟了`8`个内存空间，这也许是两个变量的意思。再往前看：

```nasm
0x000000000040120d <+9>:     mov    (%rdi),%edx
0x000000000040120f <+11>:    cmp    %esi,%edx
0x0000000000401211 <+13>:    jle    0x401220 <fun7+28>
```

`(%rdi)`就是之前不明所以的值。我们查看一下`%esi`：

```bash
(gdb) p $esi
$3 = 23
```

我们现在是在比较输入的值和这个不明所以的值！如果输入的值大于这个数，就会跳转到：

```nasm
0x0000000000401220 <+28>:    mov    $0x0,%eax
0x0000000000401225 <+33>:    cmp    %esi,%edx
0x0000000000401227 <+35>:    je     0x40123d <fun7+57>
```

满足条件就跳出当前递归了。这个不明所以的值确实很恼人，我们还记得它的地址是`0x6030f0`，我们可能需要用别的方法看看它是什么。我们不妨一次性看多一点：

```bash
(gdb) x/100 0x6030f0
0x6030f0 <n1>:  0x00000024      0x00000000      0x00603110      0x00000000
0x603100 <n1+16>:       0x00603130      0x00000000      0x00000000      0x00000000
0x603110 <n21>: 0x00000008      0x00000000      0x00603190      0x00000000
0x603120 <n21+16>:      0x00603150      0x00000000      0x00000000      0x00000000
0x603130 <n22>: 0x00000032      0x00000000      0x00603170      0x00000000
0x603140 <n22+16>:      0x006031b0      0x00000000      0x00000000      0x00000000
0x603150 <n32>: 0x00000016      0x00000000      0x00603270      0x00000000
0x603160 <n32+16>:      0x00603230      0x00000000      0x00000000      0x00000000
0x603170 <n33>: 0x0000002d      0x00000000      0x006031d0      0x00000000
0x603180 <n33+16>:      0x00603290      0x00000000      0x00000000      0x00000000
0x603190 <n31>: 0x00000006      0x00000000      0x006031f0      0x00000000
0x6031a0 <n31+16>:      0x00603250      0x00000000      0x00000000      0x00000000
0x6031b0 <n34>: 0x0000006b      0x00000000      0x00603210      0x00000000
0x6031c0 <n34+16>:      0x006032b0      0x00000000      0x00000000      0x00000000
0x6031d0 <n45>: 0x00000028      0x00000000      0x00000000      0x00000000
0x6031e0 <n45+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x6031f0 <n41>: 0x00000001      0x00000000      0x00000000      0x00000000
0x603200 <n41+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603210 <n47>: 0x00000063      0x00000000      0x00000000      0x00000000
0x603220 <n47+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603230 <n44>: 0x00000023      0x00000000      0x00000000      0x00000000
0x603240 <n44+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603250 <n42>: 0x00000007      0x00000000      0x00000000      0x00000000
0x603260 <n42+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603270 <n43>: 0x00000014      0x00000000      0x00000000      0x00000000
```

这看上去像是某种像链表的数据结构。但如果是链表，应该只会占据`16`个字节，而这里占据了`24`个字节。会不会可能是二叉树呢？我们来看看以下结构：

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;

    TreeNode(int val) : val(val), left(nullptr), right(nullptr) {}
};
```

因此增加`8`个内存单位就是为了存放一个节点指针。我们可以延续这个假设，然后来逐行分析汇编代码：

```nasm
0x0000000000401204 <+0>:     sub    $0x8,%rsp            
0x0000000000401208 <+4>:     test   %rdi,%rdi             # 检查是否是NULL，因为NULL == 0
0x000000000040120b <+7>:     je     0x401238 <fun7+52>    # 如果是NULL，跳到<+52>，返回-1
0x000000000040120d <+9>:     mov    (%rdi),%edx           
0x000000000040120f <+11>:    cmp    %esi,%edx             # 比较target和当前节点val
0x0000000000401211 <+13>:    jle    0x401220 <fun7+28>    # 如果target >= node->val，跳转到<+28>
0x0000000000401213 <+15>:    mov    0x8(%rdi),%rdi        
0x0000000000401217 <+19>:    call   0x401204 <fun7>       # 否则传入node->left
0x000000000040121c <+24>:    add    %eax,%eax             # node->val *= 2
0x000000000040121e <+26>:    jmp    0x40123d <fun7+57>    # 结束递归
0x0000000000401220 <+28>:    mov    $0x0,%eax             # 赋值为0
0x0000000000401225 <+33>:    cmp    %esi,%edx             # 比较当前节点和target
0x0000000000401227 <+35>:    je     0x40123d <fun7+57>    # 如果相等就返回，返回值就是上面设置的%eax，为0
0x0000000000401229 <+37>:    mov    0x10(%rdi),%rdi       
0x000000000040122d <+41>:    call   0x401204 <fun7>       # 传入node->right
0x0000000000401232 <+46>:    lea    0x1(%rax,%rax,1),%eax # node->val = node->val + 1
0x0000000000401236 <+50>:    jmp    0x40123d <fun7+57>    # 返回
0x0000000000401238 <+52>:    mov    $0xffffffff,%eax
0x000000000040123d <+57>:    add    $0x8,%rsp
0x0000000000401241 <+61>:    ret
```

可以写出伪代码：

```cpp
int func7(int target, TreeNode *node) {
    if (node == nullptr) {
        return -1;
    }

    if (target == node->val) {
        return 0;
    }

    if (target > node->val) {
        return func7(target, node->right) * 2 + 1;
    }

    return func7(target, node->left) * 2;
}
```

我们再生成一次二叉树的结构：

```bash
(gdb) x /200 0x6030f0
0x6030f0 <n1>:  0x00000024      0x00000000      0x00603110      0x00000000
0x603100 <n1+16>:       0x00603130      0x00000000      0x00000000      0x00000000
0x603110 <n21>: 0x00000008      0x00000000      0x00603190      0x00000000
0x603120 <n21+16>:      0x00603150      0x00000000      0x00000000      0x00000000
0x603130 <n22>: 0x00000032      0x00000000      0x00603170      0x00000000
0x603140 <n22+16>:      0x006031b0      0x00000000      0x00000000      0x00000000
0x603150 <n32>: 0x00000016      0x00000000      0x00603270      0x00000000
0x603160 <n32+16>:      0x00603230      0x00000000      0x00000000      0x00000000
0x603170 <n33>: 0x0000002d      0x00000000      0x006031d0      0x00000000
0x603180 <n33+16>:      0x00603290      0x00000000      0x00000000      0x00000000
0x603190 <n31>: 0x00000006      0x00000000      0x006031f0      0x00000000
0x6031a0 <n31+16>:      0x00603250      0x00000000      0x00000000      0x00000000
0x6031b0 <n34>: 0x0000006b      0x00000000      0x00603210      0x00000000
0x6031c0 <n34+16>:      0x006032b0      0x00000000      0x00000000      0x00000000
0x6031d0 <n45>: 0x00000028      0x00000000      0x00000000      0x00000000
0x6031e0 <n45+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x6031f0 <n41>: 0x00000001      0x00000000      0x00000000      0x00000000
0x603200 <n41+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603210 <n47>: 0x00000063      0x00000000      0x00000000      0x00000000
0x603220 <n47+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603230 <n44>: 0x00000023      0x00000000      0x00000000      0x00000000
0x603240 <n44+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603250 <n42>: 0x00000007      0x00000000      0x00000000      0x00000000
0x603260 <n42+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603270 <n43>: 0x00000014      0x00000000      0x00000000      0x00000000
0x603280 <n43+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x603290 <n46>: 0x0000002f      0x00000000      0x00000000      0x00000000
0x6032a0 <n46+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x6032b0 <n48>: 0x000003e9      0x00000000      0x00000000      0x00000000
0x6032c0 <n48+16>:      0x00000000      0x00000000      0x00000000      0x00000000
0x6032d0 <node1>:       0x0000014c      0x00000001      0x006032e0      0x00000000
0x6032e0 <node2>:       0x000000a8      0x00000002      0x00000000      0x00000000
0x6032f0 <node3>:       0x0000039c      0x00000003      0x00603300      0x00000000
0x603300 <node4>:       0x000002b3      0x00000004      0x00603310      0x00000000
0x603310 <node5>:       0x000001dd      0x00000005      0x00603320      0x00000000
0x603320 <node6>:       0x000001bb      0x00000006      0x006032d0      0x00000000
```

根据上面的代码，我们知道这是一个**二叉搜索树**。

```bash
           36
        /       \
       8         50
     /   \      /   \
    6    22    45   107
   / \   / \   / \   / \
  1   7 20 35 40 47 99 1001
```

一直不理解的`36`实际上就是头节点的值而已。那我们需要函数返回什么呢？看到`secret_phase`中这几行：

```nasm
0x0000000000401273 <+49>:    call   0x401204 <fun7>
0x0000000000401278 <+54>:    cmp    $0x2,%eax
0x000000000040127b <+57>:    je     0x401282 <secret_phase+64>
0x000000000040127d <+59>:    call   0x40143a <explode_bomb>
```

其中`%eax`是`func7`的返回值。我们需要返回值是`2`才能拆除炸弹。什么时候返回的才能是`2`呢？比如说一个函数返回了`0`，然后这个`0`是右子树来的，所以`0 * 2 + 1`得到`1`；当然也可以是从很多个左子树来的，毕竟`0 * 2`还是`0`。接着这个`1`是从左子树来的，因此`1 * 2`得到了`2`。反过来思考，我们就是先从根节点往左找，再往右找，就可以找到了，答案是`22`。也可以再从`22`的左子树一直找，这样就还能找到一个答案`20`。

最终答案是：

**`20` 或 `22`**

### 运行截图

![secret_phase_0.png](https://s2.loli.net/2025/04/14/KS3LzR2BT9nGMhv.png)

![secret_phase_1.png](https://s2.loli.net/2025/04/14/sB6ODkRyfUSt5HN.png)

---

## 答案

`phase_3` `phase_4` `phase_5` `secret_phase`答案并**不唯一**，这里只给出我有直接解出的答案。

```bash
Border relations with Canada have never been better.
1 2 4 8 16 32
5 206
0 0 DrEvil
yONEFG
4 3 2 1 6 5
20
```

or

```bash
Border relations with Canada have never been better.
1 2 4 8 16 32
5 206
0 0 DrEvil
yONEFG
4 3 2 1 6 5
22
```

---
