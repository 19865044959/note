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

## 条款04：确定对象被使用前已先被初始化

-   内置数据类型需要手动初始化，而STL中的数据类型如果没有初始化会默认初始化，为了保证所有数据能正确的初始化，需要在声明任何数据类型时首先要初始化
-   在类中，尽量使用初始化列表去初始化成员变量，而不是利用构造函数体进行赋值操作，不仅仅是效率的问题，还有：
    -   const对象只能初始化，不能赋值
    -   &对象只能初始化，不能赋值
-   类中成员变量的初始化顺序与其声明顺序相同，而与构造函数初始化列表无关！
-   前置知识：static对象分为两类
    -   local static对象，指的是在函数中被声明成static对象，其特征是在其所属的函数被调用之前，该对象并不存在，即只有在第一次调用对应函数时，local static对象才被构造出来。
    -   non local static对象：指的是定义在namespace作用域的对象、global对象、classes内的对象用static关键字修饰，这些对象在main()函数开始前就已经被构造出来了，并在main()函数结束后被析构。
-   C++对于不同cpp文件中的non-local static对象的初始化顺序没有明确定义，因此如果一个non-local static对象依赖另一个cpp内的non-local static对象，那么它所用到的这个对象有可能尚未被初始化，为了避免这类问题出现，推荐用local static来替换non-local static对象

# 2. 构造/析构/赋值运算

## 条款05： 了解C++默默编写并调用哪些函数

-   如果没有编写构造函数、拷贝构造、拷贝赋值运算符，以及析构函数，那么C++编译器会帮助你编写默认的版本。无非是将对应的非静态成员变量拷贝一份。当出现以下情况时，需要自己编写相应版本；
    -   当成员变量出现指针时，那么拷贝构造、拷贝赋值都是浅拷贝，会造成一个内存块释放多次，因此需要自定义4大函数
    -   当成员变量出现const成员变量与引用成员变量时

## 条款06：若不想使用编译器自动生成的函数，就该明确拒绝

-   怎样禁止构造、拷贝、赋值呢？
    -   将构造函数、拷贝构造函数、拷贝赋值运算符私有(private)起来，并只写声明，不写实现，这样即使是友元、成员函数试图访问，也会在链接时得到一个链接错误

## 条款07：为多态基类声明virtual析构函数

-   详见C++基础

## 条款08：别让异常逃离析构函数

-   为什么不能让析构函数吐出异常？
    -   考虑vector<A>Avec(10), 其中A是一个自定义类，有10个成员，那么当Avec被销毁时，由于成员数量>1，因此可能抛出多个异常。而C++中，两个以上异常同时存在，程序不是结束执行就是导致不明确行为，所以出现问题
-   若被析构函数调用的函数抛出了异常，那么我们可以将该异常捕获，然后吞下他们（不传播）或结束程序
-   如果非要吐出异常，也不能在析构函数中吐出，需要在class中有一个专门的普通函数去处理该异常

## 条款09：绝不在构造与析构过程中调用virtual函数

-   构造派生类对象时，首先调用基类构造函数初始化对象的基类部分。在执行基类构造函数时，对象的派生类部分是未初始化的。为了适应这种不完整，编译器视对象的类型为当前构造函数/析构函数所在的类的类类型。由此造成的结果是：在基类构造函数或者析构函数中，会将派生类对象当做基类类型对象对待。

```c++
//下面的代码初始的意思是调用派生类构造函数中的log函数，但是最终调用的却是基类构造函数与析构函数中的log函数
#include <iostream>

class Game {
public:
    Game(){
        init();
    };

    void init() {
        Log();
    }

    ~Game(){
        std::cout << "~Game() Log()" << std::endl;
    };

    virtual void Log() const {
        std::cout << "Game() Log()" << std::endl;
    }

};

class Kill : public Game {
public:
    virtual void Log() const override {
        std::cout << "Kill Log()" << std::endl;
    }
};

class BeKill : public Game {
public:
    virtual void Log() const override {
        std::cout << "BeKill Log()" << std::endl;
    }
};

int main(void)
{
    Kill a;

    return 0;
}

```

<img src="https://img-blog.csdnimg.cn/20190502191001441.png" alt="img"  />

## 条款12 复制对象勿忘其每一个成分

-   在自定义构造函数时需要注意所有的变量都需要被考虑，被遗忘的变量不会被编译器提醒
-   子类中写构造函数、拷贝构造函数、拷贝赋值运算符，一定要调用父类的构造函数、拷贝构造函数、拷贝赋值运算符来帮助派生类进行构造

```c++
#include<iostream>
using namespace std;
class B {
public:
	int b;
public:
	B(int bb = 0) :b(bb) {} //带有默认值的构造函数
	B(const B& obj) :b(obj.b) {}
	B& operator=(const B& obj) {
		if (this == &obj) //防止在operator=自我赋值（条款11）
			return *this;
		this->b = obj.b;
		return *this;
	}
};

class D :public B {
public:
	int d;
public:
	D(int bb, int dd) :B(bb), d(dd) {}
	//派生类的复构函数
	D(const D& obj) :B(obj), d(obj.d) {//调用基类的复构函数（用到了派生类对象转换为基类对象使用）
	}
	//派生类的赋值操作符函数
	D& operator=(const D& obj) {
		if (this == &obj)
			return *this;
		B::operator=(obj);
		this->d = obj.d;
	}
};
int main() {
	B b(1);
	D d1(1, 2);
	D d2(d1);//调用派生类的复构函数
	D d3(6, 8);
	d2 = d3;//调用派生类的赋值运算符函数
	return 0;
}
```

