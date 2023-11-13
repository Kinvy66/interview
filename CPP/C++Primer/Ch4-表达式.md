---
title: "第4章 表达式(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2022-08-07
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch4"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/2021-12-31.jpg
    caption: "" 
    alt: ""
    relative: false
---




## 1. 基础

一些运算符的基本概念

### 1.1 基本概念

 #### 重载运算符

赋予基本运算符不同的含义和运算方式，像之前使用的 `<<` 和 `>>` 用于cout和cin，这两个运算符本来是移位运算符，这里通过重载实现其他的运算。



#### 左值和右值

<span style="background: yellow">**左值**，使用的是对象的身份，即使用对象的内存位置；**右值**，使用的是对象的值（内容）</span>



### 1.2 优先级与结合律

基本的运算优先级和数学中的优先级一样。

括号无视优先级，在不确定默认的优先级时可以使用括号。





## 2. 算术运算符

常用算术运算符

![image-20211224155915096](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211224155915096.png)



## 3. 逻辑和关系运算符

 逻辑和关系运算符

![image-20211224160003044](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211224160003044.png)



**短路原则**：

- 对于逻辑与运算符来说，当且仅当左侧运算对象为真时才对右侧运算对象求值
- 对于逻辑或运算符来说，当且仅当左侧运算对象为假时才对右侧运算对象求值





## 4. 赋值运算符

赋值和初始化是两个不同的概念，虽然都使用 `=` 运算符

```cpp
int i = 10, k = 0;		//这里是初始化，不是赋值操作
int j;					//声明定义一个变量
j = 1;					//赋值操作
cont int ci = i;		// 初始化而非赋值
```



赋值运算符的左侧运算对象必须是一个**可修改的左值**

```cpp
1024 = k;		// 错误：字面值是右值
i + j = k;		// 错误：算术表达式是右值
ci = k;			// 错误：ci是常量（不可修改的）左值
```



## 5. 递增和递减运算符

递增（递减）运算符有前置（`++i`）和后置（`i++`）

这两种的使用区别是，前置的是先对变量执行加一的操作，再使用变量的值；后置的是先使用变量的值，在执行加一的操作

> 本质区别：前置的是加一返回对象，后置的是加一返回原始的对象的副本
>
> 如果没有特别的需求，建议使用前置的版本





## 6. 成员访问运算符

成员运算符有**点运算符** 和 **箭头运算符**

```cpp
//这两个表达式是等价的
ptr->mem;
(*ptr).mem;		//* 优先级低于 . 所以要加括号
```



## 7. 条件运算符

条件运算符 `?:` 是一个三元运算符，格式：

```cpp
cond ? expr1 : expr2;
//等价形式
if (cond)
    expr1;
else
    expr2;
```

 

> 条件运算符可以嵌套，不建议嵌套过多层，且条件运算符一般用在简单的表达式上。





## 8. 位运算符

位运算符是在二进制的层面对数据操作，以下是常用的位运算符

![image-20211224162959652](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211224162959652.png)



## 9. sizeof 运算符

`sizeof` 运算符返回一条表达式或一个类型名字所占的字节数。该运算符有两种形式

```cpp
sizeof(type);
sizeof expr;
```



sizeof运算符的结果部分地依赖于其作用的类型：

- 对char或者类型为char的表达式执行sizeof运算，结果得1
- 对引用类型执行sizeof运算得到被引用对象所占空间的daxn
- 对指针执行sizeof运算得到指针本身所占空间的大小
- 对解引用指针执行sizeof运算得到指针指向的对象所占空间的大小，指针不需有效
- 对数组执行sizeof运算得到整个数组所占空间的大小，等价于对数组中所有的元素各执行一次sizeof运算并将所得结果求和，注意sizeof运算不会把数组转换成指针来处理
- 对string对象或vector对象执行sizeof运算只能返回该类型固定部分的大小，不会计算对象中的元素占用了多少空间





## 10. 逗号运算符

 **逗号运算符** 含有两个运算对象，按照从左向右的顺序依次求值

```cpp
vector<int>::size_type cnt = ivec.size();
//将把从size到1的值赋给ivec的元素
for(vector<int>::size_type ix = 0; 
   				ix != ivec.size; ++ix, --cnt)
    ivec[ix] = cnt;
```



## 11. 类型转换

#### 隐式转换

由编译器完成，可能会出现精度损失



#### 显示转换

命名类型的强制类型转换，其形式如下：

```cpp
cast-name<type>(expression);
```

其中`type`是转换的目标类型而 `expression` 是要转换的值。如果 `type` 是引用类型，则结果是左值。

- [ ] TODO : `cast-name` 是 `static_cast`、`dynamic_cast`、`const_cast`、`reinterpret_cast`



**static_cast** , 任何具有明确定义的类型转换，只要不包含底层const，都可以使用static_cast

```cpp
//进行强制类型转换以便执行浮点数除法， j, i是int
double slope = static_cast<double>(j) / i;
```



**const_cast**, 只能改变运算对象的底层const

```cpp
const char *pc;
char *p = const_cast<char*>(pc);
```



**reinterpret_cast**, 通常为运算对象的位模式提供较低层次上的重新解释  *这里不是很明白*

```cpp
int *ip;
char *pc = reinterpret_cast<char*>(ip);
```



##### 旧式的强制类型转换

两种形式

```cpp
type (expr);	//函数形式的强制类型转换
(type) expr;	//c语言风格的强制类型转换
```







