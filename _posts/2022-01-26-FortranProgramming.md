---
layout:     post
title:      Fortran Programming
subtitle:   Fortran Programming Review
date:       2022-01-26
author:     Rain Chen
catalog: true
tags:
    - 开发与应用
---

Contents
- unordered list
{:toc}

# Fortran Programming Review

Made by Rain Chen

Dec. 2021

## 0. Fortran 简介

Fortran 是 Formula Translation 的缩写，是世界上最早推广使用的高级程序设计语言，主要适用于科学和工程问题的数值计算。

Fortran 90 增加了一些新特性。比如，自由格式源代码的输入，小写的 Fortran 关键字等等，还有改善的函数传参机制等。

## 1. Fortran 95 程序设计初步

### 1.1 程序的结构说明

类型说明语句，输入语句，赋值语句，输出语句……

```fortran
program example
    real a, b, c, d, x1, x2
    read * , a, b, c
    d = b * b - 4 * a * c
    if (d >= 0) then
        x1 = (-b + sqrt(d)) / (2 * a)
        x2 = (-b - sqrt(d)) / (2 * a)
        print * , "x1 =", x1
        print * , "x2 =", x2
    else
        print * , "方程无实根."
    end if
end program
```

在 Fortran 中使用 `!` 来表示注释。注释非常重要，没有注释的程序不能算合格的程序。

一个 Fortran 程序由一个或者若干个程序单元组成，包括主程序单元，外部子程序单元，模块单元，数据块单元。一个程序中必须有且只有一个主程序单元。每一个程序单元以 `end` 结束。

### 1.2 字符集

Fortran 95 的字符集，是在编写 Fortran 95 程序的时候能够使用的全部字符及符号的集合。包括：

- 英文字母 a ~ z, A ~ Z
- 阿拉伯数字 0 1 2 3 4 5 6 7 8 9
- 22 个特殊字符

除了字符型常量之外， Fortran 95 源程序不区分大小写。Fortran 95 字符集之外的可打印字符只能出现在注释、字符常量、字符串编辑符和输入输出记录中。

### 1.3 标识符

标识符是名称。用来在程序中标识有关实体，比如变量、符号常量、函数、程序单元、公用块、数组、模块和形参等。规定标识符只能由字母、数字、下划线和美元符号组成。

### 1.4 关键字

Fortran 95 中的一种特定的字符串。正确的关键字在代码编辑器中会显示代码高亮。关键字有特定的含义，不能随便改变以防出现语法错误。需要注意的是，Fortran 95 对关键字不予以保留，关键字可以作为其他实体的名称，但是不建议这样做。使用关键字作为实体的名称会导致程序难以理解和阅读。

常用的 Fortran 95 关键字

| 关键字 | 关键字 | 关键字 | 关键字 | 关键字 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| `program` | `implicit` | `read` | `write` | `print` |
| `format` | `call` | `real` | `integer` | `character` |
| `do` | `end` | `if` | `parameter` | `complex` |
| `logical` | `double precision` | `then` | `open` | `subroutine` |
| ... | ... | ... | ... | ... |

### 1.5 书写格式

#### 1.5.1 固定格式 Fixed Format

每行 80 个字符。分成 4 个区。

##### 1.5.1.1 标号区

- 1 到 5 列为标号区，标号最多为 5 位数字。
- 数字中的空格不起作用
- 标号区的第一个字符为 `!` 说明该行为注释行。

##### 1.5.1.2 续行区

- 第 6 列是续行区。续行标志为除去空格和零意外的任何 Fortran 字符。
- 注释行没有续行的概念。

##### 1.5.1.3 语句区

- 第 7 到 72 列为语句区
- 语句只能书写到语句区
- 一行只能写一个语句，写不下可以分多个语句写，或者续行

##### 1.5.1.4 注释区

- 注释区中的注释不需要给出注释行的标志符。

#### 1.5.2 自由格式 Free Format

语句可以从任意位置开始书写。不受分区和位置限制。每一行可以编写 132 个字符。一行可以写多个语句，语句之间用语句分隔标志符 `;` 做间隔。续行标志符 `&` 出现在前一行的末尾。使用 `!` 作为注释符号。

续行时要注意的问题：

```fortran
y = cos(atan(sqrt(x ** 3 + y ** 3) / (x ** 2 + 1))) + cos(x * y / (sqrt(x ** 2 + y ** 2))) + &
exp(a * x ** 2 + b * x + c)
```

在没有把具有特定意义的字符分成两行的时候，只需在前面一行的末尾加上一个换行符 `&`，但是在把有特定意义的字符分成两行的时候，行尾要加一个换行符，下一行的开头也要加一个换行符。

```fortran
y = cos(atan(sqrt(x ** 3 + y ** 3) / (x ** 2 + 1))) + co&
&s(x * y / (sqrt(x ** 2 + y ** 2))) + &
exp(a * x ** 2 + b * x + c)
```

