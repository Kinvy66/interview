---
title: "第2章 变量和基本类型(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2023-03-11  
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch2"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/2021-12-28.jpg 
    caption: "" 
    alt: ""
    relative: false
---





# 第2章	变量和基本类型

##  1. 基本内置类型

### 1.1 算术类型

算术类型分为两类： **整型** （包括字符和布尔类型在内）和 **浮点型**

![image-20220606100005905](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220606100005905.png)



### 1.2 类型转换

当在程序中使用一种类型而其实对象应该取另一种类型时，程序会自动进行类型转换。

```cpp
bool b = 42;		// b 为真
int i = b;			// i 的值为1
i = 3.14;			// i 的值为3
double pi = i;		// pi 的值为 3.0
unsigned char c = -1; 	// 假设 char 占8bit， c 的值为255
signed char c2 = 256;	// 假设 char 占8bit, c2 的值是未定义的
```

#### 含有无符号类型的表达式

无符号和有符号数混用带来的问题

示例1：

```cpp
unsigned u = 10;
int i = -42;
std::cout << i + i << std::endl;   // 输出 -84
std::cout << u + i << std::endl;   // 如果 int 占 32bit，输出 4294967264
```

当一个算术表达式中既有无符号数又有 `int` 值时，那个 `int` 值就会转换成无符号数

```cpp
unsigned u1 = 42, u2 = 10;
cout << u1 - u2 << endl;
cout << u2 - u1 << endl;
```

示例2：

```cpp
for (int i = 10; i >= 0; --i)
    std::cout << i << std::endl;
// 错误： 变量u永远不会小于0，循环条件一直成立
for (unsigned i = 10; i >= 0; --i)
    std::cout << i << std::endl;
```

### 1.3 字面值常量

一个形如 42 的值被称作**字面值常量** ， 每个字面值常量都对应一种数据类型，字面值常量的形式和值决定了它的数据类型。

#### 整型和浮点型字面值

整型

```cpp
20		// 十进制
024		// 八进制
0x14	// 	十六进制
```

浮点型

```
3.14159		3.14159E0		0.		0e0		.001
```



#### 字符和字符串字面值

```cpp
'a'		// 字符字面值
"Hello World!"  // 字符串字面值
```

字符字面值的类型是 `char` ，字符串字面值是字符数组类型 `const char[]`



#### 指定字面值的类型

我们可以通过下表的符号来指定字面值的类型

![image-20220606105006699](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220606105006699.png)

示例：

```cpp
L'a'		// 宽字符型字面值，类型是 wchar_t
u8"hi!"		// utf-8 字符串字面值
42ULL		// 无符号整型字面值，类型是 unsigned long long
1E-3F		// 单精度浮点型字面值，类型是 float
3.14159L	// 扩展精度浮点类型字面值，类型是 long double
```



#### 布尔字面值和指针字面值

`true` 和 `false` 是布尔类型的字面值：

```cpp
bool test = false;
```

`nullptr` 是指针字面值。

## 2. 变量

变量提供一个具名的、可供程序操作的存储空间。

###　2.1 变量定义

C++变量的定义要指定变量的类型。

```cpp
//变量定义并初始化
int a = 2;
//变量定义
int b;
//变量赋值
b = 1;
```



> 在C++中变量的初始化和赋值是有区别的， `int a = 2;` 的`= ` 运算符表示的是初始化，
>
> 而在 `b=1;`  中的 `=` 是赋值。



<span style="border:2px solid Red">C++11</span>  列表初始化

```cpp
int val = 1;
int val = {0};
int val{0};
int val(0);
```

使用 `{}` 来初始化变量，称为列表初始化，这种方式初始化变量，编译器化检查初始化的变量是否符号定义的变量的类型

```cpp
long double pi = 3.14;
int a{pi};	//错误，编译器会检查类型，无法通过编译
```

> 总结：C++变量初始化的语法形式有三种：`=` , `()` , `{}`



**默认初始化** ，<span style="background: yellow">定义变量时没有初始化变量的值，则变量会被默认初始化。</span>默认初始化的值取决于变量定义的类型。<span style="background: yellow">定义在函数体内的局部变量和类中的成员属性是不会被初始化的 </span>（不同编译器的实现可能会不同）， 所以不要试图使用任何方式去访问这些变量。

