---
title: "第7章 类(C++ Primer 5th)"
date: 2022-04-27
lastmod: 2022-08-31
author: ["Kinvy"]
keywords: ["C++","C++ Primer 5th"]
categories: ["Programming language"]
tags: ["C++","C++ Primer 5th"]
description: ""
weight: 
slug: "ch7"
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
    image: https://kinvy-images.oss-cn-beijing.aliyuncs.com/Images/th_id=OHR.WinterRoofs_ZH-CN5091303265_1920x1080.jpg 
    caption: "" 
    alt: ""
    relative: false
---





## 类的基本概念

本小节的内容对应书中第2章最后一节（2.6 自定义数据结构）

正如标题，**类**就是一种自定义数据结构，这种数据结构把多个变量和函数封装在一起。几个关于类的几个基本概念：

- **成员变量**， 也叫**类的属性** ，就是类中声明定义的变量，可以是基本类型变量或是类类型变量
- **成员函数**，也叫方法，就是类中声明的函数
- **实例对象**，通过类类型创建的变量

在后面的这些名称会混着用

定义类的语句形式：

```cpp
//使用struct关键字
struct ClassName{
  //类属性
  //...  
  
  //方法
  //...
};
//使用class关键字
class ClassName{
  //类属性
  //...  
  
  //方法
  //...
};
```



> 说明：
>
> 1. `struct` 和 `class` 定义类两者基本是等价的，只是默认的访问权限不同，关于权限的问题在7.2节会详细讲解，在这之前我们定义类都使用 `struct`
> 2. 类中的成员变量（属性）定义的位置不影响调用，即可以在类的任意位置，也就是说我们可以在声明定义一个变量之前就可以使用该变量，这个规则只限于类方法和类属性
> 3. 定义类的语法 `{}` 后面有一个 `;` ,不用漏了





## 1. 定义抽象数据类型

本小节是一个具体的实例，设计一个 `Sales_data` 类，这个类在第二章有讲到，我们不用回头看第二章，直接跟着书中的步骤就可以了。



### 1.1 设计 Sales_data 类

我们知道类有属性和方法，设计一个类首先要确定这些（这是根据类的功能确定的）。这个类是表示书的销售，这里直接给出类需要的属性和方法。

类的接口（对外可以使用的函数，部分函数不是类的成员函数）

- 一个 `isbn` 成员函数，用于返回对象的ISBN编号
- 一个 `combine` 成员函数，用于将一个 Sales_data 对象加到另一个对象上
- 一个 `avg_price`成员函数，用于返回售出数书籍的平均价格
- 一个名为 `add` 的函数，指向两个Sales_data 对象的加法
- 一个 `read` 函数，将数据从 istream 读入到 Sales_data 对象中
- 一个 `print` 函数，将Sales_data 对象的值输出到 ostream

成员变量：

- `bookNo` 表示 ISBN编号，string类型
- `units_sold` 表示某本数的销量， unsigned
- `revenue` 表示这本书的总销售收入

对应类的定义代码

```cpp
//Sales_data.h
struct Sales_data{ 
  	//成员函数
  	std::string isbn() const { return bookNo; }
  	Sales_data& combine(const Sales_data&);
    dobule avg_price() const;
    
    //成员变量
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
  
};
//Sales_data的非成员接口函数
Sales_data add(const Sales_data&, const Sales_data&);
std::ostream &print(std::ostream&, const Sales_data&);
std::istream &read(std::istream&, Sales_data&);
```



#### Sales_data 类的说明

使用下面的语句定义一个 Sales_data 类型的对象

```cpp
Sales_data total;		//total 是一个类对象
```



### 1.2 定义改进的 `Sales_data` 类

####  引入 this

使用total对象对isbn 函数的调用：

```
total.isbn();
```

我们可以看到 `isbn()` 的函数体内容是 `return bookNo;` 我们知道上面的调用语句返回的就是 `total` 对应的 `bookNo` 属性（`total.bookNo)`，但是在 `isbn()` 函数体内并没显式的指明需要返回哪个实例对象的属性，这里其实有一个隐式的参数 `this`.

`this` 是一个<span style="background: yellow">指针</span>，准确的说是一个常量指针，指向的是当前调用该函数的对象的地址。比如上面的调用语句，this 指向的是 total 的地址，所以上面的调用过程是：

```cpp
//伪代码，用于说明调用成员函数的实际过程
Sales_data::isbn(&total);
```

每个成员函数都会一个隐式的形参`this`，实际 `isbn()` 成员函数为

```cpp
std::string isbn(Sales_data *const this);	//错误代码
```

<span style="background: yellow">上面代码是为了说明成员函数有一个名为this的隐式形参，实际代码不能把这个形参写出来</span>

虽然不能写出来，但是我们在成员函数体内可以使用`this` 这个形参名，比如 isbn的函数体可以改成