## 2. 数据类型

### 2.1 基本数据类型

#### 2.1.1 数值型

##### 2.1.1.1 整数类型 Integer

- 长整型
- 短整型

整数数据类型包括正数、负数和 0。

| 整数类型名 | 字节数 | 取值范围 |
| --- | --- | --- |
| INTEGER(1) | 1 | $ - 2 ^ 7 $ ~ $ 2 ^ 7 - 1 $ |
| 短整型 INTEGER(2) | 2 | $ - 2 ^ {15} $ ~ $ 2 ^ {15} - 1 $ |
| 长整型 INTEGER | 4 | $ -2 ^ {31} $ ~ $2 ^ {31} - 1 $ |
| INTEGER(8) | 8 | $ - 2 ^ {63} $ ~ $ 2 ^ {63} - 1 $ （Alpha 系统） |

##### 2.1.1.2 实数类型 Real

又称为浮点数。数值是近似值，有误差累计。通常有两种表示形式，十进制的小数形式和指数形式。

> 注意：指数形式表示时，指数部分必须是整数。

| 实数类型名 | 字节数 | 精度（有效数字） | 取值范围 |
| --- | --- | --- | --- |
| 单精度 | 4 | 6 ~ 7 | $ \pm 3.40282347E38 $ ~ $ \pm 1.17549435E-38 $ |
| 双精度 | 8 | 16 ~ 17 | $ \pm 2.2250738585072013D308 $ ~ $ \pm 1.7976931348623158D-08 $ |

##### 2.1.1.3 复数类型 Complex

单精度复数和双精度复数。表示形式：

```fortran
(a, b)
```

例如：`(1.2, 3.5)` 表示 $ 1.2 + 3.5i $ 。

> Fortran 是目前唯一提供复数类型的计算机语言。

#### 2.1.2 非数值型

##### 2.1.2.1 字符类型 Character

只有一个字母或符号称为字符，有多个字符时称为字符串。使用一对单引号或者一堆双引号括起来，例如：

```fortran
'Fortran charater'    ! 用单引号
"hello world!"    ! 用双引号
```

##### 2.1.2.2 逻辑类型 Logical

表示判断的结果，只有两种值：是和否，即对错，真假。
```fortran
.true.    ! 真
.false.    ! 假
```

### 2.2 数组类型

数组是类型相同的一批数据的有序集合。每个数组有一个数组名。

#### 2.2.1 定义数组

##### 2.2.1.1 用 dimension 语句定义数组

```fortran
dimension 数组名(下标下界 : 下标上界, ...), ...
```

数组说明符定义了数组名、数组的维数和大小。数组的维数由维数说明符的个数确定。维数说明符最少有 1 个，最多有 7 个。数组的大小就是数组元素的个数。

例如：

```fortran
dimension g(10), s(3, 5)
dimension g(1 : 10), s(1 : 3, 1 : 5)    ! 含义相同
```

在 dimension 语句之后可以使用类型说明语句说明数组的类型，不需要重复写维数说明符。

```fortran
dimension a(1 : 10), n(10 : 15), m(-1 : 1, 0 : 2, 0 : 3), name(1 : 30)
real a, n
integer b, m
character * 8 name
```

##### 2.2.1.2 用类型说明语句定义数组

一般格式为：

```fortran
类型说明符 数组名(下标下界 : 下标上界, ...), ...
```

定义数组时必须明确数组的大小。

##### 2.2.1.3 同时使用类型说明语句和 dimension 语句定义数组

一般格式为：

```fortran
类型说明符, dimension(下标下界 : 下标上界, ...) :: 数组名[, ...]
```

例如：

```fortran
integer, dimension(10) :: a, n, w(2, 3)
```

#### 2.2.2 给数组赋初值

##### 2.2.2.1 使用数组赋值符赋值

一般格式：

```fortran
数组名 = (/取值列表/)
```

取值列表可以是类型相同的常量、变量、函数、表达式或者隐含 do 循环，它们之间用逗号隔开。

```fortran
program array
    integer a(5), b(5)
    k = 1
    a = (/1, 3, k + 4, 7, 9/)
    b = (/i, i = 2, 10, 2/)
    print * , a
    print * , b
end program
```

##### 2.2.2.2 使用 data 语句给数组赋初值

data 语句时专门用来给变量、数组、数组元素赋初值的语句。一般格式如下：

```fortran
data 数组名/常量表/, 数组名/常量表/, ...
```

数组元素个数必须和常量一一对应。

#### 2.2.3 对数组的操作

##### 2.2.3.1 对元素的操作

对数组进行操作要先定义再使用。对数组元素的操作即是对数组元素的引用。

一般格式为：

```fortran
数组名(下标, [下标], ...)
```

例如：

```fortran
integer a(2, 2), b(5)
a(1, 1) = 10    ! 对数组 a 中的第一个元素赋值为 10
do i = 1, 5
    b(i) = 3
end do
```

