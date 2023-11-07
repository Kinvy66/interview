<div align="center">
<a href="https://github.com/Kinvy66/interview">📖 Github</a>
&emsp;&emsp; | &emsp;&emsp;
📚 Docsify
</div> 
<br>

<b><details><summary>💡 关于</summary></b>

📚 本仓库是个人整理的C/C++ 技术方向的面试笔记，灵感来源于 [https://github.com/huihut/interview](https://github.com/huihut/interview) (主要原因还是自己太菜，秋招目前0offer),通过这种方式把面试中的八股整理一下。内容和文档结构部分参考上面提到的仓库。


</details>


## 📑 目录

- [➕ C/C++](#cc)
- [⭐️ Effective](#effective)
- [📦 STL](#stl)
- [〽️ 数据结构](#data-structure)
- [⚡️ 算法](#algorithm)
- [❓ Problems](#problems)
- [💻 操作系统](#os)
- [☁️ 计算机网络](#computer-network)
- [🌩 网络编程](#network-programming)
- [💾 数据库](#database)
- [📏 设计模式](#design-pattern)
- [⚙️ 链接装载库](#link-loading-library)
- [📚 书籍](#books)
- [💯 复习刷题网站](#review-of-brush-questions-website)
- [📜 License](#license)


<a id="cc"></a>

## ➕ C/C++

### const

#### 1. 作用
`const` 是一个类型修饰的关键字，被其修饰的类型具有不可变的特性。

- 1. 修饰普通变量，说明该变量不可以被改变；
- 2. 修饰指针，分为指向常量的指针(pointer to const)和自身是常量的指针(常量指针，const pointer);
- 3. 修饰引用，指向常量的引用(reference to const), 用于形参类型，级避免拷贝，同时避免函数对值的修改；
- 4. 修饰成员函数，说明该成员函数内不能改变成员变量。


#### 2. const与指针和引用
名称区分：顶层const, 即变量本身是const,其值不能改变； 底层const，即变量指向的变量的是const，自身的值可以改变，但是它指向的是一个const的变量；

- 指针
  - 指向常量的指针

    ```cpp
    const int ci = 42;
    int i = 18;
    const int *p1 = &ci; // 这里的const是底层const
    const int *p2 = &i;  // 可以用非const变量地址初始化指向常量的指针， 但是后续不能使用该指针解引用改变该变量的值
    *p2 = 10;     // 错误
    ```

  - 常量指针，指针的值不能改变
    ```cpp
    int i = 42;
    int j;
    int *const p = &i;  // 顶层const
    p = &j;   // 错误, p是常量指针，初始化后之后不能再改变指向
    *p = 10;  // 正确， 解引用改变的是它指向的变量的值
    ```
  注意：`const`的位置只与 `*` 有关，在`*`之前是指向常量的指针，在`*`之后是常量指针

    ```cpp
    const int * const p;
    int const * const p; // 等价声明
    ```

- 引用
  
  引用本身自带顶层const属性，即引用一旦初始化就不能改变指向，所以对引用加const 就是加底层const, 使引用指向一个具有const的变量
  ```cpp
  const int ci = 42;
  int i = 10;
  int& ri = i;
  const int& cri = ci;
  const int& crj = i;   // 正确
  int& rj = ci;         // 错误
  ```

#### 3. 使用
const在成员函数和函数传参中的使用

```cpp
// const成员函数
class Entity {
public:
    int size() const { 
        // m_number = 42; // 错误， 不能const成员函数中修改任何成员变量的值
        return m_size; 
    }

    int size() { // 和具有const修饰的成员函数构成重载的关系
        return m_size; 
    }

private:
    int m_size;
    int m_number;
};

// const 参数，传递只读参数
void print(const std::string& str) {
    std::cout << str << std::endl;
}

int main() {
    Entity e;
    const Entity c_e;
    auto a = e.size();  // 调用非const成员函数
    auto b =c_e.size(); // 调用const成员函数

    std::string str = "Hello";
    print(str);
    return 0;
}
```

#### 4. const与宏定义#define
| 宏定义 #define|const 常量|
|---|---|
|宏定义，相当于字符替换|常量声明|
|预处理器处理|编译器处理|
|无类型安全检查|有类型安全检查|
|不分配内存|要分配内存|
|存储在代码段|存储在数据段|
|可通过 `#undef` 取消|不可取消|


### static

#### 1. 作用
1. **修饰普通变量**，修改变量的存储区域和生命周期，使变量存储在静态区，在 main 函数运行前就分配了空间，如果有初始值就用初始值初始化它，如果没有初始值系统用默认值初始化它。
2. **修饰普通函数**，表明函数的作用范围，仅在定义该函数的文件内才能使用。在多人开发项目时，为了防止与他人命名空间里的函数重名，可以将函数定位为 static。
3. **修饰成员变量**，修饰成员变量使所有的对象只保存一个该变量，而且不需要生成对象就可以访问该成员。
4. **修饰成员函数**，修饰成员函数使得不需要生成对象就可以访问该函数，但是在 static 函数内不能访问非静态成员。



### this 指针

1. `this` 指针是一个隐含于每一个非静态成员函数中的特殊指针。它指向调用该成员函数的那个对象。
2. 当对一个对象调用成员函数时，编译程序先将对象的地址赋给 `this` 指针，然后调用成员函数，每次成员函数存取数据成员时，都隐式使用 `this` 指针。
3. 当一个成员函数被调用时，自动向它传递一个隐含的参数，该参数是一个指向这个成员函数所在的对象的指针。
4. `this` 指针被隐含地声明为: `ClassName *const this`，这意味着不能给 `this` 指针赋值；在 `ClassName` 类的 `const` 成员函数中，this 指针的类型为：`const ClassName* const`，这说明不能对 `this` 指针所指向的这种对象是不可修改的（即不能对这种对象的数据成员进行赋值操作）；
5. `this` 并不是一个常规变量，而是个右值，所以不能取得 `this` 的地址（不能 `&this`）。
6. 在以下场景中，经常需要显式引用 `this` 指针：
   1. 为实现对象的链式引用；
   2. 为避免对同一对象进行赋值操作；
   3. 在实现一些数据结构时，如 `list`



### inline 内联函数

#### 1. 特征

- 相当于把内联函数里面的内容写在调用内联函数处；
- 相当于不用执行进入函数的步骤，直接执行函数体；
- 相当于宏，却比宏多了类型检查，真正具有函数特性；
- 编译器一般不内联包含循环、递归、switch 等复杂操作的内联函数；
- 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。类内声明，类外定义，如果需要内联，必须使用 inline 关键字修饰。


#### 2. 使用
```cpp
// 普通函数的内联
inline int func(int first, int second, ...);

// 定义
inline int func(int first, int second, ...) {
    //....
}

class Entity {
public:
    // 类内定义的成员函数隐式为inline
    int func1() { 
        // ...
    }
    int func2();
};

// 类外定义的函数需要显示的使用inline
inline int Entity::func2() {

}
```


### volatile
> [C/C++ 中volatile的使用(kinvy blog)](https://www.kinvy.cn/posts/tech/coding/volatile/)

volatile 单词的意思是“易变的”。在C/C++中它作为关键字修饰变量时，是告诉编译器这个变量可能是会变化的，不要进行优化。不要从缓存或寄存器中读取值，而是每次都需要从内存重新读取最新的值。

`volatile`用在如下的几个地方：
- 中断服务程序中修改的供其它程序检测的变量需要加`volatile`；
- 多任务环境下各任务间共享的标志应该加`volatile`；
- 存储器映射的硬件寄存器通常也要加`volatile`说明，因为每次对它的读写都可能由不同意义；


### sizeof
`sizeof` 是一个运算符，不是函数。  
- `sizeof` 对数组，得到整个数组所占空间大小
- `sizeof` 对指针，得到指针本身所占空间的大小，32bit机器4字节，64bit机器8字节

?> 在对字符数组时，区分一下`strlen`, `sizeof`会计算终止字符，而 `strlen` 不会计算终止字符

```cpp
const char* s1[] = { "12345" };
char s2[] = { "12345" };
char s3[10] = { "12345" };
const char* s4 = "12345";

std::cout << "sizeof " << sizeof(s1) << " "
    << sizeof(s2) << " "
    << sizeof(s3) << " " 
    << sizeof(s4) << std::endl;

std::cout << "strlen " << strlen(s1[0]) << " "
    << strlen(s2) << " "
    << strlen(s3) << " " 
    << strlen(s4) << std::endl;
// 输出
//sizeof 4 6 10 4   // 指针大小，字符数组(包含'\0'), 字符数组大小，指针大小
//strlen 5 5 5 5   //  字符串长度，字符串长度，字符串长度，字符串长度
```


### extern 

1. 声明变量或函数是外部定义的
   ```cpp
   // add.cpp
    int add(int a, int b) {
        return a + b;
    }
   // main.cpp
   #include <iostream>
   extern int add(int a, int b);
   int main() {
    std::cout << add(10, 42) << std::endl;
    return 0;
   }
   ```

2. 被 `extern "C"` 修饰的变量和函数是按照 C 语言方式编译和链接的
   ```cpp
   #ifdef __cplusplus
    extern "C" {
    #endif

    void *memset(void *, int, size_t);

    #ifdef __cplusplus
    }
    #endif
   
   ```


### C++中的struct和class
struct 和 class 在C++中没有本质的区别，它们都是定义一个类，只是默认的访问权限不同，struct 的默认访问权限是 `public`， class的默认访问权限是 `private`. 



### union 联合体
联合（union）是一种节省空间的特殊的类，一个 union 可以有多个数据成员，但是在任意时刻只有一个数据成员可以有值。当某个成员被赋值后其他成员变为未定义状态。联合有如下特点：
- 默认访问控制符为 `public`
- 可以含有构造函数、析构函数
- 不能含有引用类型的成员
- 不能继承自其他类，不能作为基类
- 不能含有虚函数
- 匿名 `union` 在定义所在作用域可直接访问 `union` 成员
- 匿名 `union` 不能包含 `protected` 成员或 `private` 成员
- 全局匿名联合必须是静态（`static`）的

使用：
```cpp
#include<iostream>

union UnionTest {
    UnionTest() : i(10) {};
    int i;
    double d;
};

static union {
    int i;
    double d;
};

int main() {
    UnionTest u;

    union {
        int i;
        double d;
    };

    std::cout << u.i << std::endl;  // 输出 UnionTest 联合的 10

    ::i = 20;
    std::cout << ::i << std::endl;  // 输出全局静态匿名联合的 20

    i = 30;
    std::cout << i << std::endl;    // 输出局部匿名联合的 30

    return 0;
}
```

?> 联合体变量占用的内存空间是最大的那个数据类型的大小

### enum 枚举类型
#### 限定作用域的枚举类型
```cpp
enum class open_modes { input, output, append };
```

#### 不限定作用域的枚举类型
```cpp
enum color { red, yellow, green };
enum { floatPrec = 6, doublePrec = 10 };
```


### friend 友元
- 能访问私有成员
- 破坏封装性
- 友元关系不可传递、继承
- 友元关系的单向性
- 友元声明的形式及数量不受限制

### using
> [C++ using 关键字的几种的用法](https://www.kinvy.cn/posts/tech/coding/c++-using/)

#### using 声明
一条 `using` 声明 语句一次只引入命名空间的一个成员。它使得我们可以清楚知道程序中所引用的到底是哪个名字。如:
```cpp
using namespace_name::name;
```

#### 构造函数的using声明
在 C++11 中，派生类能够重用其直接基类定义的构造函数。
```cpp
class Derived : Base {
public:
    using Base::Base;
    /* ... */
};
```
如上 using 声明，对于基类的每个构造函数，编译器都生成一个与之对应（形参列表完全相同）的派生类构造函数。生成如下类型构造函数：

```cpp
Derived(parms) :: Base(args) { }
```


using 指示
using 指示 使得某个特定命名空间中所有名字都可见，这样我们就无需再为它们添加任何前缀限定符了。如：
```cpp
using namespace_name name;
```
尽量少使用 using 指示 污染命名空间
一般说来，使用 using 命令比使用 using 编译命令更安全，这是由于它只导入了指定的名称。如果该名称与局部名称发生冲突，编译器将发出指示。using编译命令导入所有的名称，包括可能并不需要的名称。如果与局部名称发生冲突，则局部名称将覆盖名称空间版本，而编译器并不会发出警告。另外，名称空间的开放性意味着名称空间的名称可能分散在多个地方，这使得难以准确知道添加了哪些名称。

using 使用

尽量少使用 using 指示
```cpp
using namespace std;
```

应该多使用 using 声明
```cpp
int x;
std::cin >> x ;
std::cout << x << std::endl;
```

或者

```cpp
using std::cin;
using std::cout;
using std::endl;
int x;
cin >> x;
cout << x << endl;
```


### explicit
- explicit 修饰构造函数时，可以防止隐式转换和复制初始化
- explicit 修饰转换函数时，可以防止隐式转换，但[按语境转换](https://zh.cppreference.com/w/cpp/language/implicit_conversion)除外

`explicit` 使用
```cpp
struct A
{
    A(int) { }
    operator bool() const { return true; }
};

struct B
{
    explicit B(int) {}
    explicit operator bool() const { return true; }
};

void doA(A a) {}

void doB(B b) {}

int main()
{
    A a1(1);        // OK：直接初始化
    A a2 = 1;        // OK：复制初始化
    A a3{ 1 };        // OK：直接列表初始化
    A a4 = { 1 };        // OK：复制列表初始化
    A a5 = (A)1;        // OK：允许 static_cast 的显式转换 
    doA(1);            // OK：允许从 int 到 A 的隐式转换
    if (a1);        // OK：使用转换函数 A::operator bool() 的从 A 到 bool 的隐式转换
    bool a6(a1);        // OK：使用转换函数 A::operator bool() 的从 A 到 bool 的隐式转换
    bool a7 = a1;        // OK：使用转换函数 A::operator bool() 的从 A 到 bool 的隐式转换
    bool a8 = static_cast<bool>(a1);  // OK ：static_cast 进行直接初始化

    B b1(1);        // OK：直接初始化
    B b2 = 1;        // 错误：被 explicit 修饰构造函数的对象不可以复制初始化
    B b3{ 1 };        // OK：直接列表初始化
    B b4 = { 1 };        // 错误：被 explicit 修饰构造函数的对象不可以复制列表初始化
    B b5 = (B)1;        // OK：允许 static_cast 的显式转换
    doB(1);            // 错误：被 explicit 修饰构造函数的对象不可以从 int 到 B 的隐式转换
    if (b1);        // OK：被 explicit 修饰转换函数 B::operator bool() 的对象可以从 B 到 bool 的按语境转换
    bool b6(b1);        // OK：被 explicit 修饰转换函数 B::operator bool() 的对象可以从 B 到 bool 的按语境转换
    bool b7 = b1;        // 错误：被 explicit 修饰转换函数 B::operator bool() 的对象不可以隐式转换
    bool b8 = static_cast<bool>(b1);  // OK：static_cast 进行直接初始化

    return 0;
}

```

### auto和decltype
> [Modern C++ -- auto、decltype](https://www.kinvy.cn/posts/tech/coding/modern-cpp/)
`auto`自动类型推导，通过初始化值的类型推导变量的类型

?> `auto` 类型推导不是万能的，不应该过度依赖于自动类型推断，它只是使编码更方便，在使用`auto`自动类型推断时应该做到“心中有数”。

`decltype` 关键字用于检查实体的声明类型或表达式的类型及值分类。语法：
```cpp
decltype ( expression )
```

decltype 和 auto 一起使用，构成后置返回的用法：
```cpp
template<typename T, typename U>
auto add(T x, U y)-> decltype(x+y) {
    return x + y;
}
```

### 范围解析运算符
#### 分类
- 全局作用域符（::name）：用于类型名称（类、类成员、成员函数、变量等）前，表示作用域为全局命名空间
- 类作用域符（class::name）：用于表示指定类型的作用域范围是具体某个类的
- 命名空间作用域符（namespace::name）:用于表示指定类型的作用域范围是具体某个命名空间的
`::` 使用

```cpp
int count = 11;         // 全局（::）的 count

class A {
public:
    static int count;   // 类 A 的 count（A::count）
};
int A::count = 21;

void fun()
{
    int count = 31;     // 初始化局部的 count 为 31
    count = 32;         // 设置局部的 count 的值为 32
}

int main() {
    ::count = 12;       // 测试 1：设置全局的 count 的值为 12

    A::count = 22;      // 测试 2：设置类 A 的 count 为 22

    fun();                // 测试 3

    return 0;
}
```

### 引用

?> 无论左值引用还是右值引用，引用变量本身不占用内存空间。

#### 1. 左值引用
常规引用，一般表示对象的身份。

#### 2. 右值引用
右值引用就是必须绑定到右值（一个临时对象、将要销毁的对象）的引用，一般表示对象的值。
右值引用可实现**转移语义（Move Sementics）**和**精确传递（Perfect Forwarding）**，它的主要目的有两个方面：

- 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率。
- 能够更简洁明确地定义泛型函数。


### 引用折叠
引用折叠主要发生在模板类型参数的变量自带引用的情况：
```cpp
template <typename T> void f(T &p);
template <typename T> void f2(const T &p);
template <typename T> void f3(T &&p);
```

- `X& &`、`X& &&`、`X&& &` 可折叠成 `X&`
- `X&& && `可折叠成 `X&&`

?> C++ 11中有万能引用（Universal Reference）的概念：使用`T&&`类型的形参既能绑定右值，又能绑定左值。

### 完美转发

- [ ] TODO

### 强制类型转换运算符

#### 1. static_cast


#### 2. dynamic_cast


#### 3. const_cast


#### 4. reinterpret_cast


### 成员初始化列表


### initializer_list 列表初始化


### 智能指针


### labmda 表达式



### 面向对象


### 封装



### 继承



### 多态


### 虚函数


### 虚析构函数


### 纯虚函数


### 虚函数指针、虚函数表


### 虚继承


### 函数模板


### 类模板、成员函数模板



### 抽象类、接口类、聚合类




### 内存分配和管理




### 运行时类型信息RTTI


### C实现CPP类

### 宏




<a id="effective"></a>

## ⭐️ Effective


<a id="stl"></a>

## 📦 STL

<a id="data-structure"></a>

## 〽️ 数据结构


<a id="algorithm"></a>

## ⚡️ 算法

<a id="problems"></a>

## ❓ Problems

<a id="os"></a>

## 💻 操作系统

<a id="computer-network"></a>

## ☁️ 计算机网络

<a id="network-programming"></a>

## 🌩 网络编程

<a id="database"></a>

## 💾 数据库

<a id="design-pattern"></a>

## 📏 设计模式

<a id="link-loading-library"></a>

## ⚙️ 链接装载库

<a id="books"></a>

## 📚 书籍

> [Kinvy66/pdf](https://github.com/Kinvy66/pdf): 📚 Computer Science Books 计算机技术类书籍 PDF


### 语言：
- 《C++ Primer》
- 《Effective C++》
- 《More Effective C++》
- 《深度探索 C++ 对象模型》
- 《STL 源码剖析》


<a id="review-of-brush-questions-website"></a>

## 💯 复习刷题网站

<a id="license"></a>

## 📜 License

本仓库遵循 CC BY-NC-SA 4.0（署名 - 非商业性使用 - 相同方式共享） 协议，转载请注明出处，不得用于商业目的。

[![CC BY-NC-SA 4.0](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](https://github.com/Kinvy66/interview/blob/master/LICENSE)