```cppp
return this->bookNo;		//正确，返回当前对象实例的bookNo属性
```



####  引入const成员函数

```cpp
std::string isbn() const { return bookNo; }
```

`isbn()` 函数还有一个之前没有出现的知识点，我们可以看到在函数的参数列表后面还有一个 `const` 关键字，把这种形式的成员函数叫做 **常量成员函数** （简称 常函数）

常量成员函数的本质实际上是对 `this` 指针的限制，前面说到 `this` 是一个常量指针，类型是 `Sales_dat *const` ，这是只有顶层const，而常量成员函数又加上了底层const，`this`指针变成了 `const Sales_data *const` 类型（关于顶层const和底层const在第二章有详细的讲解）。`isbn` 函数真正的原型为

```cpp
std::string isbn(const Sales_data *const this) { return bookNo; }		
```

<span style="background: yellow">再次说明，this是一个隐式形参，实际代码不能把这个形参写出来</span>



常量成员函数和普通的成员函数有什么区别呢？它们是不是一组重载函数？

```cpp
std::string isbn() const { return bookNo; }		//1
std::string isbn() { return bookNo; }			//2
```

<span style="background: yellow">它们是重载函数</span> ，我们把这两个函数的真实的形式写出来，以同样的方式和条件调用，看下是否只有唯一一个匹配的函数。<span style="background: yellow">再再次说明，实际代码不能把this形参显式写出来</span>

```cpp
//isbn() 的真实形式，实际代码不能把this写出来
std::string isbn(Sales_data *const this);			//普通成员函数
std::string isbn(const Sales_data *const this);		//常量成员函数	

//定义两个 Sales_data 类型变量
Sales_data total;
const Sales_data c_total;
total.isbn();		//1
c_total.isbn();		//2

//对应的伪代码，用于说明调用成员函数的实际过程
Sales_data::isbn(&total);		//调用 std::string isbn(Sales_data *const this);	
Sales_data::isbn(&c_total);		//调用std::string isbn(const Sales_data *const this);
```

传参的规则实际和变量初始化的规则一样（第二章中的内容），我们观察下面的几个变量初始化

```cpp
Sales_data total;
const Sales_data c_total;

Sales_data *const this = &total;			//正确，用一个非常量对象初始化一个顶层const
Sales_data *const this = &c_total;			//错误，&c_total包含一个底层const

const Sales_data *const this = &total;		//错误	
const Sales_data *const this = &c_total;	//正确
```



> 总结：常量对象只能调用常函数，非常量对象既可以调用常量函数，也可以调用普通函数



以上内容比较抽象，建议结合2.4节内容理解，关于常函数还有部分的知识点（常函数对类内属性的使用），后面的章节再讲



#### 类作用域和成员函数

在 `isbn()` 函数中使用了 `bookNo` 成员变量，而这个变量的声明式在函数的后面。按照之前关于作用域的知识，变量使用前必须声明，这里和编译器的处理方式有关。编译器对类的处理分两步：首先编译成员的声明，然后才轮到成员函数。因此，成员函数体可以随意使用类中的其他成员而无须在意这些成员出现的次序。



#### 在类的外部定义成员函数

`Sales_data` 类的成员函数只有 `isbn()`有定义，其他的成员函数都是只有声明。关于成员函数，它是支持类内声明，类外定义的。类外定义函数需要包含所属的类名，具体形式如下

```cpp
//avg_price() 的类外定义
double Sales_data::avg_price() const {
    if(units_sold)
        return revenue / units_sold;
    else
        return 0;
}
```

成员函数类外定义和普通函数的定义没有什么区别，只是成员函数需要在函数名的前面加上对应的类名。

<span style="background: yellow">类内定义的成员函数默认是inline函数，定义在类外的要成为内联函数，需要显显式的指定，在函数定义的前面加 inline关键字</span>



#### 定义一个返回this对象的函数

函数 `combine` 的设计初衷类似于复合赋值运算符 `+=` , 调用该函数传入的是一个 Sales_data对象，返回的还是该对象本身，这里我们可以使用 `this` 指针实现

```cpp
Sales_data& Sales_data::combine(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;		//this 是调用该函数的对象的指针，对其解引用得到的就是该对象
}
```

<span style="background: yellow">再再再次说明，this 是一个隐式形参，不能写出来，但是可以再函数体中使用</span>



### 1.3 定义类的非成员函数

`add` ，`read`，`print` 不是类的成员函数，但是它们出和该类相关的接口函数，所以把它们和类定义在同一个文件中，以下是这三个函数的定义

```cpp
istream &read(istream &is, Sales_data &item)
{
    double price = 0;
    is >> item.bookNo >> item.units_sold >> price;
    item.revenue = price * item.units_sold;
    return is;
}
ostream &print(ostream &os, const Sales_data &item)
{
    os << item.isbn() << " " << item.units_sold << " "
        << item.revenue << " " << item.avg_price();
    return os;
}
Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return sum;
}
```



