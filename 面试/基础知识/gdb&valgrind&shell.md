## gdb常用指令总结

- gdb对于检测段错误在哪儿是十分有效可行的，gdb模式下，直接run，程序停止的位置就是出段错误的位置

```shell
#首先，需要让你的程序为debug版本的
g++ -g main.cpp -o main.out
cmake -DCMAKE_BUILD_TYPE=Debug

gdb main.out

l 1 #list 1，查看行数为1的源码

b 35 #break point
b 35 if i == 5 #设置条件断点。如果我的break point是放在了一个循环中，我想查看当i = 5时变量值是如何，那么我就用这个“条件断点”来实现当i == 5时，才stop

r #run
n #next，就是下一行语句，不会进入函数中
s #step，如果遇到函数，会进入
until 16 #跳出循环，到达16行
p i #查看i变量的值
c #continue
finish #结束函数调用，并返回函数的调用点，用在不想再看某个函数后，退出该函数

set args arg1 arg2 arg3 #设置命令行参数。如果gdb调试的程序存在命令行参数，那么在进入gdb调试界面后，第1句指令就是set args xxx，表示./main.out xxx
run arg1 arg2 arg3 #等价与上述操作 

info b #查看断点信息

ptype i #查看变量i类型

backtrace #bt，查看函数的调用的栈帧和层级关系
frame #根据栈帧编号，切换函数的栈帧

display i #持续跟踪i值，即程序每next或者step后，都会print出i当前的最新值
undisplay i #取消跟踪
```

### 常见问题

- gdb中，如何在一个函数中查看另一个函数的变量值？

  ```shell
  bt #backtrace
  frame x #切换栈帧
  p i #打印出你所切换到的栈帧的变量值i
  ```

## shell语言

```shell
#下面两句话经常会在shell语言的开头看到，分别是：以/bin/bash运行和以/bin/sh运行
#!/bin/bash
#!/bin/sh
```

## awk

- awk是一种强大的文本分析工具，用法举例

```shell
awk '{ print $1 }' #print字段1，以空格或者tab作为分割符

awk -F, '{ print $1 }' #指定以','作为分割符
```

- 其他：https://www.runoob.com/linux/linux-comm-awk.html

