## 最简单的建立本地仓库到远程仓库联系的方法

-   首先在github上new一个repository
-   在本地某个地方打开git bash(命令行模式)
-   输入：git clone 地址，其中地址是repository的url
-   修改/增添文件
-   git add . //把所有的文件放入缓存区中
-   git commit -m "xxx"  //最后确认，将文件放入本地仓库(git仓库)，并附上说明文字xxx
-   git push //将git仓库所更变的文件push到远程仓库中

## 将本地仓库与远程仓库连接

```
找到本地文件夹，git bash here
git init //初始化仓库
git add .(文件name) //添加文件到本地仓库
git commit -m “first commit” //添加文件描述信息
git remote add github/gitee + 远程仓库地址 //链接远程仓库，创建主分支，最好用SSH，不用输入密码
git pull github/gitee main // 把本地仓库的变化连接到远程仓库主分支
git push -u github/gitee main //把本地仓库的文件推送到远程仓库
————————————————
原文链接：https://blog.csdn.net/aiming66/article/details/81051428
```



## git配置信息：

-   git remote add origin 地址 //本地与远程仓库建立连接

-   git config --global [user.name](http://user.name) 'xxx'

-   git config --global user.email 'xxx'

## git分支操作

### 创建分支

```
git branch dev //创建dev分支
git checkout dev //切换到dev分支
git branch //查看当前分支情况
```

### 合并分支

```
git merge dev //将dev分支合并到main之中
```

### 删除分支

```
git branch -d dev //删除分支，需要在合并后删除
```