各个函数功能不作过多的说明



### 1.4 构造函数

<span style="border:2px solid Red">C++11</span>  新标准允许在类属性定义时初始化，比如`Sales_data` 类中`units_sold ` 和 `revenue ` 都被初始化为0.但是并不是所有的属性都需要在类内初始化的，有些可能需要类的使用者自定义的初始化。这个时候我们就需要要到类的 **构造函数** ，构造函数的形式：类名就是函数名，没有返回值

```cpp
struct Entity{
    double x, y;
    //构造函数
    Entity()
    {
       x = 0;
       y = 0;
    }
    
    void print()
    {
        cout << x << "," << y << endl;
    }
}

int main()
{
    Entity e;	//等价   Entity e;   e.Entity(); 两个语句
    e.print();
    
    return 0;
}
```

构造函数不需要显示的调用，在创建对象实例时，会自动调用构造函数



#### 构造函数可以重载

关于构造函数

- 在没有写构造函数的情况下，编译器会提供一个**默认构造函数**，该函数是没有参数的，并且不执行任何操作，对于没有参数的构造函数也叫称作**无参构造**
- 构造函数可以重载，重载的有形参的构造函数也叫做**有参构造**
- 只要写了任何一种构造函数（有参构造或无参构造），编译器就不再提供默认的构造函数

对于 `Sales_data` 的实例，设计了以下构造函数

```cpp
struct Sales_data {
    //新增的构造函数
    Sales_data() {  }		//无参构造
    Sales_data(const std::string &s) : bookNo(s) { }		//有参构造1
    Sales_data(const std::string &s, unsigned n, double p):
    			bookNo(s), units_sold(n), revenue(p*n) {}   //有参构造2
    Sales_data(std::istream &);		//有参构造3
    
};
```

有参构造1和2使用了**初始化列表**的方式初始化成员属性 ，即形参列表后面和函数体`{}` 之间的语句

```cpp
//有参构造1的等价形式
Sales_data(const std::string &s) { bookNo = s; }
//有参构造2的等价形式
Sales_data(const std::string &s, unsigned n, double p)
{
    bookNo = s;
    units_sold = n;
    revenue = p*n; 
}
```

使用 `=` 赋值运算符和使用初始化列表初始化成员属性效果是一样的。但是初始化列表的方式而且更高效，<span style="background: yellow">强烈建议使用初始化列表初始化成员属性</span>

构造函数的类外定义

```cpp
//其他的构造已经在类内定义了
inline Sales_data::Sales_data(std::istream &is)
{
    read(is， *this);
}
```

<span style="background: yellow">定义在类内的函数，自动成为内联函数</span>， 在类外定义成员函数，如果需要成为内联函数，需要手动添加 `inline` 关键字



#### 构造函数的调用

构造函数不需要我们手动的调用，也没有提供给我调用的语法，构造函数的调用是编译器根据我们初始化实例对象的方式调用不同的构造函数

```cpp
//1.调用无参构造
Sales_data total;	
//使用括号调用无参构造
Sales_data();	//会创建一个临时的对象，如果需要使用这个对象就要变量接收
Sales_data total = Sales_data();	//用total接收临时变量
Sales_data total();		//错误，这不不调用无参构造创建实例对象，而是声明一个返回类型为Sales_data的函数

//2. 调用有参构造
Sales_data total("654323");		//调用 Sales_data(const std::string &s)构造函数创建实例
```

构造函数：

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造，无须手动调用,而且只会调用一次





### 1.5 拷贝、赋值和析构

c++编译器默认情况下至少给一个类添加4个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行值拷贝
4. 赋值运算符 `operator=`，对属性进行值拷贝



#### 析构函数

默认构造函数前面已经提过了，**析构函数** 也是由编译器调用的，它的调用时机是在对象销毁的时候被调用，析构函数的作用一般是用来释放内存空间（主要是堆上的内存空间），它语法形式如下

```cpp
//Sales_data的析构函数
struct Sales_data {
    //..
    
    //析构函数
    ~Sales_data()
    {
        //...
    }
};
```

