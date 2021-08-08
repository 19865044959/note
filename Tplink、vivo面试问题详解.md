## 如何让同时启动三个线程，却分别按顺序打印abc？

-   线程同步问题
    1.  通过逻辑实现线程同步
    2.  用互斥锁

```c
int i = 0; //利用全局变量来作为线程间的沟通桥梁

//构造3个回调函数
void* fcn1(void* a){
    while(i < 30){
        while(i % 3 == 1 || i % 3 == 2){
        	sleep(1); //如果满足上述条件，说明接下来打印的是B或者C，那么sleep(1)，放弃CPU
        }
        printf("A");
        i++;
        sleep(1);
    }
    pthread_exit(NULL);
}

void* fcn2(void* a){
    while(i < 30){
        while(i % 3 == 0 || i % 3 == 1){
            sleep(1); //如果满足上述条件，说明接下来打印的是A或者C，那么sleep(1)，放弃CPU
        }
        printf("B");
        i++;
        sleep(1);
    }
    pthread_exit(NULL);
}

void* fcn3(void* a){
    while(i < 30){
        while(i % 3 == 0 || i % 3 == 2){
            sleep(1); //如果满足上述条件，说明接下来打印的是A或者B，那么sleep(1)，放弃CPU
        }
        printf("C");
        i++;
        sleep(1);
    }
    pthread_exit(NULL);
}

int main(){
    //创建3个子线程
	pthread_create(&tid, NULL, fcn1, NULL);
    pthread_create(&tid, NULL, fcn1, NULL);
    pthread_create(&tid, NULL, fcn1, NULL);
    pthread_exit(NULL);
}
```

## 内存泄露的原因？如何检测内存泄露？

-   windows下，利用CRT函数库，通过包括 crtdbg.h，将 [malloc](http://msdn.microsoft.com/zh-cn/library/6ewkz86d.aspx) 和 [free](http://msdn.microsoft.com/zh-cn/library/we1whae7.aspx) 函数映射到它们的调试版本，即 [_malloc_dbg](http://msdn.microsoft.com/zh-cn/library/faz3a37z.aspx) 和 [_free_dbg](http://msdn.microsoft.com/zh-cn/library/16swbsbc.aspx)，这两个函数将跟踪内存分配和释放。 此映射只在调试版本（在其中定义了**_DEBUG**）中发生。 发布版本使用普通的 **malloc** 和 **free** 函数。

```c++
#define _CRT_SECURE_NO_WARNINGS //忽略strcpy警告
#define _CRTDBG_MAP_ALLOC
#include <stdlib.h>
#include <crtdbg.h>
#include <iostream>
using namespace std;

void GetMemory(char** p, int num)
{
    *p = (char*)malloc(sizeof(char) * num * 3);
    strcpy(*p, "abcdefg");
    //free(*p); 
}

int main(int argc, char** argv)
{
    char* str = NULL;
    GetMemory(&str, 100);
    cout << "Memory leak test!" << endl;
    _CrtDumpMemoryLeaks(); //在调用该函数前，如果存在没有释放的内存，那么当执行了该函数后，会在控制台出现警报
    return 0;
}

```

-   Linux下，利用非常著名的开源软件[valgrind](https://blog.csdn.net/qq_33336155/article/details/52608383?utm_medium=distribute.pc_relevant.none-task-blog-2~default~OPENSEARCH~default-5.baidujs&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~OPENSEARCH~default-5.baidujs)中的Memcheck来查看是否有内存泄露

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
int *p = malloc(1);
*p = 'x';
char c = *p;
printf("%c\n",c); //申请后未释放
return 0;
}
/*
    1）编译程序t4.c
    gcc -Wall t4.c -g -o t4
    2）使用Valgrind检查程序BUG
    valgrind --tool=memcheck --leak-check=full ./t4


==3221== Memcheck, a memory error detector
==3221== Copyright (C) 2002-2012, and GNU GPL'd, by Julian Seward et al.
==3221== Using Valgrind-3.8.1 and LibVEX; rerun with -h for copyright info
==3221== Command: ./t4
==3221==
==3221== Invalid write of size 4
==3221==at 0x40051E: main (t4.c:7)
==3221==Address 0x4c27040 is 0 bytes inside a block of size 1 alloc'd
==3221==at 0x4A06A2E: malloc (vg_replace_malloc.c:270)
==3221==by 0x400515: main (t4.c:6)
==3221==
==3221== Invalid read of size 4
==3221==at 0x400528: main (t4.c:8)
==3221==Address 0x4c27040 is 0 bytes inside a block of size 1 alloc'd
==3221==at 0x4A06A2E: malloc (vg_replace_malloc.c:270)
==3221==by 0x400515: main (t4.c:6)
==3221==
x
==3221==
==3221== HEAP SUMMARY:
==3221==in use at exit: 1 bytes in 1 blocks
==3221==total heap usage: 1 allocs, 0 frees, 1 bytes allocated
==3221==
==3221== 1 bytes in 1 blocks are definitely lost in loss record 1 of 1
==3221==at 0x4A06A2E: malloc (vg_replace_malloc.c:270)
==3221==by 0x400515: main (t4.c:6)   --》函数名称， malloc申请空间位置
==3221==
==3221== LEAK SUMMARY:
==3221==definitely lost: 1 bytes in 1 blocks
==3221==indirectly lost: 0 bytes in 0 blocks
==3221== possibly lost: 0 bytes in 0 blocks
==3221==still reachable: 0 bytes in 0 blocks
==3221== suppressed: 0 bytes in 0 blocks
==3221==
==3221== For counts of detected and suppressed errors, rerun with: -v
==3221== ERROR SUMMARY: 3 errors from 3 contexts
(suppressed: 6 from 6)

*/
```

![img](../Documents/note/pictures/20160921151637908)

## 了解过优先级反转吗？怎样解决？

-   通过优先级继承解决

## 操作系统如何保证线程安全？
