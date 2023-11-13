---
title: "第3章 字符串、向量和数组(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2023-03-11
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch3"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/2021-12-30.jpg
    caption: "" 
    alt: ""
    relative: false
---

## 1. 命名空间的using声明

在前面的示例程序中，输入和输出都是写成 `std::cin` , `std::cout` , 我们可以在使用前使用 `using` 声明

```cpp
#include <iostream>

using std::cin;

int main()
{
    int i;
    cin >> i;		
    cout << i;		//错误，只声明了 cin	
    std::cout << i;
    
    return 0;
}
```

在程序中使用到的命名空间成员我们多需要声明。

> 实际更常见的做法是使用 `using namespace std;` 把整个命名空间都引入，这样std命名空间下的成员都可以使用了。
>
> 注意：头文件中尽量不要引入整个命名空间，因为当在其他文件包含头文件实际是将这个头文件中的内容复制到该文件中，这样可能会和自己写的一些类名冲突。



## 2. 标准库类型 string

`string`的使用需要包含一个头文件和命名空间

```cpp
#include <string>
using namespace std;
```



### 2.1 定义和初始化string对象

```cpp
string s1;				//默认初始化， s1是一个空串
string s2(s1);			//s2是s1的副本
string s2 = s1;			//等价于s2(s1),s2是s1的副本
string s3("value");		//s3是字面值 "value" 的副本，不包含最后的空字符
string s3 = "value";	//同上
string s4(n, 'c');		//n个'c'拼成的串
```



### 2.2 string对象上的操作

```cpp
os << s;			//将s写道输出流os中，返回os
is >> s;			//从is中读取字符串赋给s,字符串以空包分隔，返回is
getline(is, s);		//从is中读取一行赋给s,返回is
s.empty();			//s为空返回true，否则返回false
s.size();			//返回s中字符的个数
s[n];				//返回s中第n个字符的引用，位置从0开始
s1 + s2;			//返回s1和s2连接后的结果
s1 = s2;			//用s2的副本代替s1中原来的字符
s1 == s2;			//判断是否一致
s1 != s2;			//判断是否不一样
< , <=, >, >=		//通过字典中的顺序比较，对字母大小写敏感
```

对于 `+` 操作， 当把 string 对象和字符字面值及字符串字面值混在一条语句中使用时，必须确保每个加法运算符的两侧的运算对象至少有一个是string:

```c++
string s3 = s1 + ", ";		// 正确： 把一个string对象和一个字面值相加
string s5 = "hello" + ", ";	// 错误：两个运算对象都不是 string
string s6 = s1 + ", " + "world"; // 正确， 第一个加法的结果是string
string s7 = "hello" + ", " + s2; // 错误，先执行第一个加法，字符字面值没有定义+操作
```



>为了与C兼容，C++语言中的字符串字面值并不是标准库类型 sting 的对象

### 2.3 处理string对象中的字符

在头文件 `cctype` 中定义了一组相关的函数

![image-20211216191219702](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211216191219702.png)



> `cctype` 是c语言的头文件，在c++中包含c的头文件有两种形式 
>
> - `#include <ctype.h>`  和c语言一样
> - `#include <cctype>` 不加 `.h` 而是在前面加一个 `c`



**基于范围的for语句**

```cpp
//语法
for (declaration : expression)
    statement
    
//示例
string str = "helloWorld";
for (auto s : str)  //使用aotu自动类型推导
{
    cout << s << endl;
}
//如果需要改变str中字符，用引用
for (auto &c : str)
{
    c = toupper(c);		//c是引用
}
cout << str << endl;
```



> 基于范围的for，只适用于可迭代的对象



## 3.  标准库类型 vector

使用vector需要包含下面的头文件和声明命名空间

```cpp
#include <vector>
using std::vector;		//或者 using namespace std; 引入std命名空间所有的成员
```



