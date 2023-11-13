---
title: "第6章 函数(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2022-08-31
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch6"
draft: false 
comments: true
reward: false 
mermaid: true 
showToc: true 
TocOpen: true 
hidemeta: false 
disableShare: true 
showbreadcrumbs: true 
cover:
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/2022-02-28.jpg
    caption: "" 
    alt: ""
    relative: false
---





## 1. 函数的基础

一个典型的函数定义包括以下部分：**返回类型** 、**函数名**、**形参列表** 。本章的内容也是围绕这几个点展开的。



#### 编写函数和调用函数

```cpp
//val的阶乘 val*(val-1)*(val-2)...*1
int fact(int val)
{
    int ret = 1;
    while (val > 1)
        ret *= val--;
    return ret;
}

//调用函数
int main()
{
    int j = fact(5);
    cout << "5! is " << j << endl;
    return 0;
}
```



### 1.1 局部对象

在c++语言中，名字有作用域，对象有生命周期

- 名字的作用域是程序文本的一部分，名字在其中可见
- 对象的生命周期是程序执行过程中该对象存在的一段时间



#### 局部变量

形参和函数内部定义的变量统称**局部变量**

- 局部变量只在函数内部起作用
- 外部全局变量和局部变量同名，局部变量会覆盖全局，这里是名称的覆盖，不是值的覆盖



#### 局部静态变量

使用`static` 关键字定义静态变量

```cpp
size_t count_calls()
{
    static size_t ctr = 0;
    return ++ctr;
}

int main()
{
    for (size_t i = 0; i != 10; ++i)
    {
        cout << count_calls() << endl;
    }
    return 0;
}
```



> 静态变量存在于程序的整个生命周期，第一次调用`count_calls`, 定义并初始化`ctr`，之后再调用函数不会再执行初始化，ctr相对于一个全局的变量



### 1.2 函数的声明

和变量名一样，函数也必须在使用之前声明，<span style="background: yellow">函数可以声明多次，但是只能定义一次。</span>函数的定义不是必须的，比如声明一个函数，我们从没有调用它，那么它可以不用定义（在15.3节会介绍）。函数的声明是没有函数体的，所以<span style="background: yellow">声明可以不写形参名，只声明类型</span>，但在定义时如果在函数用到形参，则需要写上变量名。

```cpp
int func(int, int);		//声明可以不写变量名

int func(int a, int b)	//函数体中使用了形参，需要写变量名
{
    return a+b;
}
```





## 2. 参数传递

形参初始化的机制和变量初始化一样（本节内容可结合第2章的内容看）

在c++中传参的方式主要有两种：**引用传递**、**值传递**



### 2.1 传值参数

#### 普通类型形参

当初始化一个非引用类型的变量时，初始值被拷贝给变量，在函数体内改变的是实参的副本，不会对实参有影响

```cpp
//val的阶乘 val*(val-1)*(val-2)...*1
int fact(int val)
{
    int ret = 1;
    while (val > 1)
        ret *= val--;
    return ret;
}

//调用函数
int main()
{
    int j = 5;
    cout << "5! is " << fact(j) << endl;
    cout << "5! is " << j << endl;		//j的值没有变化
    return 0;
}
```



#### 指针形参

<span style="background: yellow">指针形参也是值传递的一种方式</span>，传入的指针是实参的副本（一个拷贝出来的指针），同样在函数体中改变指针的值（指向的地址）不会影响实参的值。

```cpp
void reset(int *p)
{
    *ip = 0;	//改变指针ip所指对象的值
    ip = 0;		//改变ip所指向的地址，但是只改变局部变量，实参未被改变
}
```



> 值传递的两种方式都是通过拷贝实参进行传值的，如果传入的实参比较大，拷贝会影响程序的性能。
>
> 建议使用下面将要介绍的引用传参的方式





### 2.2 传引用参数

关于引用的内容可以参考第二章。

相比于值传递，引用传递是直接将对象传入函数，没有拷贝带来的性能损失，所以在函数体中改变通过引用传入的形参的值，会改变实参