##### 2.2.3.2 对整体的操作

用数组名代表整个数组。

```fortran
integer a(2, 2), b(5)
a = 10
b = 3
```

当数组 a 和数组 b 和数组 c 维数大小相同时，可以直接操作：

```fortran
a = b    ! 把数组 b 中的同一位置的元素的值对应赋值给 a 数组中的对应元素
a = b + c
a = b - c
a = b * c
a = b / c
```

##### 2.2.3.3 局部引用

例如：

```fortran
integer a(6)
a(3 : 5) = 0    ! 把 a(3), a(4), a(5) 赋值为 0
a(1 : 6 : 2)  = 3    ! 把 a(1), a(3), a(5) 赋值为 3
```

数组元素的三元表达式可以写成：

```fortran
初值 : 终值 : 步长
```

可以通过三元表达式引用数组的一部分。例如：

```fortran
b(4, 6) = a(1, 3)
```

### 2.3 指针类型

省略此部分内容。

### 2.4 派生类型与结构体

自己定义的数据类型。

> Fortran 90 之前的版本不支持结构体

#### 2.4.1 派生类型定义

使用 type 块来定义一个派生类型，应该写在程序说明部分，通常是写在说明的前部。派生类型是一种抽象。

定义格式：

```fortran
type 派生类型名
    member1 类型说明
    member2 类型说明
    member3 类型说明
    member4 类型说明
    ...
    membern 类型说明
end type [派生类型名]
```

#### 2.4.2 结构体的定义与引用

结构体是对派生类型的一种具体化。只有定义了派生类型结构体或结构体数组，才能对结构体或结构体数组成员进行处理。

结构体定义的一般格式如下：

```fortran
type(派生类型名) :: 结构体列表
```

引用结构体成员需要这样写：

```fortran
结构体名 % 成员名
结构体名.成员名    ! 这样也行，不过最好两种不要混用
```

在嵌套定义的结构体中，成员引用应该嵌套使用引用符 `%` 或者 `.`。例如：

```fortran
stu % date % year
```

使用 `.` 作为引用符，成员名不能使用逻辑运算符和关系运算符。

### 2.5 公用区类型

省略此部分内容。

## 3. 变量与常量

### 3.1 变量

#### 3.1.1 变量的概念

程序运行期间其值可以改变的量。变量是程序处理的主要对象。系统为程序中的每一个变量开辟一个存储单元，用来存放变量的值。

> 在每一个瞬间，变量只能有一个确定的值。

在程序中用到的变量，应该给它赋予确定的值，否则计算机系统就会给它一个不确定的随机值，也有些系统会把程序未赋值的变量的值设置为 0。

#### 3.1.2 变量名

一个变量需要用一个变量名来识别，变量名就是变量的名字。在同一个程序单元中，不能用同一个变量名来代表两个不同的变量。Fortran 的变量名只能由字母、数字、下划线和美元符号组成。

- 在变量名中大小写字母是等价的。`SUM`、`Sum`、`sum`、`suM` 都代表同一个变量。
- 变量名中的字符可以插入空格
- 变量命名要可以做到顾名思义：使用一些有意义的单词对变量命名。
- 虽然 Fortran 没有规定保留字，但是为了避免混淆，建议不要使用 Fortran 中的关键字作为变量名。

#### 3.1.3 类型

变量应该先说明后使用。声明变量的数据类型，以便分配相应大小的存储空间来存放值。

##### 3.1.3.1 隐含约定：I-N 规则

Fortran 规定凡是以 `i, j, k, l, m, n` 开头的（不区分大小写）变量名，都认为该变量是整型变量。以其他字母开头的变量为实型变量。

```fortran
i, j, max, n1, num    ! 整型变量
a, b, c, x, y, sum, aver, count, total    ! 实型变量
```

##### 3.1.3.2 使用类型说明语句

类型说明语句可以强制置顶某些变量的类型。

```fortran
integer <变量>    ! 整型说明语句
real <变量>    ! 实型说明语句
double precision <变量>    ! 双精度型说明语句
complex <变量>    ! 复数说明语句
character <变量>    ! 字符型说明语句
logical <变量>    ! 逻辑型说明语句
```

在类型声明后使用 `::` 可以对变量进行初始化。

```fortran
integer x, y, z    ! 定义 x, y, z 是整型变量
character * 30 name    ! 定义 name 是字符型变量，长度为 30
complex :: s = (1.5, 8.9)    ! 定义 s 是单精度复数型变量，对它赋初值
integer(2) :: a = 1.8    ! 定义 a, b 是短整型变量，对变量 a 赋初值
real * 8 l    ! 定义 l 是双精度型变量
```

##### 3.1.3.3 使用 implicit 语句

隐含说明语句。可以将某个字母开头的变量名指定为某个变量类型。

取消隐含声明：

```fortran
implicit none
```

更改一个隐含声明：