`vector` 类似于数组，但是比数组用于更多的操作。`vetorc` 是一个模板类，所谓模板就是该类内部中的属性没有指定某种特定的数据类型，我们可以在声明 vector时指定数据类型（包括基本类型和自定义类型）

```cpp
vector<int> ivec;		//ivec时int类型的对象集合
vector<Sales_item> Sales_vec;	//Sales_vec是Sales_item类型的对象集合
vector<vector<string>> file;	//该向量的元素是vector对象
```

> <span style="border:2px solid Red">C++11</span>  在 C++11之前 `vector<vector<string>> file;` 要写成 `vector<vector<string> > file;`
>
> 后面的两个 `>` 之间是有一个空格的



### 3.1 定义和初始化vector对象

定义vector对象的常用方法

![image-20211216194615980](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211216194615980.png)

> 注意 `()` 和 `{}` 初始化vector对象的区别



### 3.2 vector操作

vector常用的操作

![image-20211216195244036](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211216195244036.png)



```cpp
vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9};
for (auto &i : v)		//使用引用可以改变v中的值， 
{
    i *= i;				//计算平方
}
for (auto i : v)		//普通
{
    cout << i << " ";	
}
```



> vector使用注意事项：
>
> 1. 不要使用下标添加元素
> 2. 不要在范围for中改变vector的大小（比如增加元素等操作）



## 4. 迭代器

迭代器可以理解成一种特殊的指针，他有指针类似的操作，除此之外还有自己独特的一些操作。



### 4.1 使用迭代器

通常是使用 `being` 和 `end` 方法获取迭代器，`begin` 返回第一个元素，`end` 返回最后元素的==下一个位置==，所以`end` 返回的迭代器叫做**尾后迭代器**

```cpp
auto b = v.begin(), e = v.end();	//b和e类型一样，具体类型后面有说明
```

> 如果容器为空，则begin和end返回的是同一个迭代器，都是尾后迭代器



迭代器和指针类似，所以指针有的运算符，迭代器基本也有

![image-20211217154623555](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211217154623555.png)



```cpp
string s("some string");
for (auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
{
    *it = toupper(*it);		//将当前字符改成大写形式
}
```



**迭代器类型**

vector 和 string对应的迭代器类型

```cpp
vector<int>::iterator it1;	
string::iterator ii2;

vector<int>::const_iterator it3;
string::const_iterator it4;
```

> `it1`, `it2` 是对应类型的迭代器，可以读写
>
> 对于常量对象（用const修饰的对象）需要使用 `const_iterator` ,不是常量对象也可使用 `const_iterator` ，只是const迭代器只能读不能修改元素。



**begin和end**

```cpp
vector<int> v;
const vector<int> cv;
auto it1 = v.begin();		//it1的类型是vector<int>::itrerator
auto it2 = cv.begin();		//it2的类型是vector<int>::const_iterator
```



**解引用和成员访问**

```cpp
//it是vector对象的迭代器
(*it).empty();		//解引用it,得到vector对象，调用对象的empty方法
*it.empty();			//错误，这里是访问it中empty的方法，而it中并没有这个方法
it->empty();		//使用箭头运算符和 (*it).empty();一样
```



### 4.2 迭代器运算

`vector` 和 `string` 迭代器支持的运算

![image-20211217160907463](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211217160907463.png)



## 5. 数组



### 5.1 数组的定义和初始化

```cpp
unsigned cnt = 42;
constexpr unsigned sz = 42;
int arr[10];
int *parr[sz];		//42个整型指针的数组
string bad[cnt];	//错误，cnt不是常量表达式
string strs[get_size()];  //get_size是constexpr时正确；否则错误
```



#### 字符数组的特殊性

```cpp
char a1[] = {'C', 'P', 'P'};		//没有空字符
char a2[] = {'C', 'P', 'P', '\0'};	//手动添加空字符
char a3[] = "CPP";					//自动添加空字符
const char a4[3] = "CPP";			//错误，没有空间可存放空字符
```