```cpp
//这个函数，调用之后会实参的值会变成0
void reset(int &i)
{
    i = 0;	
}

int main()
{
    int j = 42;
    reset(j);
    cout << "j = " << j << endl;	//j的值是0
    return 0;
}
```



> <span style="background: yellow">建议使用引用传参</span>， 对于不需要改变引用形参的值，可以将其声明为常量引用



#### 引用形参的一种用法

我们知道函数只能返回一个，然而有时函数需要同时返回多个值，引用形参为我们一次返回多个结果提供了有效的途径。就是我们把需要返回的一个或多个需要返回的值声明为引用形参，在函数体把值写入到引用形参中。

```cpp
string::size_type find_char(const string &s, char c,
                           string::size_type &occurs)
{
    auto ret = s.size();
    occurs  = 0;
    for (decltype(ret) i = 0; i != s.size(); ++i) {
        if (s[i] == c) {
            if(ret == s.size())
                ret = i;
            ++occurs;
        }
    }
}
```



> 上面的示例是统计一个字符串在某个字符出现的次数，以及第一次出现的位置



### 2.3 const形参和实参

这里涉及到顶层const和底层const的概念。简单的回顾一下，顶层const作用于对象本身，比如 `int *const p = nullptr;` 是一个指针变量，这里的const作用于指针，即指针的指向不能改变，是一个顶层const

- 用实参初始化形参是会忽略顶层const，也就是说对于一个含有顶层const的形参，可以给它传递常量和非常量对象
- 我们可以使用非常量初始化一个底层const对象，但是反过来不行

```cpp
int i = 42;
const int *cp = &i;		//正确，使用非常量对象初始化底层cost变量
const int &r = i;		//正确，使用非常量对象初始化底层cost变量， 引用不存在顶层const
const int &r2 = 42;		//正确
int *p = cp;			//错误，将一个含有底层const的变量赋值给普通变量
int &r3 = r;			//错误， r3的类型和r的类型不匹配
int &r4 = 42;			//错误，不能用字面值初始化一个非常量引用
```

将变量的初始化规则应用到参数传递

```cpp
//函数原型: int reset(int &i); 和 int reset(int *ip);
int i = 0;
const int ci = i;
string::size_type ctr = 0;
reset(&i);		//调用形参类型为 int* 的 reset
reset(&ci);		//错误，int *ip = ci;
reset(i);		//调用行参类型为 int& 的 reset
reset(ci);		//错误， int *ip = ci;
reset(42);		//错误  int &i = 42;
reset(ctr);		//错误, 类型不匹配
```



**尽量使用常量引用**

- 但我们把一个不需要改变的形参定义成非常量的话，会给人误导
- 定义成常量引用的形参，调用着可以传递常量和非常量实参





### 2.4 数组形参

数组不能以值传递的方式传入，以下三种写法表达的意思是一样的

```cpp
//传入的都是 const int* 
void print(const int*);
void print(const int[]);
void print(const int[10]);		//这里的维度是希望传入含有10个元素的数组的指针，实际不一定
```



> 以上三个函数的形参虽然表现形式不一样，但是他们是等价的，都是`const int*` 类型形参



#### 管理指针形参

和其他使用数组的代码一样，以数组作为形参的函数也必须确保使用数组时不越界，下面介绍三种常用的管理指针形参的技术

##### 使用标记指定数组长度

指定一个结束的标记，使用这种方法的典型示例是c风格的字符串，它是以 `\0` 为结束标志，通常只要读取到 `\0`  就是读取到一个完整的字符串

```cpp
void print(const char *cp)
{
    if(cp)
        while(*cp)
            cout << *cp++;	//输出当前字符指针向前移动一个位置
}
```



> 这种方法只适用于那些有明显结束标记且该标记不会与普通数据混淆的情况，但是像int 这样所有取值多是合法值的数据就不太有效



##### 使用标准库规范

给函数传递指向数组首元素和尾后元素的指针，这种方法受到了标准库技术的启发

```cpp
void print(const int *beg, const int* end)
{
    while(beg != end)
        cout << *beg++ << endl;
}
```



