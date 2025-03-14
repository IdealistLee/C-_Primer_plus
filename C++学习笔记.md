# C-_Primer_plus

C++学习笔记

# 第四章 复合类型

常见的复合类型：数组，字符串，结构，共用体，枚举

## 数组

储存多个相同类型的值。

`````c++
// typeName arrayName[arraySize]
int int_name[10];
int int_name1[10] = {1,2,3,4,5,6,7,8,9,10}; // =  is optional
std::cout << name1[1];
`````

## 字符串

一个方便好用的字符数组，智能识别和使用

```c++
#include <cstring> //要引用头文件string
sting name;
string str1;
stirng str2 = "This is a example.";
string str3;
str3 = str2;
str1 =str2 + str3;
```



## 结构体与共用体

**结构体（struct）：**
每个成员都有自己独立的内存空间，结构体的总大小是所有成员大小之和（考虑对齐填充）。这意味着你可以同时访问所有成员，并且它们各自保存独立的数据。

**共用体（union）：**
共用体的所有成员共用同一块内存，内存大小等于其中最大成员的大小。一次只能正确存储和使用其中一个成员，因为写入一个成员会覆盖其他成员的值。

---

## 指针

指针指向的是一个地址，对指针解除引用就获得了指针指向的值。**格式**：*typeName \* pointerName;*

`int* p = &a;` p是指针，&a是指针指向的地址，a和*p都代表地址上储存的值。

### 声明指针

```c++
int * p; //声明了一个指向int类型的指针，空格是可选的
int *p; //传统C语言写法，强调*p是是int类型的值
int* p; //C++写法，强调int* 是一种类型，指向int的指针。
int a = 5;
int* p = &a; //声明一个指向int类型的指针，并初始化指针。
double* p_d;//声明一个指double类型的指针
char* p_ch;//声明一个指向char类型的指针
```

> **Note**
>
> *在对指针进行解除引用之前(\*p),一定要将**指针初始化**为一个确定的，合适的地址。*
>
> ```c++
> int* p;
> p = &a;// 重要，不然就是野指针
> cout << *p;
> ```



### new和delete分配内存

`new`和`delete`配合可以进行动态内存分配。

```c++
int* p = new int;   // 分配内存
*p = 20;            // 赋值
delete p;           // 释放内存
p = nullptr;        // 避免悬空指针
```

- **内存泄漏**：忘记 `delete` 会导致内存无法回收。
- **重复释放**：多次 `delete` 同一指针会崩溃

> 使用new和delete的规则：
>
> - 不要使用delete释放不是new分配的内存
> - 不要使用delete释放同一内存两次
> - 如果使用new []为数组分配内存，应使用delete[]来释放
> - 如果使用new为实体分配内存，应使用delete(没有方括号)释放内存
> - 对空指针使用delete是安全的

### 指针、数组和指针算术

数组名就是指针名。

|   **特性**   |         **指针**          |           **数组名**           |
| :----------: | :-----------------------: | :----------------------------: |
|   **本质**   |    变量，存储内存地址     |   符号，代表数组首元素的地址   |
| **内存分配** |   可指向动态或静态内存    |    静态分配（如栈或全局区）    |
|   **类型**   | 单独变量类型（如 `int*`） | 隐式指针类型（如 `int(*)[5]`） |
| **可修改性** |    可重新指向其他地址     |     不可修改（是常量指针）     |

- **核心区别：**数组名是常量符号，携带类型和大小信息；指针是变量，仅存储地址。

## 类型组合

数组，结构，指针之间可以随意组合。

```c++
struct year_m  //定义一个结构
{
    int year;
    int month;
};
year_m s01,s02,s03; //s01,s02,s03都是结构体
s01.year = 1997;
year_m* p =&s02;//指向结构的指针
p->year =1998;
year_m ym[3];//结构数组
ym[0].year = 2025;// ym是一个数组，ym[0]是一个结构，所以ym[0].year是结构成员
(ym+1)->year = 2026; //数组名代表的是地址，也就是一个指针，所以应该使用间接成员运算符，相当于ym[1].year =2026;
const year_m* arp[3] {&s01,&s02,&s03};//指针数组  arp是指针数组，所以arp[1]就是一个指针,指向s02
std::cout << arp[1]->year << std::endl;//输出的是结构体s02的成员year
const year_m** ppa = arp;//指向上述指针数组的指针
auto ppb =arp;// C++自动推断类型
std::cout << (*ppa)->year << std::endl;//ppa是一个指向结构指针的指针，所以*ppa是一个结构指针，所以应该用间接访问符->访问
std::cout << (*(ppb+1))->year << std::endl;//ppb+1指向arp[1]，也就是&s02
```