```fortran
implicit integer(a, f, c, t - v)    ! 把 a, f, c, t 到 v 开头的变量指定为整数类型
```

可以使用一个 `implicit` 语句声明多种类型。例如：

```fortran
implicit integer(a, b, x - z), real(i, k), character(c, n)
```

> 类型声明的优先级：类型说明语句最高，`implicit` 语句次之，I-N 规则最低。

#### 3.1.4 给变量赋初值

##### 3.1.4.1 直接赋值

先定义一个变量，然后给它赋值。

```fortran
integer a
a = 20
```

或者直接初始化。

```fortran
integer :: a = 10
```

##### 3.1.4.2 用 data 语句给变量赋初值

一般格式：

```fortran
data var1, var2, ... , varn /c1, c2, ..., cn/
```

- 可以同时给多个变量赋初值，中间用逗号隔开。
- 被赋值的常量放在一对 `//` 中间。
- 被赋值的常量的数据类型与对应的变量的数据类型要一致。
- 可以使用 `*` 来表示数据的重复。

```fortran
data m, n, k /3 * 5/
```

执行上名语句之后，变量 `m`、`n`、`k` 的值都为 5。

### 3.2 常量

常量在程序中直接给出，在程序运行时保持不变。分为直接常量和符号常量。

#### 3.2.1 直接常量

包括：整型常量、实型常量、双精度型常量、复型常量、字符型常量和逻辑型常量。

##### 3.2.1.1 整型常量

可以表示成十进制以及二至三十六进位制。

对于十进制的整数，通过整型 kind 值（类别类型参数）来确定整数的存储空间大小和取值范围。整型的 kind 值有四种：1，2，4（默认是 4 ），8（仅对 Alpha 系统有效）。

表示方法：`number_kind`。例如：

```fortran
-16_2, 18_4, 5_1
```

对于二至三十六进位制，表示形式是：`r#number` 或 `-r#number`，通常来说，十六进制常省略基数 16。

```fortran
print * , 2#1111001111001111001111
print * , 7#45644664
print * , +8#17171717
print * , 3994575
print * , #3cfecf
print * , 36#2dm8f
```

##### 3.2.1.2 实型常量和双精度型

通常表示成十进制小数和指数两种形式。必须包含小数点。

小数形式：

```fortran
n.m
n.
.m
```

指数形式：

```fortran
n.mEs
n.Es
nEs
.mEs
```

##### 3.2.1.3 复型常量

采用坐标的形式表示一个复数。可以时确定的数，也可以是一个有确定结果的表达式。

##### 3.2.1.4 字符型常量

就是字符串，使用单引号或者双引号括起来的若干字符序列。

非打印字符表示形式

| 表示形式 | 非打印字符 | 表示形式 | 非打印字符 |
| --- | --- | --- | --- |
| `\a` 或 `\A` | BELL | `\v` 或 `\V` | 垂直 Tab |
| `\b` 或 `\B` | 退格 | `\t` 或 `\T` | 水平 Tab |
| `\f` 或 `\F` | 进格 | `\\` | 输出 `\` |
| `\n` 或 `\N` | 换行 | `\xhh` | 输出十六进制编码为 hh 的任意 ASCII 字符 |
| `\r` 或 `\R` | 回车 | `\ddd` | 输出八进制编码为 ddd 的任意 ASCII 字符 |

##### 3.2.1.5 逻辑型常量

只有两个：`.true.` 和 `.false.`。注意，逻辑值两边的小数点必须有，不区分大小写。逻辑型 kind 值有 4 中，分别是 1，2，4，8（仅对 Alpha 系统有效）。对于逻辑值 `.true.`，在其存储单元内每个二进制位上都是 1，可以视为整数 -1；对于逻辑值 `.false.`，其存储单元内每个二进制位上都是 0，可以视为整数 0。

> 逻辑值可以参与数值型数据的运算。

#### 3.2.2 符号常量

是一种特殊的常量，程序单元内代表常量的标识符。

```fortran
parameter(标识符 = 常量, 标识符 = 常量, ...)
```

## 4. 表达式

Fortran 中一共四种表达式：

- 算术表达式
- 字符表达式
- 关系表达式
- 逻辑表达式

### 4.1 算术表达式

顾名思义，加减乘除。

### 4.2 字符表达式

字符连接符 `//` 把两个字符型数据连接起来，组成一个新的字符型数据。

```fortran
character * 5 ch1, ch2, ch3, ch4 8 1, ch5 * 11
ch1 = "LOVE"
ch2 = "CHINA"
ch3 = "STUDENT"
ch4 = "I"
ch5 = ch4 // " " // ch1 // "YOU!"
```

### 4.3 关系表达式

Fortran 提供了 6 个关系运算符。两种格式可以单独使用，也可以混合使用。使用字母格式时两边的点不能省略。各关系运算符优先级别相同，从前向后一次运算即可。

