---
title: "第9章  顺序容器(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2022-09-04
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch9"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/20220524th_id=OHR.KornatiNP_ZH-CN8829346235_1920x1080.jpg
    caption: "" 
    alt: ""
    relative: false
---





C++的STL可分为六个部件，分别是容器，分配器，算法，迭代器，适配器，仿函数。本章的内容主要讲平时使用比较多的容器，而容器又分为顺序容器和关联容器，这章主要是顺序容器，关联容器在第11章有详细的讲解。

一个容器就是一些特定类型对象的**集合**。 **顺序容器** 是指这些集合中的元素位置是和我们放入容器中的顺序相关的，而且这些元素在内存中是 "连续"的（这里的连续可以是物理空间的连续或逻辑连续）。





## 1. 顺序容器概述

下表是标准库中提供的所有的顺序容器

![image-20220508133428109](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220508133428109.png)

<span style="border:2px solid Red; border-radius:5px;">C++11</span> 其中 `forward_list` 和 `array` 是新 C++ 标准增加的类型。

array 与内置数组相比，是一种更安全、更容易使用的数组类型。与内置数组类似，array的大小是固定的。因此array不支持添加和删除以及改变容器大小的操作。

forward_list是一个单向链表的数据结构，它没有size操作。



##### 确定使用哪种顺序容器

 <span style="background: yellow">通常，使用 vector 是最好的选择，除非你有很好的理由选择其他容器</span>

关于容器的选择，在你不了解的这些容器底层实现使用的数据结构情况下，`vector` 是你的首选。书中也给出了一些选择容器的基本原则，这些原则都是基于容器底层数据结构的特性。我这里就不罗列这些了，只要了解每个容器的底层实现也就自然知道在不同的需求选择哪个合适的容器。

## 2. 容器库概览

容器类型的操作，某些操作是所有容器类型都提供的；另外一些操作仅对顺序容器、关联容器或无序容器；还有一些操作只适用于一小部分容器。

本小结介绍使用于**所有容器**的操作和仅使用于**顺序容器**的操作。

在第3章我们已经使用过 `vector` 容器，其他容器的使用也类似，使用这些容器都需要包含对应的头文件，头文件名称和容器名称一样。容器类型都是模板类，我们声明容器时需要指定元素的类型。

例如：

```cpp
list<Sales_data> s;		// 保存 Sales_data 对象的list
deque<double> d;		// 保存 double 的 deque
vector<vector<string>> lines;	// vector 的vector
```

<span style="border:2px solid Red; border-radius:5px;">C++11</span> 较旧的编译器（不支持C++11以上的版本) 可能需要在两个尖括号之间加上空格， 例如 `vector<vector<string> >`  



虽然我们可以在容器中保存几乎任何类型，但某些容器的操作对元素类型有其自己的特殊要求。也就是说我们使用自己定义的类型作为容器元素，容器的有些操作没有在自定义中的类型定义，那么就不能使用这些操作。例如，容器支持 `<` 关系运算，而我们自定义的类型（如 Sales_data类）并没有定义 `<` ,这时就不能使用这个运算符（虽然容器支持）。

另外一个例子，

```cpp
// 假定 noDefault 是一个没有默认构造函数的类型
vector<noDefault> v1(10, init);    // 正确： 提供了元素初始化器
vector<noDefault> v2(10);		   // 错误： 调用默认构造函数初始化，但是该类型没有定义默认构造函数
```



下表是所有容器共有的操作

![image-20220509110552713](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509110552713.png)

![image-20220509110612134](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509110612134.png)



### 2.1 迭代器

<span style="background: yellow"> 迭代器可以看作是一种特殊的指针</span>

迭代器的基本操作在第3章有讲解，比如解引用`*` ，递增  `++` 等一系列的操作，其中有些操作是某些容器不支持的。在表3.6中是迭代器支持的所有操作，其中有一个例外，forward_list 迭代器不支持递减运算符 （`--`)。表3.7列出来了迭代器支持的算术运算符，这些运算只能用于 string、vector、deque和arrary迭代器

![image-20220509185604269](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509185604269.png)

