---
title: '[CSAPP] Lab4 - Cache Lab Report'
date: 2025-05-06 21:02:38
tags:
categories: 
- CSAPP
---

## `csim.c`

**实验思想**：采用封装的思想，提高函数的复用性，有助于理解总体代码。

**前置函数**：定义在`cachelab.h`：

```c
/* 
 * printSummary - This function provides a standard way for your cache
 * simulator * to display its final hit and miss statistics
 */ 
void printSummary(int hits,  /* number of  hits */
                  int misses, /* number of misses */
                  int evictions); /* number of evictions */
```

首先我们先构思出建立一个缓存需要哪些部分。这包括：

- 缓存的基本数据结构
- 从命令行获得缓存`s` `E` `b`和读写的`t`信息
- 初始化缓存
- 获取文件读写信息
- 访问缓存
- 增加`LRU`次数
- 释放Cache

于是我们先搭建总体框架。其中函数的返回值和参数暂时不完善，我们之后按需求修改。

```c
#include "cachelab.h"

uint64_t hits = 0;
uint64_t misses = 0;
uint64_t evictions = 0;

typedef struct { 
    int valid;
    uint64_t tag;
    uint64_t lru;
} Line;

typedef struct {
    Line *lines;
} Set;

typedef struct {
    Set *sets;
    uint64_t s;
    uint64_t E;
    uint64_t b;
} Cache;

void getCommandInfo(uint64_t s, uint64_t E, uint64_t b) {

}

Cache initCache() {

}

void accessCache(Cache *cache, uint64_t address) {

}

void updateLRU(Cache *cache) {

}

void getFileInfo(char *tracefile) {

}

void freeCache(Cache *cache) {

}

int main(int argc, char *argv[]) {

    printSummary(hits, misses, evictions);
    
}
```

接下来我们逐个分析。

### `getCommandInfo`

这个函数的目的是为了读取命令行输入的指令，获得缓存`s` `E` `b`和读写的`t`信息。

```bash
Usage: ./csim-ref [-hv] -s <s> -E <E> -b <b> -t <tracefile>
```

我们可以使用`getopt`。这个头文件一般只在Linux下使用。`getopt.h`必须要包括，否则会报错，原因未知。

```c
#include <unistd.h>
#include <getopt.h>
int getopt(int argc, char * const argv[], const char *optstring);
```

**特殊变量**：

- `optarg`：当前选项的参数值（如 -b value 中的 value）
- `optind`：下一个要处理的 argv 索引
- `opterr`：设为 0 可禁止错误信息输出

`optstring`规则：

- 字符：表示允许的选项（如 "ab:c"）
  - 'a'：无参数
  - 'b:'：需要参数
  - 'c'：无参数

这个函数很明显比较适合使用在`main`函数中，所以我们把`getCommandInfo`的功能放在`main`函数中。

```c
int main(int argc, char *argv[]) {
    uint64_t s;
    uint64_t E;
    uint64_t b;
    char tracefile[fileNameLength];

    char opt;
    while ((opt = getopt(argc, argv, "s:E:b:t:")) != -1) {
        switch (opt) {
            case 's':
                s = atoi(optarg);
                break;
            case 'E':
                E = atoi(optarg);
                break;
            case 'b':
                b = atoi(optarg);
                break;
            case 't':
                strcpy(tracefile, optarg);
                break;
        }
    }

    printSummary(hits, misses, evictions);
    
}
```

这样我们就完成了获取命令行指令的函数。

### `initCache`

我们已经获得了`s` `E` `b`信息，我们可以开始初始化缓存了。我们需要给cache的`s` `E` `b`赋值，并且开辟内存空间，而且需要使每一路的`valid` `tag` `lru`都设置为0。

```c
Cache initCache(uint64_t s, uint64_t E, uint64_t b) {
    int S = 1 << s;

    Cache cache;
    cache.s = s;
    cache.E = E;
    cache.b = b;

    cache.sets = (Set *)malloc(S * sizeof(Set));
    for (int i = 0; i < S; i++) {
        cache.sets[i].lines = (Line *)malloc(E * sizeof(Line));
        for (int j = 0; j < E; j++) {
            cache.sets[i].lines[j].valid = 0;
            cache.sets[i].lines[j].tag = 0;
            cache.sets[i].lines[j].lru = 0;
        }
    }

    return cache;
}
```

在`main`函数中新增：

```c
Cache cache;
cache = initCache(s, E, b);
```

### `getFileInfo`