##### 显式的传递一个表示数组大小的形参

在形参中显式的声明一个数组大小的形参

```cpp
void print(const int ia[], size_t size)  //size 是数组的大小
{
    for (size_t i = 0; i != size; ++i) {
        cout << ia[i] << endl;
    }
}
```



#### 数组引用形参

c++允许将变量定义成数组的引用，同样形参也可以是数组的引用

```cpp
//形参是数组的引用，维度是类型的一部分
void print(int (&arr)[10])
{
    for (auto elem : arr)
        cout << elem << endl;
}
```



> 上面函数可传入 `int arr[10]` 类型，形参中维度是类型的一部分
>
> `&arr` 两端的括号不能少，下面两个函数定义不等价
>
> `f(int &arr[10])`   		//错误，形参是引用类型的数组，不存在这种类型
>
> `f(int (&arr)[])`			//正确，arr是具有10个整数类型数组的引用





#### 传递多维数组

```cpp
void print(int (*martrix)[10], int rowSize) { /*...*/}
```

上述语句将matrix声明成指向含有10个整数的数组的指针， `*matrix` 两端的括号不可少

```cpp
int *matrix[10];		//10个整型指针组成的数组， 这是个数组变量
int (*matrix)[10];		//指向含有10个整型的数组的指针， 这是个指针变量
```







### 2.5 main: 命令行选项

`main` 函数是可以带参数的，我们在命令输入的命令就是传递到main函数中，假设main函数位于可执行文件 `prog` 之内，我们可以向程序传递下面的选项：

```shell
$ prog -d -o ofile data0
```

这些命令可以通过两个形参传递给main函数

```cpp
//main函数带形参的两种形式，这两种形式是等价的
int main(int argc, char *argv[]) { ... }
int main(int argc, char **argv) { ... }
```

`argc` 表示命令行参数的个数，包括可执行程序本身的文件名，`argv` 存放命令行参数

`prog -d -o ofile data0` 命令， argc是5， argv内容为：

`argv[0] = "prog ";`

`argv[1] = "-d";`

`argv[2] = "-o";`

`argv[3] = "ofile";`

`argv[4] = "data0";`



> 当我们需要使用命令行输入的参数时，从`argv[1]` 开始读取，`argv[0]` 保存的是程序名



### 2.6 含有可变形参的函数

#### 省略符形参

省略符形参是C语言的标准，在C++中也是适用的，它的形式有以下两种

```cpp
void foo(parm_list, ...);
void foo(...);
```

<span style="background: yellow">省略符形参只能出现在形参列表的最后一个位置</span>

> 第一种形式指定了 `foo` 函数的部分形参的类型，这些形参和正常的形参一样。省略符形参所对应的传入的实参无须类型检查。在第一种形式中，形参声明后的逗号是可选的。



#### initializer_list 形参

<span style="border:2px solid Red">C++11</span> `initializer_list` 是一种标准库类型，改类型定义在同名的头文件中，它提供的操作如表

![image-20211228182857202](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211228182857202.png)

使用示例：

```cpp
void error_msg( initializer_list<string> il)
{
    for (auto beg = il.begin(); beg != il.end(); ++beg)
        cout << *beg << " ";
    cout << endl;
}
```



> `initializer_list` 是一个泛型类，使用时需指定类型，所以传递的多个参数必须是同一种类型
>
> 除了initializer_list 之外，函数也可以有其他的形参





## 3. 返回类型和return语句

`return` 语句终止当前正在执行的函数并控制返回到调用该函数的地方。return语句有两种形式

```cpp
return;		//用于无返回值的函数
return expression;	//用于有返回值的函数
```



### 3.1 无返回值函数

没有返回值的 `return` 语句只能用在返回类型是 `void` 的函数中。返回 void 的函数不要求非得有 return语句，这类函数的最后一句后面会隐式地执行 return，return可以放在函数内的其他位置，表示提前结束函数

```cpp
void swap(int &v1, int &v2)
{
    if(v1 == v2)
        return;
    int tmp = v2;
    v2 = v1;
    v1 = tmp;
    //此处无须显示的return语句
}
```



