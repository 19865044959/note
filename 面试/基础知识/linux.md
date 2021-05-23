## 实时操作系统与分时操作系统区别？

-   实时操作系统：指的是外部事件发生时，有足够快的速度去处理与相应

-   分时操作系统：把CPU运行分成若干时间片分别处理不同的请求的系统

## 举例实时与分时操作系统

-   实时操作系统：

    -   一些嵌入式芯片跑的系统都是实时操作系统，比如FREERTOS等

    -   应用领域：用于对时间处理敏感的场合，比如说各种嵌入式芯片

-   分时操作系统：

    -   linux与常见windows都是分时操作系统

    -   应用领域：流行的PC与服务器都是分时操作系统

## 什么是linux？

-   linux是免费试用和自由传播的类Unix操作系统，是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统。它继承了UNIX以网络为核心的设计思想。

## linux包括几个部分？

-   内核

-   shell：是系统的用户界面，提供了用户与内核进行交互操作的一种接口，它接收用户命令并把它送去内核执行

-   文件系统

-   应用程序

-   网络接口

-   设备驱动

## linux常用命令

-   文件和磁盘空间：
    -   fdisk - l列出所有分区表
-   文件目录与管理：

    -   cd 切换目录

    -   pwd 查看当前绝对路径

    -   mkdir 创建目录

    -   rmdir 删除目录

    -   ls 查看当前目录下的文件

    -   cp 拷贝文件

    -   mv 移动文件
-   查看文件内容：

    -   cat -n 打开文件，并对行进行编号

    -   tac

    -   more

    -   less

    -   head

    -   tail
-   文件目录与权限：

    -   chmod

    -   chown

    -   chgrp

    -   umask
-   文件查找：

    -   which

    -   whereis

    -   locate

    -   find
-   进程管理：(process status)

    -   ps -u 显示所有用户进程
    -   ps -af 显示所有跟终端相关的进程详细信息
    -   ps -ax 列出所有进程的信息
    -   top 实时显示进程状况、CPU占用率、内存占用率
-   前台运行程序：

    -   ctrl + c 结束一个前台正在运行的程序
-   ctrl + z 停止一个前台正在运行的程序
-   删除：
    -   rm -rf 无提示递归删除本目录下的所有文件及其子文件

## linux编辑、编译与链接

-   首先创建一个.c文件，touch hello.c

-   编辑器：利用gedit编辑器打开 hello.c文件：gedit hello.c

-   编写完程序后，保存，利用gcc编译器来编译与链接.c文件，生成可执行文件gcc hello.c -o hello

## 利用g++编辑第一个c++程序

-   保存twoSum.cpp文件
-   保存twoSum.h文件
-   保存main2.cpp文件
-   利用g++ main2.cpp twoSum.cpp -o twoSum.exe来预编译、编译、汇编、链接twoSum.cpp与main2.cpp与静态库，最终生成可执行文件twoSum.exe

## STDIN_FILENO & STDOUT_FILENO

-   STDIN_FILENO：接收键盘的输入
-   STDOUT_FILENO：向屏幕输出

```c++
#include <stdio.h>
#include <string.h>
#include <unistd.h>
 
int main(int argc, char *args[])
{
	// 定义读取文件的缓冲区
	char buf_read[1024];
	// 定义写入文件的缓冲区
	char buf_write[1024];
	
	// 循环读取用户从键盘输入的信息
	while (1)
	{
		// 清空读取文件缓冲区中的内存
		memset(buf_read, 0, sizeof(buf_read));
		// 清空写入文件缓冲区中的内存
		memset(buf_write, 0, sizeof(buf_write));
		
		// 打印提示信息
		char input_message[100] = "input some words : ";
		write(STDOUT_FILENO, input_message, sizeof(input_message));
		// 读取用户的键盘输入信息
		read(STDIN_FILENO, buf_read, sizeof(buf_read));
		// 判断用户输入的内容是否为quit
		if (strncmp(buf_read, "quit", 4) == 0)
		{
			// 如果用户输入的是quit，程序退出循环
			break;
		}
		// 如果用户输入的不是quit
		// 把内容拷贝到写入文件缓冲区中
		strcpy(buf_write, buf_read);
		// 打印提示信息
		char output_message[100] = "output some words : ";
		write(STDOUT_FILENO, output_message, sizeof(output_message));
		// 将信息显示在屏幕上
		write(STDOUT_FILENO, buf_write, strlen(buf_write));
	}	
	
	return 0;
}
/*
输出：
input some words : 123
output some words : 123
input some words : 234
output some words : 234
input some words : quit
*/
```

