# GDB调试

## 1. 启动调试

### 传递参数
- `gdb --args <exe>`: 启动调试，`gdb --args a.out  1 2 "3 4"` 启动调试a.out 传入参数1，2, ‘3 4’ ， 参数有空格用引号
- `set args <args>` : 启动后，在执行前设置参数
- `r <args>` : 运行时给出参数

### 附加到进程

程序已经启动了，使用进程的id
- `gdb attach [pid]`: 调试运行的程序(pid)
- `gdb --pid [pid]`: 同上

使用 `detach` 退出，进程不会终止

### 逐过程执行
- `next` / `n` : 不会进入函数内部
- `next instruction` -> `ni` : 执行一条汇编指令，用于调试内核汇编代码

### 逐语句
- `step` -> `s` : 单步执行，遇到函数会进入函数
- `step instruction` -> `si` :执行一条汇编指令，会进入调用内部，用于调试内核汇编代码
- `si N`: 一次执行 N 条汇编指令

### 退出当前函数
- `finish` : 退出当前函数，回到函数调用前的下一条语句

### 退出调试
- `detach` : 分离，程序不会终止
- `quit` --> `q` : 退出gdb并终止运行

## 2. 断点

### 设置断点
- `break/b file:line`: 在指定文件指定行设置断点，`b hello.c:13`, 如果空行，会设置到下一行有代码处
- `b function` : 在指定函数设置断点， `b main`在函数`main`第一行代码设置断点，如果多个函数名称一样，则所有同名的都会有断点
- `rb reg` : 使用正则表达式设置断点，所有符合表示的函数都会被设置断点
- `b bpoint condition` : 设置条件断点， `b main.cpp:13 if i==90`
- `tb bpoint` : 设置临时断点，只命中一次
- `b addr`: 设置在内存地址addr处打断点，`b *0x7c00`


### 查看断点
- `i b` --> `info break` ： 查看所有断点
- `disable/enable [bp]` : 禁用，启用断点
- `delete [bp]` : 删除断点


## 3. 查看/修改变量

### 查看变量
- `info args` : 查看函数参数
- `print/p var`: 查看变量var的值
- `set print null-stop`: 设置字符串显示的规则，不显示`\0`
- `set print pretty`: 设置结构体显示格式，一行一个属性
- `set print array on`: 设置数组显示格式，一行一个
- 使用gdb内嵌函数, 比如 `sizeof`、`strlen`


### 修改变量
- `p var=42`: 设置变量var的值为42

## 4. 查看/修改内存

### 查看内存

- `x /fmt addr` : fmt 查看格式， addr地址
  - `x /s str`
  - `x /d`
  - `x /4d`
  - `x /16s`

- `x /nfu addr`: n显示的个数，f显示格式，u显示粒度，[详细说明](https://sourceware.org/gdb/onlinedocs/gdb/Memory.html)



`help x` 的说明
```bash
x/FMT ADDRESS.
ADDRESS is an expression for the memory address to examine.
FMT is a repeat count followed by a format letter and a size letter.
Format letters are o(octal), x(hex), d(decimal), u(unsigned decimal),
  t(binary), f(float), a(address), i(instruction), c(char), s(string)
  and z(hex, zero padded on the left).
Size letters are b(byte), h(halfword), w(word), g(giant, 8 bytes).
The specified number of objects of the specified size are printed
according to the format.  If a negative number is specified, memory is
examined backward from the address.

Defaults for format and size letters are those previously used.
Default count is 1.  Default address is following last thing printed
with this command or "print".
```

### 修改内存
- `set addr=var` : 设置内存addr处的值为var


## 5. 查看/修改寄存器

### 查看寄存器

- `i/info r/register` : 查看所有通用寄存器的值
- `i all-registers`: 查看所有寄存器的值
- `i r reg`: 查看指定寄存器的值， `i r eax`

### 修改寄存器
- `set var $pc=xxx`: 修改`pc`寄存器的值
- `p $rip=xxx`: 同上


## 6. 源代码查看/管理

- `list/l` ： 显示源代码，默认显示10行，当前运行代码的前后5行
- `set listzie xx`: 设置显示的行数
- `list function`: 查看指定函数的代码
- `list main.cpp:15`: 查看指定文件指定行代码

### 搜索源代码
- `search regx`: 使用正则表达式搜索
- `forward-search regx`: 使用正则表达式搜索，向前
- `reverse-search regx`: 使用正则表达式搜索，向后

- `dirctory path`: 设置源代码搜索路径



## 7. 函数调用栈管理

- `backtrace/bt` : 查看栈回溯信息
- `frame n`: 切换栈帧
- `inf f n` : 查看栈帧信息