> 返回值为 `void` 的函数体中不可以使用 `return experssion;` 返回语句



### 3.2 有返回值函数

示例

```cpp
bool str_subrange(const string &str1, const string &str2)
{
    if(str1.size() == str2.size())
        return str1 == str2;
    auto size = (str1.size() < str2.size())
        		? str1.size() : str2.size();
    for(decltype(size) != 0;  i != size; ++i) {
        if(str1[i] != str2[i])
            return;		//error1
    }
    //errro2
}
```



> error1，要求返回bool类型
>
> error2，前面的两个return可能不会执行，导致最后没有return语句



<span style="background: yellow">不要返回局部对象的引用或是指针</span>

```cpp
const string &manip()
{
    string ret;
    if (!ret.empty())
        return ret;		//错误：返回局部对象的引用！
    else
        return "Empty";	//错误： "Empty"是一个局部临时对象
}
```



> 局部对象的指针或是引用，在返回时就会被释放，用变量接收函数的返回值得到的是不存在的对象。
>
> 有的IDE(VS)会对这种行为做一次保留，即保留第一次使用返回的指针是可以使用，后面就不可以使用了



**引用返回左值**

```cpp
char &get_val(string &str, string::size_type ix)
{
    return str[ix];
}
int main()
{
    string s("a value");
    cout << s << endl;
    get_val(s, 0) = 'A';	//等价于 s[0] = 'A';
    cout << s << endl;
    return 0;
}
```



**列表初始化返回值**

```cpp
vector<string> process()
{
    //...
    //expected 和 actual是string对象
    if(expected.empty())
        return {};
    else if(expected == actual)
        return {"functionX", "okay"};
    else
        return {"functionX", expected, actual};
}
```





**递归**

递归是在返回语句中调用自己

```cpp
int factorial(int val)
{
    if (val > 1)
        return factorial(val-1)*val;
    return 1;
}
```

<span style="background: yellow">递归调用必须要有终止条件</span>



### 3.3 返回函数指针

数组不能拷贝，所以函数不能返回数组。不过函数可以返回数组的指针或是引用。直接定义一个返回数组的指针或是引用的函数比较烦琐，先看下使用别名的方式。

```cpp
typedef int arrT[10];	//arrT 是一个类型别名，他表示的类型是含有10个整数的数组
using arrT = int[10];	//和上面的等价
arrT* func(int i);		//使用类型别 定义一个返回10个整数的数组的指针的函数
```



#### 声明一个返回数组指针的函数

首先对下面的定义区分一下

```cpp
int arr[10];		//arr是一个含有10个整数的数组
int *p1[10];		//p1是一个含有10个整型指针的数组
int (*p2)[10] = &arr;	//p2是一个指针，它指向含有10个整数的数组
```

返回数组指针的函数的形式如下

```cpp
Type (*function(parameter_lis)) [dimension]
```

`dimension` 是指数组的维度，比如和上面使用别名定义的函数的等价形式

```cpp
int (*func(int i)) [10];
```

对于这个函数的定义，我们可以逐层的理解： `func(int i)` 表示一个名为 `func`，参数为 `int i` 的函数； `*`  表示的返回的是指针类型，`int* [10]` 表示是数组类型的指针



#### 使用尾置返回类型

<span style="border:2px solid Red">C++11</span>  新标准提供了一种简便的方式定义这样的函数，就是使用 **尾置返回类型** ，它的形式是这样的

```cpp
auto func(int i) -> int(*)[10];
```



#### 使用 decltype

还有一种情况，如果我们知道函数返回的指针将指向哪个数组，可以使用 `decltype` 关键做声明返回类型

```cpp
int odd[] = {1, 3, 5, 7, 9};
int even[] = {0, 2, 4, 6, 8};
//返回一个指针，该指针指向含有5个整数的数组
decltype(odd) *arrPtr(int i)  //decltype(odd) 后需要加 * 表示对应的指针类型
{
    return (i % 2) ? &odd : &even;	//返回一个指向数组的指针
}
```





