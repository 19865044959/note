## STL六大组件

-   容器（Container）

-   算法（Algorithm）

-   迭代器（Iterator）

-   仿函数（Function object）

-   适配器（Adaptor）

-   空间配置器（allocator）

## vector

-   原理：vector的底层实际上就是一块连续的地址空间，该地址空间有3个指针（迭代器），分别指向vector的首地址、尾地址、最后一个元素的地址

    -   _Myfirst 和 _Mylast 可以用来表示 vector 容器中目前已被使用的内存空间，即vec.size()

    -   _Mylast 和 _Myend 可以用来表示 vector 容器目前空闲的内存空间，即vec.capacity() - vec.size()

    -   _Myfirst 和 _Myend 可以用表示 vector 容器的容量，即vec.capacity()

-   运用好这3个迭代器，就能够实现vector几乎所有功能：![img](C:\Users\huany\Documents\note\pictures\7ac5b521-116e-485e-8aee-bc93b17112f8-4786849.jpg)

-   [vector扩容](https://blog.csdn.net/gettogetto/article/details/77804094?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-1.vipsorttest&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-1.vipsorttest)/string扩容有了解过吗？
    -   发生场合：当使用push_back向vector尾部加入一个新元素时，当超过了vector目前的最大容量时，就会发生扩容。
    
    -   扩容大小：一般有2倍扩容与1.5倍扩容，扩容因子越大，那么意味着预留空间越大，浪费空间越多，但是如果扩容因子过小，就会时常产生预留空间不足，那么就需要重新开辟一段空间，把原有数据复制到新空间上，这时候时间浪费越大。因此，扩容因子的取值权衡了空间与时间的优化。
    
-   map/set

    -   底层实现：红黑树，查找/删除节点的时间复杂度：O(logn)
    -   为什么要用红黑树？红黑树是一种平衡二叉树。
    -   那平衡二叉树有什么优点？平衡二叉树查找元素的最坏时间复杂度也是O(logn)，克服了二叉搜索树因退化成链表而致使的搜索时间复杂度变成O(n)这一情况
    -   为什么有了平衡二叉树还需要红黑树？因为平衡二叉树的条件过于严苛：左右子树的高度差不超过1，这使得在插入/删除节点后几乎都会破坏该规则，进而需要我们左旋、右旋来调整，对于频繁插入、删除的场景来讲，平衡二叉树的性能大大降低，为了解决这个问题，于是出现了红黑树。
    -   红黑树引入了颜色概念，使得平衡条件得以简化，降低了对旋转的要求，从而提高了性能

## string

-   改进：C中的char* a[] = "abcde"不允许扩容，否则会出现数组越界的情况。C++中，包括vector与string，都可以动态申请内存。即：当长度超过了它的容量capacity时，它就会在内存中重新寻找一段连续内存，经过实验，得出扩容大小与vector相同，都是之前的1.5倍

## unordered_map

-   底层实现：哈希表

-   哈希表原理是什么？ 实现键值对的一一映射

-   怎样通过关键字找到相应的值呢？/怎样构造hash函数？

    -   除留余数（最常用的构造方法）

        -   address(key) = key % p (p <= m， 其中m为表长)

        -   一般来说，需要把p取成接近m的质数，因为质数没有多余的因数。

    -   直接定址法

        -   address(key) = a * key + b

        -   用处：用在对一些关键字连续的情况

    -   数字分析法

        -   场合：关键字比较大，并且关键字分布比较均匀

        -   分析这些关键字，哪一部分是完全不同的，比如手机后4位，那么用这个作为address(key)

    -   平方取中法

        -   对关键字先平方，再取中位数

        -   场合：关键字位数不大

-   哈希冲突

    -   什么是哈希冲突？
        -   不同的关键字对应到了相同的地址值，那么前一个关键字的值会被后一个关键字的值覆盖掉，导致前面的关键字对应的值丢失的一种情况

    -   怎样解决？

        -   开放定址法（线性探测法、二次探测法、随机探测法）

            -   原理：一旦地址发生冲突，那么就去寻找下一个空的散列地址。

            -   f(key) = (f(key ) + di) % m (di = 1, 2, ... m - 1)

            -   缺点：容易出现堆积现象（不是同义词的两个关键字争夺同一个地址的情况）

            -   改进：二次探测：(di = 1, -1, 2^2, -2^2 ..)(不让关键字集中在某个区域) 随机探测法

        -   再散列函数法

            -   原理：存放多个hash函数，出现hash冲突，那么就换另一个函数

            -   缺点：增加了计算时间

        -   链地址法

            -   原理：将存在hash冲突的所有关键字用一个链表来存放，那么在对应的地址上只存放这个链表的指针

            -   优点：对出现多冲突的hash函数而言，提供了绝对的保障

            -   缺点：单链表遍历耗费资源

        -   公共溢出区法

        -   原理：对于所有出现hash冲突的关键字，统一找一个区域保存（叫做公共溢出区）

## priority_queue

-   实现：利用堆结构进行实现
-   [堆的具体实现](https://guguoyu.blog.csdn.net/article/details/81283998?utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-1.vipsorttest&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~BlogCommendFromMachineLearnPai2~default-1.vipsorttest)：

    -   具体来说分为两个部分：堆的初始构建、堆的插入、堆的删除

    -   堆的初始构建：假设给的数组是完全二叉树的层序遍历，那么它最右下角拥有孩子节点的节点的idx是n / 2 - 1，从该节点开始一直到根节点，如果父节点 < 两个孩子节点中的最大值，那么就与孩子节点交换，直到叶子节点为止。这样从右下节点一直到根节点遍历一遍，那么会使得每个父节点都是>=它所有的孩子节点。复杂度是O(n)

    -   插入元素：首先将新元素作为堆的最后一个节点（即二叉树最底层最右侧节点），然后一层一层的寻找父亲，并交换节点使得父节点值最大，直到达到根节点，复杂度：O(logk)其中k是插入元素后堆大小

    -   删除元素：首先将根节点弹出，然后将堆的最后一个节点放到根节点上，一层一层的遍历，让父亲节点最大，最终到达叶子节点为止，复杂度：O(logk)其中k是删除元素后堆的大小
-   [建立、插入、删除堆元素](https://blog.csdn.net/SZU_Crayon/article/details/81812946?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~default-18.vipsorttest&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~default-18.vipsorttest)