析构函数语法 ：`~类名() {}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同,在名称前加上符号 `~`
3. 析构函数不可以有参数，因此不可以发生重载
4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次



#### 拷贝构造函数

下面的语句会调用拷贝构造函数

```cpp
//trans 是一个已经定义的 Sales_data 对象
Sales_data total(trans);	//1
Sales_data total = trans;	//2
//下面的语句不是调用拷贝构造
Sales_data total;
total = trans;		//调用的是赋值操作
```

默认拷贝构造函数的操作等价于把属性进行值拷贝，等价的语句

```cpp
//1 和 2等价的语句
total.bookNO = trans.bookNo;
total.units_sold = trans.units_sold;
total.revenue = trans.revenue;
```



#### 赋值

赋值操作是通过重载赋值运算符来实现的,  `operator=`

```cpp
Sales_data total;
total = trans;		//调用的是赋值操作
```

编译器提供的默认赋值运算和默认拷贝构造操作是一样的，就是对属性值进行值拷贝。

> 拷贝构造函数和赋值操作也可以自定义，如何自定义以及相关的使用在后面章节有详细的讲解，这里只是一个简单的介绍

## 2. 访问控制与封装

到目前为至我们定义类都是使用 `struct` 关键字，前面有提过 `class` 以及两者的区别。这小节是讲访问控制与封装。

首先，回顾一下面向对象编程的三大特点之一，封装。像之前那样把属性（变量）和方法（函数）集中到一个类中，这就是封装。但是真正意义的封装还需要加上访问控制，所谓访问控制就是对于类中的某些方法和属性在外部是无法访问的。举个例子，比如前面定义的`Sales_data` 类，我们使用的是`struct` ,这种定义方式我们可以随意的修改类内的属性值，甚至多不许要调用方法

```cpp
Sales_data total;			//声明一个 Sales_data 的实例对象 total
total.units_sold = 10;		//通过类实例对象直接修改属性值
```

上面这种操作通常是不被允许的，如果类的设计者和使用这都是我们自己的话，我们明白（但代码量多到一定程度也不一定）不能直接这样修改类属性，特别是对于某些有限定范围的属性。所以这种类就不具有封装性，真正意义的封装除了代码的集合，还有属性和方法的对外可见性。

比如下面的示例

```cpp
struct Person
{
    int age;
    std::string;
    
    void print()
    {
        std::cout << "age = " << this.age << std::endl
    }
};