## 4. 函数重载

如果同一作用域内的几个函数名字相同但形参列表不同，我们称之为**重载函数**， 下面的几个参数列表不同的函数

```cpp
void print(const char *cp);
void print(const int *beg, const int *end);
void print(const int ia[], size_t size);
//函数调用时，编译器会根据传入的参数类型调用不同的函数
int j[2] = {0, 1};
print("Hello world");		//调用 print(const char*)
print(j, end(j) - beging(j));	//调用print(const int*, size_t)
print(begin(j), end(j));		//
```



#### 定义重载函数

重载函数唯一区分的指标就是形参列表的数量和类型，<span style="background: yellow">只有返回类型不同的函数不是重载函数</span>

下面三组是错误的重载函数

```cpp
int print(const int&);
void print(const int&);		//错误，只有返回类型不同的不是重载函数

int print(const int& a);
int print(const int&);		//错误，只是省略形参名，类型还是和上面的一样

typedef int Integer;
void print(const int&);
void print(const Integer&);	//错误，Integer是int的别名

```



<span style="background: yellow">重载函数虽然函数名称一样，但是它们是完全不同的函数（函数签名不同）</span>



#### 重载和const形参

**顶层const是无法区分形参** ，所以顶层cosnt形参无法实现重载

```cpp
int print(int);
int print(const int);		//和上面声明等价，重复声明int print(int);

int print(int*);
int print(int* const);		//和上面声明等价，重复声明int print(int*);
```



如果形参是某种类型的指针或引用，则通过区分其指向的是常量对象还是非常量对象可以实现函数重载，此时的const是底层const：

```cpp
int print(int&);			//函数作用于int的引用
int print(const int&);		//重载函数，作用于常量引用

int print(int*);			//作用于int类型的指向
int print(const int*);		//重载函数，作用于指向常量的指针
```

> 在本章的第2节中讲函数的参数时，有提到形参是常量对象的函数既可以传入常量对象，也可以传入非常量对象，比如上面两组函数中的第二个，它们可以传入常量对象也可传入非常量对象。
>
> 在这里，因为它们都有非常量形参的重载函数，那么在传入非常量对象时编译器会优先选用非常量版本的函数。



#### const_cast和重载

TODO



#### 调用重载的函数

当调用重载函数时有三种可能的结果

- 编译器找到一个于实参最佳匹配的函数，并生成调用改函数的代码。
- 找不到任何一个函数与调用的参数匹配，此时编译器发出无匹配的错误信息。
- 有多于函数可以匹配，但是每一个都不是明显的最佳选择。此时也将发生错误，称为二义性调用。



### 4.1重载与作用域

<span style="background: yellow">不要在某个语句块（函数体）的内部声明和外部名字一样的变量和函数！！！</span>

```cpp
string read();
void print(const string&);
void print(double);
void fooBar(int val)
{
    bool read = false;
    string s = read();		//错误，此时的 read 是bool变量
    void print(int);		//新作用域,隐藏了之前的print
    print("vlaue: ");		//错误，void print(const string&);被隐藏了
    print(ival);			//正确，调用 void print(int);
    print(3.14);			//正确，调用 void print(int); void print(double);被隐藏了
}
```





## 5. 特殊用途语言特性

### 5.1 默认实参

有些时候，我们调用函数时，某些形参的值总是被赋予同样的值，只是在少数情况下需要要不同的值。这时我们可以把这样的形参赋予一个默认的值

```cpp
//一个创建窗口的函数，窗口的默认高80，宽180
string screen(string name, int h = 80, int w = 180);  
//调用
screen("window1");			//不传入h和w，使用默认值
screen("window2", 100);		//只传入h, w使用默认值
```

> 注意：
>
> - 默认形参必须定义在形参列表的最后
> - 实参是按位置解析的，比如需要改变 w 的值，那么h的值也必须传入





### 5.2 内联函数和constexpr函数

#### 内联函数