![image-20220509185623707](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509185623707.png)



#### 迭代器的范围

一个**迭代器范围**是有一对迭代器表示，两个迭代器分别指同一个容器的元素或者尾元素之后的未知，这两个迭代器通常被称为 `begin` 和 `end` ，如下图所示

![image-20220509191901004](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509191901004.png)



`begin` 迭代器指向的是容器的第一个元素，对其解引用操作就可以获取到容器第一个位置元素的值；

`end` 迭代器指向的是容器最后一个元素的<span style="background: yellow">后一个位置</span> , 对其解引用是无法获取到任何元素的。

这种元素范围被称为**左闭区间**， 用数学符号描述为:  [begin, end)



#### 使用左闭合范围蕴含的编程假定

使用左闭范围主要是为了方便迭代元素，这种范围具有以下三种性质：

- 如果 begin 与 end 相等，则范围为空
- 如果 begin 与 end 不等，则范围至少包含一个元素，且 begin 指向该范围的第一个元素
- 我们可以对 begin 递增若干次， 使得 begin == end

示例代码

```cpp
while(begin != end) {
	*begin = val;	// 正确： 范围非空， 因此 begin 指向一个元素
    ++begin;		// 移动迭代器， 获取下一个元素
}
```



### 2.2 容器类型成员

下面是容器定义的类型，在这之前我们已经使用过了 `size_type` 、`iterator` 和 `const_iterator`

![image-20220509194452634](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220509194452634.png)

使用这些类型，我们必须显示说明容器具体的类型

```cpp
// iter 是通过 list<string> 定义的一个迭代器类型
list<string>::iterator iter;
// count 是通过 vector<int> 定义的一个 difference_type 类型
vector<int>::difference_type count;
```

通常具体容器的类型别名写起来比较麻烦，一般我们会使用自动类型推断 `auto` 或 `decltype`



### 2.3 begin 和 end 成员

begin 和 end 有多个版本：带 `r` 版本返回反向迭代器；以 `c` 开头的版本返回 `const` 迭代器

示例：

```cpp
list<string> a = {"Milton", "Shakespeare", "Austen"};
auto it1 = a.begin();		// list<string>::iterator
auto it1 = a.rbegin();		// list<string>::reverse_iterator
auto it1 = a.cbegin();		// list<string>::const_iterator
auto it1 = a.crbegin();		// list<string>::const_reverse_iterator
```

不以 `c` 开头的 函数都是被重载过的。也就是说，实际上有两个名为 begin 的成员，一个返回常量成员，一个返回非常量成员。

当 `auto` 与 `begin` 或 `end` 结合使用时，获得的迭代器类型依赖于容器类型，与我们想要如何使用迭代器毫不下相干，如果想使用 `const` 版本可以显示的声明。

```cpp
auto it = a.being();	// it的类型取决于a的类型
```

<span style="background: yellow">当不需要写访问时，应使用 cbegin 和 cend</span>



### 2.4 容器定义和初始化

以下是容器定义和初始化

![image-20220521140016751](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220521140016751.png)

这些操作适用于大部分的容器，特别注意 `array` 容器的限制比较多。



#### 将一个容器初始化为另一个容器的拷贝

将一个新容器创建为另一个容器的拷贝的方法有两种：可以直接拷贝整个容器，或者（array 除外）拷贝由一个迭代器对指定的元素范围。

```cpp
list<string> authors = {"Milton", "Shalespeare", "Austen"};  // 列表初始化
vector<const char*> articles = {"a", "an", "the"};

list<string> list2(authors);		// 正确: 拷贝同一类型的整个容器
deque<string> authList(authors);	// 错误: 容器类型不匹配
// 正确: 可以将 const char* 元素转换为 string
forward_list<string> words(articles.begin(), articles.end());
// 迭代器it 表示 authors 中的一个元素
deque<string> authList1(authors.begin(), it);  // 拷贝元素, 直到(但不包括)it 指向的元素
```



> 当将一个容器初始化为另一个容器的拷贝时，两个容器的容器类型和元素类型都必须相同



#### 与顺序容器大小相关的构造函数

