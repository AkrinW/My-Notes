# Cpp Note

Tags:  `cpp`

[TOC]

## 前言

关于Cpp，我已经不知道学了多少遍，却依然感觉没有入门。一方面，Cpp的内容确实太多了，很多语法细节，可以说绝大部分的Cpp程序员都不知道，也没有必要知道，要用到的时候再去用就可以了。另外，我感觉掌握语言还是得多去写些大型的项目，目前这方面差的实在太多了。

关于Cpp的语法，基础的也没有必要写了，主要记一些难用、少用、比较复杂的语法。

---

## 代码规范

讲语法前，必须先讲代码规范，写出好看的代码，不仅让自己看懂，也能让团队成员，同事看懂，更要让几个月后的自己还能看懂。

代码的规范主要关注点在结构、缩进、空格、命名、注释五个方面。

### 基础要求

* 每行长度不超过80个字符

    80个字符，其实就是编辑器正常一行能显示的，实际上远远不够，有些复杂的函数，光变量名就超过80个字符了。
    可以用`alt+z`实现单行的自动换行。
    观察Cpp的标准库，会发现他们写长代码时，会主动进行换行。实际上，Cpp对`space`，`\t`，`\n`是不敏感的，也就是说，在符合语法分割的区域，添加很多换行符或制表符，并不会影响编译，因此，你可以把代码写成这样：

    ```cpp
    int fun(std::vector
    <int>
    & 
    nums) 
    {
        // codes
    }
    ```

    这样写可以通过编译，但是很难看，因此也不必过度换行，保证可读性就可以了。
* 正常情况下避免使用非 ASCII 字符，必须使用 UTF-8 编码
    不同的编码方式会出现乱码，没有必要自找麻烦。
* 缩进为 2 个空格，避免使用制表符
    似乎是Linux上编程的规范？在Linux系统`tab`就是2个空格缩进。但是我在Windows的代码一直用制表符，这点的影响并不太大。

### 操作符

