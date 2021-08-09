# C++ 学习笔记

## C/C++ 中的 volatile 关键字

### 为什么要用volatile？

C/C++ 中的 volatile 关键字和 const 对应，用来修饰变量，通常用于建立语言级别的 memory barrier。这是 BS 在 "The C++ Programming Language" 对 volatile 修饰词的说明：

> *A volatile specifier is a hint to a compiler that an object may change its value in ways not specified by the language so that aggressive optimizations must be avoided.*

volatile 关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素更改，比如：操作系统、硬件或者其它线程等。遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问。声明时语法：**int volatile vInt;** 当要求使用 volatile 声明的变量的值的时候，系统总是重新从它所在的内存读取数据，即使它前面的指令刚刚从该处读取过数据。而且读取的数据立刻被保存。例如：

```c++
volatile int i=10;
int a = i;
...
// 其他代码，并未明确告诉编译器，对 i 进行过操作
int b = i;
```

volatile 指出 i 是随时可能发生变化的，每次使用它的时候必须从 i的地址中读取，因而编译器生成的汇编代码会重新从i的地址读取数据放在 b 中。而优化做法是，由于编译器发现两次从 i读数据的代码之间的代码没有对 i 进行过操作，它会自动把上次读的数据放在 b 中。而不是重新从 i 里面读。

```c++
#include <stdio.h>
 
void main()
{
    int i = 10;
    int a = i;
 
    printf("i = %d", a);
 
    // 下面汇编语句的作用就是改变内存中 i 的值
    // 但是又不让编译器知道
    __asm {
        mov dword ptr [ebp-4], 20h
    }
 
    int b = i;
    printf("i = %d", b);
}
```

然后，在 Debug 版本模式运行程序，输出结果如下：

```c++
i = 10
i = 32
```

然后，在 Release 版本模式运行程序，输出结果如下：

```c++
i = 10
i = 10
```

输出的结果明显表明，Release 模式下，编译器对代码进行了优化，第二次没有输出正确的 i 值。

把i的声明加上volatile后，分别在Debug和Release版本运行，输出都是：

```c++
i = 10
i = 32
```

说明volatile关键字发挥了作用。

### 拓展：C++ 中的类型限定符

类型限定符提供了变量的额外信息。

| 限定符   | 含义                                                         |
| :------- | :----------------------------------------------------------- |
| const    | **const** 类型的对象在程序执行期间不能被修改改变。           |
| volatile | 修饰符 **volatile** 告诉编译器，变量的值可能以程序未明确指定的方式被改变。 |
| restrict | 由 **restrict** 修饰的指针是唯一一种访问它所指向的对象的方式。只有 C99 增加了新的类型限定符 restrict。 |

## C++ 日期&时间

### 结构类型tm

有四个与时间相关的类型：**clock_t、time_t、size_t** 和 **tm**。类型 clock_t、size_t 和 time_t 能够把系统时间和日期表示为某种整数。

结构类型 **tm** 把日期和时间以 C 结构的形式保存，tm 结构的定义如下：

```c++
struct tm {
  int tm_sec;   // 秒，正常范围从 0 到 59，但允许至 61
  int tm_min;   // 分，范围从 0 到 59
  int tm_hour;  // 小时，范围从 0 到 23
  int tm_mday;  // 一月中的第几天，范围从 1 到 31
  int tm_mon;   // 月，范围从 0 到 11
  int tm_year;  // 自 1900 年起的年数
  int tm_wday;  // 一周中的第几天，范围从 0 到 6，从星期日算起
  int tm_yday;  // 一年中的第几天，范围从 0 到 365，从 1 月 1 日算起
  int tm_isdst; // 夏令时
}
```

### C/C++ 时间函数

下面是 C/C++ 中关于日期和时间的重要函数。