顺序容器（除array）还提供一个接受一个容器大小和一个（可选的）元素初始值的构造函数。

```cpp
vector<int> ivec(10, -1);	// 10 个 int 元素，每个都初始化为 -1
list<string> svec(10, "hi！");  // 10 个 string, 每个初始化为 "hi！"
forward_list<int> ivec(10);		// 10 个元素， 每个初始化为int默认值 0
deque<string> svec(10);			// 10 个元素，每个都是空string
```

如果元素类型是类类型，不指定元素的初始值，会自动调用默认的构造函数。所以对于没有默认构造函数的类类型，必须指定一个显式的元素初始值。

> 只有顺序容器的构造函数才接受大小参数，关联容器并不支持



#### 标准库 array 具有固定大小

和内置的数组一样，array 必须指定大小，且不能改变。

```cpp
array<int, 42>		// 类型为： 保存42个int的数组  int[42]
array<string, 10>	// 类型为： 保存10个string的数组  string[10]
    
array<string>   // 错误， 没有指定容器大小
```

> `array` 类型包含两部分，元素的类型，包含元素的个数

定义和初始化 array

```cpp
array<int , 10> ial;		// 10个默认初始化的int
array<int, 10> ia2 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};	// 列表初始化
array<int, 10> ia3 = {43};	// ia3[0]为42， 剩余元素为0
```

对于内置数组不能进行拷贝和对象赋值操作，但 array 并没有这种限制

```cpp
int digs[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
itn cpy[10] = digs;		// 错误， 内置数组不支持拷贝或赋值
array<int, 10> digits =  {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
array<int, 10>  copy = digits;		// 正确，只要数组类型匹配即合法
```

此外，array 的赋值和拷贝大小必须要一样，因为大小是 array 类型的一部分。



### 2.5 赋值和 swap

下表中列出的与赋值相关的运算符可用于所有容器

![image-20220521145045230](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220521145045230.png)

```cpp
array<int, 10> a1 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};	
array<int, 10> a2 = {0};     // 所有元素均为0
a1 = a2;		// 替换a1中的元素
a2 = {0};		// 错误，不能将一个花括号列表赋予数组
```

> 由于右边对象大小可能与左边运算对象的大小不同，因此 array 类型不支持 assign ，也不支持用花括号包围的值列表进行赋值



#### 使用 assign （仅顺序容器）

```cpp
list<string> names;
vector<const char*> oldstyle;
names = oldstyle;	// 错误，容器类不匹配， 使用 = 必须两侧类型一样
// 正确： 可以将const char* 转换为 string
name.assign(oldstyle.begin(), oldstyle.end());
```



#### 使用swap

`swap` 操作交互两个相同类型容器的内容。调用 `swap` 之后，两个容器中的元素将交换

```cpp
vector<string> svec1(10);	// 10个元素
vector<string> svec2(24);	// 24个元素
swap(svec1, svec2);
```

调用 swap 后，svec1 将包含24个string元素，svec2将包含10个string， swap 只是交互容器的内部数据结构。

元素不会被移动的事实意味着，除 string 外，指向容器的迭代器、引用和指针在 swap 操之后都不会失效。它们仍指向 swap 操作之前所指向的那些元素。但是，在 swap 操作之后，这些元素已经属于不同的容器了。例如，假定 `iter` 在 swap 之前指向 `svec1[3]` 的 string， 那么在 swap 之后它指向 `svec2[3]` 的元素。

- [ ] TODO 关于 `swap` 后面章节有更详细的介绍



### 2.6 容器大小操作

表9.2 列出了三个与大小相关的操作

- `size()`  返回容器中的元素数目
- `max_size()`  容器可保存的最大元素数目
- `empty()` 若容器中存储了元素，返回 `false` ，否则返回 `true`



### 2.7 关系运算符

每个容器类型都支持相等运算符（ `==` 和 `!=` ）；除了无需关联容器外的所有容器都支持关系运算符（ `>`、`>=` 、`<` 、`<=` ）。

比较两个容器实际是进行元素的逐对比较，工作方式和 `string` 的关系运算符类似：