//类使用
int main()
{
    Person A, B;
    A.age = -10;		//人的年龄不可能是负数，但是代码并没有错误
    A.age = 1000;		//人的年龄不可能这么大
    A.print();
    B.print();
    
    return 0;
}
```

对于某些不能是负数的属性我们虽然可以通过类型限定，但是像年龄这样的属性（值在一定的范围内）只用类型限定就不太现实了。这是我们引入访问控制，使类的使用者不能够直接给属性设置值（可以通过调用方法给属性设定值）。

在C++中，我们使用访问说明符实现类的可见性的封装：

- 定义在`public` 说明符之后的成员在整个程序可被访问（以类示例对象的方式访问），一般 `public` 成员定义类的接口。
- 定义在 `private` 说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码（通常是指类的实例对象）访问。

* 定义在 `protected` 说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码（通常是指类的实例对象）访问，在类的继承中还会继续讲到 `protected`

我们使用访问说明符重新定义 `Sales_data` 类

```cpp
class Sales_data {
public:
    Sales_data() {  }		
    Sales_data(const std::string &s) : bookNo(s) { }		
    Sales_data(const std::string &s, unsigned n, double p):
    			bookNo(s), units_sold(n), revenue(p*n) {} 
    Sales_data(std::istream &);	
    std::string isbn() const { return bookNo; }
  	Sales_data& combine(const Sales_data&);
    
private:
    dobule avg_price() const;
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
    
};
```

**使用 class 或 struct 关键字**，上面的类的定义使用了  , 这两者只是形式的变化，它们的唯一区别就是`class` 和 `struct` 的默认访问权限不一样。

> 说明：
>
> 1. 从访问说明符开始一直到下一个访问说明符，访问权限是不变的，例如`public` 后（直到下一个访问说明符）都是属于 `public` 的作用域
> 2. 在一个类中不同的访问说明符是没有顺序和数量的限制。
> 3. `struct` 和 `class` 基本是一样的，只是默认的访问权限不同，前者默认的是 `public` ,后者是 `private` . 如果手动的添加了访问说明符完全可以两者相互替换。



### 2.1 友元

除了 `Sales_data` 的成员函数，我们还定义了三个类外的成员，它们也是对该类的操作，所以一般会把它们放到同一个文件中。这三个函数需要操作类中的属性，但是属性已经是 `private` 成员，外部是无法访问的，这里我们可以使用 **友元**。将某个类或函数声明为另一个类的友元，那么它们就可以访问另一个类中的私有成员。

具体语法 `friend 类或函数`  

```cpp
class Sales_data
{
friend Sales_data add(const Sales_data&, const Sales_data&);
friend std::ostream &print(std::ostream&, const Sales_data&);
friend std::istream &read(std::istream&, Sales_data&);
    //后面的成员定义同上
};
```

说明

> 在类内声明了友元函数，不能看做函数的声明，这个只是声明友元，<span style="background: yellow">在类外还是需要声明函数的原型</span> 。
>
> 友元的声明位置没有限制可以在类内的任何位置，一般建议集中声明在类的开始或是结束。







## 3. 类的其他特性

### 3.1 类成员再探

示例： 一个 `Screen`类

```cpp
class Screen {
public:
    typedef std::string::size_type pos;		//1
    Screen() = default;		//2
	Screen(pos ht, pos wd, char c)
        : height(ht), width(wd), contents(ht * wd, c) {}
    char get() const 					//读取光标处的字符
    	{ return  contents[cursor]; }	//隐式内联
    inline char get(pos ht, pos wd) const;	//显示内联
    Screen &move(pos r, pos c); 			//能在之后被设为内联
    
private：
    pos cursor = 0;
    pos height = 0, width = 0;
    std:string contents;
};
```



> 说明
>
> 代码1使用别名将属性的类型抽象出来
>
> 代码2，因为定义了有参构造函数，编译器不会再提供默认的构造，而我们需要一个默认的构造，用这行代码告诉编译器提供一个默认的构造函数



####   成员函数和普通的函数一样也可以重载

重载的法则和普通函数重载一样



#### 可变数据成员

在前面讲过常函数，关于常函数还有一个特性：<span style="background: yellow">常函数内不可以修改成员属性</span>

```cpp
class Person {
public:
    int m_A;
    int m_B;
    
public:
    void func() const 
    {
        m_A = 100;		//错误 cosnt函数不可以修改属性的值
        std::cout << m_A << std::endl;	//正确，可以读取
    }
};
```

如果我们有需要在常函数内改变属性值的需求，可以使用 `mutable` 关键字

```cpp
class Person {
public:
    mutable int m_A = 42;    
public:
    void func() const 
    {
        m_A = 100;		//正确 m_A 有mutable修饰
        std::cout << m_A << std::endl;	//正确，可以读取
    }
};
```





### 3.2 返回 `*this` 的成员函数

关于类的 `this` 指针前面已经有过介绍

在Screen类中有一个 `set` 的方法，该方法是在当前光标位置插入一个字符，`move` 方法是移动光标

```cpp
class Screen {
public:
    typedef std::string::size_type pos;		
    // 其他无关代码
    Screen &set(char)
    {
        contents[cursor] = c;
        return *this;
    }
    Screen &set(pos r, pos col, char ch)		//重载，在指定位置插入一个字符
    {
        contents[r*width + col] = ch;
        return *this;
    }
    Screen &move(pos r, pos c)
    {
        pos = row = r * width;
        cursor = row + c;
        return *this;
    }
private：
    pos cursor = 0;
    pos height = 0, width = 0;
    std:string contents;  
};
```

定义一个实例对象`myScreen`，我们可以用下面的形式调用完成将光标移动指定位置后在设置该位置的字符值

```cpp
myScreen.move(4, 0).set('#');
```

这种方式叫做<span style="background: yellow">链式编程</span>，为什么可以连续的调用呢，我们回到函数的定义

这两个函数的返回值都是 `*this` ，先看前面部分 `myScreen.move(4, 0)` 这个调用返回的是 `*this` 对应到这里就是实例对象 `myScreen`, 既然是实例对象本身，那么我可以继续调用 `set('#')` , 两次调用都会作用到同一个实例对象上。

和上面链式调用等价的语句

```cpp
myScreen.move(4, 0);
myScreen.set('#');
```



 使用 `Screen` 而非引用 `Screen&` 类型接收返回的值会发生什么？



一个简单的说明示例

```cpp
#include <iostream>
using namespace std;

struct A
{
    int m_num = 0;
    
    A func1()
    {
        cout << "1.func1 num :" << m_num << endl;
        m_num = 1;
        cout << "2.func1 num :" << m_num << endl;
        return *this;
    }
    
    A func2()
    {
        cout << "1.func2 num :" << m_num << endl;
        m_num = 2;
        cout << "2.func2 num :" << m_num << endl;
        return *this;
    }
};

int main()
{
    A a;
    cout << "a.m_num = "  << a.m_num << endl; 
    a.func1().func2();
    cout << "a.m_num = "  << a.m_num << endl; 
}
```

运行结果

```shell
a.m_num = 0
1.func1 num :0
2.func1 num :1
1.func2 num :1
2.func2 num :2
a.m_num = 1
```

可以发现，上面的代码经过链式调用，预期的结果，最后输出的 m_num应该是2。这是因为func1 和 func2 返回的不是引用，那么相当于返回了两个不同的A对象。



**常函数返回 `*this` ** 

常函数返回 `*this` , （用引用接收）返回的是一个常量引用



### 3.3 类类型

每个了定义了一个唯一的类型，对于两个类来说，即使它们的成员完全一样，这两个类也是不同的类型。例如

```cpp
struct A {
  int memi;
    int getMem();
};