普通的函数调用，在进入函数之前，需要把当前的状态和数据存储起来，函数返回之后，再把状态和数据读出来，继续从调用点执行。这种执行方式对于规模小的函数，整个过程比较麻烦，毕竟函数体就一两句代码，还需要这么费事的去执行。对于这种情况，我们可以把它声明成**内联函数**，内联函数不会有调用的过程，而是直接再调用点把函数体内的语句嵌入进来。

声明内联函数

```cpp
inline const string &
shorterString(const string &s1, const string &s2)
{
    return s1.size() <= s2.size() ? s1 : s2;
}

int main()
{
    string s1 = "hello";
    string s2 = "world";
    
    cout << shorterString(s1, s2) << endl; //等价 cout<< s1.size() <= s2.size() ? s1 : s2 << endl
    
    return 0;
}
```



> 内联说明只是给编译器的一个建议，编译器可以选择不执行内联的操作。比如说把一个100行的函数声明为一个内联函数，那么编译器很大可能不会执行内联操作。



### constexpr函数

在第2章中有讲到，使用`constexpr`关键字定义常量，并且可以使用函数的返回值初始化定义的常量。这里使用的函数就是 `constexpr函数`, 语法形式如下

```cpp
constexpr int new_sz() { return 42; }
constexpr int foo = new_sz();	//用constexpr函数的返回值初始化一个constexpr变量
```

constexpr函数 在编译阶段就已经计算出了返回值，对于constexpr函数的调用是直接用计算的值替代的。为了编译过程随时展开，constexpr函数被隐式地指定为内联函数。

>  constexpr函数需遵循：
>
>  - 函数的返回类型及所有的形参类型都得是字面值类型
>  - 函数体中必须有且只有一条 return 语句

<span style="background: yellow">内联函数和constexpr函数通常定义在头文件中</span>





### 5.3 调试帮助

#### assert预处理宏

`assert`是一种预处理宏，所谓预处理宏其实是一个预处理变量，它的行为有点类似于内联函数。assert宏使用一个表达作为它的条件：

```cpp
assert(expr);
```

首先对 `expr` 求值，如果表达式为假（即0），assert输出信息并终止程序的执行。如果表达式为真（即非0），assert什么也不做。

assert宏定义在`cassert`头文件中，预处理名字由预处理器而非编译器管理，所以我们可以直接使用预处理名字而无需提供 using 声明。

assert宏常用于检查<span style="background: yellow">“不能发生”</span>的条件。例如，一个对输入文本进行操作的程序可能要去所以给定单词的长度都大于某个阈值。此时程序可以包含一条如下所示的语句：

```cpp
assert(word.size() > threshold);
```



#### NDEBUG 预处理变量

assert的行为依赖于一个名为 `NDEBUG` 的预处理变量的状态。如果定义了 NDEBUG，则assert什么也不在。默认状态下没有定义NEDBUG。定义NDEBUG既可以在程序中定义，如下

```cpp
#define NDEBUG
```

也可以在编译是加上NDEBUG这个参数

```shell
$ CC -D NDEBUG main.c	#等价于 #define NDEBUG
```



除了assert之外，我们也可以使用NDEBUG编写自己的条件调试代码

```cpp
void print(const int ia[], size_t size)
{
#ifndef NDEBUG
    cerr << __func__ << ": array size is " << size << endl;
#endif
    //...
}
```

上面代码中 `__func__` 是编译器定义的变量，它是 const char 的一个静态数组，存放当前函数的名字，除了这个，还有其他变量

- `__FILE__` 存放文件名的字符串字面值
- `__LINE__` 存放当前行号的整型字面值
- `__TIME__` 存放文件编译时间的字符串字面值
- `__DATE__` 存放文件编译日期的字符串字面值

我们可以利用上面这些变量提供错误的详细信息



## 6. 函数匹配

重载函数的匹配，在形参数量不同的情况下，我们可以很容易确定调用的是那个函数，但是当重载函数的参数数量一样，只是类型不同的情况下，这就变得有点困难了

```cpp
void f();
void f(int);
void f(int , int);
void f(double, double = 3.14);
f(5.6);			//调用 void f(double, double)
```