- 如果两个容器具有相同大小且所有元素都两辆对应相等，则这两个容器相等；否则两个容器不等
- 如果两个容器大小不同，但较小容器中每个元素都等于较大容器中的对应元素，则较小容器小于大容器
- 如果容器都不是另一个容器的前缀子序列，则它们的比较结果取决于第一个不相等的元素的比较结果

例子：

```cpp
vector<int> v1 = {1, 3, 5, 7, 9, 12};
vector<int> v2 = {1, 3, 9};
vector<int> v3 = {1, 3, 5, 7};
vector<int> v4 = {1, 3, 5, 7, 9, 12};
v1 < v2;		// true
v1 < v3;		// false
v1 == v4;		// true
v1 == v2;		// false
```

> 只有当其元素类型也定义了相应的比较运算符时，才可以使用关系运算符来比较两个容器

<span style="background: yellow">容器的相等运算符实际使用元素的`==` 运算符实现比较，而其他关系运算符是使用元素的 `<` 运算符</span>

```cpp
vector<Sales_data> storeA, storeB;
if (storeA < storeB) // 错误：Sales_data 没有 < 运算符 
```





## 3. 顺序容器的操作

表9.2 列举了所有容器都支持的操作，下面将介绍顺序容器所特有的操作



### 3.1 向顺序容器添加元素

下表列出了向顺序容器（非array）添加元素的操作

![image-20220522093343407](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522093343407.png)





#### 使用 push_back

`push_back` 是将一个元素追加到容器的尾部，<span style="background: yellow">除 array 和 forward_list</span> ，每个顺序容器都支持该操作。

```cpp
// 从标准输入读取数据，将每个单词放到容器末尾
string word;
while (cin >> word)
    container.push_back(word);
```

对 `push_back` 的调用在 container 尾部创建一个新的元素，将 container 的 size 增大 1 。该元素的值为 word 的一个**拷贝**。

`container` 的类型可以是 list，vector 或 deque



#### 使用 push_front

`push_front` 将元素插入到容器的头部。<span style="background: yellow">除了 arrray、vector 和 string</span>，其他顺序容器都支持该操作

```cpp
list<int> ilist;
// 将元素添加到 ilist 开头
for (size_t ix = 0; ix != 4; ++ix)
    ilist.push_front(ix);
```

上面这段代码将元素 1、2、3、4 添加到 ilist 头部。



> 注意，deque 像 vector 一样提供了随机访问元素的能力，但它提供了 vector 所不支持的 push_front。deque 保证在容器尾部进行插入和删除元素的操作都只花费常数时间。与 vector 一样，在deque 尾部之外的位置插入元素会很耗时。



#### 在容器的特定位置添加元素

`insert` 成员提供了更一般的添加功能，它允许我们在容器的任意位置插入 0 个或多个元素。支持的容器有 vector、deque、list 和 string。forward_list 提供了特殊版本的 `insert`  （3.4 节）。

`insert` 提供了三个参数不同的重载形式，具体可参考 表9.5

```cpp
vector<string> svec;
list<string> slist;

// 等价于 slist.push_front("Hello!"); 
slist.inser(slist.begin(), "Hello!");

// vectoe 不支持 push_front，但是我们可以使用 insert 实现插入元素到头部
svec.insert(svec.begin(), "Hello!");	// 插入到 vector 末尾之外的任何位置都可能很慢

svec.insert(svec.end(), 10, "Anna");  // 将10个元素插入到svec的末尾，元素值为 "Anna"

vector<string> v = {"quasi", "simba", "frollo", "scar"};
// 将 v 的最后两个元素添加到 slist 的开始位置
slist.insert(slist.begin(), v.end() - 2, v.end());
// 运行时错误： 迭代器表示要拷贝的范围，不能指向与目的的位置相同的容器
slist.insert(slist.begin(), slist.begin(), slist.end());
```

<span style="border:2px solid Red; border-radius:5px;">C++11</span> 新标准下， `insert` 操作返回指向第一个新加入元素的迭代器。（旧版本的标准库中，这些操作返回 `void` ）。如果范围为空，不插入任何元素，`insert` 操作会将第一个参数返回。

`insert` 返回值使用