struct B {
  int memi;
  int getMem();
};
A a;
B b = a;	//错误 A，B是不同的类，不能赋值
```



#### 类声明

类和函数一样也可声明和定义分开，类的声明形式，这种也叫**前向声明**

```cpp
class Screen;		//Screen 类的声明
```

对于类型 Screen来说，在它声明之后定义之前是一个**不完全类型**，它的使用非常有限：可以定义指向这种类型的指针或引用，也可以声明（但不能定义）以不完全类型作为参数或者返回类型的函数。



### 3.4 友元再探

不仅函数可以作为一个类的友元，一个类也可以作为另一个类的友元

```cpp
class Screen {
  // Window_mgr作为友元，Window_mgr的成员可以访问Screen类的私有部分
  friend class Window_mgr;
  //...
};
```

令一个类中的某些函数作为另一个类的友元

```cpp
class Screen {
  //Window_mgr类的clear函数做友元
  friend void Window_mgr::clear(ScreenIndex); 
  //...
};
```



友元函数是可以定义在内部，但是还是需要声明友元函数

```cpp
struct X {
    friend void f() { /* 友元函数可以定义在类的内部 */ }
    X() { f(); }	//错误： f还没有被声明
    void g();
    void h();
};
void X::g() { return f(); }		//错误： f还没有被声明
void f();						//声明那个定义在X中的函数
void X::h() { return f(); }		//正确，现在f的声明在作用域中了
```



<span style="background: yellow">友元的声明不能替代函数的声明</span>



## 4. 类的作用域

关于类的作用域，我们在之前已经接触过了，就是当函数声明在类内，定义在类外的时候，定义需要加上类名表示该函数是该类的成员

```cpp
void Window_mgr::clear(ScreenIndex  i)
{
    Screen &s = screens[i];
    s.contents = string(s.height * s.width, '');
}
```



### 4.1 名字查找与类的作用域

类的定义分两步处理：

- 首先，编译成员的声明
- 直到类全部可见后才编译函数体。

> 编译器处理类中的全部声明后才会处理成员函数的定义。所以类中的成员变量可以定义在类内的任何位置



**用于类成员声明的名字查找**

```cpp
typedef double Money;
string bal;
class Account {
public:
    Money balance() { return bal; }
private:
    Money bal;
    //...
}
```

当编译器看到 `balance` 函数的声明时，首先会在 `Account` 内找 `Money`。此时只考虑Account内使用Money前出现的声明，很显然没有。于是就会在Account外层寻找，找到 `typedef double Money;` 。

另一方面 `balance` 函数体在整个类可见后才被处理，所以`return` 返回的时类内成员 `bal` 而并不是 string 对象





## 5. 构造函数再探

### 5.1 构造函数初始化列表

构造函数的作用通常是用来初始化成员属性的，成员属性的初始化也可以在定义的时候初始化，但是有时候初始值可能是需要类的使用者自定义，那么就只能在构造函数中初始化了。

```cpp
//构造函数初始化成员变量的一种写法
Sales_data::Sales_data(cont string &s, unsigned cnt, double price)
{
    bookNo = s;
    units_sold = cnt;
    revenue = cnt * price;
}
```

上面这段代码在之前是已经出现过的，正确来说这不是对成员的初始化，而是赋值操作。这样写虽然合法，但却不是最好的方式，列表初始化才是最优的方式，而且有些情况下我们必须使用列表初始化。比如下面的例子

```cpp
class ConstRef {
public:
    ConstRef(int ii);
private:
    int i;
    const int ci;
    int &ri;
};
```

假设`ConstRef` 类的成员需要类的使用者初始化（大多数情况下是这样的），很显然使用下面的赋值操作是错误的

```cpp
//错误： ci 和ri必须被初始化
ConstRef::ConstRef(int ii)
{
    i = ii;		//正确
    ci = ii;	//错误 不能给const赋值
    ri = i;		//错误 ri没有被初始化
}
```

正确的写法，列表初始化

```cpp
ConstRef::ConstRef(int ii) : i(ii), ci(ii), ri(i) {}
```



#### 成员初始化的顺序

```cpp
class X {
    int i;
    int j;
public:
    //为定义的：i在j之前被初始化
    X(int val) : j(val), i(j) {}
};
```

一般来说，初始化的顺序没什么特别要求，不过如果一个成员是用另一个成员来初始化的那么这两个成员的初始化顺序就很关键。

上面的例子从形式来看是先用 `val` 初始化 `j` ，在用 `j` 初始化 `i`。实际的初始化顺序和成员的定义顺序一样，是先初始化 `i` ,再初始化 `j`

> 最好令构造初始值的顺序域成员声明的顺序保持一致，而且如果可能的话，尽可能避免使用某些成员初始化其他成员

```cpp
X(int val): i(val), j(val) {}
```



#### 默认实参和构造函数

```cpp
class Sales_data {
public:
    Sales_data(std::string s = "") : bookNo(s) {}
    //...
};
```



> 如果一个构造函数为所有参数都提供了默认实参，则它实际上也定义了默认构造函数





### 5.2 委托构造函数

<span style="border:2px solid Red">C++11</span> 在一个类中，不同的构造函数中可能存在冗余的语句，这时我们可以使用**委托构造函数**。

一个委托构造函数使用它所属类的其他构造函数指向它自己的初始化过程，或者说它把自己的一些（或者全部）职责委托给其他构造函数

```cpp
class Sales_data {
public:
    //非委托构造函数使用对应的实参初始化成员
    Sales_data(std::string s, unsigned cnt, double price):
    	bookNo(s), units_sold(cnt), revenue(cnt*price) { }
    //其余构造函数全都委托给另一个构造函数
    Sales_data() : Sales_data("", 0, 0) { }
    Sales_data(std::string s) : Sales_data(s, 0, 0) { }
    Sales_data(std::istream &is) : Sales_data() { read(is, *this); }
};
```



### 5.3 默认构造函数的作用

如果我们自己定义了构造函数（无参构造和有参构造）编译器都不会提供一个默认的构造函数（无参构造函数），没有默认构造函数其实不会有任何的错误，当然前提是我们初始化对象时不能使用无参构造（默认构造）。例如下面的示例

```cpp
class NoDefault{	//只有有参构造的类
public:
    NoDefault(const std::string&);
    //....
};

