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


### this 指针


### inline 内联函数


### volatile


### sizeof


### extern 


### C++中的struct和class


### union 联合体


### enum 枚举类型


### friend 友元


### using


### explicit


### auto和decltype


### 范围解析运算符


### 引用

### 引用折叠


### 完美转发


### 强制类型转换运算符


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

<a id="review-of-brush-questions-website"></a>

## 💯 复习刷题网站

<a id="license"></a>

## 📜 License

本仓库遵循 CC BY-NC-SA 4.0（署名 - 非商业性使用 - 相同方式共享） 协议，转载请注明出处，不得用于商业目的。

[![CC BY-NC-SA 4.0](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](https://github.com/Kinvy66/interview/blob/master/LICENSE)