* 一元操作符与操作数之间无空格
* 二元操作符前后需空格
* 圆括号内无紧邻空格
* .->符号两侧无需空格
* &符号仅有一侧空格
    加空格很重要，在合适的地方加空格，在不合适的地方不要加空格，典型例子是`x --> 0`
    [What is the '-->' operator in C/C++?](https://stackoverflow.com/questions/1642028/what-is-the-operator-in-c-c)

### 分支循环语句

* `if`、圆括号、大括号之间应有空格
* 无 `else` 的简单单行语句可以写在 `if` 语句同一行
    不推荐这样写，依然会降低可读性。最好的选择是该加的地方都加上大括号。
* `if` 任一分支使用大括号时，所有分支均需使用大括号
* 空循环体不能直接省略，可用 `{}` 或 `continue;`

### 函数定义

* 右圆括号和左大括号间总是有一个空格
* 所有形参应尽可能对齐
* 参数换行应保持 4 个空格缩进
* 返回值是简单表达式不加括号，复杂表达式可以加括号

### 留白

* 原则上空行越少越好
* 禁止两行及以上空行

### 命名

命名最好避免缩写，拼音，单个随机字母。要能明确的表达这个命名对象的含义。

* 下划线命名法
    `motor_control`, `file_io`
    通常用于文件名
* 帕斯卡命名法（大驼峰）
    `YourName`, `SteinsGate`, `kNumMax`
    用于类型名、函数名、常量名(小写k开头)
* 驼峰命名法（小驼峰）
    `meaninglessNumber`, `reallyBigNumber`
    用于普通变量名
* 匈牙利命名法
    `m_lpszStudentID`, `g_lYear`

### 注释

写注释是为了让大家都能看懂。没有人能轻松地读别人的代码。

#### 文件

* 每一个文件开头均应有许可证说明
* 文件开头应有对本文件功能的简要说明

#### 类与函数

* 每个类的定义都需附带一份详细的说明注释
* 在函数声明处注释描述函数功能、接口、默认值、安全性等
* 在函数定义处描述具体实现的方式或理由
* 类的成员变量以及全局变量应有注释说明用途

#### 开发过程中

* 暂未完成或不够完美的代码可以使用 `TODO` 注释

#### 注释使用方式

* 在 `//` 和 `/* */` 选择上应保持统一
* 使用 `//` 更多
* 单行注释应在行末空两格进行注释
* 多行连续注释应对齐以确保可读性
* 类(函数)的注释都应以类(函数)作为主语(可省略)

---

## 位运算

直接从位运算开始？因为基础的运算已经没必要讲了。
位运算是直接对整数的二进制位进行计算操作。位运算的计算速度和加法相同，比乘法更快，功耗会相对更低。
学位运算，需要关注的是数学上的技巧。

* 按位与&
    `1100&0011 = 0000;`
* 按位或|
    `1100|0011 = 1111;`
* 按位取反~
    `~0011 = 1100;`

> 位掩码Bit Mask:
    是位和掩码的组合词。位指代着二进制数据当中的二进制位，而掩码指的是一串用于与目标数据进行按位操作的二进制数字。组合起来，就是用一串二进制数字（掩码）去操作另一串二进制数字的意思。
    比如你需要1个全是1的位掩码，~0就比一串1要简洁很多（当然你可以用0xFF…）

* 按位异或^
    `0101^0011 = 0110;`
    异或的计算技巧比较多
    `a^b^b = a;`
    `n^(n-1)//找到二进制位中最小的非0位`
    `n&(n-1)//每次执行，二进位中少一个1`
* 移位>>= <<=
    左移或者右移n位，空出来的数字会用0补
    `0001 <<= 2; //0100`
    `1000 >>= 1; //0100`

## 写Cpp时遇到的错误

各种语法错误层出不穷，在此做总结分析

### 模板类编译不通过

Cpp模板的声明和实现需要放在同一个文件里。不能在`.h`声明，然后在`.cpp`实现，编译将会无法通过

### 堆栈溢出Memory Limit Exceed

栈主要存储函数，堆存储的是动态变量。溢出说明函数迭代次数过多，或者存储变量超过内存上限了，需要优化写法。
把函数迭代改成`while`+`stack`的写法。

### 超时Time Limit Exceed

写的算法时间复杂度太高了，需要改变思路。

### 数组越界Addition of unsigned offset

报错多种多样，归根结底的原因就是数组越界。

### 类型转换错误

需要进行强制类型转换，不然c++会用最小的类型方式存储。
还有有符号数和无符号数值域的差异需要考虑。
`INT_MAX=2147483647`,`INT_MIN=-2147483648`
在临界的数需要特别注意处理。注意`-INT_MIN`是溢出int上限的。

### 指针问题

`delete p`删除的是p指向地址的内容，p依然会指向这个地址，为了避免野指针，需要把p指向`nullptr`

### 函数传参问题

需要传入引用`&`才能对参数进行更改。如果传入的是指针，则指向的地址内容会被无&函数更改。但是如果想要改变指针本身指向的地址，需要传入`*&`。

## Cpp高级语法

在高版本添加的新语法功能，有些可以帮助写出更漂亮的代码。有些利用范围有限。

### auto

可以用`auto`自动判断变量类型，可以用于初始化，函数返回值，与for循环。在新的for循环，传入引用可以改变其本身的值。

```cpp
std::vector<int> vec;
auto p = vec.begin();
for (auto v: vec) {
    std::cout << v << ' ';
}
std::vector<double> price;
for (double &x : price) {
    x *= 0.8;
}
```

### 初始化优化

可以直接用`{}`对数组，动态数组，vector等进行优化，写法上有了很大改进。

```cpp
std::vector<int> vec{1,2,3,4,5};
int *p = new int[4]{0};
```

### lambda表达式

可以在函数内写匿名函数了，方便函数内进行调用。这个方法的好处在于比如一个函数内的临时变量需要用函数赋值，但是函数写在外面，没有办法找到这个临时变量，就必须要把这个变量作为参数传入。用lambda表达式就避免了这一过程，可以直接在函数内写实现，函数内调用。
需要注意，如果使用auto，lambda表达式就不能递归。因为递归发生时，编译器还未能得知表达式的返回值，所以要使用std::function。

```cpp
void fun() {
    // version1
    auto add = [&](int x, int y) {
        return x+y;
    };
    // version2
    std::function<void(int, int)> dfs = [&](int u, int n) -> void {
    if (u > n) return;
    cout << u << ' ';
    dfs(u + 1, n);
    };
    dfs(1,6);
    // version3
    std::sort(a, a+n, [](const auto& a,const auto& b){return a>b;});
}
```

### 智能指针

对于普通的指针，需要进行delete操作，但是调用智能指针，可以省去这一步骤，由标准库自动delete处理。

```cpp
std::unique_ptr<int> prt(new int(42));
```

### 类型别名

方便写代码，也会方便读代码理解。

```cpp
using Vec= std::vector<int>;
typedef char byte;
typedef char* byte_pointer; // better than #define
```

### 右值引用

`&&`可以绑定一个在`=`右边的临时变量的引用，主要用于实现移动语义（深拷贝）和完美转发（保留左右值）。

```cpp
int &&a = 5;
// 移动语义
Array(Array&& temp_array) {
    data_ = temp_array.data_;
    size_ = temp_array.size_;
}
template <class T>
void swap(T & a, T & b) {
    T temp = std::move(a); 
    a = std::move(b);
    b = std::move(temp);
}
// 完美转发
string s1, s2, s3, s;
s = std::move(s1);              // string&&
s = std::forward<string>(s2);   // string&&
s = std::forward<string&>(s3);  // string&
```

### 委托构造函数Delegating constructors

在类构造函数的多个重载中，由一个调用另一个构造函数。

```cpp
classMyClass {
public:
    MyClass(int x): MyClass(x, 0) {}
    MyClass(int x,int y): x_(x), y_(y) {}
private:int x_, y_;
};
```

---

## Cpp的冷门语法

有些写法看起来很怪也不常见，实际上能编译通过，记下来以便以后读代码更容易理解。

### 基于范围插入Range-based insert

对于vector数组的插入新写法，使用迭代器确定位置。可能还是有问题，因为不知道在迭代器的左边还是右边进行插入。

```cpp
std::vector<int> vec;
vec.insert(vec.end(), {1, 2, 3});
```

### Union

一种结构体，但是一个变量只能存储结构体中的一个数据，可以放进其他结构体里当匿名对象，在初始化时确定好要存储的类型。

```cpp
union a {
    int a_num;
    std::string a_string;
};
// anonymous union
struct A {
    bool flag;
    union {
        int num;
        std::string str;
    };
};
```

### 枚举类型

其实是创建了一系列常量，与整型数相对应，数组中的值类型默认是前一个数+1。

```cpp
enum {
    red,orange=0,yellow,green,blue=5,violet
};
//red = 0, violet = 6;相当于对6个const int初始化。
```

### 判断字符类型

写一些字符串算法题时比较有用。

```cpp
#include <cctype>
int flag = isdigit(object);
```

### const指针

根据`const`和`*`的位置，表达含义会不一样。

```cpp
const int * ptr;//a pointer to a (const int) type.
int * const ptr;//a const pointer to an int;
```

### 指向函数的指针

一般用来传参，正常要指定类型和返回值，还是挺麻烦的。

```cpp
double func(int x){return x;}
double (*pf)(int);//函数指针,参数为int,返回double
pf = &func;
auto pf = func;//自动推断成函数指针
```

### 模板函数具体化

对某个特定的模板函数，给出具体实例。个人感觉违背了模板的设计理念。

```cpp
template <typename T>
void func(T value){};
template <>
void func(int value){};
```

### decltype

帮助确定变量类型，用在模板函数，不确定返回类型时使用。

```cpp
template <typename T1, typename T2>
void func(T1 x, T2 y) {
    decltype (x+y) xpy = x + y;
}
template <typename T1, typename T2>
auto func(T1 x, T2 y) -> decltype(x+y) {
    return x + y;
}
```

### volatile

一个限定符，提示即使程序不对变量修改，其值也有可能改变。一般是硬件的端口。

### mutable

限定符，提示const常量的成员可以被修改。

### this

`this`与`*this`，`this`表示地址，而`*this`表示对象本身。

### protected

派生类可以访问，但是类外的普通函数不能使用。

### 纯虚函数

一个基类有纯虚函数`virtual void fun() = 0`，就不能对其进行初始化，只能派生。

---

未完待续，Cpp的语法写不完的，遇到新的需要继续补充。
