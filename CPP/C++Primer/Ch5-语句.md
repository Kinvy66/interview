---
title: "第5章 语句(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2022-08-12
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch5"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/2022-02-21.jpg
    caption: "" 
    alt: ""
    relative: false
---



## 1. 简单语句

#### 表达式语句

表达式加上分号就是一条语句

```cpp
ival + 5;		//一条没有实际作用的表达式语句
cout << ival;	//一条有用的表达式语句
```



#### 空语句

一个分号 `;` 就是空语句

```cpp
;	//空语句
ival = v1 + v2;;	//第二分号是空语句，没有影响
while (iter != svec.end()) ;	//分号是空语句，导致下面的语句不会出现在循环中执行
	++iter;						//不属于循环体的一部分
```



> 空语句单独不会有什么作用，但是和其他语句组合会有不同的作用
>
> 注意：别漏写分号，也别多写分号





#### 复合语句

复合语句也就**块**， 就是用一对 `{}` 括起来的多个语句

```cpp
while (val < 10) {
    sum += val;
    ++val;
}
```

> 块不以分号作为结束，但是在声明定义类时`{}` 后要加 `;`



## 2. 语句作用域

可以在 `if` 、`switch` 、`while` 和 `for` 语句的控制结构内定义变量。定义在控制结构当中的变量只有在相应语句的内部可见，一旦语句结束，变量也就超出了其作用范围了：

```cpp
while (int i = get_num()) // 每次迭代时创建并初始化 i
    cout << i << endl;
i = 0;		// 错误： 在循环外部无法访问i 
```



## 3. 条件语句

### 3.1 if 语句

`if` 语句的语法形式：

```cpp
if (condition)
    statement
//if else 语句
if (condition)
    statement
else
    statement2
```



使用注意：

```cpp
//1. 注意使用花括号
if (grade < 60)
    lettergrade = scores[0];
else	//错误： 下面的语句应该全部属于else的部分，需要加花括号
    lettergrade = scorses[(grade - 50)/10];
	if(grade != 100)
        if(grade % 10 > 7)
            lettergrade += '+';
		else if(grade % 10  < 3)
            lettergrade += '-';

//2.悬垂else
if (grade % 10 > = 3)
    if(grade % 10 > 7)
        lettergrade += '+';
else
    lettergrade += '-';

```



> - 尽量在每个if-else中的语句块加上花括号`{}`
> - if-else的匹配原则是就近原则，空格和缩进在c++是不会作用的，不像某些语言



### 3.2 switch 语句

格式：

```cpp
switch(lables) {
    case lable1:
        statement;
        break;
    case lable1:
        statement;
        break;
        ...
    default:
        statement;
        break;
}
```



> 注意：
>
> - lable1, lable2,... 必须是常量
> - 如果不是特殊的需求，每个case都需要加 `break`, 否则会一直执行下面的标签
> - `default` 标签建议写上
> - 在不同两个标签定义的变量，不要跨标签使用





## 4. 迭代语句

迭代语句就是我们所说的循环语句，常见的形式有以下几种



### 4.1 while语句

```cpp
while (condition)
    statement
```



### 4.2 do-while 语句

```cpp
do
    statement
while (condition);
```



### 4.3 传统的for语句

```cpp
for (init-statement; condition; expression)
    statement
```



### 4.4 范围for语句

<span style="border:2px solid Red">C++11</span> 

```cpp
for (declaration : expression)
    statement
```



## 5. 跳转语句

### 5.1 break 语句

`break` 负责终止离它最近的 while、do while、for、或switch语句



### 5.2 continue 语句

结束当前迭代，并进入下次迭代



### 5.3 goto语句

语法形式

```cpp
goto label;
```



> goto语句很好用，但是不建议用，会导致代码结构比较乱



## 6. try语句块和异常处理



### 6.1 throw表达式

throw用于抛出异常

```cpp
if(item1.isbn() != item2.isbn())
    throw runtime_error("Data must refer to same ISBN")；
```



### 6.2 try 语句块

语法形式

```cpp
try{
    program-satements;
} catch (exception-declaration) {
    handler-statements;
} catch (exception-declaration) {
    handler-statements;
}// ...
```



>  program-satements 是我们用运行的语句
>
> catch (exception-declaration) 表示的是可能会出现的异常以及相对应的异常处理语句



### 6.3 标准异常

c++标准库定义了一组类，用于报告标准库函数遇到的问题，它们分别定义在4个头文件中

- `exception` 头文件定义了最通用的异常类 ,它只报告异常的发生，不提供任何额外信息
- `stdexcept` 头文件定义几种常用的异常类，详细见下表
- `new` 头文件定义了 bad_alloc异常类型
- `type_info` 头文件定义了bad_cast异常类型

![image-20211225124654765](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20211225124654765.png)