
# 第1章 开始





本章以一个实际问题，书店问题，来简单的介绍C++的基本特性。这个问题的代码将贯彻整本书，后面的章节会逐一讲解代码中涉及到的 C++ 语言特性。

这个问题具体，就是要保存书店的所有销售记录的档案，每条记录保存了某本书的一次销售信息，包含三个数据：

```
0-201-70353-X	4	24.99
```

它们分别是书的 ISBN，售出的册数，书的单价。有时，书店老板需要查询此档案，计算每本书的销售量、销售额及平均售价。

这个问题会涉及到的有：

- 定义变量
- 进行输入和输出
- 使用数据结构保存数据
- 检测两条数据是否有相同的 ISBN
- 包含一个循环来处理销售档案中的每条记录

我们首先介绍如何使用 C++ 来解决这些子问题，然后编写书店程序。

[C++ Primer, 5th Edition 代码下载](https://www.informit.com/store/c-plus-plus-primer-9780321714114)



## 1. 一个简单的C++程序

从一个简单的C++程序开始

```c++
int main()
{
    return 0;
}
```

由这个程序简单的介绍了函数的定义。函数定义包含四部分：**返回类型** 、**函数名** 、一个括号包围的**形参列表** （允许为空） 以及 **函数体** 。`main` 是一个比较特殊的函数，但其定义与其他函数是一样的。

简单的介绍了c++程序在不同的平台编译运行的方式。



### 1.1 编译、运行程序

![image-20220604174737974](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220604174737974.png)

上图是一个 C 程序（C++ 程序类似）的编译过程，可供参考。

更详细参考：[【6】【Cherno C++】【中字】C++编译器是如何工作的](https://www.bilibili.com/video/BV1er4y1c7nj/?spm_id_from=333.788)



## 2. 初识输入和输出

在C++语言中并未定义任何输入输出（IO）语句，取而代之，包含了一个一个全面的标准库来提供 IO 机制。当然，我们也可可以使用 C 语言中的 `printf` ，C++ 是兼容 C 的，但是并不建议，所以本书中的示例都是使用标准库中提供的 `iostream` 库。`iostream` 库包含两个基础类型 `istream` 和 `ostream` ，分别表示输入流和输出流。

#### 标准输入输出对象

标准库定义类4个 IO 对象。输入， 使用一个名为 `cin` 的 `istream`  类型的对象。 输出，使用一个名为 `cout` 的 `ostream` 类型的对象。标准库还定义了其他两个 `ostream` 对象，名为 `cerr` 和 `clog` 。 

#### 一个使用 IO 库的程序

```cpp
#include <iostream>
int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The sum of " << v1 << " and " << v2
              << " is " << v1 + v2 << std::endl;
    return 0;
}
```



## 3. 注释

c++的注释两种：

```c++
//注释1
std::cout << "hello" << std::endl;
/*
注释2
*/
std::cin >> val;
```



## 4. 控制流

循环和判断语句

### 4.1 while

示例： 求 1 到 10 的和

```cpp
#include <iostream>
int main()
{
    int sum = 0, val = 0;
    while (val <= 10)
    {
        sum += val;
        ++val;
    }
    std::cout << "Sum of 1 to 10 inclusive is "
              << sum << std::endl;
    return 0;
}
```



### 4.2  for

`for` 循环， 将上面 `while` 循环改成  `for`

```cpp
#include <iostream>
int main()
{
    int sum = 0; 
	for (int val = 1; val <= 10; ++val)
        sum += val;
    std::cout << "Sum of 1 to 10 inclusive is "
              << sum << std::endl;
    
    return 0;
    
}
```



### 4.3 读取数量不定的输入数据

```cpp
#include <iostream>
int main()
{
    int sum = 0, val = 0;
    while (std::cin >> val)
        sum += val;
    std::cout << "Sum  is " << sum << std::endl;
    return 0;
}
```

上面程序在输入结束后除了按回车键，还需要按**结束符** 

>在Windows 系统中输入结束符是按 `Ctrl + Z`
>
>在 UNIX系统中，包括 Mac OS X 系统中，文件结束符输入用 `Ctrl + D`



###   4.3 if

一个示例

读取一串数据并统计每个数字出现的次数 ，windows下的终止键 `ctrl` + `z` 

```c++
#include <iostream>
using namespace std;
int main()
{
	int currVal = 0, val = 0;
	if (cin >> currVal) {
		int cnt = 1;
		while (cin >> val) {
			if (val == currVal) {
				++cnt;
			}
			else {
				cout << currVal << " occurs "
					<< cnt << " times" << endl;
				currVal = val;
				cnt = 1;
			}
		}
		cout << currVal << " occurs "
			<< cnt << " times" << endl;
	}
	system("pause");
	return 0;
}
```



## 5. 类的简介

在解决书店程序之前，我们还需要了解的唯一一个特性，就是如何定义一个**数据结构**来表示销售数据。在C++中，我们通过定义**类** 来定义自己的数据结构。一个类定义了一个类型（**类类型**），以及与其关联的一组操作。

本节中我们使用一个类，需要了解三件事：

- 类名是什么？
- 它是在哪里定义的？
- 它支持什么操作？

对于书店程序来说，我们假定类名为 `Sales_item`，头文件 `Sales_item.h` 中已经定义了这个类。



### 5.1 Sales_item 类

`Sales_item` 类的作用是表示一本书的销售总额、售出册数和评价售价。我们现在不需要关心这些数据是如何存储、如何计算，我们只需要直了解怎么使用它。

我们可以把一个类当作是一种类型（类类型），就和C++语言内置的类型（ `int` `float` 等）一样，其用法也是和内置类型一致的。

定义类类型的变量

```cpp
Sales_item item;
```

上面语句表达的是定义了一个变量 （对象）`item`，它的类型是 `Sales_item` 类型。

除了可以定义 `Sales_item` 类型变量之外，我们还可以：

- 调用一个名为 `isbn` 的函数从一个 `Sales_item` 对象中提取 ISBN 书号
- 用输入运算符（`>>`）和输出运算符 （`<<`） 读、写 `Sales_item` 类型的对象
- 用赋值运算符 （`=`）将一个 `Sales_item` 对象的值赋予另一个 `Sales_item` 对象
- 用加法运算符 （`+`） 将两个 `Sales_item` 对象相加。两个对象必须表示同一本书（相同的 ISBN）。加法的结果是一个新的 `Sales_item` 对象，其 ISBN 与两个运算符对象相同，而其总销售额和售出册数是两个运算对象的对应值之和
- 使用复合运算符 （`+=`） 将一个 `Sales_item` 对象加到另一个对象上。



#### 读写 Sales_item

把 `Sales_item` 当做是一个普通的变量（对象），它的操作和内置类型是一样的

```cpp
#include <iostream>
#include "../Sales_item.h"   // 包含书中提供的Sales_item类代码
int main() 
{
    Sales_item book;
    // 读入 ISBN 号、售出的册数以及销售价格
    std::cin  >> book;
    // 写入 ISBN、售出的册数、总销售额和平均价格
    std::cout << book << std::endl;
    return 0;
}
```

如果输入：

```
0-201-70353-X 4 24.99
```

则输出为：

```
0-201-70353-X 4 99.96 24.99
```

99.96 表示的是以每本22.49的价格售出 4 本的总销售额。



#### Sales_item 对象的加法

像操作两个`int` 变量一样，将两个 `Sales_item` 对象相加

```cpp
#include <iostream>
#include "../Sales_item.h"
int main() 
{
    Sales_item item1, item2;
    std::cin  >> item1 >> item2;	// 读取一对交易记录
    std::cout << item1 + item2 << std::endl;	// 打印它们的和
    return 0;
}
```

输入：

```
0-201-78345-X 3 20.00
0-201-78345-X 2 25.00
```

输出：

```cpp
0-201-78345-X 5 110 22
```



### 5.2 初识成员函数

将两个 `Sales_item` 对象相加的程序首先应该检查两个对象是否具有相同的 ISBN，`Sales_item` 提供了获取 ISBN 的方法（成员函数）

```cpp
#include <iostream>
#include "../Sales_item.h"
int main() 
{
    Sales_item item1, item2;
    std::cin >> item1 >> item2;
    if(item1.isbn() == item2.isbn()) {
        std::cout << item1 + item2 << std::endl;
        return 0;
    } else {
        std::cerr << "Data must refer to same ISBN" 
                << std::endl;
        return -1;
    }
}
```



#### 什么是成员函数？

```cpp
item1.isbn() == item2.isbn()
```

**成员函数**是定义为类的一部分的函数，有时也称为**方法**

我们通常是使用一个类对象来调用成员函数，如下代码，使用点运算符 （`.`） 来调用对应的成员

```cpp
iterm1.isbn();
```





## 6. 书店程序

```cpp
#include <iostream>
#include "../Sales_item.h"
int main()
{
    Sales_item total;       // 保存下一条交易记录的变量
    // 读入第一条交易记录，并确保有数据可以处理
    if (std::cin >> total) {
        Sales_item trans;       // 保存和变量
        // 读入并处理剩余交易记录
        while (std::cin >> trans){
            // 如果我们仍在处理相同的书
            if (total.isbn() == trans.isbn())
                total += trans;     // 更新总销售额
            else {
                // 打印前一本的结果
                std::cout << total << std::endl;
                total = trans;      // total 现在表示下一步书的销售额
            }
        }
        std::cout << total << std::endl;    // 打印最后一本书的结果
        
    }
    else{
        // 没有输入！警告读者
        std::cerr << "No data?!" << std::endl;
        return -1;  // 表示失败
    }

    return 0;
}
```