```cpp
#include <iostream>
using namespace std;

int init;
struct A {
    int m;
    void print()
    {
        cout << m << endl;
    }
};

void test()
{
    int un_init;
    cout << un_init << endl;
}

void test01()
{
    A a;
    a.print();
}

int main()
{
    test01();
    cout << init << endl;
    return 0;
}
```





### 2.2 声明和定义

**声明**，使程序知道变量（对象）的存在

**定义**，负责创建于名字关联的实体 



```cpp
extern int i;	//声明i
extern int i = 1;	//错误，给i赋值extern失效
int j;		//声明并定义j
```

<span style="background: yellow">变量能且只能被定义一次，但是可以被多次声明</span>

`extern` 的使用参考：https://www.jianshu.com/p/165b3410b7fa



###  2.3标识符

#### 标识符

变量命名按照规范，不要使用保留关键字。

<span style="background: yellow">命名规则，供参考：</span>

- 普通的局部变量和函数参数名使用小驼峰（第一个单词首字母小写，其他单词首字母大写）， 例： `userName`
- 全局变量前加 `g_`, 后面的按小驼峰规则 ， `g_userName`
- 静态变量前加 `s_` , 后面按小驼峰规则， `s_userName`
- 类名使用大驼峰，所有单词的首字母大写 ,  `UserManage`
- 类属性（成员变量）前面加 `m_` ,后面按小驼峰规则  ， `m_userName`
- 常量全部使用大写，多个单词用`_` 分割， `MAX_NUMBER`



### 2.4 名字的作用域

局部变量不能和全局的变量重名，嵌套的块，内部的不要和外部的重名。重名的变量采取就近原则。



## 3. 复合类型

复合类型的声明语句是由一个 **基本数据类型** 和紧随其后的一个 **声明符** 列表组成。

### 3.1 引用

**引用** 就是为变量（对象）起一个别名

```cpp
int val = 1024;
int val1 = 102;
int& refVal = val;		//refVal指向val
refVal = val1;			//refVal引用并没有改变，只是改变了refVal指向的变量val的值，val = val1
int &refVal2;			//错误，引用必须初始化
```



#### 引用定义

```cpp
int i = 1024, i2 = 2048; 	// i和i2都是 int
int &r = i; r2 = i2;		// r是一个引用， 与i绑定在一起，r2是int
int i3 = 1024, &ri = i3;	// i3 是int，ri是一个引用，与i3绑定在一起
int &r3 = i3, &r4 = i2;		// r3和r4都是引用

// 类型必须一致
int &refVal4 = 10;		// 错误： 引用类型的初始值必须是一个对象
double dval = 3.14;
int &refVal5 = dval;	// 错误：此处引用类型的初始值必须是 int 型对象
```



>注意:
>
>1. 引用必须初始化，且不能改变
>2. `&` 符号可以紧靠基本类型(int), 也可以紧靠变量名
>3. 引用绑定的对象类型必须和引用类型是一致的



**以上说的引用都是左值引用，C++11还有右值引用**



### 3.2 指针

**指针** 是 “指向” 另外一种类型的复合类型。指针和引用的不同，指针本身是一个对象，允许对指针赋值和拷贝；指针无须在定义时赋初值。

指针的定义，将声明符写成 `*d` 的形式，其中 `d` 是变量名，如果一条语句种定义类几个指针变量，每个变量前面都必须有符号 `*`

```cpp
int *p1, *p2;	// ip1和ip2都是指向int型对象的指针
double dp, *dp2;	//dp2实质性double型对象的指针，dp是double型对象

```



> 指针使用建议：
>
> 1. 指针定义是可以不初始化，但建议定义时初始化，如果没有想好指向哪个变量，可以初始化为空指针
> 2. 操作指针时，须确定操作的不是空指针和野指针（无效指针）



#### 获取对象的地址

指针存放某个对象的地址，要想获取该地址，需要使用 **取地址符** （操作符`&`）：

```cpp
int ival = 42;
int *p = &ival;		// p 存放变量ival的地址，或者说p是指向变量ival的指针
```

指针类要和所指对象类型匹配

```cpp
double dval;
double *pd = &dval;
double *pd2 = pd;

int *pi = pd;		// 错误
pi = &dval;			// 错误
```



> 引用不是对象，没有实际地址，所以不能定义指向引用的指针