## 数组、vector和array

**vector：**

```c++
#include <vector>
using namespace std;
vector<double> vt(10); //vector<typeName> arrayName(n_elem);(n is optional)
vector<int> vi;
```

**array：**

```c++
#include <array>
#include <array>
using namespace std;
array<double,10> arr; //varray<typeName,n_elem> arrayName;
array<int,3> arr1 {5,6,8};
```

**核心区别：**

|    **特性**    |            **原生数组**             |          **`std::vector`**           |           **`std::array`**           |
| :------------: | :---------------------------------: | :----------------------------------: | :----------------------------------: |
|  **内存分配**  |     栈或静态存储区（固定大小）      |       堆（动态分配，自动扩容）       |      栈或静态存储区（固定大小）      |
| **大小可变性** |             ❌ 固定大小              |              ✅ 动态扩容              |              ❌ 固定大小              |
|  **边界检查**  |    ❌ 无（越界访问是未定义行为）     |        ✅ 通过 `at()` 提供检查        |        ✅ 通过 `at()` 提供检查        |
|  **内存管理**  | 手动管理（若为堆数组需 `delete[]`） |        自动管理（RAII 机制）         |    自动管理（栈内存无需手动释放）    |
|    **性能**    |       ⚡ 最高效（无额外开销）        |     ⚠️ 动态扩容可能触发内存重分配     | ⚡ 接近原生数组（固定大小无扩容开销） |
| **接口丰富性** |            ❌ 无成员方法             | ✅ 丰富（`push_back()`, `size()` 等） |  ✅ 支持迭代器、`size()` 等 STL 接口  |
|  **传递方式**  |     退化为指针（丢失大小信息）      |           可传递副本或引用           |   可传递副本或引用（保留大小信息）   |

# 第五章 循环和关系表达式

## for 循环

![for循环](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/for%E5%BE%AA%E7%8E%AF.jpg "for循环")

```c++
for (i=1;i<5;i++) 
    cout << i;  
for (initialization; test_experssion; update_experssion)
	body
```

for 和括号之间一般加一个空格，函数之间省略空格 ，视觉上强化函数调用和控制语句的区别。

```c++
for (···)；
cin.getline(···)；
```

> **NOTE**
>
> a++和++a，a++是先使用a的表达式，之后将a的值+1；++a是先将a的值+1
>
> ```c++
> int x = 5;
> int y = ++x; // change x,then assign to y
> 			 // y is 6, x is 6
> int z = 5;
> int y = Z++; // assign to y,then change z
> 			 // y is 5, z is 6
> ```

## while循环

![while循环](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/while%E5%BE%AA%E7%8E%AF.jpg "while循环")

> **NOTE：**设计循环时的原则
>
> - 指定循环终止的条件
> - 首次循环开始之前初始化条件
> - 条件再次被测试之前更新条件

## do while循环

![do while循环](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/do%20while%E5%BE%AA%E7%8E%AF.jpg "do while循环")

- for循环和while循环是入口条件（entry condition），do while循环是出口条件（exit condition）。

## 嵌套循环和二维数组

二维数组有数据行和数据列

- **二维数组的定义：**

```c++
int temps[4][5];  // row = 4 ,col =5
```

- **二维数组的赋值：**

```c++
int temps[4][5] =
{
    {1,2,3,4,5};
    {21,22,23,24,25};
    {31,32,33,34,35};
    {41,42,43,44,45}
};
```

- **for循环打印内容：**

```c++
for (int row = 0;row < 4;++row)
{
	for (int col = 0;col < 5;++col)
		cout << temp[row][col]<< "\t";
	cout endl;
}
```



# 第六章 分支语句和逻辑运算符

## if语句

### if

![if语句结构](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/if.jpg "if结构")

```C++
if (test-condition)
	statement
```

### if else

![if else语句结构](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/if_else.jpg "if else结构")

```C++
	if (test-condition)
        statement1
    else
        statement2          
```

### if else if else

if else if else结构实际上一个if else被包含在另一个if else里面。

```c++
if (ch == 'A')
    a++;
else if (ch == 'B')
    b++;
else 
    other++;
```

## 逻辑表达式

逻辑运算符有逻辑OR(`||`)、逻辑AND(`&&`)、逻辑NOT(`!`)。

### 逻辑OR(`||`)

- OR表示只要有一个为真，结果就为真。

- **`||`在C++中是一个顺序点，先修改左侧的值，再对右侧的值进行判定。如果左侧表达式为真，则不会对右侧进行判断。**

