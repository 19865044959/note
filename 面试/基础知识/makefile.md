## Makefile

### 一个规则

格式 目标：依赖条件

​	(一个tab缩进) 命令

- 目标的时间必须晚于依赖条件的时间，否则，更新目标

- 依赖条件如果不存在，找寻新的规则去产生依赖条件。

### 两个函数

- src = ${wildcard *.c} #匹配当前工作目录下的所有.c 文件。将文件名组成列表，赋值给变量 src。  src = add.c sub.c div1.c

- obj = ${patsubst %.o, %.c, ${src}} #将参数3中，包含参数1的部分，替换为参数2。 obj = add.o sub.o div1.o

  clean:(无依赖)

  ​	-rm -rf ${obj} a.out #“-”：作用是，删除不存在文件时，不报错。顺序执行结束。

### 三个自动变量

- $@ 在规则的命令中，表示规则中的目标。
- $< 在规则的命令中，表示第一个依赖条件。如果将该变量应用在模式规则中，它可将依赖条件列表中的依赖依次取出，套用模式规则。
- $^ 在规则的命令中，表示所有依赖条件。

### 模式规则

%.o:%.c

​	gcc -c $< -o $@

### 静态模式规则

- 有时候，我们需要让某个模式规则只适用于某个目标，那么可以在规则前面加上${obj}来限定，只是obj变量对应的那些目标与依赖条件才套用

### 伪目标

- 有的时候我们写的target不一定是用来生成的，比如说clean，只是用作清理功能。那么这个时候如果文件夹中出现了一个clean文件，那么makefile文件夹中的clean目标就会被影响。原因是makefile以为你要生成clean目标，但是当前文件夹里面已经有一个clean目标了，并且由于没有依赖条件，clean不满足目标时间早于依赖条件时间，此时target便不会被更新，换句话说clean就不会被执行。
- 解决：将clean与ALL变成伪目标，意思是不管时间条件满足与否，target都会被更新（语句被执行）

### 参数

- -n：模拟执行make、make clean 命令

- -f：指定文件执行 make 命令。			make -f	xxxx.mk

```makefile
src = ${wildcast *.c}
obj = ${patsubst %.c, %.o, ${src}}

ALL:a.out #终极目标，如果不加ALL，需要把a.out放在首行

${obj}:%.o:%.c
	gcc -c $< -o $@
	
a.out:${obj}
	gcc $< -o $@
	
clean:
	-rm -rf ${obj} a.out
	
.PHONY: ALL clean

```