#### 复杂的数组声明

```cpp
int arr[10];
int *ptrs[10];				//可存放10个 int* 类型
int &refs[10] = /* ? */ ;	//错误，不存在引用的数组
int (*Parray)[10] = &arr;	//Parray是一个指向 int[10] 类型的指针 
int (&arrRef)[10] = arr;	//arrRef是一个 int[10]类型的引用
int *(&arry)[10] = ptrs;	//arry是一个引用，指向的是 含有10个int*的数组
```

对于上面这些声明，使用<span style="background: yellow">从内至外</span>的方法读比较合适。



> 对于数组的访问，需要注意的是下标越界问题，数组越界编译器不会报错的，只有在运行是才会出现错误。



#### 使用数组初始化 vector对象


```cpp
int int_arr[] = {0, 1, 2, 3, 4, 5};
vector<int> ivec(begin(int_arr), end(int_arr));
```



### 5.2  指针和数组

在c++中，数组名就是指针，保存的是数组变量的首地址

```cpp
int arr[10];
//p1和p2是等价的
int *p1 = arr;
int *p2 = &arr[0];
```



> 数组就是指针，在把数组作为参数传入函数中，必须手动维护一个数组大小的变量
>
> 因为传入的数组是指针，无法获取到数组的长度



<span style="border:2px solid Red">C++11</span> **标准库函数 `begin` 和 `end`**

这两个函数可以获取数组的头元素和尾后元素（最后一个元素的后面的地址）

```cpp
int ai[] = {0, 1, 2, 3, 4, 5, 6, 7};
int *beg = begin(ia);	//指向ia首元素的指针
int *last = end(ia);	//指向ia尾元素的下一个位置的指针
```



### 5.3 C风格的字符串

在c语言中通常是 `const char* ` 表示字符串，并且以空字符 `\0` 为结尾。

```cpp
const char* str = "heo\0ll";	//定义一个c风格的字符串，并且在中间加入一个结束符
//因为结束符的原因，c的函数库一些操作会出现意想不到的结果
strlen(str);	//计算str的长度，结果是3，计算方式是遇到空字符结束
```



> c中的字符串是以空字符 `\0` 判断字符串结束，如果我们自己的定义的字符数组或是字符常量中没有 `\0` 或是字符中间有 `\0` 都不能得到正确的结果



**C和C++字符串的转换**

在C++中是定义了一个 `string` 类作为字符串类型,  `string` 类中重载了系列的运算符，所以从c 风格 （`const char*`） 到  c++风格（`string`）的转换都是自动完成的。以下是一些注意点

```cpp
//从 c到 c++   const char* ---> string
//1. 使用字符串字面v初始化string类型，本章第二节
string s3("value");		//s3是字面值 "value" 的副本，不包含最后的空字符
string s3 = "value";	//同上
//2. string 重载了 + 运算符，可以直接拼接， s1，s2是string类型
string s4 = s1 + ", ";				//正确
string s5 = "hello" + ", ";			//错误
string s6 = s1 + ", " + "world";	//正确
string s7 = "hello" + ", " + s2;	//错误

//从 c++到 c,  string ---> const char* 
//在string类中定义了一个c_str的成员函数，可以返回 const char*
char *str = s;		//错误：不能直接使用stringdvx初始化char*
const char *str = s.c_str();	//正确，s是一个string对象，调用c_str()方法
```



> 说明：在c++的string类种是重载了`+`, 改运算符返回的也是string类型，分析
>
> 1. s4, `s1`是string类型，会调用重载的 `+`
> 2. s5, `hello` 和 `,` 都是 `const char*` 类型，该类型并没有定义 `+` 运算
> 3. s6,  `(s1 + ", ")`   和s4 一样，得到的是一个临时的 string， 再   `+ "world"`
> 4. s7,  ` "hello" + ", "` 的错误和s5一样





## 6. 多维数组

略
