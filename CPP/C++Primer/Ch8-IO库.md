---
title: "第8章 IO库 (C++ Primer 5th)"
date: 2022-09-01T10:09:47+08:00
lastmod: 2022-09-01T10:09:47+08:00
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch8"
draft: false # 是否为草稿
comments: true
reward: false # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/th_id=OHR.Migliarino_ZH-CN0744250844_1920x1080.jpg #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---



## 1. IO 类

为了支持不同种类的IO处理操作，在 `istream` 和 `ostream` 之外，标准库还定义了其他一些IO类型，我们之前都已经使用过了。下表列出了这些类型，分别定义在三个独立的头文件中： `iostream` 定义了用于读写流的基本类型，`fstream` 定义了读写命名文件的类型， `sstream` 定义了读写内存 `string` 对象的类型。

![image-20220902190052646](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220902190052646.png)



### 1.1 IO 对象无拷贝或赋值

我们不能拷贝或对IO对象赋值：

```cpp
ofstream out1, out2;
out1 = out2;		// 错误：不能对流对象赋值
ofstream print(ofstream);	// 错误：不能初始化 ofstream 参数
out2 = print(out2);			// 错误：不能拷贝流对象
```

由于不能拷贝IO对象，因此我们也不能将形参或返回类型设置为流类型，进行IO操作的函数通常以**引用方式** 传递和返回流。读写一个IO对象会改变其状态，因此传递和返回的引用不能是 `const` 的。



### 1.2 条件状态

IO操作一个与生俱来的问题是可能发生错误。下表列出了IO类定义的一些函数和标志，可以帮助我们访问和操纵流的条件状态

![image-20220902191021098](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220902191021098.png)

![image-20220902191041245](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220902191041245.png)

一个IO错误的例子：

```cpp
int ival;
cin >> ival;
```

如果我们在标准输出键入 Boo，读操作就会失败。代码中的输入运算符期待读取一个 int，但却得到一个字符 B。这样，`cin` 会进入错误状态。一个流一旦发生错误，其后续的IO操作都会失败。



#### 查询流的状态

将流作为条件使用，只能告诉我们流是否有效，而无法告诉我们具体发生了什么。IO库定义了一个机器无关的`iostate` 类型，它提供了表达流状态的完整功能。具体可参考表8.2



### 1.3 管理输出缓冲

每个输出流都管理一个缓冲区，用来保存程序读写的数据，例如：

```cpp
os << "please enter a value: ";
```

文本串可能立即打印出来，但也可能被操作系统保存在缓冲区中，随后再打印。缓冲机制可以不用频繁的写设备，使性能有很大的提升。

下面是导致缓冲刷新的原因：

- 程序正常结束，作为 `main` 函数的 `return` 操作的一部分，缓冲刷新被执行。
- 缓冲区满时，需要刷新缓冲，而后新的数据才能继续写入缓冲区。
- 我们可以使用操纵符如 `endl` 来显式刷新缓冲区。
- 在每个输出操作之后，我们可以用操纵符 `unitbuf` 设置流的内部状态，来清空缓冲区。默认情况下，对 `cerr` 是设置 `unitbuf` 的，因此写到 `cerr` 的内部都是立即刷新的。
- 一个输出流可能被关联到另一个流。在这种情况下，当读写被关联的流时，关联到的流的缓冲区会被刷新。例如，默认情况下，`cin` 和 `cerr` 被关联到 `cout` 。因此读 `cin` 或写 `cerr` 都会导致 `cout` 的缓冲区刷新。



#### 刷新输出缓冲区

除了 `endl` ，IO库中还有两个类似的操纵符用来刷新缓冲区：`flush` 和 `ends`

```cpp
cout << "hi!" << endl;		// 输出 hi 和一个换行，然后刷新缓冲区
cout << "hi!" << flush;		// 输出hi, 然后刷新缓冲区，不附加额外字符
cout << "hi!" << ends;		// 输出hi和一个空字符，然后刷新缓冲区
```