| 序号 | 函数 & 描述                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | [**time_t time(time_t \*time);**](https://www.w3cschool.cn/c/c-function-time.html) 该函数返回系统的当前日历时间，自 1970 年 1 月 1 日以来经过的秒数。如果系统没有时间，则返回 .1。 |
| 2    | [**char \*ctime(const time_t \*time);**](https://www.w3cschool.cn/c/c-function-ctime.html) 该返回一个表示当地时间的字符串指针，字符串形式 *day month year hours:minutes:seconds year\n*。 |
| 3    | [**struct tm \*localtime(const time_t \*time);**](https://www.w3cschool.cn/c/c-function-localtime.html) 该函数返回一个指向表示本地时间的 **tm** 结构的指针。 |
| 4    | [**clock_t clock(void);**](https://www.w3cschool.cn/c/c-function-clock.html) 该函数返回程序执行起（一般为程序的开头），处理器时钟所使用的时间。如果时间不可用，则返回 .1。 |
| 5    | [**char \* asctime ( const struct tm \* time );**](https://www.w3cschool.cn/c/c-function-asctime.html) 该函数返回一个指向字符串的指针，字符串包含了 time 所指向结构中存储的信息，返回形式为：day month date hours:minutes:seconds year\n\0。 |
| 6    | [**struct tm \*gmtime(const time_t \*time);**](https://www.w3cschool.cn/c/c-function-gmtime.html) 该函数返回一个指向 time 的指针，time 为 tm 结构，用协调世界时（UTC）也被称为格林尼治标准时间（GMT）表示。 |
| 7    | [**time_t mktime(struct tm \*time);**](https://www.w3cschool.cn/c/c-function-mktime.html) 该函数返回日历时间，相当于 time 所指向结构中存储的时间。 |
| 8    | [**double difftime ( time_t time2, time_t time1 );**](https://www.w3cschool.cn/c/c-function-difftime.html) 该函数返回 time1 和 time2 之间相差的秒数。 |
| 9    | [**size_t strftime();**](https://www.w3cschool.cn/c/c-function-strftime.html) 该函数可用于格式化日期和时间为指定的格式。 |

## 拷贝构造函数

**拷贝构造函数**是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。拷贝构造函数通常用于：

- 通过使用另一个同类型的对象来初始化新创建的对象。
- 复制对象把它作为参数传递给函数。
- 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会自行定义一个。如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。拷贝构造函数的最常见形式如下：

```c++
classname (const classname &obj) {
   // 构造函数的主体
}
```

例如

```c++
Line::Line(const Line &obj)
{
    cout << "Copy constructor allocating ptr." << endl;
    ptr = new int;
   *ptr = *obj.ptr; // copy the value
}
```

## 重载与多态

重载与多态与重写的区别：

### 重载 

*函数名相同，但是函数的参数不同，调用时根据参数的不同决定调用哪一个函数。*

### 多态 

*函数名相同，函数形参相同。调用时根据函数类型时虚函数还是普通函数决定调用哪一个。*

### 重写 

*若子类和父类的某个函数具有相同的函数名，相同的形参列表，且父类中的函数被定义为虚函数，则子类对该函数的实现被称为函数的重写。*
caution！

若函数不声明为虚函数，只能通过类名限定名的方式访问父类或者子类的方法。这叫做隐藏。

若函数声明为虚函数，则可以通过指针访问每一个方法，这叫做覆盖。

## C++ 高级教程

### 文件和流

C++文件和流在C++中的标准库fstream中，它定义了三个新的数据类型。

`ofstream`：该数据类型表示输出文件流，用于创建文件并向文件写入信息。

`ifstream`：该数据类型表示输入文件流，用于从文件读取信息。

`fstream`：该数据类型通常表示文件流，且同时具有 ofstream 和 ifstream 两种功能，这意味着它可以创建文件，向文件写入信息，从文件读取信息。

#### 打开文件

在从文件读取信息或者向文件写入信息之前，必须先打开文件。**ofstream** 和 **fstream** 对象都可以用来打开文件进行写操作，如果只需要打开文件进行读操作，则使用 **ifstream** 对象。

下面是 open() 函数的标准语法，open() 函数是 fstream、ifstream 和 ofstream 对象的一个成员。

```c++
void open(const char *filename, ios::openmode mode);
```

在这里，**open()** 成员函数的第一参数指定要打开的文件的名称和位置，第二个参数定义文件被打开的模式。

| 模式标志   | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| ios::app   | 追加模式。所有写入都追加到文件末尾。                         |
| ios::ate   | 文件打开后定位到文件末尾。                                   |
| ios::in    | 打开文件用于读取。                                           |
| ios::out   | 打开文件用于写入。                                           |
| ios::trunc | 如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为 0。 |

模式可以结合使用，比如想以写入模式打开文件，并希望截断文件，防止文件以及存在。

```c++
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc );
```

或者打开文件用于读写。

```c++
fstream  afile;
afile.open("file.dat", ios::out | ios::in );
```

#### 关闭文件

使用close()函数，文件不用时应关闭，养成习惯。

#### 写入文件

在 C++ 编程中，我们使用流插入运算符（ << ）向文件写入信息，就像使用该运算符输出信息到屏幕上一样。唯一不同的是，在这里您使用的是 **ofstream** 或 **fstream** 对象，而不是 **cout** 对象。

#### 读取文件

在 C++ 编程中，我们使用流提取运算符（ >> ）从文件读取信息，就像使用该运算符从键盘输入信息一样。唯一不同的是，在这里您使用的是 **ifstream** 或 **fstream** 对象，而不是 **cin** 对象。

### 异常处理

#### C++异常处理

异常是程序在执行期间产生的问题。C++ 异常是指在程序运行时发生的特殊情况，比如尝试除以零的操作。

异常提供了一种转移程序控制权的方式。C++ 异常处理涉及到三个关键字：**try、catch、throw**。

- **throw:** 当问题出现时，程序会抛出一个异常。这是通过使用 **throw** 关键字来完成的。
- **catch:** 在您想要处理问题的地方，通过异常处理程序捕获异常。**catch** 关键字用于捕获异常。
- **try: try** 块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 catch 块。

如果有一个块抛出一个异常，捕获异常的方法会使用 **try** 和 **catch** 关键字。try 块中放置可能抛出异常的代码，try 块中的代码被称为保护代码。使用 try/catch 语句的语法如下所示：

```c++
try
{
   // 保护代码
}catch( ExceptionName e1 )
{
   // catch 块
}catch( ExceptionName e2 )
{
   // catch 块
}catch( ExceptionName eN )
{
   // catch 块
}
```

#### 抛出异常

使用 **throw** 语句在代码块中的任何地方抛出异常，其中表达式的类型决定了异常的类型。

#### 捕获异常

**catch**跟在**try**后面，来捕获异常，可以指定捕获的异常种类，用...来捕获所有异常

> 抛出的异常是指针类型，什么类型取决于抛出的异常

#### 定义新异常

可以通过继承和重载 **exception** 类来定义新的异常，如下。

```c++
#include <iostream>
#include <exception>
using namespace std;

class MyException : public exception  // 继承struct 和 class 皆可
{
  const char * what () const throw ()  // 标准写法，后面的const throw()是异常规格说明
  {
    return "C++ Exception";
  }
};
 
int main()
{
  try
  {
    throw MyException();
  }
  catch(MyException& e)  // catch 参数传引用或者指针？
  {
    std::cout << "MyException caught" << std::endl;
    std::cout << e.what() << std::endl;
  }
  catch(std::exception& e)
  {
    //其他的错误
  }
}
```

### 动态内存

#### C++ 动态内存

C++程序内存分为两部分：

- **栈：**在函数内部声明的所有变量都将占用栈内存。
- **堆：**这是程序中未使用的内存，在程序运行时可用于动态分配内存。

使用`new`运算符给变量运行时分配堆内的内存，返回分配的地址。不用时，`delete`删除分配的内存

语法如下

```C++
new data-type;
```

例：定义一个double类型的指针，并分配内存

```c++
double* pvalue  = NULL; // 初始化为 null 的指针
pvalue  = new double;   // 为变量请求内存
```

#### 多维数组分配内存

```c++
int ROW = 2;
int COL = 3;
double **pvalue  = new double* [ROW]; // 为行分配内存

// 为列分配内存
for(int i = 0; i < COL; i++) {
    pvalue[i] = new double[COL];
}
```

### 命令空间

为了区分不同的库中的同名函数，引入了命名空间（namespace）

#### 定义命名空间

命名空间的定义使用关键字 **namespace**，后跟命名空间的名称，如下所示：

```c++
namespace namespace_name {
   // 代码声明
}
```

调用带命名空间的函数或者变量：

```c++
name::code;  // code 可以是变量或函数
```

#### using 指令

使用using 指令可以不用在变量或函数前加命名空间

也可以指定特定项目

```c++
using std::cout;
```

#### 嵌套命名空间

命名空间可以嵌套，您可以在一个命名空间中定义另一个命名空间，使用::运算符来访问嵌套的命名空间中的成员。

### 模板

模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式编写代码。

#### 函数模板

模板函数定义的一般形式如下所示：

```c++
template <typename type> ret-type func-name(parameter list)
{
   // 函数的主体
}  
```

> **注意：**模板的声明或定义只能在全局，命名空间或类范围内进行。即不能在局部范围，函数内进行，比如不能在main函数中声明或定义一个模板。

函数参数一般使用引用方式传参

#### 类模板

类模板定义的一般形式如下所示：

```c++
template <class type> class class-name {
.
.
.
}
```

> **注意：** **c++ 函数前面和后面 使用const 的作用：**
>
> - 前面使用 const 表示返回值为const
> - 后面加 const 表示函数不可以修改class的成员
> - const 修饰引用表示禁止通过修改引用值改变被引用的对象

与函数模板不同的是，类模板在实例化时必须显式地指明数据类型，编译器不能根据给定的数据推演出数据类型。如下所示：

```c++
Point<int, int> p1(10, 20);
Point<int, float> p2(10, 15.5);
Point<float, char*> p3(12.4, "东经180度");
```

除了对象变量，我们也可以使用对象[指针](http://c.biancheng.net/c/80/)的方式来实例化：

```c++
Point<float, float> *p1 = new Point<float, float>(10.6, 109.3);
Point<char*, char*> *p = new Point<char*, char*>("东经180度", "北纬210度");
```

> **注意：**赋值号两边都要指明具体的数据类型，且要保持一致

模板类更多应用可见CSDN文章：[C++ 模板_南宫封清的博客-CSDN博客](https://blog.csdn.net/ly1390811049/article/details/118979444)

### 预处理器

#### #define预处理器

\#define 预处理指令用于创建符号常量。该符号常量通常称为**宏**，指令的一般形式是：

```c++
#define macro-name replacement-text 
```

当这一行代码出现在一个文件中时，在该文件中后续出现的所有宏都将会在程序编译之前被替换为 replacement-text。

#### 条件编译

有几个指令可以用来有选择地对部分程序源代码进行编译。这个过程被称为条件编译。

条件预处理器的结构与 if 选择结构很像。请看下面这段预处理器的代码：

```c++
#ifndef NULL
   #define NULL 0
#endif
```

调试开关

```c++
#ifdef DEBUG
   cerr <<"Variable x = " << x << endl; #endif 
```

使用 #if 0 语句注释掉程序的一部分，如下所示：

```c++
#if 0
   不进行编译的代码
#endif
```

#### #和##运算符

\# 运算符会把 replacement-text 令牌转换为用引号引起来的字符串。相当于printf，如下所示：

```c++
#include <iostream>
using namespace std;

#define MKSTR( x ) #x  //printf(x)

int main ()
{
    cout << MKSTR(HELLO C++) << endl;

    return 0;
} 
```

上面的代码运行结果为：`HELLO C++`

##运算符用于连接两个令牌，下面是一个例子：

```c++
#include <iostream>
using namespace std;

#define concat(a, b) a ## b
int main()
{
   int xy = 100;
   
   cout << concat(x, y);  //concat(x, y) 会被编译器转换为 xy
   return 0;
}
```

#### C++中的预定义宏

| 宏       |                             描述                             |
| :------- | :----------------------------------------------------------: |
| __LINE__ |                这会在程序编译时包含当前行号。                |
| __FILE__ |               这会在程序编译时包含当前文件名。               |
| __DATE__ | 这会包含一个形式为 month/day/year 的字符串，它表示把源文件转换为目标代码的日期。 |
| __TIME__ | 这会包含一个形式为 hour:minute:second 的字符串，它表示程序被编译的时间。 |

### 信号处理

信号是由操作系统传给进程的中断，会提早终止一个程序。在 UNIX、LINUX、Mac OS X 或 Windows 系统上，可以通过按 Ctrl+C 产生中断。

有些信号不能被程序捕获，但是下表所列信号可以在程序中捕获，并可以基于信号采取适当的动作。这些信号是定义在 C++ 头文件 <csignal> 中。

| 信号    | 描述                                         |
| :------ | :------------------------------------------- |
| SIGABRT | 程序的异常终止，如调用 **abort**。           |
| SIGFPE  | 错误的算术运算，比如除以零或导致溢出的操作。 |
| SIGILL  | 检测非法指令。                               |
| SIGINT  | 接收到交互注意信号。                         |
| SIGSEGV | 非法访问内存。                               |
| SIGTERM | 发送到程序的终止请求。                       |

#### signal() 函数

C++ 信号处理库提供了 **signal** 函数，用来捕获突发事件。以下是 signal() 函数的语法：

```c++
void (*signal (int sig, void (*func)(int)))(int); 
```

这个函数接收两个参数：第一个参数是一个整数，代表了信号的编号；第二个参数是一个指向信号处理函数的指针。

使用`signal()`捕获SIGNT信号，必须使用`signal`函数来注册信号，并将其与信号处理程序相关联。如下：

```C++
#include <iostream>
#include <csignal>
#include <unistd.h>

using namespace std;

void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";

    // 清理并关闭
    // 终止程序  

   exit(signum);  

}

int main ()
{
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);    // 用于捕获信号

    while(1){
       cout << "Going to sleep...." << endl;
       sleep(1);
    }

    return 0;
}
```

编译运行：

```c++
Going to sleep....
Going to sleep....
Going to sleep....
```

按 Ctrl+C 来中断程序:

```c++
Going to sleep....
Going to sleep....
Going to sleep....
Interrupt signal (2) received.
```

#### raise() 函数

**raise()** 生成信号，该函数带有一个整数信号编号作为参数，语法如下：

```c++
int raise (signal sig);
```

**sig** 是要发送的信号的编号，这些信号包括：SIGINT、SIGABRT、SIGFPE、SIGILL、SIGSEGV、SIGTERM、SIGHUP。以下是我们使用 raise() 函数内部生成信号的实例：

```c++
#include <iostream>
#include <csignal>

using namespace std;

void signalHandler( int signum )  // 必须要传值
{
    cout << "Interrupt signal (" << signum << ") received.\n";

    // 清理并关闭
    // 终止程序 

   exit(signum);  

}

int main ()
{
    int i = 0;
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);    // 捕获到raise()发出的SIGINT信号

    while(++i){
       cout << "Going to sleep...." << endl;
       if( i == 3 ){
          raise( SIGINT);
       }
       sleep(1);
    }

    return 0;
}
```

运行结果：

```c++
Going to sleep....
Going to sleep....
Going to sleep....
Interrupt signal (2) received.
```

### 多线程

#### 进程与线程的区别总结
线程具有许多传统进程所具有的特征，故又称为轻型进程(Light—Weight Process)或进程元；而把传统的进程称为重型进程(Heavy—Weight Process)，它相当于只有一个线程的任务。在引入了线程的操作系统中，通常一个进程都有若干个线程，至少包含一个线程。

- **根本区别**：进程是操作系统资源分配的基本单位，而线程是处理器任务调度和执行的基本单位

- **资源开销**：每个进程都有独立的代码和数据空间（程序上下文），程序之间的切换会有较大的开销；线程可以看做轻量级的进程，同一类线程共享代码和数据空间，每个线程都有自己独立的运行栈和程序计数器（PC），线程之间切换的开销小。

- **包含关系**：如果一个进程内有多个线程，则执行过程不是一条线的，而是多条线（线程）共同完成的；线程是进程的一部分，所以线程也被称为轻权进程或者轻量级进程。

- **内存分配**：同一进程的线程共享本进程的地址空间和资源，而进程之间的地址空间和资源是相互独立的

- **影响关系**：一个进程崩溃后，在保护模式下不会对其他进程产生影响，但是一个线程崩溃整个进程都死掉。所以多进程要比多线程健壮。

- **执行过程**：每个独立的进程有程序运行的入口、顺序执行序列和程序出口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制，两者均可并发执行

C++ 不包含多线程应用程序的任何内置支持。相反，它完全依赖于操作系统来提供此功能。

本教程假设您使用的是 Linux 操作系统，我们要使用 POSIX 编写多线程 C++ 程序。POSIX Threads 或 Pthreads 提供的 API 可在多种类 Unix POSIX 系统上可用，比如 FreeBSD、NetBSD、GNU/Linux、Mac OS X 和 Solaris。

> 待补充...

### STL（标准模板库）

C++ STL（标准模板库）是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如向量、链表、队列、栈。

C++ 标准模板库的核心包括以下三个组件：

| 组件                | 描述                                                         |
| :------------------ | :----------------------------------------------------------- |
| 容器（Containers）  | 容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 |
| 算法（Algorithms）  | 算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。 |
| 迭代器（iterators） | 迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。 |

### map用法

map是一种键值对容器，每一对中第一个值成为关键字（key），每个关键字只出现一次；第二个成为关键字的对应值。

#### Map的使用

1. 需要导入头文件

```c++
#include <map> // STL头文件没有扩展名.h
```

2. map是一个模板类，需要关键字和存储对象两个模板参数

```c++
 std::map<int , std::string> person;
```

3. 可以对模板进行类型定义，方便使用

```c++
typedef std::map<int , std::string> MAP_INI_STRING;
MAP_INI_STRING person;
```

#### Map的构造

##### map 最基本的构造函数

```c++
std::map<int , std::string> mapPerson;
```



##### map 添加数据

1) insert 函数插入 pair 数据

```c++
std::map < int , std::string > mapPerson;
mapPerson.insert(pair < int,string > (1,"Jim"));
```

2)insert 函数插入 value_type 数据

```c++
mapPerson.insert(std::map < int, std::string > ::value_type (2, "Tom"));
```

3)用数组方式插入数据

```c++
mapPerson[3] = "Jerry";
```

##### Map 数据的遍历

三种最常用的遍历方法：

1)前向迭代器

```c++
std::map < int ,std::string > ::iterator it;
    std::map < int ,std::string > ::iterator itEnd;
    it = mapPerson.begin();
    itEnd = mapPerson.end();
    while (it != itEnd) {
	cout<<it->first<<' '<<it->second<<endl;  
	it++;
}
```

2)反向迭代器

```c++
std::map < int, string > ::reverse_iterator iter;  
for(iter = mapPerson.rbegin(); iter != mapPerson.rend(); iter++) 
	cout<<iter->first<<"  "<<iter->second<<endl;  
```

3)数组形式

```c++
mapPerson.insert(std::map<int, std::string>::value_type (1, "Tom"));
mapPerson[2] = "Jim";
mapPerson[3] = "Jerry";

int nSize = mapPerson.size();
for(int n = 1; n <= nSize; n++)
	qDebug()<<QString::fromStdString(mapPerson[n]);
```

三种都是遍历，建议使用前向迭代器，慎用使用数组形成（角标开始位置谨慎）。

##### map 中元素的查找

  find() 函数返回一个迭代器指向键值为 key 的元素，如果没找到就返回指向 map 尾部的迭代器。    

```c++
 map<int ,string > ::iterator l_it;; 
   l_it = maplive.find(112);
   if(l_it == maplive.end())
                cout<<"we do not find 112"<<endl;
   else cout<<"wo find 112"<<endl;
```

##### map 中元素的删除

  如果删除 112；

```c++
iterator erase（iterator it)	;//通过一个条目对象删除
iterator erase（iterator first，iterator last）；	//删除一个范围
size_type erase(const Key&key);	//通过关键字删除
clear()；//就相当于enumMap.erase(enumMap.begin(),enumMap.end());
```

##### map 中 swap 的用法

 Map 中的 swap 不是一个容器中的元素交换，而是两个容器交换；

 示例：

```c++
 #include <map> 
 #include <iostream>
  using namespace std;
  int main( )
  {
      map < int, int > m1, m2, m3;
      map < int, int >::iterator m1_Iter;
      m1.insert ( pair < int, int >  ( 1, 10 ) );
      m1.insert ( pair < int, int >  ( 2, 20 ) );
      m1.insert ( pair < int, int >  ( 3, 30 ) );
      m2.insert ( pair < int, int >  ( 10, 100 ) );
      m2.insert ( pair < int, int >  ( 20, 200 ) );
      m3.insert ( pair < int, int >  ( 30, 300 ) );
   cout << "The original map m1 is:";
   for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
      cout << " " << m1_Iter->second;
      cout   << "." << endl;
   // This is the member function version of swap
   //m2 is said to be the argument map; m1 the target map
   m1.swap( m2 );
   cout << "After swapping with m2, map m1 is:";
   for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
      cout << " " << m1_Iter -> second;
      cout  << "." << endl;
   cout << "After swapping with m2, map m2 is:";
   for ( m1_Iter = m2.begin( ); m1_Iter != m2.end( ); m1_Iter++ )
      cout << " " << m1_Iter -> second;
      cout  << "." << endl;
   // This is the specialized template version of swap
   swap( m1, m3 );
   cout << "After swapping with m3, map m1 is:";
   for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
      cout << " " << m1_Iter -> second;
      cout   << "." << endl;
}
```



##### map 的 sort 问题

 Map 中的元素是自动按 key 升序排序,所以不能对 map 用 sort 函数：

 示例：

```c++
 #include <map>  #include <iostream>
 using namespace std;
 int main( )
 {
   map < int, int > m1;
   map < int, int >::iterator m1_Iter;
   m1.insert ( pair < int, int >  ( 1, 20 ) );
   m1.insert ( pair < int, int >  ( 4, 40 ) );
   m1.insert ( pair < int, int >  ( 3, 60 ) );
   m1.insert ( pair < int, int >  ( 2, 50 ) );
   m1.insert ( pair < int, int >  ( 6, 40 ) );
   m1.insert ( pair < int, int >  ( 7, 30 ) );
   cout << "The original map m1 is:"<<endl;
   for ( m1_Iter = m1.begin( ); m1_Iter != m1.end( ); m1_Iter++ )
      cout <<  m1_Iter->first<<" "<<m1_Iter->second<<endl;
}
```

### vector使用方法

#### 什么是vector？

可以简单的认为，向量是一个能够存放任意类型的动态数组。

#### 容器特性

##### 顺序序列

顺序容器中的元素按照严格的线性顺序排序。可以通过元素在序列中的位置访问对应的元素。

##### 动态数组

支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算述进行该操作。操供了在序列末尾相对快速地添加/删除元素的操作。

##### 能够感知内存分配器的（Allocator-aware）

容器使用一个内存分配器对象来动态地处理它的存储需求。

#### 基本函数

##### 1.构造函数

- `vector()`:创建一个空vector
- `vector(int nSize)`:创建一个vector,元素个数为nSize
- `vector(int nSize,const t& t)`:创建一个vector，元素个数为nSize,且值均为t
- `vector(const vector&)`:复制构造函数
- `vector(begin,end)`:复制[begin,end)区间内另一个数组的元素到vector中

##### 2.增加函数

- `void push_back(const T& x)`:向量尾部增加一个元素X
- `iterator insert(iterator it,const T& x)`:向量中迭代器指向元素前增加一个元素x
- `iterator insert(iterator it,int n,const T& x)`:向量中迭代器指向元素前增加n个相同的元素x
- `iterator insert(iterator it,const_iterator first,const_iterator last)`:向量中迭代器指向元素前插入另一个相同类型向量的[first,last)间的数据

##### 3.删除函数

- `iterator erase(iterator it)`:删除向量中迭代器指向元素
- `iterator erase(iterator first,iterator last)`:删除向量中[first,last)中元素
- `void pop_back()`:删除向量中最后一个元素
- `void clear()`:清空向量中所有元素

##### 4.遍历函数

- `reference at(int pos)`:返回pos位置元素的引用
- `reference front()`:返回首元素的引用
- `reference back()`:返回尾元素的引用
- `iterator begin()`:返回向量头指针，指向第一个元素
- `iterator end()`:返回向量尾指针，指向向量最后一个元素的下一个位置
- `reverse_iterator rbegin()`:反向迭代器，指向最后一个元素
- `reverse_iterator rend()`:反向迭代器，指向第一个元素之前的位置

##### 5.判断函数

- `bool empty() const`:判断向量是否为空，若为空，则向量中无元素

##### 6.大小函数

- `int size() const`:返回向量中元素的个数
- `int capacity() const`:返回当前向量所能容纳的最大元素值
- `int max_size() const`:返回最大可允许的 vector 元素数量值

##### 7.其他函数

- `void swap(vector&)`:交换两个同类型向量的数据
- `void assign(int n,const T& x)`:设置向量中前n个元素的值为x
- `void assign(const_iterator first,const_iterator last)`:向量中[first,last)中元素设置成当前向量元素

##### 总结

1.push_back 在数组的最后添加一个数据

2.pop_back 去掉数组的最后一个数据

3.at 得到编号位置的数据

4.begin 得到数组头的指针

5.end 得到数组的最后一个单元+1的指针

6．front 得到数组头的引用

7.back 得到数组的最后一个单元的引用

8.max_size 得到vector最大可以是多大

9.capacity 当前vector分配的大小

10.size 当前使用数据的大小

11.resize 改变当前使用数据的大小，如果它比当前使用的大，者填充默认值

12.reserve 改变当前vecotr所分配空间的大小

13.erase 删除指针指向的数据项

14.clear 清空当前的vector

15.rbegin 将vector反转后的开始指针返回(其实就是原来的end-1)

16.rend 将vector反转构的结束指针返回(其实就是原来的begin-1)

17.empty 判断vector是否为空

18.swap 与另一个vector交换数据

#### 基本用法

```C++
#include < vector> 
using namespace std;
```

#### 简单生成vector

1. Vector<类型>标识符
2. Vector<类型>标识符(最大容量)
3. Vector<类型>标识符(最大容量,初始所有值)
4. Int i[5]={1,2,3,4,5}Vector<类型>vi(I,i+2);//得到i索引值为3以后的值
5. Vector< vector< int> >v; 二维向量//这里最外的<>要有空格。否则在比较旧的编译器下无法通过

#### 使用实例

**使用vector注意事项：**

1、如果你要表示的向量长度较长（需要为向量内部保存很多数），容易导致内存泄漏，而且效率会很低；

2、Vector 作为函数的参数或者返回值时，需要注意它的写法：

```c++
double Distance(vector<int>&a, vector<int>&b)
```

 其中的“&”绝对不能少！！！

##### vector对象的定义和实例化

| vector<T> v1;       | 保存类型为 T 对象。默认构造函数 v1 为空。 |
| ------------------- | ----------------------------------------- |
| vector<T> v2(v1);   | v2 是 v1 的一个副本。                     |
| vector<T> v3(n, i); | v3 包含 n 个值为 i 的元素。               |
| vector<T> v4(n);    | v4 含有值初始化的元素的 n 个副本。        |

> **注意：**
>
> 1、若要创建非空的 vector 对象，必须给出初始化元素的值；
>
> 2、当把一个 vector 对象复制到另一个 vector 对象时，新复制的 vector 中每一个元素都初始化为原 vectors 中相应元素的副本。但这两个 vector 对象必须保存同一种元素类型；
>
> 3、可以用元素个数和元素值对 vector 对象进行初始化。构造函数用元素个数来决定 vector 对象保存元素的个数，元素值指定每个元素的初始值。