```cpp
list<string> lst;
auto iter = lst.begin();
while (cin >> word)
    iter = lst.insert(iter, word);  // 等价于调用 push_front
```



#### 使用 emplace 操作

<span style="border:2px solid Red; border-radius:5px;">C++11</span> 新标准引入三个新成员—— `emplace_front` 、`emplace` 和 `emplace_back` 。这些操作分别对应 `push_front` 、`insert` 、`push_back` 。 它们的不同之处，<span style="background: yellow">新标准的三个操作是将参数传递给元素的构造函数，直接在容器管理的内存空间构造元素，而 push 和 insert则是先将元素构造出来，再拷贝到容器的内存空间中。</span> 很显然新标准的插入操作更加的高效。这种差异使得用法也有些许的不同

```cpp
// 在 c 的末尾构造一个 Sales_data 对象
// 使用三个参数 Sales_data 构造函数
c.emplace_back("978-0590353403", 25, 15.99);
// 错误; 没有接受三个参数的 push_back 版本
c.push_back("978-0590353403", 25, 15.99);
// 正确： 创建一个临时的 Sales_data 对象传递给push_back
c.push_back(Sales_data("978-0590353403", 25, 15.99));
```

> `emplace` 函数在容器中直接构造元素。参数必须与元素的构造函数相匹配



### 3.2 访问元素

下表列出了我们可以用来在顺序容器中访问元素的操作。如果容器中没有元素，访问操作的结果是未定义的。

![image-20220522105447502](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522105447502.png)

`back` 和 `front` 分别返回首元素和为元素的引用：

```cpp
// 在解引用一个迭代器或调用 front 或 back 之前检查是否有元素
if (!c.empty())
{
    // val1 和 val2 是 c 中第一个元素值的拷贝
    auto val1 = *c.begin(), val2 = c.front();
    // val3 和val4 是 c 中最后一个元素的拷贝
    auto last = c.end();
    auto val3 = *(--last);		// 不能递减 forward_list 迭代器
    auto val4 = c.back();		// forward_list 不支持
}
```

`back` 和 `front` 返回的是引用，但是用 `auto` 接受的只是值拷贝，如果需要得到元素的引用，应该使用 `auto&` 

在容器访问元素的成员函数（即 `front` 、`back`、下标和 `at`）返回的都是引用。如果一个容器是一个 const 对象，则返回值是一个 const 引用。

```cpp
if (!c.empty())
{
    c.front() = 42;		// front 返回的是第一元素的引用，将42赋予c中的第一个元素
    auto &v = c.back();	// 获得指向最后一个元素的引用
    v = 1024;			// 改变c中的元素
    auto v2 = c.back();	// v2 不是一个引用，它是 c.back() 的一个拷贝
    v2 = 0；			   // 未改变c中的元素
}
```



#### 下标操作和安全的随机访问

```cpp
vector<string> svec;	// 空vector
cout << svec[0];		// 运行时错误： svec 中没有元素
cout << svec.at(0);		// 抛出一个 out_of_range 异常
```



### 3.3 删除元素

下表是与删除相关的成员函数

![image-20220522171026712](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522171026712.png)

> 删除元素的成员函数并不检查其参数。在删除元素之前，必须确保它们是存在的。



#### pop_front 和 pop_back 成员函数

`pop_front` 和 `pop_back` 成员函数分别删除首元素和尾元素。vector 和 string 不支持 `pop_front` ， forward_list 不支持 `pop_back`

```cpp
while (!ilist.empty())
{
    process(ilist.front());		//对ilist 的首元素进行一些处理
    ilist.pop_front();			// 完成处理后删除首元素
}
```

这些操作返回 `void` 。 如果需要删除的元素，就必须在执行弹出操作之前保存。



#### 从容器内部删除一个元素

成员函数 `erase` 从容器中指定位置删除元素。我们可以删除由一个迭代器指定的单个元素，也可以删除一对迭代器指定的范围内的所有元素。两种形式的 `erase` 都返回指向删除的（最后一个）元素之后的位置的迭代器。即， 若 j 是 i 之后的元素，那么 `erase(i)` 将返回指向 j 的迭代器。