#### 指针值

指针值（即地址）应属于下列4种状态之一：

1. 指向一个对象
2. 指紧邻对象所占空间的下一个位置
3. 空指针，意味着指针没有指向任何对象
4. 无效指针，也就是上述情况之外的其他值

#### 利用指针访问对象

如果指针指向一个对象，则允许使用 **解引用符** （操作符`*` ）来访问对象

```cpp
int ival = 42;
int *p = &ival;
cout << *p;		// 由符号 * 得到指针p所指向的对象，输出 42

*p = 0;			// 由符号 * 得到指针p所指向的对象，即可经由p为变量ival赋值
cout << *p;		// 输出0
```



> 解引用操作仅使用与那些切实指向了某个对象的有效指针



#### 空指针

空指针不指向任何对象，在操作指针之前必须确定为一个非空指针。下列几个生成空指针的方法

```cpp
int *p1 = nullptr;		// 等价于 int *p1 = 0;
int *p2 = 0;			// 直接将 p2初始化为字面常量0
// 需要首先 #include <cstdlib>
int *p3 = NULL;			// 等价于 int *p3 = 0;
```



#### 赋值和指针

指针变量的赋值是指将一个地址值赋值给指针变量，从而指向一个新的对象

```cpp
int i = 42;
int *pi = 0;	// pi被初始化，但没有指向任何对象
int *pi2 = &i;	// pi2 被初始化，存有i的地址
int *pi3;		// 如果 pi3 定义于块内，则pi3的值是无法确定的

pi3 = pi2;		// pi3和pi2指向同一个对象
pi2 = 0;		// 现在pi2不执行任何对象
```

区分对指针指向对象的赋值

```cpp
*pi = 0;	// ival 的值被改变，指针pi并没有改变
```



#### 其他指针操作

只要指针拥有一个合法值，就能将它用在条件表达式中。如果指针的值是 0， 条件取 `false`，任何非0的指针对应的条件值都是 `true`

```cpp
int ival = 1024;	
int *pi = 0;	// pi 合法，是一个空指针
int *pi2 = &ival;	// pi2是一个合法的指针，存放着ival的地址
if (pi)			// pi 的值是0，条件的值为false
    // ...
if (pi2)		// pi2 指向ival，它的值不为0， 条件的值是true
    // ...
```

对于两个类型相同的合法指针，还可以用相等操作符 （`==`）或不相等操作符 （`!=`）来比较它们，比较的结果是布尔类型。

#### void* 指针

指针就是一个整数，没有实际的数值大小，只是一个编号，这个编号指向的是内存中的某个地址。指针无论定义成什么基本类型，其值都是一个固定位数的整数，指针类型数据的大小取决于系统的位数，32bit的系统指针变量大小是4byte = 32 bit, 64 bit系统指针是 8 byte = 64bit。

`void*` 是一种特殊的指针类型，可用于存放任意对象的地址。它所存放的地址就仅仅是内存空间的一个地址，因为没有指定具体对象的类型，所以无法用 `void*` 指针访问所指向的地址。



```cpp
double obj = 3.14, *pd = &obj;
					// 正确： void* 能存放任意类型对象的地址
void *pv = &obj;	// obj 可以是任意类型的对象
pv = pd;			// pv 可以存放任意类型的指针
```



> 定义指定类型的指针只是为了提供操作数据时需要操作的字节数。
>
> 例如，`int` 型的指针，在使用指针改变指向的数据时，改变的是以该指针变量为首地址的4个字节内存，
>
> 同样对`int` 型指针的加或减的操作也是以4个字节为基本单位

### 3.3 理解复合类型的声明



```cpp
int i = 42;
int *p;			//p是int型的指针
int *&r = p;	//r是一个对指针p的引用

r = &i;			//r是一个指针引用，因此给r赋值&i就是令p指向i
*r = 0;			//解引用r,就是解引用指针p,将p指向的变量i的值改为0
```



> Tip: 面对一条比较复杂的指针或引用的声明语句时，从右向左读有助于弄清楚它的真实含义。

## 4. const 限定符

const 用于定义一个不能改变的变量, 所以定义时就必须初始化

```cpp
cont int bufSize = 512;		//用字面值常量初始化
cont int i = get_size();	//用函数返回值初始化， 运行时初始化
int j = 10;
cont int k = j;				//用其他变量初始化
```