int main()
{
	NoDefault a;	//错误，调用无参构造
    NoDefault b("hello");	//正确
}
```

上面的这个例子很明显的可以观察到会调用无参构造，而类没有提供无参构造出现错误，但是下面的这个示例可能就不那么明显了

```cpp
class NoDefault{	//只有有参构造的类
public:
    NoDefault(const std::string&);
    //....
}；
struct A {
    NoDefault my_mem;
};
A a;		//错误：不能为A合成构造函数

struct B {
    B() {}	//错误： b_member没有初始值
    NoDefault b_member;
};
```



> 在实际中，如果定义了其他构造函数，那么最好也提供一个默认构造函数

默认构造函数的调用

```cpp
Sales_data obj();	//错误
Sales_data obj2;		//正确
```

上面第一行代码没有语法错误，但是这不是调用默认构造函数创建一个实例对象，而是声明了一个`obj`函数，其返回值是`Sales_data` 类型



### 5.4 隐式的类类型转换

下面的代码用到了本章之前所定义的`Sales_data`的相关成员

```cpp
// 相关成员的原型
Sales_data& combine(const Sales_data&);		//combine
Sales_data(const std::string &s);			//构造函数1
Sales_data(std::istream &);					//构造函数2

//item 是一个 Sales_data的实例，combine是将两个Sales_data实例对象相加
string null_book = "9-999-99999-9";
item.combine(null_book);
```

上面的7，8两行代码是正确。`combine`定义的参数类型是 `const Sales_data`，但是我们传入一个 `string` 类型的也是可以的。这是发生了类类型的隐式转换，把传入的`string`隐式的转换为了 `Sales_data` ，这种转换是由**转换构造函数**实现的，所谓转换构造函数就是只定义了一个实参的构造函数，上面的构造函数1和2都是转换构造函数。所以执行`item.combine(null_book);` 会自动的调用`Sales_data(const std::string &s);	` 创建一个临时的`Sales_data`对象。

<span style="background: yellow">**只允许一步类类型转换**</span>

```cpp
//错误： 需要用户定义的两种转换：
//（1）把 "9-999-99999-9" 转换成string
// (2) 再把这个（临时的）string转换成Sales_data
item.combine("9-999-99999-9");
```

可以显示地把字符串转换成string或者Sales_data

```cpp
//正确： 显示地转换成string，隐式地转换成Sales_data
item.combine(string("9-999-99999-9"));
//正确：隐式地转换成string，显示地转换成Sales_data
item.combine(Sales_data("9-999-99999-9"));
```

说明：`"9-999-99999-9"` 字符串字面值并不是`string` 类型，它的类型应该是 `const char*`



#### 抑制构造函数定义的隐式转换

有时候我们并不希望发生类类型的隐式转换，我们可以通过将构造函数声明为 `explicit` 加以阻止：

```cpp
class Sales_data {
public:
    Sales_data() = default;
    Sales_data(const std::string &s, unsigned n, double p):
    			bookNo(s), units_sold(n), revenue(p*n) {}  
    explicit Sales_data(const std::string &s) : bookNo(s) { }		
    explicit Sales_data(std::istream &);
    //...
};
//此时下面的代码就是错误的，无法通过编译
item.combine(null_book);	//错误： string构造函数是explicit的 
item.combine(cin);			//错误： istream构造函数是explicit的
```

`explicit`使用注意

- `explicit` 只对一个实参的构造函数有效，（多个实参的构造函数也不能用于执行隐式转换）
- 只能在类内声明构造函数时使用`explicit`关键字，在类外定义时不能重复
- `explicit` 构造函数只能用于直接初始化，不能用于拷贝形式的初始化（使用 `=`）



#### 类类型的显示转换

```cpp
//正确：实参是一个显示构造的Sales_data对象
item.combine(Sales_data(null_bokk));
//正确： static_cast 可以使用explicit的构造函数
item.combine(static_cast<Sales_data>(cin));
```



##### 标准库中的应用

- 接受一个单参数的 `const char*` 的 `string` 构造函数不是 `explicit`

  ```cpp
  string s = "hello";  // hello 是 const char* ，需要调用非explicit 转换构造函数
  ```

- 接受一个容量参数的 `vector` 的构造函数是`explicit` 的

  ```cpp
  vector<int> vec(5);     // 显示的调用形参为一个的构造函数
  vector<int> vec = 5；   // 错误，对应的转换构造函数是 explicit， 不能隐式调用
  ```

  



### 5.5 聚合类

当一个类满足如下条件时，我们就说它是一个聚合类：

- 所有成员都是`public`的
- 没有定义任何构造函数
- 没有类内初始值
- 没有基类，也没有 `virtual`函数（虚函数，后面的章节会介绍）

例如

```cpp
struct Data {
  int ival;
  string s;
};
```

聚合类的初始化，使用`{}` ，值的顺序必须和声明的顺序一致

```cpp
//val1.ival = 0; val1.s = string("Anna")
Data val1 = {0, "Anna"};
```



### 5.6 字面值常量类

在第六章中提到过 `constexpr` 函数的参数和返回值必须式字面值类型，类也可以是字面值类型。和其他类不同，字面值类型的类可能含有 `constexpr` 函数成员。这样的成员必须符合 `constexpr` 函数的所有要求，它们是隐式`const` 的。

数据成员都是字面值类型的聚合类是字面值常量类。如果一个类不是聚合类，但它符合下述要求，则它也是一个字面值常量类：

- 数据成员都必须是字面值类型。
- 类必须至少含有一个 `constexpr` 构造函数
- 如果一个数据成员含有类内初始值，则内置类型成员的初始值必须是一条常量表达式；或者如果成员属于某种类类型，则初始值必须使用成员自己的 `constexpr` 构造函数。
- 类必须使用析构函数的默认定义，该成员负责销毁类的对象。



#### `constexpr` 构造函数

一个字面值常量类必须至少提供一个 `constexpr` 构造函数，通过前置关键字 `constexpr` 就可以声明一个 `constexpr` 构造函数了：

```cpp
class Debug {
public:
    constexpr Debug(bool b = true) : hw(b), io(b), other(b) {}
    constexpr Debug(bool h, bool i, bool o) :
                    hw(h), io(i), other(o) {}
    
