-   最简单的建立本地仓库到远程仓库联系的方法

    -   首先在github上new一个repository

    -   在本地某个地方打开git bash(命令行模式)

    -   输入：git clone 地址，其中地址是repository的url

    -   修改/增添文件

    -   git add . //把所有的文件放入缓存区中

    -   git commit -m "xxx"  //最后确认，将文件放入本地仓库(git仓库)，并附上说明文字xxx

    -   git push //将git仓库所更变的文件push到远程仓库中

-   git配置信息：

    -   git remote add origin 地址 //本地与远程仓库建立连接

    -   git config --global [user.name](http://user.name) 'xxx'

    -   git config --global user.email 'xxx'