```bash
 <操作类型> <地址>,<大小>
 L 10,4 
 S 18,4
 L 20,4
```

我们只需要处理`L`(load) `S`(store) `M`(modify load/store)，不需要理会`L`。其中`M`进行了两次的内存访问。

为了安全地从文件中读取`uint64_t`，可以如下操作：

```c
#include <inttypes.h>
fscanf(fp, " %c %" SCNx64 ",&d", &opt, &address, %size) == 3
```

完整的函数如下：

```c
void getFileInfo(char *tracefile, Cache *cache) {
    FILE *fp = fopen(tracefile, "r");
    if (fp == NULL) {
        return;
    }

    char opt;
    uint64_t address;
    int size;

    while (fscanf(fp, " %c %" SCNx64 ",%d", &opt, &address, &size) == 3) {
        switch (opt) {
            case 'L':
                accessCache(cache, address);
                break;
            case 'S':
                accessCache(cache, address);
                break;
            case 'M':
                accessCache(cache, address);
                accessCache(cache, address);
                break;
        }
        updateLRU(cache);
    }
}
```

在`main`函数加上：

```c
getFileInfo(tracefile, &cache);
```

接下来我们需要完成`accessCache`和`updateLRU`，后者较为简单，我们从易到难。

### `updateLRU`

实现很简单，每次进行了一次操作都需要更新`valid`位置不为0的`LRU`，以便于我们找到使用最不频繁的路进行替换。

```c
void updateLRU(Cache *cache) {
    int S = 1 << cache->s;
    int E = cache->E;

    for (int i = 0; i < S; i++) {
        for (int j = 0; j < E; j++) {
            if (cache->sets[i].lines[j].valid) {
                cache->sets[i].lines[j].lru++;
            }
        }
    }
}
```

### `accessCache`

这个函数比较复杂。首先我们需要处理传入的地址，区分出`tag` `index` `block_offset`，只不过对于这个实验`block_offset`可以不考虑。之后访问相对应的缓存路，分为三类情况：

- **命中了**：比对`tag`并且确认`valid`不为0。再把缓存的`lru`设置为0，表示其刚刚被访问了，`hits++`。
- **未命中且有空的路**：加载到空的路中，`tag`更新，`valid = 1`，`misses++`。
- **未命中且没有空的路**：加载到`lru`最大的路中，`tag`更新，`valid = 1`，`lru = 0`，`evictions++`，`misses++`。

```c
void accessCache(Cache *cache, uint64_t address) {
    uint64_t s = cache->s;
    uint64_t b = cache->b;

    uint64_t tag = address >> (s + b);

    uint64_t mask = UINT64_MAX;
    mask >>= 64 - (s + b);
    uint64_t temp = address & mask;

    uint64_t index = temp >> b;

    int isEmpty = 0;
    for (int i = 0; i < cache->E; i++) {
        if (cache->sets[index].lines[i].valid && cache->sets[index].lines[i].tag == tag) {
            cache->sets[index].lines[i].lru = 0;
            hits++;
            return;
        }

        if (cache->sets[index].lines[i].valid == 0) {
            isEmpty = 1;
        }
    }

    if (isEmpty) {
        for (int i = 0; i < cache->E; i++) {
            if (cache->sets[index].lines[i].valid == 0) {
                cache->sets[index].lines[i].valid = 1;
                cache->sets[index].lines[i].tag = tag;
                misses++;
                return;
            }
        }
    } else {
        uint64_t MaxLRU = 0;
        for (int i = 0; i < cache->E; i++) {
            MaxLRU = cache->sets[index].lines[i].lru > MaxLRU ? cache->sets[index].lines[i].lru : MaxLRU;
        }

        for (int i = 0; i < cache->E; i++) {
            if (MaxLRU == cache->sets[index].lines[i].lru) {
                cache->sets[index].lines[i].lru = 0;
                cache->sets[index].lines[i].tag = tag;
                misses++;
                evictions++;
                return;
            }
        }
    }
}
```

### `freeCache`

释放内存即可，逐层释放。

```c
void freeCache(Cache *cache) {
    int S = 1 << cache->s;
    for (int i = 0; i < S; i++) {
        free(cache->sets[i].lines);
    }
    free(cache->sets);
}
```

在`main`函数加上：

```c
freeCache(&cache);
```

### **完整代码**