> `const` 定义的变量只对本文件可见，要使其他文件也可见需使用 `extern` 



### 4.1 const的引用

```cpp
const int ci = 1024;
const int &r1 = ci;		//正确，引用r1和ci都是常量
r1 = 42;				//错误， r1 是对常量的引用
int &r2 = ci;			//错误，不能让一个常量引用指向一个常量对象
```



```cpp
int i = 42;
cont int j = 10;
const int &r1 = i;			//正确，允许将 const int&绑定到一个普通int对象
r1 = 10;					//错误，不能通过常量引用改变i的值
const int &r2 = 42;			//正确
const int &r3 = r1 * 2;		//正确
int& r4 = j;				//错误
int &r4 = r1 * 2;			//错误
```



<span style="background: yellow">组合关系</span>

|               | `int i` | `cont int i` |
| :-----------: | :-----: | :----------: |
|   `int &r `   |    ✔    |      ❌       |
| `cont int &r` |    ✔    |      ✔       |



### 4.2 指针和const

拷贝关系：

|               | `int i` | `cont int i` |
| :-----------: | :-----: | :----------: |
|   `int *p `   |    ✔    |      ❌       |
| `cont int *p` |    ✔    |      ✔       |



#### const指针



```cpp
int errNumb = 0;
int *const curErr = &errNumb;	//curErr将一直指向errNumb,不可以改变指向
const double pi = 3.14;
const double *const pip = &pi;		//pip是一个指向常量对象的常量指针
```

<span style="background: yellow">从右向左读</span>



> const 和指针
>
> - **指向常量的指针** ：指向的是一个常量，不能通过指针改变其所指的对象的值
> - **常量指针** ：指针变量本身为常量，必须初始化，一旦初始化完成，它的值（也就是存放在指针中的那个地址）就不能在改变了。

`const` 在 `*` 之后说明指针式一个常量，在 `*` 之前说明指向的是一个 `const` 对象

```cpp
const double *const pip = &pi;		//pip是一个指向常量对象的常量指针
double const * const pip = &pi;		// 和上面等价，但不建议这样写
```



### 4. 3 顶层const

**顶层const** : 表示该变量（对象）本身是常量，不可以改变

**底层const**: 表示指向的变量（对象）是一个常量



```cpp
int i = 0;
int *const p1 = &i;		//p1是指针，p1的指向不能改变，顶层
const int ci = 42;		//ci是普通变量，ci的值不能改变，顶层
const int *p2 = &ci;	//p2是一个指针，它必须指向 const int型的数据，但是本身的指向可以改变，底层
const int *const p3 = p2;	//第一个底层，第二个顶层
const int &r = ci;			//用于声明引用的const都是底层
```



<span style="background: yellow">引用类型的变量自带顶层const</span> 即引用一旦赋值（指向某个变量）就不可以在变化（指向另一个变量）



### 4.4 constexpr和常量表达式

<span style="border:2px solid Red">C++11</span>  用 `constexpr` 关键字声明一个变量，编译器会检查该表达式是否是一个常量表达式。



**constexpr和指针**

```cpp
const int *p1 = nullptr;
constexpr int *p2 = nullptr;
int *const p3 = nullptr;
```

> p2 和p3是等价的，`constexpr`修饰指针变量是被定义为顶层const

```c++
constexpr const int *pi = nullptr;
// 等价定义
const int *const pi = nullptr;
```



## 5. 处理类型

为了复杂程序更加易读易写，通常会给类型取别名，或是利用C++提供的特性自动推导复杂类型。



### 5.1 类型别名

#### typedef

传统的方法是使用 `typedef` 关键字定义类型别名

```cpp
typedef double wages;		//wages表示是double类型
typedef wages base, *p;		//base = wages = double, p = double*
//数组的别名
typedef int arrT[10];		//arrT是一个类型别名，他表示的类型是含有10个整数的数组
using arrT = int[10];		//和上面的等价
```



#### using

<span style="border:2px solid Red">C++11</span> 提供了一种新的方式，使用 `using` 

```cpp
using SI = Sales_item;		//Sales_item是一个类类型， SI表示是该类的别名
```

> 这里的 `using` 要和 `using namespace std;` 中的 `using` 区分开。后者是表示引入命名空间，类似于java和python的导包操作



