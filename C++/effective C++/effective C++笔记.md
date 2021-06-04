# 1. 让自己习惯C++



## 条款01：视C++为一个语言联邦

-   C++由4个部分组成：
    -   C语言保留部分
    -   面向对象部分，即class部分
    -   template模板部分，即泛型编程
    -   STL

## 条款02：尽量以const，enum，inline来替换#define

-   #define缺点
    -   #define只是进行简单的替换，不做类型检查，一些简单的错误没法被发现
    -   #define由于在预编译阶段已经被替换了，使得替换后的文件可读性变得非常差，而const变量是存放在符号表中，因此没有这个问题
    -   const关键字声明的常量可以调试，但是#define却不行
-   建议：
    1.  **对于单纯的常量**，尽量用const int MAX = 10000等替换#define MAX 10000
    2.  **对于函数体短小的宏**，尽量用inline内联函数来替换用#define定义的宏（宏看起来像函数，但是它不会招致函数调用带来的额外开销）

```c++
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b)) //极易出错，不用

template<typename T>
inline void callWithMax(const T& a, const T& b){
    f(a > b ? a : b); //选取a与b中更大的值将其作为函数值传入f(x)函数中
}
```

-   #define现在被用到的场合
    -   #ifndef XXX
    -   #define XXX
    -   #endif

## 条款03：尽可能使用const

[(194条消息) C语言中const关键字的用法_xingjiarong的专栏-CSDN博客_c const](https://blog.csdn.net/xingjiarong/article/details/47282255)