```c
#include "cachelab.h"
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#include <stdint.h>
#include <inttypes.h>
#include <stdio.h>
#include <getopt.h>

#define fileNameLength 1000

uint64_t hits = 0;
uint64_t misses = 0;
uint64_t evictions = 0;

typedef struct { 
    int valid;
    uint64_t tag;
    uint64_t lru;
} Line;

typedef struct {
    Line *lines;
} Set;

typedef struct {
    Set *sets;
    uint64_t s;
    uint64_t E;
    uint64_t b;
} Cache;

Cache initCache(uint64_t s, uint64_t E, uint64_t b) {
    int S = 1 << s;

    Cache cache;
    cache.s = s;
    cache.E = E;
    cache.b = b;

    cache.sets = (Set *)malloc(S * sizeof(Set));
    for (int i = 0; i < S; i++) {
        cache.sets[i].lines = (Line *)malloc(E * sizeof(Line));
        for (int j = 0; j < E; j++) {
            cache.sets[i].lines[j].valid = 0;
            cache.sets[i].lines[j].tag = 0;
            cache.sets[i].lines[j].lru = 0;
        }
    }

    return cache;
}

void accessCache(Cache *cache, uint64_t address) {
    uint64_t s = cache->s;
    uint64_t b = cache->b;

    uint64_t tag = address >> (s + b);

    uint64_t mask = UINT64_MAX;
    mask >>= 64 - (s + b);
    uint64_t temp = address & mask;

    uint64_t index = temp >> b;

    int isEmpty = 0;
    for (int i = 0; i < cache->E; i++) {
        if (cache->sets[index].lines[i].valid && cache->sets[index].lines[i].tag == tag) {
            cache->sets[index].lines[i].lru = 0;
            hits++;
            return;
        }

        if (cache->sets[index].lines[i].valid == 0) {
            isEmpty = 1;
        }
    }

    if (isEmpty) {
        for (int i = 0; i < cache->E; i++) {
            if (cache->sets[index].lines[i].valid == 0) {
                cache->sets[index].lines[i].valid = 1;
                cache->sets[index].lines[i].tag = tag;
                misses++;
                return;
            }
        }
    } else {
        uint64_t MaxLRU = 0;
        for (int i = 0; i < cache->E; i++) {
            MaxLRU = cache->sets[index].lines[i].lru > MaxLRU ? cache->sets[index].lines[i].lru : MaxLRU;
        }

        for (int i = 0; i < cache->E; i++) {
            if (MaxLRU == cache->sets[index].lines[i].lru) {
                cache->sets[index].lines[i].lru = 0;
                cache->sets[index].lines[i].tag = tag;
                misses++;
                evictions++;
                return;
            }
        }
    }
}

void updateLRU(Cache *cache) {
    int S = 1 << cache->s;
    int E = cache->E;

    for (int i = 0; i < S; i++) {
        for (int j = 0; j < E; j++) {
            if (cache->sets[i].lines[j].valid) {
                cache->sets[i].lines[j].lru++;
            }
        }
    }
}

void getFileInfo(char *tracefile, Cache *cache) {
    FILE *fp = fopen(tracefile, "r");
    if (fp == NULL) {
        return;
    }

    char opt;
    uint64_t address;
    int size;

    while (fscanf(fp, " %c %" SCNx64 ",%d", &opt, &address, &size) == 3) {
        switch (opt) {
            case 'L':
                accessCache(cache, address);
                break;
            case 'S':
                accessCache(cache, address);
                break;
            case 'M':
                accessCache(cache, address);
                accessCache(cache, address);
                break;
        }
        updateLRU(cache);
    }
}

void freeCache(Cache *cache) {
    int S = 1 << cache->s;
    for (int i = 0; i < S; i++) {
        free(cache->sets[i].lines);
    }
    free(cache->sets);
}

int main(int argc, char *argv[]) {
    uint64_t s;
    uint64_t E;
    uint64_t b;
    char tracefile[fileNameLength];

    char opt;
    while ((opt = getopt(argc, argv, "s:E:b:t:")) != -1) {
        switch (opt) {
            case 's':
                s = atoi(optarg);
                break;
            case 'E':
                E = atoi(optarg);
                break;
            case 'b':
                b = atoi(optarg);
                break;
            case 't':
                strcpy(tracefile, optarg);
                break;
        }
    }

    Cache cache;
    cache = initCache(s, E, b);

    getFileInfo(tracefile, &cache);

    freeCache(&cache);

    printSummary(hits, misses, evictions);
}
```

### **跑分截图**

![csim.c.png](https://s2.loli.net/2025/05/06/Yx1SKaBADcPtQEH.png)

---

## `trans.c`

// TODO

---
