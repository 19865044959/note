# 第一章：git基础

## 查看手册

man git-checkout/ git-log / git-branch ...

## 给文件进行重命名的方法

![image-20210827112813007](../../pictures/image-20210827112813007.png)



可不可以用1条指令替换3条指令呢？

**git mv** 

![image-20210827113049675](../../pictures/image-20210827113049675.png)



## git log查看版本演变历史



git checkout -b temp SHA1  #在SHA1号的那个commit处新建一个temp分支

![image-20210827114648023](../../pictures/image-20210827114648023.png)

![image-20210827114853035](../../pictures//image-20210827114853035.png)

![image-20210827114914219](../../pictures/image-20210827114914219.png)





在有2个分支以上的情况下，git log查看的是当前分支

![image-20210827115059051](../../pictures/image-20210827115059051.png)



还可以指定查看的分支

![image-20210827115725032](../../pictures/image-20210827115725032.png)





git log --all #查看所有分支信息，包括远程分支

![image-20210827115141244](../../pictures/image-20210827115141244.png)



  git log --graph --all #以图形化方式来查看git log

![image-20210827115501804](../../pictures/image-20210827115501804.png)



显示4行，每个commit以1行的方式显示

![image-20210827115615053](../../pictures/image-20210827115615053.png)



## gitk图形界面来查看版本历史

## .git目录

![image-20210827120635660](../../pictures/image-20210827120635660.png)



HEAD里面的内容是头指针所指的分支

![image-20210827120609596](../../pictures/image-20210827120609596.png)



![image-20210827120847846](../../pictures/image-20210827120847846.png)



refs存放的是各个分支与tag的信息

refs/heads里面存储着的是所有的分支的头结点指针

![image-20210827121125505](../../pictures/image-20210827121125505.png)



object：

重要！

![image-20210827121249080](../../pictures/image-20210827121249080.png)





## commit、tree、blob三个对象之间的关系



![image-20210827121738049](../../pictures/image-20210827121738049.png)

个人认为：一个commit对应了一个tree，里面存放着commit那一瞬间的snapshot，然后这颗tree的节点都是blob，对应着文件，而blob是否相同是通过blob里面内容是否相同来识别的，而不是通过名字（其实没有blob名），好处是大大节约了存储空间



## 分离头指针

含义：detached head，即我们正工作在一个没有分支的commit上

危险：在分离头指针后，由于工作在一个没有分支的状态下，即commit没有绑定分支，因此任意commit都会被丢弃，但我们也可以利用这一点，对代码做一些非常大胆的试验，最后checkout回去后，不用理会变更了什么

![image-20210827123114291](../../pictures/image-20210827123114291.png)





## HEAD与branch

 

-a选项查看本地与远程仓库

![image-20210827123713409](../../pictures/image-20210827123713409.png)



总结：HEAD不一定会指向分支头，比如说你用分离头指针操作；但是当你在checkout时，HEAD也会发生改变，指向分支头commit



可以用HEAD HEAD~ HEAD~n来表示当前分支头，父亲，爷爷

![image-20210827124443939](../../pictures/image-20210827124443939.png)



解决两个分支没有公共祖先，即non-fast-forward的情况下的冲突：

- rebase
- merge

# 第二章：独自使用git时常见的场景

## 删除不需要的分支

![image-20210827150130950](../../pictures/image-20210827150130950.png)

git branch -d 分支名 #删除已经合并了的分支

git branch -D 分支名 #删除未合并的分支

## 修改最近的commit message

git commit --amend



## 修改以前的commit message

git rebase #变基



要将good commit 变成good commit ！

![image-20210827154534595](../../pictures/image-20210827154534595.png)



拷贝good commit所在commit的父哈系数

![image-20210827154646000](../../pictures/image-20210827154646000.png)



git rebase -i a90f85146e9468bab3a9589e59d

![image-20210827154741433](../../pictures/image-20210827154741433.png)



由于我们需要change commit message，因此选择r

> 注：pick的意思是保留原来的commit的一切东西，包括commit tree blob



之后会又弹出一个交互界面，里面将good commit修改，加上！然后用git log查看，就能看到message已经多了！![image-20210827160159838](../../pictures/image-20210827160159838.png)

![image-20210827160028967](../../pictures/image-20210827160028967.png)



可以发现，分离头指针之后，在变基后面的commit都是新的commit，所有的哈希号都不同，但是内容不变，即blob不变

![image-20210827160652426](../../pictures/image-20210827160652426.png)

![image-20210827160733401](../../pictures/image-20210827160733401.png)

 

## 将多个commit整合成1个

将前4个commit整合到1个commit之中，那么base就应该是b.cpp了

![image-20210827161605478](../../pictures/image-20210827161605478.png)



选中1个作为融合的主体commit，其他选择squash作为被融合的commit![image-20210827162013095](../../pictures/image-20210827162013095.png)



这时候又会跳出一个页面，输入融合后的commit，如下：

![image-20210827162215068](../../pictures/image-20210827162215068.png)



成功整合4个commit到1个commit中去

![image-20210827162249583](../../pictures/image-20210827162249583.png)

![image-20210827162317727](../../pictures/image-20210827162317727.png)