| 字母格式 | 符号格式 | 英语含义 | 数学含义 |
| --- | --- | --- | --- |
| `.lt.` | `<` | less than | 小于 |
| `.le.` | `<=` | less than or equal to | 小于等于 |
| `.eq.` | `==` | equal to | 等于 |
| `.ne.` | `/=` | not equal to | 不等于 |
| `.gt.` | `>` | greater than | 大于 |
| `.ge.` | `>=` | greater than or equal to | 大于等于 |

一般表达式：

```fortran
<算术量1> 关系运算符 <算术量2>
```

> 谨慎使用等于或不等于关系判断实型。实型数据是使用近似值存储的，可能存在误差。

### 4.4 逻辑表达式

Fortran 提供了 6 种逻辑运算符。

| 逻辑运算符 | 名称 | 运算举例 |
| --- | --- | --- |
| `.and.` | 逻辑与 | `a .and. b` |
| `.or.` | 逻辑或 | `a .or. b` |
| `.not.` | 逻辑非 | `not. a` |
| `.eqv.` | 逻辑等 | `a .eqv. b` |
| `.neqv.` | 逻辑不等 | `a .neqv. b` |
| `.xor.` | 逻辑运算 | `a .xor. b` |

一般表达式：

```fortran
<逻辑量1> 逻辑运算符 <逻辑量2>
```

## 5. 程序的结构

### 5.1 顺序结构

顺序结构只能从上到下依次执行一条语句。很常见。

#### 5.1.1 赋值语句

赋值语句用于赋值。一般格式就是：

```fortran
V = e    ! Variable = expression
```

仔细理解，没有别的了。

#### 5.1.2 输出语句

- 按系统隐含的标准格式输出：自由格式输出，又叫表控输出。
- 按用户指定格式输出：有格式输出。例如：`100 format(...)`
- 无格式输出：以二进制形式输出数据，只适用于向磁盘等输出，对文件进行操作。

##### 5.1.2.1 表控输出语句

```fortran
print * , 输出列表项
```

`*` 表示在系统隐含指定输出设备（一般是显示器）上按系统隐含的标准格式输出数据。如果 `*` 后没有任何输出项，相当于一个换行。

或者可以写成：

```fortran
write(*, *) 输出列表项
```

第一个 `*` 表示系统隐含指定的输出设备，第二个 `*` 表示用表控格式输出。

##### 5.1.2.2 其他输出

省略此部分内容。

#### 5.1.3 输入语句

##### 5.1.3.1 表控输入语句

```fortran
read * , 输入项列表
```

`*` 表示从系统隐含指定的输入设备（一般是键盘）按表控格式输入数据。例如：

```fortran
read * , a, b, c
```

也可以写成这样：

```fortran
read(* , *) 输入表
```

其中第一个 `*` 表示系统隐含的指定输入设备（键盘），第二个 `*` 表示表控输入。

##### 5.1.3.2 其他输入

省略此部分内容。

#### 5.1.4 end、stop、pause 语句

##### 5.1.4.1 end 语句

结束语句。

- 结束本程序单元的运行
- 作为一个程序单元的结束标志，写在所在程序单元的最后一行

end 语句在主程序中兼有 `stop` 语句的作用，使程序停止运行，在子程序中兼有 `return` 语句的功能，控制返回到调用程序。

##### 5.1.4.2 stop 语句

一般格式：

```fortran
stop [n]
```

其中 `n` 是一个不超过五位数的数字或者一个字符串。执行 `stop` 语句的时候输出整数或者字符串，供程序员辨别程序流程。

> 除非必要，不要把 `stop` 命令放在主程序结束之外的其他地方，因为如果一个程序有太多的结束终点，容易出错。`stop` 命令不是必要的，因为程序执行完毕之后会自动停止。

##### 5.1.4.3 pause 语句（弃用）

让系统把程序暂时挂起，等待程序员完成其他工作。可以理解为一个断点（breakpoint），便于调试程序。

一般格式：

```fortran
pause [n]
```

> 由于现在的编译环境中可以直接设置断点，所以 `pause` 语句一般不再使用。如果使用，可能会出现警告。

### 5.2 选择结构

#### 5.2.1 逻辑 if 语句

```fortran
if (表达式e) 可执行语句s
```

表达式可以是逻辑表达式或者关系表达式。

#### 5.2.2 块 if 语句

##### 5.2.2.1 单分支

```fortran
if (表达式e) then
    <then 块>
end if
```

##### 5.2.2.2 双分支

```fortran
if (表达式e) then
    <then 块>
else
    <then 块>
end if
```

##### 5.2.2.3 多分支

```fortran
if (表达式e1) then
    <then 块1>
else if (表达式e2) then
    <then 块2>
    ...
else if (表达式en) then
    <then 块n>
end if
```

#### 5.2.3 块 case 结构

多重判断除了使用 if 结构完成之外，还可以使用 case 结构。