### 6.1 实参类型转换

为了确定最佳匹配，编译器将实参类型到形参类型的转换划分成几个等级，具体如下所示

1. 精确匹配，包括以下情况：
   - 实参类型和形参类型相同
   - 实参从数组类型或函数类型转换成对应的指针类型（函数指针下小节会讲）
   - 向实参添加顶层const或者从实参中删除顶层const
2. 通过const转换实现的匹配
3. 通过类型提升实现的匹配
4. 通过算术类型转换实现的匹配
5. 通过类类型转换实现的匹配







## 7. 函数指针

**函数指针**是指针，它指向的是函数。函数的类型由它的返回类型和形参类型共同决定，与<span style="background: yellow">函数名无关</span>。例如：

```cpp
bool lengthCompare(const string &, const string &);
//上面函数的类型是
bool(const string&, const string&)
//声明一个对应的函数指针
bool (*pf)(const string&, const string&);	//指针pf是函数指针，未初始化
```

<span style="background: yellow">*pf两端的括号不能少</span>

```cpp
//没有括号的话是定义一个名为pf的函数，返回值是 bool*
bool *pf(const string&, const string&);
```



#### 使用函数指针

函数名和数组名一样，直接使用名字（不用取地址符）会自动地转换为指针，例如

```cpp
pf = lengthCompare;		//pf 指向名为lengthCompare的函数
pf = &lengthCompare;	//和上面的等价，取地址符是可选的
```

此外，我们还能直接使用指向函数的指针调用该函数，无须提前解引用指针：

```cpp
bool b1 = pf("hello", "wolrd");			//调用lengthCompare函数
bool b1 = （*pf）("hello", "wolrd");		//等价的调用
bool b3 = lengthCompare("hello", "wolrd");	//另一个等价的调用
```

函数指针也可以赋予 `nullptr` , 函数指针赋值要和定义的类型一致才可以赋值。



#### 函数指针形参

和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针

```cpp
//第三个参数是函数类型，它会自动转换成函数的指针
void useBigger(const string &s1, const string &s2,
               bool pf(const string&, const string&))；
//等价声明
void useBigger(const string &s1, const string &s2,
               bool (*pf)(const string&, const string&))；
//函数调用
useBigger(s1, s2, lengthCompare); //函数名lengthCompare自动转换为函数指针
```

使用别名简化写法

```cpp
//Func和Func2是函数类型
typedef bool Func(const string&, const string&);
typedef decltype(lengthCompare) Func2;		//等价类型
//FuncP 和FuncP2是指向函数的指针
typedef bool(*FuncP)(const string&, const string&);
typedef decltype(lengthCompare) *FuncP2;	//等价的类型

//使用上面的别名声明带函数指针形参的函数
void useBigger(const string&, const string&, Func);
void useBigger(const string&, const string&, FuncP2);
```



#### 返回指向函数的指针

和数组类似，我们不能返回函数，但是可以返回函数指针

```cpp
//定义别名，简化写法
using F = int(int*, int);	//F是函数类型，不是指针
using PF = int(*)(int*, int);	//PF是函数指针类型

//声明返回函数指针的函数
PF f1(int);		//正确， PF是指针函数，f1返回指向函数的指针
F f1(int);		//错误，F是函数类型，不能返回函数
F *f1(int);		//正确，显示地指定返回类型是指向函数的指针

//原始的不使用别名声明方式
int (*f1(int))(int*, int);
```

原始的声明是从里向外读， `(*f1(int))` 表示 `f1` 是一个函数，参数是`int` ,返回的是指针 `*` , 指针指向的是函数类型 `int(int*, int)`

在前面我们声明返回数组指针的函数使用过返回类型后置，同样这里也适用

```cpp
auto f1(int) -> int(*)(int*, int);
```

使用 `decltype` 自动检测类型

```cpp
string::size_type sumLength(const string&, const string&);
decltype(sumLength) *getFcn(const string&);
```

<span style="background: yellow">decltype作用于函数时返回的时函数类型，所以需要显示地加上 `*` 声明为指针</span>