```cpp
// 循环删除一个list 中的所有奇数元素
list<int> lst = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
auto it = lst.begin();
while (it != lst.end())
{
    if (*it % 2)	
        it = lst.erase(it);
	else
        ++it;
}    
```



#### 删除多个元素

接受一对迭代器的 `erase` 版本允许我们删除一个范围的元素：

```cpp
// 删除两个迭代器表示的范围内的元素
// 返回指向最后一个被删除元素之后位置的迭代器
elem1 = slist.erase(elem1, elem2);		// 调用后，elem1 == elem2
```

迭代器 `elem1` 指向我们要删除的第一个元素，`elem2` 指向我们要删除的最后一个元素之后的位置。

```cpp
// 删除所有元素
slist.clear();		// 删除人容器中的所有元素
slist.erase(slist.begin(), slist.end());  // 等价调用
```



### 3.4  特殊的 forward_list 操作

forward_list 的特殊操作是由该容器的数据结构决定的，forward_list 是一个单向链表，下图分别是从 forward_list 从删除和添加一个元素的示意图。

![image-20220522185238536](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522185238536.png)



![image-20220522185306051](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522185306051.png)

可以看到删除和添加元素都是需要先找到需要删除和添加位置的**前一个位置**的元素。

 下表是 forward_list 提供的操作

![image-20220522185801487](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522185801487.png)

当在操作 forward_list 时，我们必须关注两个迭代器——一个指向我们要处理的元素，另一个指向其前驱

```cpp
// 删除 forward_list 中的奇数元素
forward_list<int> flst = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
auto prev = flst.before_begin();		// 表示 flst 的 "首前元素"
auto curr = flst.begin();				// 表示 flst 中的第一个元素
while (curr != flst.end())
{
    if (*curr % 2)
        curr =  flst.erase_after(prev);
    else
    {
    	prev = curr;		// 移动迭代器 curr, 指向下一个元素，prev 指向
        ++curr;				// curr 之前的元素
    }
}
```



### 3.5 改变容器大小

下表是容器提供的改变大小的操作

![image-20220522191408655](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220522191408655.png)



### 3.6 容器操作可能使迭代器失效

向容器中添加元素和从容器中删除元素的操作可能会使指向容器元素的指针、引用或迭代器失效。

**向容器添加元素后：**

- 如果容器是 vector 或 string，且存储空间被重新分配，则指向容器的迭代器、指针和引用都会失效。如果存储空间未重新分配，指向插入位置之前的元素迭代器、指针和引用仍有效，但指向插入位置之后元素的迭代器、指针和引用会失效。
- 对于deque，插入到除首尾位置之外的任何位置都会导致迭代器、指针和引用失效。如果在首尾位置添加元素，迭代器会失效，但指向存在的元素的引用和指针不会失效。
- 对于 list 和 forward_list，指向容器的迭代器（包括尾后迭代器和首前迭代器）、指针和引用仍有效。

当我们从一个容器中删除元素后，指向被删除元素的迭代器、指针和引用会失效，这是很显然的。

**当我们删除一个元素后：**

- 对于 list 和 forwar_list，指向容器其他位置的迭代器（包括尾后迭代器和首前迭代器）、引用和指针仍有效。
- 对于 deque，如果在首尾之外的任何位置删除元素，那么指向被删除元素外其他元素的迭代器、引用或指针也会失效。如果是删除 deque 的尾元素，则尾后迭代器也会失效，但其他迭代器、引用和指针不受影响；如果删除首元素，这些也不会受影响。
- 对于 vector 和 string，指向被删除元素之前元素的迭代器、引用和指针仍有效。注意：但我们删除元素时，尾后迭代器总是会有效。

> 使用失效的迭代器、指针或引用是严重的运行时错误。



```cpp
// 灾难：此循环的行为是未定义的
auto begin = v.begin(),
	end = v.end();		// 保存尾迭代器的值是一个坏主意
while (begin != end)    // 循环中添加了新的元素，此时end迭代器已经失效了
{
    // 做一些处理
    // 插入新值，对begin重新赋值，否则的话它就会失效
    ++begin;	// 向前移动begin，因为我们想在此元素之后插入元素
    begin = v.insert(begin, 42);	// 插入新值
    ++begin;	// 向前移动begin 跳过我们刚刚加入的元素
}
```