```fortran
select case(表达式e)
case(数值1)
    <块1>
case(数值2)
    <块2>
    ...
case(数值n)
    <块n>
case default
    <块n+1>
end select
```

块 case 结构有限制。表达式 e 只能时整型、字符型或者逻辑型。每个 `case` 语句中所使用的数值必须是固定的常量，不能使用变量，类型要与表达式 e 的类型一致。

#### 5.2.4 多重 if 结构嵌套

注意内层和外层不能交叉，包含关系必须清楚。

> 为了使程序清晰，应该使用缩进，同一层级对齐。

### 5.3 循环结构

#### 5.3.1 do 循环

##### 5.3.1.1 一般 do 循环

程序执行的重复次数已知时可用。

```fortran
program doloop
    do i = 1, 10
        print * , "Hello", i, "次"
    end do
end program
```

`do` 循环语句的一般格式写成：

```fortran
do V = e1, e2[, e3]
```

中括号表示可以没有。`e1` 表示循环变量的初值，`e2` 表示循环变量的终值，`e3` 表示循环变量的步长（增量）。`V` 表示循环控制变量（计数器），即循环变量。

循环变量在循环体中只能被引用，不能被赋值。

举个「水仙花数」的例子：

```fortran
program flower
    integer n, n1, n2, n3
    do n = 100, 999
        n1 = n / 100
        n2 = mod(n / 10, 10)
        n3 = mod(n, 10)
        if (n1 ** 3 + n2 ** 3 + n3 ** 3 == n) then
            print * , n, "是水仙花数"
        end if
    end do
end program
```

##### 5.3.1.2 隐含 do 循环

这样写：

```fortran
print * , (W, V = e1, e2[, e3])    ! 输出
read * , (W, V = e1, e2[, e3])    ! 输入
```

`W` 是输入或者输出项的列表。

> 隐含 do 循环在一行中输出，而一般的 do 循环将换行输出。

#### 5.3.2 do while 循环

一般格式：

```fortran
do while (逻辑表达式)
    循环体
end do
```

该循环由三部分组成，分别是 `do while` 语句、循环体和 `end do` 语句。

> 在使用 do while 循环的时候，一定要避免死循环。保证循环中至少有一条控制循环条件的语句。

#### 5.3.3 exit 语句

顾名思义，跳出循环。do 循环和 do while 循环都可以使用 `exit` 语句。

#### 5.3.4 cycle 语句

略过循环体 `cycle` 语句之后的所有语句，直接跳回开头执行下一次循环。do 循环和 do while 循环都可以使用 `cycle` 语句。

## 6. 函数与子程序

循环不能消除不同程序单元之间的重复行为，为了解决这类问题，Fortran 设计语言产生了一种新技术：子程序。子程序是能结局某个特定问题的相对独立的程序单元。

> 子程序不能单独运行，只能被调用。

### 6.1 语句函数

定义形式：

```fortran
函数名(x1, x2, x3, ..., xn) = 表达式
```

#### 6.1.1 语句函数名

命名方法与变量命名方法相同。注意，语句函数名不能与本程序单元中的任何其他变量同名。例如：

```fortran
root1(a, b, c) = (- b + sqrt(b ** 2 - 4.0 * a * c)) / (2.0 * a)
da(a, b) = sqrt(b * b + a * a * a)
integer db
db(a, b) = sqrt(b * b + a * a)
```

#### 6.1.2 形式参数

如果语句函数没有形式参数时，括号也不能省略。当形式参数多于一个时，它们之间用逗号隔开。形式参数不代表任何实在的值，因此，它可以与程序中的变量同名。

#### 6.1.3 语句函数的调用

一旦声明了语句函数，就可以在同一程序单元中调用它。一般写成：

```fortran
函数名(实参表)
```

### 6.2 函数子程序

语句函数只能解决一些较为简单的问题，当函数关系较为复杂，用一个语句无法定义的时候就需要函数子程序。

#### 6.2.1 函数子程序的定义

```fortran
[类型说明符] function 函数名(形式参数表)
    函数体
end [function [函数名]]
```

函数名的类型说明既可以放在函数名之前，也可以放在函数体的开头。

#### 6.2.2 函数子程序的调用

方法与调用标准函数、语句函数相同。这样写：

```fortran
函数名(实参列表)
```

### 6.3 子例行子程序

Subroutine，子例行子程序。子例行子程序的名字不代表一个具体的值，与函数子程序不同，它只是提供调用时的一个识别符号，因此需要类型说明。

#### 6.3.1 子例行子程序的定义

```fortran
subroutine 子例行子程序名(形式参数表)
    程序体
end [subroutine [子例行子程序名]]
```

#### 6.3.2 子例行子程序的调用

```fortran
call 子例行子程序名(实参表)
```

注意，当子例行子程序没有形式参数时，调用方式如下：

```fortran
call 子例行子程序名
```