#### 指针、常量和类型别名

```cpp
typedef char* pstring;
const pstring cstr = 0;		//char *const cstr = 0;
const pstring *ps;			//char **const ps;
```

<span style="background: yellow">第二行的定义不能理解成</span> `const char *cstr = 0;` 



>  `const pstring` 中 `const` 是对 `pstring` 的修饰，而 `pstring` 是一个 `char*` 类型，因此 `const petring` 是指向 char的 [常量指针](#const point) ，而并不是指向常量字符的指针



### 5.2 auto 类型说明符

<span style="border:2px solid Red">C++11</span>  `auto` 类型说明符可以让编译器分析表达式所属的类型。

```cpp
int val1 = 1, val2 = 3;
auto val = val1 + val2;		//编译器可以自动推出val为int类型
auto i = 0, *p = &i;		//正确，编译器通过字面值推出i为int,p为int*
auto sz = 0; pi = 3.14;		//错误，编译器无法推出类型， sz， pi类型不一致无法统一
```



#### 复合类型、常量和auto

- 当使用引用类型推导类型是，`auto`推导的类型是引用指向变量的实际类型

  ```cpp
  int i = 0; &r = i;
  auto a = r;		// r是int型的引用,因此a是int型
  ```

- `auto`会忽略掉顶层const, 同时底层const则会保留下来

  ```cpp
  const int ci = i, &cr = ci;
  auto b = ci;	//int b = ci;
  auto c = cr;	//int c = cr;  cr是ci的别名，ci本身是一个顶层const
  auto d = &i;	//int *d = &i;
  auto e = &ci;	//const int *e = &ci; 对常量对象取地址是一种底层const
  ```

  如果希望推断出的auto类型是一个顶层const，需要明确指出：

  ```cpp
  const auto f = ci;		// const int f = ci;
  ```

  指定引用类型

  ```cpp
  auto &g = ci;		//const int &g = ci;
  auto &h = 42;		//错误，int &h = 42;
  const auto &j = 42; //const int &j = 42;
  ```

  

> auto 使用建议：
>
> 使用auto声明变量一定要做到心里有数，你知道编译器会推断出的什么样的类型
>
> 通常使用auto是对于一些类型名比较复杂的变量，使用auto写起来更方便



### 5.3 decltype 类型指示符

<span style="border:2px solid Red">C++11</span>  `decltype` 可以不执行表达式，编译器自动推断出表达式的返回值类型

```cpp
decltype(f()) sum = x;		//sum的类型和f()的返回类型一样
```

通过 `f()` 推断出返回类型，但是并不会执行 `f()`



#### decltype 和const

`decltype`处理顶层 consth和引用的方式和auto有点不同，如果 decltype使用的表达式是一个变量，则decltype返回该变量的类型（<span style="background: yellow">包括顶层const和引用在内</span>）

```cpp
const int ci = 0, &cj = ci;
decltype(ci) x = 0;		//const int x = 0
decltype(cj) y = 0;		//const int &y = 0;
decltype(cj) z;			//错误， const int &z;  引用必须初始化
```





#### decltype 和引用

```cpp
int i = 42, *p = &i, &r = i;
decltype(r + 0) b;		//int b;
decltype(*p) c;			//错误， int &c; 引用需要初始化
```

> `r` 是引用  `decltype(r)` 是引用，但是 `r + 0` 是一个int型数据
>
> 解引用指针得到的是指针所指的对象，，因此 `decltype(*p)` 是 `int&`

变量加上 `()` 得到的是引用类型

```cpp
decltype((i)) d;   	//错误， int& d; 引用类型需要初始化
decltype(i) e;		// int e;
```

<span style="background: yellow">`decltype((variable))` 的结果永远是引用</span>





## 6. 自定义数据结构

这里的自定义数据结构就是指类类型的数据，在C++中定义类的关键字有`class` 和 `struct`

```cpp
class ClassName{
	//属性
	//方法
};

struct ClassName{
	//属性
	//方法
};
```

> `class` 和 `struct` 在功能上是完全一样的，两者唯一的不同是默认的权限不同
>
> `class`默认的权限是私有的(private), 而 `struct` 是公有的(public)



*注意：c语言中的结构体是不能有方法（函数）*

关于类更具体的介绍在后面的章节











 