#### ubitbuf 操纵符

如果想在每次输出操作后都刷新缓冲区，我们可以使用 `unitbuf` 操纵符。它告诉流在接下来的操作每次写操作都进行一次 `flush` 操作。而 `nounitbuf` 操纵符则重置流，使其恢复以用正常的系统管理的缓冲区刷新机制：

```cpp
cout << unitbuf;				// 所有输出操作后都会立即刷新缓冲区
// 任何输出都立即刷新，无缓冲
cout << nounitbuf；			// 回到正常的缓冲方式
```

 <span style="background: yellow"> 如果程序崩溃，输出缓冲区不会被刷新</span>



#### 关联输入和输出流

标准库将 `cout `  和 `cin` 关联在一起，因此下面语句

```cpp
cin >> ival;
```

导致 `cout` 的缓冲区被刷新。

> 交互式系统通常应该关联输入和输出流。这意味着所有输出，包括用户提示信息，都会在读操作之前打印出来。





## 2. 文件输入输出

头文件 fstream 定义了三个类型来支持文件IO：

- `ifstream` 从一个给定文件读取数据
- `ofstream` 向一个给定文件写入数据
- `fstream` 可以读写给定文件

这些类型提供的操作与 `cin` 和 `cout` 的一样，用IO运算符 （`<<` 和 `>>`） 来读写文件。除了继承自`iostream` 类型的行为之外，`fstream` 中定义的类型还增加了一些新的成员来管理与流关联的文件。

![image-20220903090952586](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220903090952586.png)



### 2.1 使用文件流对象

创建文件流对象时，我们可以提供文件名（可选的）。如果提供一个文件名，则 `open` 会自动被调用：

```cpp
ifstream in(ifile);			// 创建一个 ifstream 流对象 in,并打开给定文件
ofstream out;				// 输出文件流对象out,未关联到任何文件
```

文件名`ifile` 是一个 `string` 类型的字符串，在新标准中也可以是 C 风格的字符数组。



##### 用 `fstream` 代替 `iostream&`

7.1.3 节中 `read` 和 `print` 的函数原型

```cpp
istream &read(istream &is, Sales_data &item);
ostream &print(ostream &os, const Sales_data &item);
```

在这两个函数中，形参是 `iostream` 类型的引用，我们可以传入其继承类 `fstream` （或 `sstream`) 类型来调用。如果一个函数接受一个 `ostream&` 参数，可以传递给它一个 `ofstream` 对象，对 `istream&`  和 `ifstream` 也是类似的。

在下面的示例中，我们假定输入和输出文件的名字是通过传递给 `main` 函数的参数来指定的

```cpp
ifstream input(argv(1));		// 打开销售记录文件
osftream output(argv[2]);		// 打开输出文件
Sales_data total;				// 保存销售总额的变量
if (read(input, total)) {
    Sales_data trans;
    while(read(input, trans)) {
        if (total.isbn() == trans.isbn()) {
            total.combine(trans);
        } else {
            print(output, total) << endl;
            total  = trans;
        }
    }
    print(output, total) << endl;
} else {
    cerr << "No data?" << endl;
}
```





#### 成员函数 open 和 close

定义一个空文件流对象，调用 open 将它与文件关联

```cpp
ifstream in(ifile);			// 创建一个iftream 并打开给定文件
ofstream out;				// 输出文件流未与任何文件关联
out.open(ifile + ".copy");	// 打开指定文件
if (out)   // 检查 open 是否成功
    // 成功，科室使用文件
```



> 当一个 `fstream` 对像被创建时会自动调用 `open` ，被销毁时会自动调用 `close`



### 2.2 文件模式

每个流都有一个关联的文件模式，用来指出如何使用文件，如下表

![image-20220903145853264](https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/image-20220903145853264.png)

 



## 3. string 流



- [ ] TODO