### 逻辑AND(`&&`)

- AND表示两个都为真，结果才为真。
- **`&&`也是一个顺序点，即首先判定左侧，右侧被判定之前产生所有的副作用。如果左侧为false，则不会判定右侧。**

### 逻辑NOT(`!`)

- **`!`** 对后面的表达式取反 

- **`!`运算符优先级高于所有关系运算符和算术运算符。**

> **NOTE**
>
> 逻辑AND的优先级高于逻辑OR
>
> 使用括号将测试条件进行分组，程序更加易读，也不容易出错
>
> 逻辑符号也可以用`and or not`表示，C语言中需要包含头文件iso646.h，C++不需要头文件

## `?:`运算符

通用格式：

```C++
expression1 ? expression2 : expression3
```

如果expression1的表达式为真，则表达式的值为expression2的值，否则为expression3的值。

```C++
5 > 3 ? 10 : 12 ; // 5>3 is true, so expression value is 10
3 == 9 ? 25 : 18; // 3==9 is false, so expression value is 18
```

## `switch`语句

通用格式

```C++
switch (integer-experssion)
{
    case label1 : statement(s)
    case label2 : statement(s)
            ···
    default : statement(s)
}
```

![switch语句结构](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/switch.jpg "switch语句结构")

> switch只能处理单独的值，无法处理浮点值。case标签必须是常量。

## `break`和`continue`语句

`break`跳过循环，`continue`跳过循环体中余下代码，并开始新一轮循环。

![break和continue](https://github.com/IdealistLee/C-_Primer_plus/blob/main/img/break%20continue.jpg "break continue结构")

## 简单文件输入和输出

### 写入文本文件

> **写入到文本文件的主要步骤：**
>
> 1. 包含头文件fstream
> 2. 创建一个ofstream对象
> 3. 将该ofstream对象同一个文件关联起来
> 4. 像使用cout一样使用ofstream对象

```C++
ofstream outFile; //create aobject for output
outFile.open("carinfo.txt"); // associate with a file 
cout << "Hello";
outFile << "Hello too";
cout.precision(2);		//use a precision of 2 for the display
outFile.precision(4);	//use a precision of 4 for file output
···
outFile.close();
```

> **NOTE:**
>
> ***打开已有文件接受输出时，默认会清空文件原来的内容。***

### 读取文本文件

> **读取文本文件的主要步骤：**
>
> 1. 包含头文件fstream
> 2. 创建一个ifstream对象
> 3. 将该ifstream对象同一个文件关联起来
> 4. 像使用cin一样使用ofstream对象

```C++
ifstream inFile; //inFile an ifstream object
inFile.open("bowling.txt"); // associate with a file 
double wt;
inFile >> wt;  //read a number from bowling.txt
···
inFile.close();
```

**检查文件是否被成功打开**

```C++
inFile.open("bowling.txt");
if (!inFile.is_open())
{
    exit(EXIT_FALLURE)  // <cstdlib>
}
```

# 第7章 函数

## 函数基础知识

**创建函数基础流程：**

- 提供函数定义
- 提供函数原型
- 调用函数

### 定义函数

没有返回值的函数和有返回值的函数。

没有返回值的函数，也叫**void函数**：

```c++
void functionName(parameterList)
{
    statement(s)
    return;  // optional
}
```

有返回值的函数，会生成一个值，返回给调用函数：

```c++
typeName functionName(parameterList)
{
    statement(s)
    return value; // value is type cast to type typeName
}
```

>  **NOTE：**
>
> - 有返回值的函数，必须使用返回语句
> - 返回值可以是常量、变量、表达式
> - 返回值类型必须为typeName类型或者可以被转换为typeName
> - C++不可以返回数组，其他任何类型都可以(整数、浮点数、指针、结构和对象等)

### 函数原型和函数调用

- 函数原型描述了函数到编译器的接口
- 原型的功能
  - 编译器正确处理函数返回值
  - 编译器检查使用的参数数目是否正确
  - 编译器检查使用的参数类型是否正确。如果不正确，则转换为正确的类型（如果可能）。

## 函数参数和按值传递

C++按值传递参数。用于接收传递值得变量被称为形参，传递给函数得值为实参。C++标准用参数(argument)表示实参，用参量(parameter)表示形参。参数传递将参数赋给参量。

- 函数中声明的变量为函数私有，是局部变量，也是自动变量。

### 多个参数

函数可以有多个参数，调用函数时，用逗号将参数分开即可。

- 函数的两个参数类型相同时，必须分别指定每个参数的类型。

```c++
double melon_density(double weight,double volume);
```

 