## 4. vector 对象是如何增长的

vector 是将元素连续存储，在内存中每个元素紧挨着前一个元素。vector 容器大小是可变的，在我们向容器中添加元素的时候，如果容器此时没有了空间。那么容器会在内存中寻找一个更大的地方，把当前所有元素复制到新的位置，然后添加新的元素，释放原来的存储空间。如果在容器满了之后，每添加一个元素，vector 就执行一次内存分配和释放操作，性能会慢到不可接受。

为了减少容器空间重新分配次数，标准库的实现一般采用这样的策略，<span style="background: yellow">当需要分配新的通常会分配比新空间需求更大的内存空间</span>，比如说分配当前空间的 2 倍（不同的标准库实现可能不同）。



#### 管理容量的成员函数

下表容器大小的相关操作。`capacity` 操作告诉我们容器在不扩张内存空间的情况下可以容纳多少个元素，`reserve` 操作允许我们通知容器它应该准备保存多少个元素。

![image-20220523092048973](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220523092048973.png)

> `reserve` 并不改变容器中元素的数量，它仅影响 vector 预先分配多大的内存空间

一个 vector 容器的 capacity 是指可以存放多少个元素， size是当前容器中的元素个数。调用 `reserve` 可以将容器调整到指定的大小 n，但是如果 n 小于当前的 capacity 时，reserve 什么也不做；只有当 n 大于当前的 capacity  时才会调整容器的空间。

`shrink_to_fit` 操作时将容器的 capacity 调整到等于 size。但是在不同的标准库的实现可能不同，有的可能并不保证一定退回内存空间。



#### capacity 和 size

`capacity ` 是容器的空间大小（可以存放元素的个数），`size` 是容器当前元素的个数。两者的关系是 capacity 大于等于 size。具体两者相差多少取决于标准库的实现中对内存空间的分配策略。

```cpp
vector<int> ivec;
cout << " ivec: size: " << ivec.size()
	<< " capacity: " << ivec.capacity() << endl;

for (vector<int>::size_type ix = 0; ix != 24; ix++)
{
	ivec.push_back(ix);
}
cout << " ivec: size: " << ivec.size()
	<< " capacity: " << ivec.capacity() << endl;
```

上面代码在 vs2022 的输出：

```shell
ivec: size: 0 capacity: 0
ivec: size: 24 capacity: 28
```

此时 ivec 的空间状态如下图所示：

![image-20220523171032382](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220523171032382.png)

## 5. 额外的 string 操作

除了顺序容器共同的操作之外，string 类型还提供了一些额外的操作。



### 5.1 构造 string 的其他方法

在第 3 章我们已经介绍过了一些构造函数，除此之外，string 还支持另外三个构造函数，如下表

![image-20220524105009076](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524105009076.png)

```cpp
const char *cp = "Hello World!!!";      // 以空字符结束的数组
char noNull[] = {'H', 'i'};             // 不是以空字符结束
string s1(cp);  // 拷贝cp中的字符直到遇到空字符； s1 == "Hello World!!!"
string s2(noNull, 2);   // 从 noNull 拷贝两个字符； s2 == "Hi"
string s3(noNull);      // 未定义： noNull 不是以空字符结束
string s4(cp + 6, 5);   // 从 cp[6] 开始拷贝5个字符； s4 == "World"
string s5(s1, 6, 5);    // 从 s1[6] 开始拷贝5个字符； s5 == "World"
string s6(s1, 6);       // 从 s1[6] 开始拷贝，直至 s1 末尾； s6 == "World!!!"
string s7(s1, 6, 20);   // 正确， 只拷贝到 s1 末尾； s7 == "World!!!"
string s8(s1, 16);      // 抛出一个 out_of_range 异常
```



#### substr 操作

`substr` 操作返回一个字符串的指定的子串

![image-20220524162808729](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524162808729.png)