    constexpr bool any() { return hw || io || other;}
    void set_io(bool b) {io = b;}
    void set_hw(bool b) {hw = b;}
    void set_other(bool b) {other = b;}
private:
    bool hw;
    bool io;
    bool other;

};
```

 `constexpr` 构造函数用于生成  `constexpr` 对象以及  `constexpr` 函数的参数或返回类型：

```cpp
constexpr Debug io_sub(false, true, false);			// 调试IO
if(io_sub.any) 			
    cerr << "print appropriate error messages" << endl;
constexpr Debug prod(false);						// 无调试
if(prod.any())
    cerr << "print an error messages" << endl;
```





## 6. 类的静态成员

类和实例的工作机制，我们创建一个类的实例，这个实例就会有这个类的所有成员属性和成员方法，而且同一个类的不同实例的成员之间是完全独立的。简单的理解就是类的实例会把类的成员拷贝一份给自己用。

而静态的成员（属性，方法）是只有一份的，就是所有的实例对象共享同一个成员。

**声明静态成员**，银行账户示例：

```cpp
class Account {
public:
    void calculate() { amount += amount * interestRate; }
    static double rate() { return interestRate; }
    static void rate(double);
private:
    std::string ower;
    double amount;
    static double interestRate;
    static double initRate();
};
```



#### 静态成员的使用

对于静态成员我们既可以使用类名调用，也可以使用实例对象调用，因为静态成员是所有实例共享一份，所以使用两种方法调用都是一样，建议使用类名调用。

```cpp
double r;
r = Account::rate();	//使用类名调用static void rate(double);
Account ac1;
r = ac1.rate();			//使用实例对象调用static void rate(double);
```



<span style="background: yellow">静态成员的注意：</span>

- 对于静态的成员属性，必须类内声明类外初始化（如果需要初始化）

- 成员函数可以定义在类内和类外，
- 成员（函数，属性）定义在类外时不需要 `static` 关键字
- 静态成员函数内不可以访问非静态成员属性











