此时调用的子例行子程序名后面没有括号。

### 6.4 递归子程序

> Fortran 在 90 版本之前是不支持递归调用的。

小心设计！递归调用容易造成死循环。递归函数的一般形式如下：

```fortran
recursive function 函数名([形式参数表]) result(函数结果名)
    调用该函数本身
end [function [函数名]]
```

递归子程序的一般形式则是这样：

```fortran
recursive subroutine 子程序名([形式参数表])
    调用该子程序本身
end [subroutine [子程序名]]
```

## 7. 文件

### 7.1 文件的基本概念

File，就是一组相关信息的集合，给定一个标识符，并将其存放在计算机的某一个存储介质上。在对文件进行操作的时候是以记录为基本单位的。

### 7.2 什么是记录

- 有格式记录
  - 是有序的格式化数据序列，每个记录以回车符作为结束标志
- 无格式记录
  - 由二进制代码直接传输，在输入输出是无需格式转换
  - 传输速度较快，占用磁盘空间较小
- 文件结束记录
  - 是文件结束的标志，由操作系统来规定
  - 不作为数据的内容来处理

### 7.3 文件的特性

#### 7.3.1 文件标识

简单来说，就是文件所在的路径。计算机根据文件标识符来寻找文件。

#### 7.3.2 文件的存取方式

两种。

- 顺序存取
- 直接存取

顺序文件的存取操作总是从第 1 个记录开始，然后依次按文件记录的逻辑顺序逐个往下进行。如果要操作第 n 个记录，那么就要先操作第 n - 1 个记录。

直接文件的存取操作可以按任意的次序进行。可以直接按存取操作指定的记录号进行。

#### 7.3.3 文件的结构

有格式存放、无格式存放和二进制存放。除了第一种有格式存放使用 ASCII 字符存放之外，其余两种都使用二进制存放。

#### 7.3.4 文件的定位

文件指针。控制文件的读写操作。当文件指针位于第一个记录前的位置，称为「文件头」。当文件指针位于某一个记录中时，称该记录为「当前记录」。当文件指针位于最后一个记录后的位置时，称文件定位在「文件尾」。

### 7.4 文件的操作语句

#### 7.4.1 文件的打开和关闭

文件的打开语句，这样：

```fortran
open(unit = number, file = "filename", form = "...", status = "...", access = "...", recl = length,&
    err = label, iostat = var, blank = "...", position = "...",&
    action = action, pad = "...", delim = "...")
```

文件的关闭语句，这样：

```fortran
close(unit = number, err = label, iostat = var, status = "...")
```

#### 7.4.2 文件的输入输出

文件打开之后就可以进行读写操作了，也就是输入输出。输入语句格式如下：

```fortran
read(unit = number, fmt = format, nml = namelist, rec = record, iostat = stat,&
    err = errlabel, end = endlabel, advance = advance, size = size)
```

文件的输出与文件输入的格式完全相同，只不过把 `read` 改为 `write` 而已。

```fortran
write(unit = number, fmt = format, nml = namelist, rec = record, iostat = stat,&
    err = errlabel, end = endlabel, advance = advance, size = size)
```

## 8. Fortran 并行计算基础

### 8.1 MPI 使用概述

MPI 消息传递接口。多台机器同时计算时，一边计算一边传递信息。同一个程序，在每一个节点上运行一个进程。

```fortran
include 'mpif.h'
integer b, y, ierr, myid, num, status(mpi_status_size), j2
call mpi_init(ierr)
call mpi_comm_rank(mpi_comm_world, myid, ierr)
call mpi_comm_size(mpi_comm_world, num, ierr)
call mpi_bcast(dmbig, i, mpi_logical, 0, mpi_comm_world, ieer)
call mpi_barrier(mpi_comm_world, ieer)
call mpi_barrier(mpi_comm_world, ieer)
call mpi_recv(delta1, 1, mpi_double_precision, 1, 0, mpi_comm_world, status, ieer)
call mpi_send(delta, 1, mpi_double_precision, 0, 0, mpi_comm_world, ieer)
call mpi_finalize(ieer)
```

Fortran90 以上支持并行计算，子程序。

```fortran
call mpi_init(ierr)
```

初始化。

```fortran
call mpi_barrier(mpi_comm_world, ieer)
```

等所有的节点都执行到这一句之后，接下来才继续执行。

```fortran
call mpi_recv(delta1, 1, mpi_double_precision, 1, 0, mpi_comm_world, status, ieer)
call mpi_send(delta, 1, mpi_double_precision, 0, 0, mpi_comm_world, ieer)
```

向其他节点发送以及从其他节点接受消息，用于同步数据。

### 8.2 编译含有 MPI 的并行计算程序

编译，可以使用 GCC 或者 Intel 的编译器，建议使用 Intel 的 ifort。
编译前需要使用命令方式配置编译环境。配置方法：在天河计算机上执行下面指令。

