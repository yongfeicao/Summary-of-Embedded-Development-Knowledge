###编码规范的目的
编码规范是为了减少代码的bug
提升代码的可维护性和可以移植性
以低成本开发可靠的嵌入式软件
编码规范可以帮助团队中的成员减少理解阅读代码的时间

###编码规范的局限性
编码规范不可以百分之百避免软件bug

###编码规范要求
####1 通用规则
#####1.1 C语言相关
所有的代码编写者必须保证编写的代码符合C99标准
使用c++编译器时通过编译选项将其设置为标准的c语言
尽量避免与编译器语言相关的关键字拓展 #pragma
不能使用#define的宏定义c语言的关键字
示例：
```c
#define begin  {        // Don’t do something like this... 
#define end    }        // ... nor this. 
... 
for (int row = 0; row < MAX_ROWS; row++) 
    begin 
        ...      
    end                 // Let C be C, not some language you once loved. 
```
######1.2 行宽度
每行代码不能超过80个字符
理由：方便打印方便使用diff工具和竖屏幕查看代码

######1.3大括号
a. 只有单条语句或者语句为空，也要使用大括号
b.大括号单独占用一行，并且同级别需要对其
示例：
```c
{ 
    if (depth_in_ft > 10) dive_stage = DIVE_DEEP;    // This is legal... 
    else if (depth_in_ft > 0) 
        dive_stage = DIVE_SHALLOW;                   // ... as is this. 
    else   
    {                                               // But using braces is always safer. 
        dive_stage = DIVE_SURFACE; 
    } 
    ... 
} 
```
理由：如果以后追加逻辑或者修改逻辑，不使用大括号管理的话会导致逻辑错误

#####1.4小括号
不要相信c语言隐性的的计算优先级，使用括号显性规定计算优先级，来增强代码的可读性和安全性。
除非它是单个标识符或常量，否则逻辑与的每个操作数（&&）和逻辑OR（||）运算符用括号括起来。
示例：
```c
if ((depth_in_cm > 0) && (depth_in_cm < MAX_DEPTH)) 
{ 
    depth_in_ft = convert_depth_to_ft(depth_in_cm); 
} 
```

#####1.5常用缩写
请参照英文文档的P57页

#####1.6 Casts(中文翻译不明) 
每一个存在数据溢出可能的强制类型转换都需要添加注释并解释不会出问题的原因
示例：
```c
int abs (int arg) 
{ 
    return ((arg < 0) ? -arg : arg); 
} 
 
... 
    uint16_t sample = adc_read(ADC_CHANNEL_1); 
 
    result = abs((int) sample);             // WARNING: 32-bit int assumed. 
```
上面的示例，如果abs函数的参数是signed 16bit类型的化，上面的代码就会有数据类型溢出的风险。(PS：对于这一条规范的理解有些困难，因此解读可能存在误导)
#####1.7 避免的关键字
a.禁止使用auto来自动化变量类型
b.禁止使用register来修饰变量,将其中变量分配的区域交给编译器来设置
c.尽量避免使用goto，如果使用goto只能跳转到同一个label中或者一个附近的代码块中
d.尽量避免使用continue

理由：auto是c语言的一个不必要的历史特性，个人认为使用auto使得嵌入式软件变量处于一种不可控的状态。容易造成变量的数据类型溢出。
register关键字使得程序员是干预编译器的行为
因此以上两个关键字在现代编程中是不必要的

goto和continue仍然在c语言中使用，但是他们的使用会导致代码混乱，特别是在结构化编程中乱用goto是糟糕的。但是
使用goto语句来处理异常，从而来简化代码这是可以的

#####1.8 建议使用的关键字
a.应当使用static来申明只在module内部使用的函数和变量
b.在下面的情况下建议使用const来修饰
> i.  初期化之后就不变的变量
>  ii. 要定义不应修改的引用调用函数形参(例如char const * param)
>  iii.在结构或联合中定义不应被修改的字段(例如，在内存映射的I/O外围寄存器的结构覆盖中)
>  iv. 作为数字常量的强类型替代#define

c.在下面的情况下有必要使用关键字volatile
>i.  声明一个可以被任何中断服务例程访问(通过当前使用或作用域)的全局变量
>ii. 声明一个可被两个或多个线程访问(按当前使用或作用域)的全局变量
>iii.声明一个指向内存映射I/O外围寄存器集的指针(例如，timer_t volatile * const p_timer)
>iv.声明一个延迟循环计数器。

2 注释规则
2.1 接受的规则

3 空格规则
对其规则
空行规则
缩进规则
tab规则
换行规则

模块的规则
模块命名规则
头文件
源文件
文件模板

变量类型规则
位宽
有符号和无符号
浮点型
结构体和联合
bool类型

程序规则
函数
类似函数的宏定义
线程函数
中断服务函数

变量规则
语句规则
变量的定义
条件语句
开关语句
循环
跳转
等于条件

常见代码中缩写
初期化