```cpp
string s("hello world");
string s2 = s.substr(0, 5);     // s2 = hello
string s3 = s.substr(6);        // s3 = world
string s4 = s.substr(6, 11);    // s4 = world (书中写的： s3 = world)
string s5 = s.substr(12);       // 抛出一个 out_of_range 异常
```



### 5.2 改变 string 的其他方法

string 类型除了顺序容器共有的一些操作（赋值运算符、assign、insert、erase），它还定义了额外的 insert 和 erase 版本。

![image-20220524163829412](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524163829412.png)

![image-20220524163840185](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524163840185.png)



相关的示例代码和说明略。



### 5.3 string 搜索操作

​	下表是 string 提供的字符串搜索操作

![image-20220524170424466](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524170424466.png)

![image-20220524170434820](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524170434820.png)

相关的示例代码和说明略。



### 5.4 compare 函数

compare 函数有6个版本，如下表

![image-20220524170605513](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524170605513.png)

相关的示例代码和说明略。



### 5.5 数值转换

string 提供的数值转换操作如下表

![image-20220524170714772](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524170714772.png)

相关的示例代码和说明略。



## 6. 容器适配器

除了顺序容器外，标准库还定义了三个顺序容器适配器：`stack`、`queue` 、`priority_queue`。**适配器** 是标准库中的一个通用概念。容器、迭代器和函数都有适配器。本质上，一个适配器是一种机制，能使某种事物的行为看起来像另外一种事物一样。一个适配器接受一种已有的容器类型，使其行为看起来像一种不同的类型。比如，`stack` 适配器接受一个顺序容器（除 array 或 forward_list 外），并使其操作起来像一个 stack 一样。下表列出了所有容器适配器都支持的操作和类型。

![image-20220524191942616](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524191942616.png)

#### 定义一个适配器

每个适配器都定义两个构造函数：默认构造函数创建一个空对象，接受一个容器的构造函数拷贝该容器来初始化适配器。

```cpp
stack<int> stk(deq);		// 从 deq 拷贝元素到 stk
```

默认情况下，stack 和 queue 是基于 deque 实现的， priority_queue 是在vector 之上实现的。我们可以在创建一个适配器时将一个命名的顺序容器作为第二个类型参数，来重载默认容器类型

```cpp
//  在 vector 上实现的空栈
stack<string, vector<string>> str_stk;
// str_stk2 在 vector 上实现，初始化时保存 svec 的拷贝
stack<string, vector<string>> str_stk2(svec)；
```

所有适配器都要求容器具有添加和删除元素的能力。因此，适配器不能构造在 array 之上。类似的，我们不能用 forward_list 来构造适配器，因为所有适配器都要求容器具有添加、删除以及访问尾元素的能力。

- `stack` 只要求 `push_back`、`pop_back` 和 `back` 操作，因此可以使用除了 array 和 forward_list 之外的任何容器构造 stack。
- `queue` 适配器要求 `back` 、`push_back` 、`front` 和 `push_front` ，因此它可以构造于 list 或  deque 之上，但不能基于 vector 构造。
- `priority_queue` 除了 `front` 、`push_back` 和 `pop_back` 操作之外还要随机访问能力，因此它可以构造于 vector 或 deque 之上，但不能基于 list 构造。

#### 栈适配器

`stack` 类型定义在 `stack` 头文件中。下表列出了 stack 所有支持的操作

![image-20220524193924464](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524193924464.png)

```cpp
stack<int> intStack;	// 空栈
// 填满栈
for (size_t ix = 0; ix != 10; ++ix)
    intStack.push(ix);		// intStack 保存 0 到 9 十个数
while (!intStack.empty()) 
{ // intStack 中有值就继续循环
    int value = intStack.top();
    // 使用栈顶值得代码
    intStack.pop();  // 弹出栈顶元素，继续循环
}
```



#### 队列适配器

`queue` 和 `priority_queue` 适配器定义在 `queue` 头文件中。下表列出了它们都支持得操作

![image-20220524194713909](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524194713909.png)

![image-20220524194729390](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220524194729390.png)

`queue` 和 `priority_queue`  适配器的访问策略就是和对应的数据结构一致。