```shell
source /opt/intel/Compiler/11.1/059/bin/intel64/iccvars_intel64.sh
source /opt/intel/Compiler/11.1/059/bin/intel64/ifortvars_intel64.sh
```

或者

```shell
source /vol6/appsoftware/intel/11.1/059/bin/intel64/iccvars_intel64.sh
source /vol6/appsoftware/intel/11.1/059/bin/intel64/ifortvars_intel64.sh
```

把 Fortran90 源文件上传至天河机之后，使用合适的 MPI 编译器编译。

```shell
mpif90 <源程序.f90> -o <编译完成后的程序名>
```

例如：

```shell
mpif90 expmtt.f90 -o cyrain
```

### 8.3 实际操作

「纸上得来终觉浅，绝知此事要躬行。」

#### 8.3.1 例子

对 8 * 8 的矩阵进行转置，使用四台机器。mat01.txt, mat02.txt, mat03.txt, mat04.txt 四块 4 * 4 的矩阵。下面是用于测试的源代码。

```fortran
program main
    include 'mpif.h'
    character *(3) fname
    dimension mymat (4, 4), mytmat(4, 4), mymatr(4, 4)
    integer mymat, mytmat, mymatr
    integer ierr, myid, num, status(mpi_status_size)
    call mpi_init(ierr)
    call mpi_comm_rank(mpi_comm_world, myid, ierr)
    call mpi_comm_size(mpi_comm_world, num, ierr)
    fname = 'mat'
    open(1, file = fname//char(48 + myid)//'1.txt', status = 'old')
    open(2, file = fname//char(48 + myid)//'2.txt', status = 'new')

    if (myid .eq. 0) then
        write(2, *) 'The primary process'
        write(2, *) 'myid =', myid
        write(2, *) 'number of processors =', num
    else
        write(2, *) 'An assistance process'
        write(2, *) 'myid =', myid
        write(2, *) 'number of processors =', num
    end if

    do i = 1, 4
        read(1, 100) (mymat(i, j), j = 1, 4)
    end do
    100 format(4i5)
    call mpi_barrier(mpi_comm_world, ierr)
    ! Wait for end of read data from file 1, part of the total 8*8 matrix
    if ((myid .eq. 0) .or. (myid .eq. 3)) then
        do i = 1, 4
            do j = 1, 4
                mytmat(i, j) = mymat(j, i)
            end do
        end do
    else
        if (myid .eq. 1) then
            call mpi_send(mymat, 16, mpi_integer, 2, 0, mpi_comm_world, ierr)
            call mpi_recv(mymatr, 16, mpi_integer, 2, 0, mpi_comm_world, status, ierr)
        else
            if (myid .eq. 2) then
                call mpi_send(mymat, 16, mpi_integer, 1, 0, mpi_comm_world, ierr)
                call mpi_recv(mymatr, 16, mpi_integer, 1, 0, mpi_comm_world, status, ierr)
            end if
        end if
    end if
    call mpi_barrier(mpi_comm_world, ierr)
    if ((myid .eq. 1) .or. (myid .eq. 2)) then
        do i = 1, 4
            do j = 1, 4
                mytmat(i, j) = mymatr(j, i)
            end do
        end do
    end if
    do i = 1, 4
        write(2, 100) (mytmat(i, j), j = 1, 4)
    end do
    close(1)
    close(2)
    call mpi_finalize(ierr)
end program main
```

#### 8.3.2 遇到问题

以下是在登录天河计算机时的界面展示。

```shell
[stu05@ln1%tianhe ~]$ ls
ChenChangyu   les7         result4.log  results.log   results3.log  results6.log  ?????????  ?????????  ?????????  ??????
NPB3.3.1      result1.log  result5.log  results1.log  results4.log  results7.log  ?????????  ??????     ?????????
NPB3.3.1.tgz  result3.log  result6.log  results2.log  results5.log  ?????????     ?????????  ?????????  ??????
[stu05@ln1%tianhe ~]$ cd ChenChangyu
[stu05@ln1%tianhe ChenChangyu]$ ls
MPI-Matrix  example
[stu05@ln1%tianhe ChenChangyu]$ cd example
[stu05@ln1%tianhe example]$ ls
MAT01.TXT  MAT11.TXT  MAT21.TXT  MAT31.TXT  expmtt.f90
[stu05@ln1%tianhe example]$ mpif90 expmtt.f90 -o /ChenChangyu/example/expmtt
ifort: internal error: error generating temporary file name, check disk space and permissions  (shared/driver/hostutil.c, line 853)
[stu05@ln1%tianhe example]$
```

可以看到，编译失败啦！哈哈哈哈哈哈！一开始以为权限不够，于是尝试着更改权限。

```shell
chmod 777 /example
```

现在我的 `example` 文件夹下的所有文件的权限都是 `777`，可读可写可执行，但是问题仍然没有解决。注意，更改权限是很危险的行为，一般不建议使用。
