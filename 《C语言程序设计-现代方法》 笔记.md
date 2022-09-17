# 《C语言程序设计-现代方法》 笔记

# 第1章 C语言概述

# 第2章 C语言基本概念
* %f默认输出6个小数
* 在编译时，编译器用空格替代每条注释
* 一个数字，无小数点默认为int型，有小数点默认double型
# 第3章 格式化输入/输出

## 3.1 printf

返回：输出的字符数

**%[flags\][width\][.prec\][hil\]type**

| flag                       | width或.prec                 | hil            |
| -------------------------- | ---------------------------- | -------------- |
| -：左对齐，和width同时使用 | number：最小字符数           | hh：单个字节   |
| +：在前面放+               | *：下一参数是字符数          | h：short       |
| (space)：正数留空          | .：number小数点后的位数      | l：long        |
| 0：0填充                   | .*：下一参数是小数点后的位数 | ll：long long  |
|                            |                              | L：long double |

| type    | 含义               | type | 含义            |
| ------- | ------------------ | ---- | --------------- |
| i或d    | int                | g或G | float           |
| u       | unsigned int       | a或A | 十六进制浮点    |
| o       | 八进制             | c    | char            |
| x       | 十六进制           | s    | 字符串          |
| X(大写) | 字母大写的十六进制 | p    | 指针            |
| f或F    | float, 6           | n    | 读入/写出的个数 |
| e或E    | 指数               |      |                 |



```c
printf("%9d\n", 123);		//123
printf("%.9d\n", 123);		//000000123
printf("%+9d\n", 123);		//+123
printf("%-9d\n", 123);		//123
printf("%+-9d\n", 123);		//+123
printf("%9.2f\n", 123);		//0.00,前后格式不对
printf("%9.2f\n", 123.0);	//123.00
printf("%-*.2f\n", 9, 123.0);	//9代入*的位置
printf("%-9.*f\n", len, 123.0);	//len这个变量代入*的位置
printf("%hhd\n", (char)12345);	//12345转化为十六进制为0x3039，单字节（低位）为0x39，即十进制的57
```
```c
#include <stdio.h>
#include <windows.h>    /*Sleep函数要用到*/

int main()
{
    int i;
    for (i = 0; i <= 100; i++)
    {
        printf("Percent completed: %3d%%\r", i);    /*\r表示光标换到本行行首。意味着可以覆盖*/
        Sleep(1000);    /* 里面填写毫秒数。注意Sleep大写*/
    }
    return 0;
}
```
* 显示%号，用printf("%%");
## 3.2 scanf

返回：读入的项目数
* 当scanf函数读到不符合要求的字符时，会把该字符“放回原处”，以供下次使用。看下面程序，输入1-20.3-4.0e3，输出i = 1, j = -20, x = 0.300000, y = -4000.000000。因为第一个%d要求读入一个数字，而"-"不能跟在数字后面，所以读到第一个"-"号时结束第一个%d的读取。下一个%d从刚才的"-"号开始，读到-20。小数点不是%d应该出现的东西，所以在这里第二个%d结束，得到-20。
* "放回原处"指的是，用户在键盘输入时，数据没有直接被读取，而是存放在缓冲区。然后再由scanf函数决定是否读取。
```c
scanf("%d%d%f%f", &i, &j, &x, &y);
printf("i = %d, j = %d, x = %f, y = %f", i, j, x, y);
```

**%[flag\][type\]**

| flag | 含义       | flag | 含义         |
| ---- | ---------- | ---- | ------------ |
| *    | 跳过       | l    | long, double |
| 数字 | 最大字符数 | ll   | long long    |
| hh   | char       | L    | long double  |
| h    | short      |      |              |

| type | 含义                                              | type       | 含义           |
| ---- | ------------------------------------------------- | ---------- | -------------- |
| d    | int                                               | a, e, f, g | float          |
| i    | 整数，可能为十六进制或八进制（输入时前面加0x或0） | c          | char           |
| u    | unsigned int                                      | s          | 字符串（单词） |
| o    | 八进制                                            | […]        | 所允许的字符   |
| x    | 十六进制                                          | p          | 指针           |

```c
int number;
scanf("%*d%d", &number);	//假设输入123 456
printf("%d\n", number);		//只输出456
```

```c
int num;
int i1 = scanf("%i", &num);		//输入1234
int i2 = printf("%d\n", num);	//这里输出1234
printf("%d:%d", i1, i2);		//输出1:5(i2包含换行符)
```



# 第4章 表达式

## 4.1 

## 4.2 赋值运算符

* 运算符=是右结合的，所以i = j = k = 0 等价于i = (j = (k = 0))。同时，还会带来**类型转化**的问题。

  * ```c
    int i;
    float f;
    f = i = 33.f; /*f = 33.0, i = 33 */
    ```

* |      | 说明                     | 举例                                                         |
  | ---- | ------------------------ | ------------------------------------------------------------ |
  | 左值 | 存储在计算机内存中的对象 | 变量。左值表达式表示了一块内存区域。正因此，可以用`&`取地址的表达式 |
  | 右值 |                          |                                                              |

* 以下表达式均是错误的

* ```c
  12 = i;	//错误
  i + j = 0;  //错误
  ```


## 4.3

# 第5章 选择语句
C99开始，才对布尔值进行了定义。
```c
_Bool flag; /*_Bool本质是无符号整数，不过其值只能存0或1*/
````
```c
#include <stdbool.h>
bool flag;    /*也可以用bool来定义，不过要包含stdbool.h头文件*/
## 5.1 逻辑表达式

数字越小，优先级越高。

***\*！ > 算术运算符 > 关系运算符 > && > || > 赋值运算符\****

| 优先级 |            |               | 结合方式 | 备注                                                         |
| ------ | ---------- | ------------- | -------- | ------------------------------------------------------------ |
| 1      |            |               |          |                                                              |
| 2      |            |               |          |                                                              |
| 3      |            |               |          |                                                              |
| 4      |            |               |          |                                                              |
| 6      |            |               |          |                                                              |
| 7      | 关系运算符 | <， <=，>，>= | 左结合   | i < j < k等价于(i < j) < k，即先判断括号里的真假，再和k比较，再输出真假 |
| 8      | 判等运算符 | ==，!=        | 左结合   | i < j == j < k等价于(i < j) == (j < k)                       |
| 9      |            |               |          |                                                              |
| 10     |            |               |          |                                                              |
| 11     |            |               |          |                                                              |
| 12     | 逻辑与AND  | &&            | 左结合   |                                                              |
| 13     | 逻辑或OR   | \|\|          | 左结合   |                                                              |
| 14     |            |               |          |                                                              |
| 15     |            |               |          |                                                              |
| 16     |            |               |          |                                                              |
| 17     |            |               |          |                                                              |

## 5.2 if语句

**格式：**

```c
if (表达式)
	单语句;
===================
if (表达式)
{
	单语句;
    单语句;
}
=====================
if (表达式)
	语句;
else
    语句;
=====================
if (表达式)
    语句;
else if (表达式)
    语句;
else if (表达式)
    语句;
else
    语句;
```

* 执行语句时， 先计算圆括号内表达式的值。如果表达式的值非零(C语言把非零值解释为真值）， 那么接着执行圆括号后边的语句。

* 默认if后面只执行一条语句，多语句记得用大括号！

* else与最近的且未匹配的if配对，和缩进无关。

**条件表达式**

  ```c
  if 表达式1?表达式2:表达式3
  ```

* 读作 “如果表达式l成立， 那么表达式2, 否则表达式3。”条件表达式求值的步骤是：
  * 首先计算出表达式1的值，如果此值不为零，那么计算 表达式2的值， 并且计算出来的值就是整个条件表达式的值；
  * 如果表达式1的值为零， 那么表达 式3的值是整个条件表达式的值。
* 如果表达式1是int型，表达式2是float型，那么整个表达式还是float型

```c
if (i > j) 
	return i; 
else
	return j; 
等价于return i > j ? i : J; 
============================
```

## 5.3 switch-case语句

**格式**

```c
switch (控制表达式){
    case 常量:
        语句;
        break;
    case 常量:
        语句;
        break;
    case 常量:
        ...
    default:
        语句;
        break;
}
```

* switch-case语句可以起到多个else-if语句的功能。不同之处在于

  * else-if语句要不断判断条件，直到条件符合。当条件很靠后时，会很费时间。而switch-case可以直接调到相应的case。
  * 控制表达式里面的必须为**整数**型的结果。
  * 一个case可以写多条语句，不需要用括号括起来。

* 常量可以是常数，**也可以是常数计算的表达式**

* case表示进入的路标，break表示推出switch-case的路标。如果一个case里面没有break，那么程序会继续往下执行直到遇到break。有时故意这样设计以达到多个case公用同样的表达式。见下面的代码。给不同日期加上不同的英文后缀

* ```c
  switch (day)
  {
  	case 1: case 21: case 31:
  	printf("st");break;
  	case 2: case 22:
  	printf("nd");break;
  	case 3: case 23:
  	printf("rd");break;
  	default: printf("th");break;
      }
  ```

* switch语句不要求一定有default分支。如果default不存在，而且控制表达式的值和任何一个分支标号都不匹配的话，控制会直接传给switch语句后面的语句。

# 第6章 循环

## 6.1 while和do-while语句

**格式**

```c
while (条件)
	语句;

while (条件)
{
    语句;
    语句;
}
```

* 类似的if语句，当有多条语句时，采用大括号括起来

* printf函数本身可以做到循环变量改变的特点，如下

```c
int main()
{
    int i = 10;
    while (i > 0)
    {
        printf("T minutes %d and counting.\n", i--);
    }
      
  return 0;
}
  ```
* 上述循环中，在不严格的情况下可以写成下面的形式。但考虑到更普遍的情况，当i初始值不为正数时，旧循环会直接终止，新循环会一直循环下去。
```c
while (i)
    printf("T minutes %d and counting.\n", i--);
```
* 如何做到两列整齐的输出？

  * <img src="C:\Users\lsy\AppData\Roaming\Typora\typora-user-images\image-20220220124334203.png" alt="image-20220220124334203" style="zoom:50%;" />

  * 做法是

    ```
    printf("%10d%10d\n", i, i * i);
    ```

    * %10d表示数据宽度为10，不满10前面的不显示，且数据右对齐。

  * **i++和++i的区别？**

    * i++表示先参与运算，再令i自增。++i表示先自增，再参与运算

    * ```c
      int i = 1;
      //打印时，按1 2 3 4 .。。打印
      do
      {
      	printf("%d\n", i++);
      }
      //===================================
      int i = 1;
      //打印时，按2 3 4 5.。。打印
      do
      {
      	printf("%d\n", ++i);
      }
      ```

## 6.2 for语句

**格式：**

```c
for	(表达式1;表达式2;表达式3)
    语句;
//=============
for	(表达式1;表达式2;表达式3)
{
    语句;
    语句3;
}
```

* 等价于

* ```c
  表达式1; //即初始化
  while (表达式2)
  {
      语句;
      表达式3;
  }
  ```

* 可以省略3个表达式中的部分，但此时省略的部分需要在其他地方标明。

* ```c
  i = 10；
  for	(;i > 0;--i)
  {
  	printf("%d\n",i)
  }
  //============================
  for	(i = 10;i > 0;)
  {
  	printf("%d\n",i--)
  }
  ```
```c
for (; ;)
{
    读入数据;
    if (数据的第一条测试)
        continue;
    if (数据的第二条测试)
        continue;
    if (数据的第n条测试)
        continue;
    处理数据;
}
```
* **在C99中，允许在表达式1进行声明，且可以进行多个声明。但此声明只能在for循环里使用。**看下面

* ```c
  int N = 10;
  for (sum = 0, i = 1; i <= N; i++)
  {
      sum += i;
  }
  ```

## 6.3 循环的退出

* break：用于跳出当前循环。当有多层循环时，只能跳出当前的一层。能用于switch和循环（while、do-whie和for）。

* continue：用于跳到当前循环的最末尾，但仍没有跳出循环。只能用于循环（while、do-whie和for）。
```c
n = 0;
sum = 0;
while (n < 10)
{
    scanf("%d", &i;)
    if (i == 0)
        continue;
    sum += i;
    n++;
    /*continue jumps to here*/
}
```

* goto：配合标识符使用。【标号语句为】：标识符：语句。【goto语句】：goto 标识符

* ```c
  for (d = 2; d < n; d++)
      if (n % d == 0)
          goto done;
  done:
  if (d < n)
      printf("%d is divisible by %d\n", n, d);
  else
      printf("%d is prime\n", n)
  ```

* **标识符后的语句必须和goto语句在同一个函数中。**

# 第7章 基本类型

## 7.1 整数类型

* 有符号数：C语言默认。最左边一维表示符号，0表示正数/零，1表示负数。例如有符号16位的最大值位2^15^-1

* 无符号数：需要用unsigned声明。无符号16位整型的最大值为2^16^-1

* 可用sizeof(int)等语句测试该类型的取值范围。在32和64位机器中，int为4 bytes，所以取值范围为-2^31^~2^31^-1

* long long int的取值范围是-2^63^~2^63^-1

* 类型声明的组合：

  * long/short, signed/unsigned,说明符可自由组合，不分先后顺序，如long unsigned int 等价于unsigned long int。
  * int可省略。unsigned short int 等价于 unsigned short。

* 整数常量：

  * 十进制：不能以零开头
  * 八进制：必须以零开头，如077
  * 十六进制：必须以0x开头，后面的字母大小写均可。
  * 可在后面加上U/u（无符号）或L/l（长整型）指定数据类型。UL顺序、大小无关紧要。

* 整数溢出：此时仅仅改变类型不够，还要审视其他涉及到此数据的语句，如printf中的%d

* 读写整数：

  * | 无符号整数 | %u<br />%o<br />%x | 十进制<br />八进制<br />十六进制 |
    | ---------- | ------------------ | ------------------------------- |
    | 短整型     | 前面加上h           | %hu<br />%ho<br />%hx           |
    | 长整型     | 前面加上l           |                                 |
    | 长长整型   | 前面加上ll          |                                 |

## 7.2 浮点类型

* IEEE浮点标准里，浮点数由符号、指数、小数三部分组成

| float       | 单精度     | 32位 |
| ----------- | ---------- | ---- |
| double      | 双精度     | 64位 |
| long double | 扩展双精度 |      |

* 只能在scanf函数里面使用l，如下。C99中允许在printf里面写，但视作无效

```c
double d;
scanf("lf", &d);    /* %f表示读取float类型，%lf表示读取double类型。但在显示float和double时，都用%f */
```
```c
float annwer = 17 / 13;             /*结果为1.00，因为右侧是两个整型相除，得到1。然后类型转换为浮点型，为1.00 */
float answer = 17.0 / 13.0;        /*结果为1.30769 */
float answer = 17 / (float) 13;    /*结果为1.30769*/
  
float f;
float c = 5 / 9.0 * (f - 32);         /*虽然上一行已经将f声明为浮点数，但这里的顺序是，先算括号，得到的是浮点数，然后有乘除，从左到右的顺序算，所以直接写5/9所得结果会是一个整型的0 */
float c = (f - 32) * 5 / 9;    /*等价于上一条，这里已经有浮点了，从左往右计算，后面5/9即可 */
```

## 7.3 字符类型

常见字符对应数字

| 字符 | 十进制数 |
| ---- | -------- |
| A    | 65       |
| a    | 97       |
| 0    | 48       |
| 空格 | 32       |



* 字符往往需要用单引号‘ 括起来，如’A’

* 字符常量其实是int类型，而不是char类型。所以可用对char类型的数据进行计算、比较

* ```c
  char ch;
  int i;
  
  i = 'a'; 		//i is now 97
  ch = 65; 		//ch is now 'A'
  ch = ch + 1; 	//ch is now 'B'
  ch++; 			//CH is now 'C'
  ```

  ```c
  if ('a' <= ch && ch >= 'z')
      ch = ch - 'a' + 'A'; 	//把小写字母转化成大写字母
  
  //=======相当于以下函数==============
  #inlcude <ctype.h>	//使用toupper必须含义此头文件
  ch = toupper(ch)	//当参数是小写时，返回大写。否则返回本身
  
  ```

* 有符号字符和无符号字符

  * 有符号字符：-128~127
  * 无符号字符：0~255
  * 标准C中允许用signed或unsigned来修饰 char

* ![image-20220220201831067](D:\Documents\C\images\image-20220220201831067.png)

* 使用%c来在scanf和printf中读写字符

* 当需要跳过空格时，需要写成scanf(“ %c”, ch)，注意%c前面有个空格

* getchar和putchar表示读和写单个字符。**getchar函数返回的是一个int类型**。getchar函数比scanf函数更有效率。

* ```c
  /*计算一个句子由多长*/
  int main()
  {
      int length = 0;
      char ch;
      
      printf("Input a sentence:");
      while (getchar() != '\n')
          length++;
      printf("Your message is %d character(s) long.", length);
      return 0;
  }
  ```
* getchar惯用法
```c
  while (getchar() != '\n')    /*跳过换行后的内容*/
    ;
```
```c
while ((ch = getchar()) == ' ')    /*跳过空格*/
    ;
```
* scanf函数和getchar函数最好不要混用，因为scanf函数会留下没有消耗掉的字符，比如换行符。下一次getchar读入时，就会读到这个换行符。很容易出错。
## 7.4 类型转化

**赋值转化**

* 右边的类型会变成左边的类型。浮点变整型的过程中，小数点丢失，而不是四舍五入。例如

```c
int i;
char c;
i = 843.56; 	//now i is 843
i = -843.56;	//now i is -843
i = i + c;    //c会被转化为
```

**算术转化**

* 往容量更大的数据转化。如float——double——long double, int——unsigned int——long int——unsigned long int

**强制转化**

使用**(类型名)表达式**来强制转化。如

```c
long i;
int j = 1000;
i = j * j;	//有溢出。因为j是int型，j*j还是int型，1000000大于int的范围，i被转化为int通用出问题。

i = (long)j * j; /*强制转化运算符优先级最高，把左边的j转化为long int，右边的j，赋值左边的i都变成long in */
```

## 7.5 类型定义

采用类型定义会使得编译器将新类型加入类型列表中

```c
typedef int Bool;
Bool flag //same as int flag    
```

* 要点：Bool这里的名词一般以大写开头
* 区别#define预处理。#define原型在后面。如下面
```c
#define BOOL int
```
* 类型定义的优点：

  * 更易理解。如

    * ```c
      typedef float Dollars;
      Dollars case_in, case_out;
      ```

  * 更易修改和移植。如要把float修改成double型，只需要把前面修改成type double Dollars;在代码移植过程中，前后机器的类型的取值范围不同，使用类型定义可方便修改数据类型。
* **可移植性**
不同位机器，如32位机器和16位机器对于int的取值范围是不同的。为了使移植时不出错，那么可使用typedef类型定义来快速修改。这些因实现的不同而改变 **(实现定义的, implementation-defined)** 的类型名经常以_t为后缀。如下
```c
typedef unsigned long int size_t;
typedef int wchar_t;
```
* 类型定义比宏定义功能更强大。例如，数组和指针类型是不能被定义为宏的。
# 第8章 数组

## 8.1 一维数组

* 数组声明

* ```c
  #define N 10	/* 为了避免以后更改数组的长度，可以用宏来定义 */
  int a[N]; /*前面int表示的是元素的类型，一个数组里所以元素的类型相同 */
  ```

* ​	a[i]的表达式是左值，可以当成变量一样使用。如下

* ```c
  scanf("%d\n", &a[i]); //注意&符号
  a[i + j * 10] = 0;
  ```

* **数组初始化**

```c
int a[5] = {0, 1, 2, 3, 4};
int a[10] = {0, 1, 2, 3, 4, 5}; //a[6]~a[10]均为0
int a[10] = {0}; //a[0] = 0, 其他未填默认为0。利用此特性可快速初始化所有元素为0.
int a[] = {0, 1, 2, 3, 4}; //也可以不指定数组的长度
int a[100] = {5, 1, 9, [6] = 9, 56, [12] = 2, [5] = 99} /*前面三个数是5，1，9,a[6]和a[7]分别为9和56,a[5为99]，其余为0.这样的初始化方式，不需要顺序。 */
int a[] = {[6] = 9, 56, [12] = 2} /*这样写时，数组长度为13 */
```
* 当重复初始化时，会按照原来的顺序继续下去。按从左往右，开始有4， 9， 1， 8；然后`[0]=5`使得a[0]上的4被替换成5，此时下标继续往下走，所以a[1]=7。最终数组`a[4] = {5, 7, 1, 8}`.
```c
int a[] = {4, 9, 1, 8, [0]=5, 7};
```
* **数组的复制问题!**
数组a[]和b[]不能直接用赋值符号=来复制，因为底层中数组名和指针相关。一个笨办法是遍历元素逐一复制。另一个办法是使用<string.h>中的memcpy(内存复制)函数。格式为
```c
memcpy(a, b, sizeof(a));
```
* 计算数组长度

  * ```c
    sizeof(a)；					//计算数组a所占字节数。返回一个无符号类型size_t
    sizeof(a[0])；   			//计算元素所占字节数。返回一个无符号类型size_t
    sizeof(a) / sizeof(a[0]); 	//得出长度。sizeof返回一个无符号类型size_t
    (int) (sizeof(a) / sizeof(a[0])); //强制转化为有符号整数，以免报错
    #define SIZE (int) (sizeof(a) / sizeof(a[0]))  //宏定义，以免下面的代码太难写
    ```
* **常量数组**：数组前使用const可另数组变成常量，不允许修改
* 随机函数srand和rand
```c
void srand(unsigned int seed);
```
* srand()函数就是用来设置rand()函数的种子的。根据不同的输入参数可以产生不同的种子。通常使用time函数作为srand函数的输入参数。time函数会返回1970年1月1日至今所经历的时间（以秒为单位）。在使用 rand() 函数之前，srand() 函数要先被调用，并且在整个程序中只需被调用一次。
```c
/*
 *发牌程序，关键点在于发了一个牌之后要判定这个牌有没有发过。所以引入了true和false===
 */
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <time.h>

#define NUM_SUITS 4
#define NUM_RANKS 13

int main()
{
    bool in_hand[NUM_SUITS][NUM_RANKS] = {false};
    int num_card, rank, suit;
    const char rank_code[] = {'2', '3', '4', '5', '6', '7', '8', '9', 'T', 'J', 'Q', 'K', 'A'};
    const char suit_code[] = {'d', 'c', 'h', 's'}; /*"diamond", "club", "heart", "spade"*/

    srand((unsigned) time(NULL));   /*使得每次程序运行的随机数都不一样*/

    printf("Enter number(s) of cards in hand: ");
    scanf("%d", &num_card);

    printf("Your hand: ");

    while (num_card > 0)
    {
        rank = rand() % NUM_RANKS;
        suit = rand() % NUM_SUITS;
        if (!in_hand[rank][suit])
        {
            in_hand[rank][suit] = true;
            num_card--;
            printf("%c_%c  ",rank_code[rank], suit_code[suit]);
        }
    }
    
    return 0;
}
```
* **变长数组（C99 only）**：

```c
scanf("%d", &n);
int a[n]; /*由变量n决定*/
int b[3 * n +5];    /*也可以用表达式决定*/
```

  

## 8.2 二维数组

**逗号运算符**

**格式**

```c
表达式1,表达式2
```

计算过程：分别计算处表达式1和2的值，并且**把表达式2作为整个表达式的值**。

* 二维数组的声明：

  ```c
  int a[m][n]; //该声明产生m行n列的二维数组
  ```

* 访问第m行n列的元素时，写成a\[m][n]的形式，不能写成a[m, n]（不要和数学上的混淆）,否则方括号里面的会被当成逗号运算符，a[m, n] == a[n]。

* 二维数组在内存中是按行排列的，即先排完第1行的内容，再排第二行的内容。

* 二维数组的初始化

* ```c
  int m[5][9] = {{1,2,3,4,5,6,7,8,9},{2,3,4,5,6,7,8,9,10}} /*即大括号里面有小括号，小括号括住的是一行的内容。其他初始方法和一维数组类似 */
  ```

# 第9章 函数

## 9.1 函数的定义和调用

**格式**

* double是函数返回的类型。不带返回值时，前面为void。当返回类型很长，如unsigned long int时，返回来行可单独成一行。如

  * ```c
    unsigned long int
    average(double a, double b)
    ```

* 函数名为average

* a和b为形参（parameter），每一个形参都需要**单独说明类型**。没有时写void。

* { }括起来的部分为函数体

* **函数只能返回一个值**

```c
double average(double a, double b)
{
	return (a + b) / 2;
}
```

* 当调用函数时，需要写出函数名及跟随其后的实际参数(argument)列表。如average(x,y)的效果就是把变量x和y的值复制给形式参数a和b。实际参数不一定是变量，如average(3, 5)
* return 0 时，表示没有错误；return 1时，表示有错误
```c
int line;
printf("How many lines?\n");
scanf("%d", &line);
if (line < 1)
{
    printf("Sorry, that makes no sense.\n");
    return 1;    /*若用户输入一个非正整数，提示报错*/
}
```

* 问：不使用函数原型，直接把函数定义都放在main函数前面可以吗？
* 答：当只有main函数调用其他函数时可以。但其他函数有相互调用的情况时，要斟酌其他函数的顺序。否则会出现函数未定义的情况。所以最稳妥的方法是先放函数原型，在main函数之后放函数定义。
* 问：函数声明中，指定一维数组的形式参数的长度，会怎么样？
* 答：编译器会忽略长度值。例如在`int product(int v[3], int w[3])`中，虽然程序编写者想提示应该传长度为3的数组进函数，**实际可以传任意长度的数组**。
## 9.2 函数原型（functional prototype）声明

**格式**

```c
返回类型 函数名(形式参数)；
double average(double a, double b);
double average(double, double);			//也可行，
```

C语言要求在调用函数时编译器已经知道函数，所有有两种方法

1. 把函数定义放在main函数前面；
2. main函数前面写函数声明，main函数之后再写函数定义。

* | 形式参数（parameter） | 出现在函数定义中       |
  | --------------------- | ---------------------- |
  | 实际参数（argument）  | 出现在函数调用的表达式 |
* 参数类型的转换：按照实际参数来。如调用时用了double，形参是int，那么按int来。
```c
void square(int x);

int main()
{
    double x = 2.5;
    square(x);    /*输出4，而不是6.25*/
    return 0;
}

void square(int x)
{
    printf("square(x) = %d\n", x * x);
}
```

## 9.3 数组型实际参数

数组经常被当作实际参数，当形式参数是一维数组时，可以（实际经常这么做）不说明数组的长度。如果需要知道数组的长度，必须传入相关的参数。

```c
int f(int a[]) /* no length specified */
{
    ...
}
/* ========================================= */
int f(int[], int) /*更简便的写法，省略形式参数的名 */
{
    ...
}
/* ========================================= */
int sum(int a[][LEN], int n); /*当形参为二维数组时，列的数量必须要表明*/
```

```c
int sum_array(int a[], int n); /* 该函数声明了一个可以求数组所有函数的和的函数，第一个形参为数组，第二个形参为数组的长度 */
total = sum_array(a, n); /* 实际参数只需要写名，不能写成a[] */
```
* 因为数组当形参时，是用指针来传递的，不是通过复制形参作为实参的，所以调用函数之后可以改变形参中的数组。在下面的例子中，传入的数组a[]的元素被置为0了。
```c
void store_zero(int a[], int n)
{
    int i;
    for (i = 0; i < n; i++)
        a[i] = 0;
}

store_zero(b, 100);    /*数组b被改变*/
```
* 可变长度数组作形参

```c
int sum_array(int n, int a[n])
{
    ...
}
int sum_array(int n, int m, int a[n][m]) /*二维变长数组*/
{
    ...
}
  
/* 声明时可以按以下声明 */
int sum_array(int n, int a[n])；
int sum_array(int n, int a[*])；	//用*代替长度 */
int sum_array(int, int [*])；
int sum_array(int n, int a[])	//函数中空
int sum_array(int, int [])； 
```
```c
int sum_array(int a[static 3], int n) //表面数组a的长度至少是3
{
    ...
}
```
* 如果数组是多维的，static仅可用于第一维（行数）
* **数组中的复合字面量**

  **格式**

  (给定类型){元素的值}

  ```c
  /* b作为一个变量声明，需要在调用前进行初始化。如果b不作它用，比较浪费。*/
  int b[] = {3, 0, 3, 4, 1};
  total = sum_array(b, 5);
  
  total = sum_array((int []){3, 0, 3, 4, 1}, 5); /*这就是复合字面量*/
  total = sum_array((int []) {2*i, i+j, j*k}, 3);    /*复合字面量可以包含表达式*/
  ```

## 9.4 return和exit语句

**格式**

```c
return 表达式;
```

* 当return语句的表达式和函数返回类型不匹配时，会被强制转化成函数的返回类型。
* return可以用在void类型的函数中，非必需。
```c
return;
```

**main函数**

* main函数返回的是**状态码**，return 0;程序正常终止；为了表示异常终止，可以返回其他值。

**exit函数**

* exit函数也可用于终止程序。

  ```c
  #include <stdlib.h>		//EXTIT_SUCCESS和EXIT_FAILURE定义在此宏中，使用时要加上
  
  exit(0);				/*0表示正常终止*/
  exit(EXTIT_SUCCESS);	/*等效于exit(0)*/
  exit(EXIT_FAILURE);		/*程序异常终止，等效于exit(0)*/
  ```

  | 函数   | 说明                               |
  | ------ | ---------------------------------- |
  | return | 仅当main函数调用时才会导致程序终止 |
  | exit   | 任何函数调用都会导致程序终止       |

## 9.5 递归

函数可以调用它本身

```c
/* 利用n! == n * (n-1)!实现阶乘 */
int fact(int n)
{
    if (n <= 1)    /*一定要设置边界*/
        return 1;
    else
        return n * fact(n-1);
}
```
```c
int fact(int n)
{
    return n <= 1 ? 1 : n * fact(n-1);    /*可精简成这样*/
}
```
* 运用分治法(divide-and-conquer)实现快速排序。快速排序的步骤为：
1. 选定一个参照元素ref;
2. 将数组中小于ref的数放在左边；
3. 将数组中大于ref的数放在右边。
* 进行一次排序后，得到 | . . . | ref | . . . |左右两个小区间。左边区间的数都比ref小，右边都比ref大。然后运用递归的思想对小区间进行快速排序。程序实现如下
```c
void quicksort(int a[], int left, int right)    /*left和right表示区间端点*/
{
    if (left < right)    /*以免出bug*/
    {
        int i = left, j = right, ref = a[left];    /*i和j是游离的下标，是用来扫描元素和ref比较的*/
        while (i < j)
        {
            while (i < j && a[j] >= ref)
                j--;    /*从右往左，找到a[j] < ref的下标j，即需要调换区间的那个元素的下标*/
            if (i < j)
                a[i++] = a[j];    /*将a[j]调换到a[i]，然后i++使i右移*/
            while (i < j && a[i] <= ref)
                i++;
            if (i < j)
                a[j--] = a[i];
        }
        a[i] = ref;    /*此时i == j,所以把ref放到a[i]这个位置。第一次分出了两个小区间。下面是利用递归对小区间排序*/
        quicksort(a, left, i - 1);
        quicksort(a, i + 1, right);
    }
}
```

# 第10章 程序结构

## 10.1 局部变量

**定义**：函数体内声明的变量称为该函数的局部变量。

**自动存储期限（storage duration）**：局部变量的存储单元在函数被调用时“自动”分配，在函数返回时收回分配，。

* **static**关键字的含义：
1. 用于隐藏。用static定义的变量和函数，只能在它所在的文件/函数内被访问。在同时编译多个文件时，未加static的全局变量和函数具有全局可见性；
2. 只初始化一次；
3. 未手动初始化时，默认初始化为0；自动变量位于栈区，加了static后就到了“静态数据区”，这里的所有字节默认为0x00；
```C
/*文件a.c*/
char a = 'A'; // global variable
void msg()
{
    printf("Hello\n");
}
```
```c
/*main.c*/
int main(void)
{    
    extern char a;    // extern variable must be declared before use
    printf("%c ", a);
    (void)msg();
    return 0;
}
```
程序运行结果：`A Hello`因为未加static所以a.c中的变量和函数能在main.c中被使用

```c
#include <stdio.h>

int fun(void){
    static int count = 10;
    return count--;
}

int count = 1;

int main(void)
{    
    printf("global\t\tlocal static\n");
    for(; count <= 10; ++count)
        printf("%d\t\t%d\n", count, fun());    
   
    return 0;
}
```
上述程序运行结果：因为只初始化一次，fun()不会一直返回10
```c
global          local static
1               10
2               9
3               8
4               7
5               6
6               5
7               4
8               3
9               2
10              1
```
**块作用域**：所以其他函数可以把同名变量用于别的用途。

**静态局部变量**：在变量声明中使用static，可以使变量具有**静态存储期限**而不是自动存储期限。拥有静态存储期限的变量获得永久的存储单元，不会丢失其值。但身为局部变量，有块作用域，静态局部变量仍不能被其他函数可见。


* 形式参数和局部变量很相似，区别是每次函数调用形式参数时，会对其初始化。

<img src="D:\Documents\C\images\image-20220224211643247.png" alt="image-20220224211643247" style="zoom:50%;" />

```c
f();	//打印1和3
f();	//打印3和5
f();	//打印5和7

int f()
{
    static int all = 1;
    printf("%d\n", all);
    all += 2;
    printf("%d\n", all);
    return all;
}
```

* <img src="D:\Documents\C\images\image-20220224212230962.png" alt="image-20220224212230962" style="zoom:50%;" />

## 10.2 全局变量

全局变量又称为外部变量。全局变量的特点

**静态存储期限**：其中的值永久被保留

**文件作用域**：从变量被声明的点到文件末尾，所有函数都可访问（并修改）它。

**初始化**：只能用常量来初始化，不能用一个变量来初始化。

**注意**：如果函数内有和全局变量同名的局部变量，那么全局变量会被隐藏。离开该函数，全局变量就恢复。

## 10.3 程序结构

```c
#include指令
#define指令
类型定义
外部变量的声明
除main函数之外的函数原型
main函数的定义
其他函数的定义
```

# 第11章 指针
## 11.1 指针类型
指针的类型和指针所指向的类型很明显是不一样的东西，但好多情况下却容易忽视它们的区别。
* 指针的类型是指针自身的类型；
* 指针所指向的类型是指针指向的数据（内存）的类型。
从语法上看，声明中去掉指针名字，剩下的部分就算指针类型。
```c
int *ptr;         //指针的类型是 int*
char *ptr;        //指针的类型是 char*
int **ptr;        //指针的类型是 int**
int (*ptr)[3];    //指针的类型是 int(*)[3]
int *(*ptr)[4];   //指针的类型是 int*(*)[4]
```
从语法上看，去掉*号和指针名字，剩下的就算指针所指的类型。
```c
int *ptr;         //指针所指向的类型是 int
char *ptr;        //指针所指向的的类型是 char
int **ptr;        //指针所指向的的类型是 int*
int (*ptr)[3];    //指针所指向的的类型是 int()[3]
int *(*ptr)[4];   //指针所指向的的类型是 int*()[4]
```
```c
int *p[3];    /*定义一个指针数组，即数组元素都是指针*/
for (int i = 0; i < 3; i++)
{
    p[i] = i;    /* 发生强制转换，由int类型转换为int*类型。编译报warning */
}
```
## 11.2 指针变量

&：取得变量的的地址，操作的对象必须是变量。下面的表达是错误的

```c
&(a++);
&(--a);
```

指针变量：专门用于存放地址的变量。如果用其它变量存放（比如int a），可能会因为强制转化把地址丢失掉。

| 指针变量的值 | 具有实际值的变量的地址 |
| ------------ | ---------------------- |
| 普通变量的值 | 实际的值               |

**注意**：

* 指针即地址
* *和&是一对互为相反的操作
* 不是说加了*才是指针，正常命名如p也可以是指针
* 在声明时，必须加*表示后面的p是指针
* 输入出书时，根据地址/指针用%p，指针指代的变量用其他如%d等。
* 建议声明时就令*p为零。

```c
#include <stdio.h>

void f(int *p);

int main()
{
    int i = 100;
    printf("&i = %p\n", &i); /* 注意此处为%p */
    f(&i);

    return 0;
}

void f(int *p)
{
    printf("p = %p\n", p);
    printf("*p = %d\n", *p); /* 注意此处为%d */
}
```

```c
/* 以上程序输出结果 */
&i = 000000000061FE1C
p = 000000000061FE1C
*p = 100
```

*是一个单目运算，用来访问指针的值所表示的地址上的**变量**。可以作左值也可以作右值。

```c
int k = *p;
*p = k + 1;
```

指针应用场景一：交换两个值 

```c
#include <stdio.h>

void swap(int *pa, int *pb); /*参数是变量*/

int main()
{
    int a = 5, b = 6;
    swap(&a, &b);     /*用地址来达到交换的目的*/
    printf("a = %d, b = %d", a, b);
    return 0;
}

void swap(int *pa, int *pb)
{
    int t = *pa;
    *pa = *pb;
    *pb = t;
}
```

指针应用场景二：返回多个值

```c
#include <stdio.h>

void minmax(int a[], int len, int *max, int *min);

int main()
{
    int a[] = {5, 4, 0, -9, 100, 66, 7};
    int min, max;
    minmax(a, sizeof(a)/sizeof(a[0]), &min, &max);
    printf("min = %d, max = %d", min, max);
    return 0;
}

void minmax(int a[], int len, int *max, int *min)
{
    int i;
    *min = *max = a[0];
    for (i = 0; i < len; i++)
    {
        if (a[i] < *min)
            *min = a[i];
        if (a[i] > *max)
            *max = a[i];
    }
}
```

指针应用场景二b：函数返回运算的结果，结果通过指针返回

```c
/* 如果除法成功则返回1，否则返回0 */

#include <stdio.h>

int divide(int a, int b, int *result);

int main()
{
    int a = 5;
    int b = 3;
    int c;
    if (divide(a, b, &c)) //1为真，则能进行除法
        printf("%d/%d=%d",a,b,c);
    return 0;
}

int divide(int a, int b, int *result)
{
    int ret = 1;
    if (b == 0) ret = 0;
    else *result = a/b;
    return ret;
}
```

**注意：定义了指针，在没有指向任何变量前，不要对齐进行赋值。否则可能会把内存中不知名的区域数据进行了更改。**

## 11.3 指针作为返回值

```c
/* 返回最大值的地址 */
int *max(int *a, int *b) /* 函数名前面加*表示这是一个返回的指针的指针型函数*/
{
    if (*a > *b)
        return a;
    else
        return b;
}
```

注：不要返回执行自动局部变量的指针，如

```c
int *f(void)
{
	int i;
	...
	return &i
}
```



# 第12章 指针和数组

* 指向数组的指针相当于指向数组的第一个元素。

在函数参数表中，数组即指针。以下四个表达式在参数表中是等效的

```c
int sum(int *ar, int n);
int sum(int *, int);
int sum(int ar[], int n);
int sum(int [], int);
```

```c
int a[10], *p = a;
//p[i]这样的写法没有问题，因为指针可以用作数组名
```

由于在函数参数中的数组是以指针存在的，所有在函数中用sizeof(a)==sizeof(int *)，即不能用sizeof(a)/sizeof(a[0])来计算数组长度。

```c
int a[10];
int *p = a; /*无需用&取地址*/
a == &a[0]; /* 数组的单元表达的是变量，需要用&取地址 */
p[0] == a[0]; /* []可以对数组做，也可以对指针做 */
*a = 25; /*运算符可以对指针做，也可以对数组做 */
```

数组变量是const的指针，所有不能被赋值。如下

```c
int b[]; /*等价于int * const b*/
b = a； /*错误，因为b为const，不能被赋值 */
```

##   12.1 const的问题

* 指针是const：表示一旦得到了某个变量的地址，不能再指向其他变量

  ```c
  int * const q = &i;	//q是const
  *q = 26; 			//OK
  q++;				//ERROR
  ```

* 所指是const：不能通过这个指针修改那个变量。变量可以变，指针可以变，但不能修改.

  ```c
  const int *p = &i;
  *p = 26; //ERROR
  i = 26; //OK
  p = &j; //OK
  ```

  ```
  int i;
  const int* p1 = &i; /*const修饰的是int变量，即所指不能修改，但指针p1可以修改*/
  int const* p2 = &i; /*同上*/
  int *const p3 = &i; /*const修饰的是常量指针p3，指针不可修改，所指可以修改*/
  ```
  例子1：
```c
int a = 10;
int b = 20;
const int *c = &a;    //const修饰的是int，也即是*c的值不可变，但c指针可变
//int const *d = &a;  //同上句代码作用等同
//*c = 20;            //取消注释此句会报错，因为*c的内容不可变
c = &b;               //可行的
```
例子2：
```c
int a = 10;
int b = 20;
int *const c = &a;    //const修饰的是指针c，所以c是常量指针，但存储的地址所指向的内容可变
//c = &b;             //取消注释此句会报错，因为c是常量指针
*c = 30;
```
例子3：
```c
void func(const int *p)
{
    int j;
    
    *p = 0;    /*非法的*/
    p = &j;    /*合法的*/
}

void func1(const int * const p)    /*所指和指针均需用const保护*/
{
    int j;
    *p = 0;    /*非法的*/
    p = &j;    /*非法的*/
}
```
* 数组是const：用来保护数组不被破坏

```c
int sum(const int a[], int length)
```

## 12.2 指针运算

C语言只支持3种指针运算

* 指针加上整数
* 指针减去整数
* 两个指针相减（两个指针指向同一个数组时才有意义）

指针+1的结果取决于指针的类型。

```c
sizeof(char) = 1;
sizeof(int) = 4;
```

```c
char ac[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
char *p = ac;
printf("p   = %p\n", p);
printf("p+1 = %p\n", p+1);

int ai[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
int *q = ai;
printf("q   = %p\n", q);
printf("q+1 = %p\n", q+1);
```

```c
/* 输出结果 */
p   = 000000000061FE06
p+1 = 000000000061FE07
q   = 000000000061FDD0
q+1 = 000000000061FDD4
```

```c
/* 承接上述程序 */
*(p+1) ==>ac[1];
*(p+n) ==>ac[n];
*(q+n) ==>ai[n];
*q + 1; //该运算往往没有实际意义，*q+1=000000000061FDD1,没有意义
```

* *p++
* <img src="D:\Documents\C\images\image-20220223164740816.png" alt="image-20220223164740816" style="zoom:50%;" />

```c
char ac[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9, -1};
char *p = ac;
    
for (p = ac; *p != -1; p++)	/* 中间表达式也可写成p<&ac[N],N为数组长度。虽然不存在a[N],但对它取地址是合法的。 中间也可写成p< ac +N */
    printf("%d\n", *p);

for (p = ac; *p != -1; p)	//上面的替换写法
    printf("%d\n", *p++);

while (*p != -1)			//上面的替换写法
    printf("%d\n", *p++);
```

* NULL：零地址
* <img src="D:\Documents\C\images\image-20220223165918539.png" alt="image-20220223165918539" style="zoom:50%;" />

* 无论指向什么类型，所有的指针大小都是一样的，因为都是地址；但指向不同类型的指针不能相互赋值，例如上面的例子中，另char的p指针和int的q指针相等，那么另*q = 0的时候，char的数组的连续4个元素都要变成0
* 指针的类型转换：<img src="D:\Documents\C\images\image-20220223170807461.png" alt="image-20220223170807461" style="zoom:50%;" />

* ```c
  int a[N], *P;
  for (p = a; p < a + N; p++)
      scanf("%d", p)			/* p本身是地址了，不必用&p */
  ```

  

 

## 12.3 动态内存分配

**malloc**：分配内存空间（byte为单位），与free搭配使用

```c
#include <stdlib.h>
void* malloc(size_t size);
```
```c
int *x;
x = malloc(sizeof(int));    /*申请内存*/
*x = 42;                    /*往内存写入东西，注意前面的星号*/
```
* 使用时要添加头文件
* 返回的是void* 需要类型转换，如(int*)(n\*sizeof(int))

**free()**：把申请来的空间还给“系统”，只能还申请得来空间的首地址。（如p是申请来的，p++后，free(p)是不允许的。

## 12.4 指针和多维数组

* 原则上可把二维数组当成一维数组来处理。如用for循环把所有元素置0，不需使用两层for，使用一层也可以。

  <img src="D:\Documents\C\images\image-20220223183340652.png" alt="image-20220223183340652" style="zoom:67%;" />
  
  对于二维数组而言，p[i]表示第i行的首地址，p[i]+j表示第i行地点j列的元素。

```c
int a[row][col], *p; //假设有row行col列的二维数组
for (p = &a[0][0]; p <= &a[row-1][col-1]; p++) /* 中间不能用p < &a[row][col] */
    *p = 0;
```

* 处理多维数组的行

  ```c
  p = &a[i][0];	//p为指针
  p = a[i];		//上述程序的简写
  /*推导：因为对任意数组而言a[i]=*(a+i),故&a[i][0]=&(*(a[i]+0))=&*a[i]=a[i]。注意*和&是一对互为相反的操作 */
  
  ```

* 处理多维数组的列

  ```c
  /* 把row行col列的数组的第i列*/
  int a[row][col], (*p)[col], i; /* 把p声明成长度为col的整型数组的指针。*p是需要括号的，如果没有括号，编译器会把p认作指针数组，而不是指向数组的指针。p++表示移到下一行（而不是下一个元素）*/
  for (p = &a[0]; p < &a[row]; p++)
      (*p)[i] = 0;	/* *p表示a的一整行,(*p)[i]表示选中该行的第i列元素。括号不可少 */
  ```

在初始化二维数组时，需要把二维数组的列说明

```c
char a[][10] = {"hello", "world"};
```

二维数组指针举例：

```c
#include <stdio.h>
int main()
{
    int a[3][4], *p;
    int i, j;
    p = a[0];	/*等价于p = &a[0][0]，但不能写成 p = &a[0],因为此时不是一个一维数组。*/
    for (i = 0; i < 3; i++)
        for (j = 0; j < 4; j++)
            scanf("%d", p++);
    p = a[0];	
    for (i = 0; i < 3; i++)
    {
        for (j = 0; j < 4; j++)
            printf("%4d", *p++);
        printf("\n");
    }
    return 0;
}
```
## 12.5 指针数组
意义：一个数组，里面存放的全是指针
**格式**：类型说明符 * 数组名[数组长度]
如： char *str[4]；
注意：
```c
int *ptr[5]; /*指针数组*/
int (*ptr)[5];/*一个指针变量，指向数组*/
```


# 第13章 字符串

## 13.1 基础知识

**字符串常量**：以双引号“”括起来的序列，C语言中称为字符串字面量。以 ‘\0’为结束的字符数组。

[^1]: 八进制转义序列，在3个数字之后结束。如“\1234”包含\123和4，“\189”包含\1，8和9。十六进制转义序列则到第一个非十六进制数字截止。
[^2]: 字符串常量和字符常量不同。字符串常量“a”使用指针来表示的，字符常量‘a’使用整数来表示的。

```c
char ch;
ch = "abc"[1]; //ch的值为b
char digit_to_hex_char(int digit)
{
    return "0123456789ABCDEF"[digit];
} /* 返回0-15对应的十六进制字符形式 */
```

惯用法:

```c
#define STR_LEN 80
...
char str[STR_LEN+1];
```
```c
int main()
{
    int a;
    char *s = "Hello World!";
    printf("%s\n", &s[6]);    /*打印处World,因为要填入一个字符串，这里后面有'\0',而且是一个地址*/
    printf("%c\n", *s);    /*打印，因为s是首地址，即H*/    
return 0;
}
```

**字符串变量**：

由于字符串以数组的形式存在，所以下方传递



* 在计算机底层中，数组又是和指针（地址）息息相关所以，若要比较两个字符串是否相同，如果用if (s1 == s2)，比较的是两个地址。
```c
#include <stdio.h>

int main()
{
    char s1[10], s2[10];
    printf("input the first string: ");
    scanf("%s", s1);
    printf("input the second string: ");
    scanf("%s", s2);

    if (s1 == s2)    /*这里比较的是两个地址，所以输出一样的内容还是会直跳到else处*/
    {
        printf("string1 == string2\n");
        printf("%s\n", s1);
        printf("%s\n", s2);
    }
    else
    {
        printf("string1 != string2\n");
        printf("the address of %s is %p\n", s1, s1);
        printf("the address of %s is %p\n", s2, s2);
    }   
    return 0;
}
```

* 以0（整数0）(又称空字符)结束的一串字符。注意，0和‘\0’是一样的，和‘0’不一样，后者是ASCIII码的字符0。
```c
char *s1;
scanf("%s", s1);
char *s2 = malloc((strlen(s1) + 1) * sizeof(char));    /*考虑'\0'*/
int n = sizeof(strlen(s1));
for (int i = 0; i < n; i++)
    s2[i] = s1[i];
s2[n] = '\0';    /*需要手动在后面加上'\0'*/
```
* 0标志着字符串的结束，但不是字符串的一部分。计算字符串长度的时候不包含0。
* 字符串以数组的形式存在，以数组或指针的形式访问。更多是以指针的形式。
* string.h里面有很多处理字符串的函数
* 用双引号引出来的脚字符串字面量（字符串常量），可以用来初始化字符数组。


```c
char *str = "hello";
char word[] = "hello";	/*编译器会自动在后面加0*/
char word[10] = "hello";/*编译器会自动在后面加0*/
printf("hello""world");/* 两个双引号之间没有逗号是可以的，此时编译器会把两个字符串拼成一个字符串*/
printf("hello,\
       world"); /*打印出来会带有一个table*/
printf("hello,\
world"); /*打印出来没有table*/
```

```c
printf("%s", __func__);//返回函数名，两个下横线
```

字符串常量：

* **如果要修改子字符串，应该用数组形式声明**。如s3[]
  * 数组：这个字符串在这里，作为本地变量被回收
  * 指针：这个字符串不知道在哪里，可用来处理参数和动态分配空间

```c
int i;
char *s = "hello";
char *s2 = "hello";
char s3[] = "hello";
printf("&i = %p\n", &i); 	/* &i = 000000000061FE0C */
printf("s  = %p\n", s); 	/* s  = 0000000000404000 */
printf("s2 = %p\n", s2); 	/* s2 = 0000000000404000 */
s3[0] = 'H'; /* 注意这里是单引号*/
printf("s3 = %p\n", s3);	/* s3 = 000000000061FE06 */
printf("here, s3[0] = %c\n", s3[0]); /* here, s3[0] = B */
```

* 两个地址一样，是因为char *s 相当于 const char *s，由于历史原因，编译器接受不带const的写法。

* putchar和getchar的使用
  * getchar读入一个字符并将其作为int的类型返回，为了保存这个字符，必须使用赋值操作。

```c
int ch;
ch = getchar();
```

```c
while (getchar() != '\n')
    ;

while ((ch = getchar() ==  ' '))
    ;
```



## 13.2 字符串的输入输出
字符串赋值：
```c
char *t = "title";
char *s;
s = t;
/* 以上只是指针赋值，不是字符串赋值*/
```

字符串上输入输出：

```c
char ch[] = "abcdefg";
printf("%.3s\n", ch);	//只显示前3位
printf("%3s\n", ch);	//显示完
printf("%10s\n", ch);	//10个宽度，右对齐
printf("%-10s\n", ch);	//10个宽度，，左对齐
```

gets和scanf的区别：前者遇到空格、table或换行均会结束；后者只会在换行符结束。

```c
char string[8];
scanf("%s", string); /*数组名即指针*/
scnaf("%7s",string); /* 最多允许写入7个字符*/
printf("%.3s", string); /* 只显示前3个*/
```

* scanf意味着读入一个单词，到空格、tab或回车位置。scanf是不安全的，因为不知道读入的内容的长度。
* 当输入的字符长度超过数组长度时，数组出错。所以要在canf里面规定长度如%7s。里面的数字应该比数组的大小小1。

注意点：

```c
char buffer[100] = "";	/* 空字符串，bufer[0]='\0' */
char buffer[] = "";		/* 数组的长度只有1 */
```

## 14.3 字符串数组

* char **a。a是一个指针，指向另一个指针，那个指针指向一个字符串。

* char *a[]: 是一个数组，里面元素存放的是指针，每一个指针指定了一个字符串

  * ```c
    char *a[] = {
    	"hello",
    	"world",
    	"!"
    };
    ```

程序参数：

```c
int main(int argc, int const *argv[])
{
}

int main(int argc, int const **argv)    /*和上面的等价*/
{
}

// argv[0]是命令本身，后面是命令后的字符串
```

```c
int main(int argc, char const *argv[])
{
    for (int i = 0; i < argc; i++) {
        printf("%d: %s\n", i, argv[i]);
    }
    return 0;
}
```

<img src="D:\Documents\C\images\image-20220301224256735.png" alt="image-20220301224256735" style="zoom:50%;" />



## 13.4 访问字符串的字符

* **用指针访问更方便**

```c
int count_spaces(const char s[])	/* 统计 有多少个空格*/
{
    int count = 0;
    for (int i = 0; s[i] != ' '; i++)	/* */
    {
        count++;
    }
    return count;
}
/* ================================================== */
int count_spaces(const char *s)	/* 用指针访问更方便。*/
{
    int count = 0;
    for (;*s != '\0'; s++)		/* 可减少变量i */
    {
        if (*s == ' ')
            count++;
    }
    return count;
}
```

* 在第二个例子中，const并没有阻止count_spaces的修改，而是阻止函数对s指向的字符的修改。因为s是传递给count_spaces的指针的副本，自增对原始指针不会有影响。

## 13.5 使用C语言的字符串库

先看一些错误范例:

* 把数组名用作=的左操作数是违法的，使用=初始化字符串数组是合法的

```c
char str1[10], str2[10];
str2 = str1;				/* WRONG */
str2 = str1 = "abc";
if (str1 == str2);			/* 比较是是指针,不是数组的内容 WRONG */ 
```

使用字符串函数时，应该在前面包含**#include <string.h>**

**strcpy函数**：

```c
char *strcpy(char *restrict *s1, const char *s2);	/* 函数原型,由此可知，是复制的是指针，不是数组本身。其中restrict表示不会重叠*/
/* 返回的是s1这个指针 */
```

<img src="D:\Documents\C\images\image-20220302172718073.png" alt="image-20220302172718073" style="zoom:50%;" />

```c
strcpy(str2, "abcd");
strcpy(str2, str1);
```

常用套路：

```c
char *dst = (char *)malloc(strlen(src) + 1);	//分配空间，不要忘了+1
strcpy(dst, src);
```

```c
while (src[idx] != '\0'){}

while (src[idx]){}	/* 和上面等价*/
```

**strlen函数**：返回字符串长度，不包含末尾的‘\0’。

**strcat函数**：拼接两个函数，并且返回拼接后的指针。

```c
strcpy(str1, "abc")；
strcat(str1, "def");	//str1 = abcdef
strcat(str2, str1);
strcat(str1, strcat(st2, "ghi"))	//利用返回值进行计算
```

**strcmp函数**：比较两个字符串的大小，根据两个字符串的大小返回一个大于、小于或等于0的值

```c
int strcmp(const char *s1, const char *s2);	/*函数原型*/
```

s1小于s2的情况：

* 前i个字符相同，第i+1个小于s2；
* s1短于s2；

[^1]: 大写字母都小于小写字母。69是A，97是a
[^2]: 数字小于字母
[^3]: 空格小于所有可打印字符（ASCII的值为32）

# 第14章 预处理器

## 14.1 预处理器：

用来把预处理指令替换成相应的代码，把宏替换，把注释去掉

## 14.2 预处理指令

* 都以#开头，并不需要在行首，前面有空格就行
* 都已换行符结束，除非有\标明没结束
* 符号间可以插入空格或制表符。如 #  define    N     100
* 注释和指令可以放同一行
* #pragma para: 对编译器进行参数化设置,其中para为参数。如进行字节对齐等。

**#include**：是一个预处理指令，和宏一样，编译之前就处理了。把文件的全部文本内容**原封不动**地插入到它所在的地方。

## 14.3 宏

用#定义的语句，不需要用分号结束，因为这些句子不是C的句子。

```c
#define PI 3.14159		//无分号
#define FORMAT "%f\n"	//完全的文本替换也是可行的
printf(FORMAT, 2*PI);
#define PRT printf("%f\n", 2 * PI); \
			printf("%f\n", 2 * PI)			//这里没有分号
```

* 如果一个宏超过一行，那么最后一行前的行末需要加反斜杠\

* 注意，空格也会被当做宏，所以一行后不要带多余的空格

**预先定义的宏**：可用来检测错误或调试

```c
__LINE__	//返回行号
__FILE__	//返回文件名
__DATE__	//返回日期（mm dd yyyy）
__TIME__	//返回时间（hh:mm:ss)
__STDC__	//如果编译器符合C标准，则返回1
__func__	//返回所在函数名
```

**带参数的宏**

```c
#define cube(x) ((x)*(x)*(x))
printf("%d\n",cube(5));

#define MIN(a, b) ((a)>(b)?(b):(a)) /* 宏可以带多个参数 */

```

编写原则：

* 整个值要有一个括号；
* 参数出现的地方要用括号。

反例：

```c
#define cube(x) (x*x*x)
cube(3+2) == (3+2*3+2*3+2) //运算不对
```

## 14.4 条件编译

* #if语句
```c
#if 整型常量表达式1    /*注意这里是常量表达式，不能是变量*/
    程序段1
#elif 整型常量表达式2    /*#elif可省，如下*/
    程序段2
#elif 整型常量表达式3
    程序段3
#else
    程序段4
#endif
```
```c
#include <stdio.h>
int main(){
    #if _WIN32
        printf("This is Windows!\n");    /*识别出这里是windows系统*/
    #else
        printf("Unknown platform!\n");
    #endif
   
    #if __linux__
        printf("This is Linux!\n");
    #endif

    return 0;
}
```
* #ifdef （#ifndef同理）
```c
#ifdef  宏名
    程序段1
#else
    程序段2
#endif
```
```c
#include <stdio.h>
#include <stdlib.h>
int main(){
    #ifdef _DEBUG
        printf("正在使用 Debug 模式编译程序...\n");    
    #else
        printf("正在使用 Release 模式编译程序...\n");
    #endif
    system("pause");
    return 0;
}
```
* VS/VC 有两种编译模式，Debug 和 Release。在学习过程中，我们通常使用 Debug 模式，这样便于程序的调试；而最终发布的程序，要使用 Release 模式，这样编译器会进行很多优化，提高程序运行效率，删除冗余信息。
# 第15章 编写大型程序

* 新建一个C项目，而不是新建文件
* 头文件：把**函数原型（不是函数定义）**放到一个头文件（.h）中，在需要调用这个函数的源代码文件（.c）中#include即可。所有需要用到这个函数的地方#include（包括函数定义的文件）。**一般只有声明可以放在头文件中**，否则容易出现重复声明。

```c
#include <stdio.h>	//<>让编译器只在指定的目录找
#include "max.h"	/* 此为自己编写发头文件。“”让编译器先在当前目录（.c所在）找，找不到再去编译器指定的目录去找。 */
```

* 一般做法是任何.c都有同名的.h，把所有对外公开的函数的原型和全局变量都放进去。全局变量也可以在多个文件中共享。
* 函数前面加static就使得它成为只能在所在的编译单元被使用的函数
* 全局变量前面加上static就使得它成为只能在所在的编译单元被使用的全局变量

```c
/* main.c */
#include <stdio.h>
#include "max.h"

int main()
{
    int a = 5;
    int b = 6;
    printtf("%f\n", max(a, gAll));
    return 0;
}
```

```c
/* max.h */
#ifndef _MAX_H_
#define _MAX_H_

double max(doublea, double b);
extern int gAll;	//告诉编译器，在某个地方有个gALL

#endif
```

```c
/* max.c */
#include "max.h"
int gAll = 12;

double max(double a, double b)
{
    return a>b?a:b
}
```

变量的声明和定义

```c
extern int i; 	//变量的声明
int i;			//变量的定义
```

在max.h中，下面的语句是检查是否重定义的，建议所有的.h都包含这三句

```c
#ifndef _MAX_H_
#define _MAX_H_
...
#endif
```



# 第16章 结构、联合和枚举

* 结构：可能具有不同类型的成员的集合；
* 联合：和结构类似，成员共享同一存储空间。但每次只能存储一个成员；
* 枚举：一种整数类型。

## 16.1 结构

结构-成员：在其他语言种，成为记录-字段（record - field）

* **结构声明的3种形式**

  ```c
  struct point{			//point表示结构标记
  	int x;
  	int y;
  };						//注意这里的分号
  struct point p1,p2;		//p1,p2宝表示结构变量
  ```

  ```c
  struct {
  	int x;
      int y;
  } p1, p2;				//注意这里的分号
  ```

  ```c
  struct point{			//第3种更常见
  	int x;
      int y;
  } p1, p2;				//注意这里的分号
  ```

* **结构的成员在内存种是按顺序存储的**

* 每个结构代表一个新的作用域，任何声明在此作用域的名字都不会和程序中的其他名字冲突。即每个结构为它的成员设置了独立的名字空间。

  ```c
  struct {
      int number;
      char name [NAME_LEN+1];
      int on_hand;
  } part1, part2;
  /* ========================= */
  struct {
      char name [NAME_LEN+1];
      int number;
      char sex;
  } employee1, employee2;
  ```

  在上述两个结构中，part1和part2的成员number和name不会和employee1和employee2的成员number和name冲突。

* **结构的初始化**

  ```c
  struct {
      int number;
      char name [NAME_LEN+1];
      int on_hand;
  } part1 = {528, "disk drive", 10},
    part2 = {914, "printer cable", 5};	/* 初始化的值必须按照结构成员的顺序。未填写部分默认未0或空字符 */
  ```

  ```c
  /* 指定初始化 */
  { .number = 528, .name = "disk drive", .on_hand = 10 } //可以不按顺序
  ```

* **结构的操作**

  与数组通过下标操作相似，结构通过成员名来操作。

  ```c
  printf("Part name: %s\n", part1.name);
  part1.number = 566;
  part1.on_hand++;			 //结构的成员为左值
  scanf("%d", &part1.on_hand); //.号的优先级比&高
  part2 = part1;               //把part1的所有成员对应赋值给part2
  part2 == part1;				 //错误操作，不能这么比较！
  ```

* **结构类型的定义**

  ```c
  typedef int number; /* 以前见过的类型定义，中间是原来的东西，后面是新的名词 */
  
  typedef struct {
      int number;
      char name[NAME_LEN+1];
      int on_hand;
  } Part;			/* 名字出现在这里，而不是跟在struct后面 */
  ```

* **一般来说，如果把一个结构当成参数来传入/传出函数，函数要生成结构中所有成员的副本。所有为了避免系统开销过大，一般使用指针作为参数**

  ```c
  struct date {
      int month;
      int day;
      int year;
  } myday;
  
  struct date *p = &myday; /* p为指针，myday前面必须要有&号。和数组不同*/
  (*p).month = 12;			/* (*p)必须要用括号括起来，因为.的优先级比较高 */
  p->month = 12;				/* 上一行的简便写法 */
  ```

  ```c
  #include <stdio.h>
  
  struct point
  {
      int x;
      int y;
  };
  
  struct point* getStruct(struct point*);
  void output(struct point);
  void print(const struct point *p);
  
  int main(int argc, char const *argv[])
  {
      struct point y = {0, 0};
      getStruct(&y);			//得到一个指向结构的指针
      output(y);
      output(*getStruct(&y));	
      print(getStruct(&y));
      getStruct(&y)->x = 0;	//(*p).x = 0
      *getStruct(&y) = (struct point){1,2};	//结构初始化
      return 0;
  }
  
  struct point* getStruct(struct point *p) /*套路：传入一个指针，利用它做了修改后再传回来*/
  {
      scanf("%d", &p->x);
      scanf("%d", &p->y);
      printf("getStruct: %d, %d\n", p->x, p->y);
      return p;  
  }
  
  void output(struct point p)
  {
      printf("output: %d, %d\n", p.x, p.y);
  }
  
  void print(const struct point *p)
  {
      printf("print: %d, %d\n", p->x, p->y);
  }
  ```
  
  ```c
  /* 如果r结构下面有pt1, pt2下面两个子结构 */
  struct rectangle r, *rp;
  rp = &r;
  
  /* 那么以下四种形式等价 */
  r.pt1.x;
  (r.pt1).x;
  r->pt1.x;
  (r->pt1).x;
  
  //不能用rp->pt1->x,因为pt1不是指针
  ```
  
  

* **结构的嵌套**

  ```c
  struct person_name {
      char first[FIRST_NAME_LEN+1];
      char middle_initial;
      char last[LAST_NAME_LEN+1];
  };
  
  struct student {
      struct person_name name;
      int_id, age;
      char sex;
  } student1, student2;
  
  strcpy(student.name.first, "Fred");
  ```

* **结构数组**：其元素为结构的数组，可构建简单的数据库。

格式：

```c
struct part inventory[100];
print_part(inventory[i]);	//访问存储在位置i的零件
inventory[i].number = 833;	//赋值写法
inventory[i].number[0] = '\0';	// 名字变为空字符串
```



## 16.2 联合

和结构类似，其成员可以拥有不同的类型。但编译器只为其最大的成员分配足够的内存空间。联合的成员彼此覆盖。例如下面的联合中，对i的改写会改变d，对d的改写会改变i。用途举例：比如一个选礼物的场景，每次只能选一个礼物。

```c
union {
    int i;
    double d;
} u;
struct {
    int i;
    double d;
} s;
```

<img src="D:\Documents\C\images\image-20220224205225629.png" alt="image-20220224205225629" style="zoom:50%;" />



## 16.3 枚举

来源：一般来说，常量应该符号化，即1、2这些数字应该有具体的名字，以便阅读方便。所以解决办法有

```c
cons int s=1; 		 //法1

#define CLUBS 		0 //法2
#define DIAMONDS	1
#define HEARTS		2
#define SPADES		3
```

但以上两种方法都不够好，所以催生了枚举类型(enumeration type)

```c
enum suit {CLUBS, DIAMONDS, HEARTS, SPADES}; //默认第一个为的值为0,后面的加1

enum suit {CLUBS = 1, DIAMONDS = 100, HEARTS = 100, SPADES}; //可以自己修改里面的值
```



# 第17章 指针的高级应用

## 17.5 链表

数组：在内存上是连续的区域。具有随机访问性。增加/删除元素比较麻烦

链表：往往不是连续的区域。由于必须由上一节点找出下一节点，所以不具有随机访问性。可方便插入和删除

<img src="D:\Documents\C\images\image-20220225174502714.png" alt="image-20220225174502714" style="zoom:50%;" />

* 声明节点结构

```c
struct node {		//node为结构标记
    int value;
    struct node *next;
};
/* 在结构中有一个指向相同结构类型的指针成员时，要求使用结构标记 */
struct node *first = NULL;	//链表初始为空
```

**创建节点**

* 为节点分配内存；
* 把数据存到节点中；
* 把节点插到链表中。

```c
struct node *new_node;	//临时变量指向要创建的节点
new_node = malloc(sizeof(struct node));	//为新节点分配内存空间
(*new_node).value = 10;	//把数据存到新节点中。注意圆括号
new_node->value = 10;	//上一行的替代写法
```

**插入节点**

```c
struct node *first = NULL;//链表初始为空
new_node = malloc(sizeof(struct node));//为新节点分配内存空间
nex_node->node = first;	//先修改新节点的指针next,使其成为first指针NULL
first = new_node;		//使first指向新节点
```

以上步骤可以写成一个函数

```c
struct node *add_to_list(struct node *list, int n)
{
    struct node *new_node;
    new_node = malloc(sizeof(struct node));
    if (new_node == NULL) {
        printf("Error: malloc failed to add to list\n");
        exit(EXIT_FAILURE);
    }
    new_node->value = n;
    new_node->next = list;
    return new_node;	//没有修改指针，而是返回指向新产生的节点的指针
}
```

**搜索链表**：

```c
for (p = first; p = != NULL; p = -> next)
{
    ...
}
```

惯用法：

```c
struct node *search_list(struct node *list, int n)	//搜索值为n的链表
{
    struct node *p;
    for (p = first; p ！= NULL; p = -> next)
    {
        if (p -> value == n)
            return p;								//	找到了就返回指针
        return NULL;								//没找到返回NULL
    }
}
```

```c
struct node *search_list(struct node *list, int n)
{
    for (; list != NULL; list = list-> next)		//除去p，用list来跟踪节点
        if (p -> value == n)
            return list;
    return NULL;
}
```

```
struct node *search_list(struct node *list, int n)
{
    for (; list != NULL && list->value != n; list = list-> next)
    ;
    return list;
}
```

**删除节点**

* 定位要删除的节点
* 改变前一个节点，使它“绕过”删除节点
* 调用free函数回收删除节点占用的内存空间

## 17.6 指针与函数

每个函数在编译链接后总是占用一段连续的内存区，函数名就是该函数所占内存区的入口

**格式**：类型说明符 (*指针名)()

```c
#include <stdio.h>

int add(int a, int b);
int sub(int a, int b);
void result(int (*pf)(), int a, int b);

int main()
{
    int a, b;
    int (*pf)(); //定义一个指向函数的指针
    printf("input two integer: ");
    scanf("%d %d", &a, &b);
    pf = add;
    result(pf, a, b);
    pf = sub;
    result(pf, a, b);
    return 0;
}

int add(int a, int b)
{
    return a+b;
}
int sub(int a, int b)
{
    return a-b;
}
void result(int (*p)(), int a, int b)//这里的指针p没必要和pf相同
{
    int value;
    value = (*p)(a, b); //这里的指针p没必要和pf相同
    printf("%d\n", value);
}
```


# 第18章 声明

# 第19章 程序设计

# 第20章 底层程序设计
* **栈区**：形参、局部变量、返回值等
## 20.1 位域
* 有些信息在存储时，并不需要占用一个完整的字节，而只需占几个或一个二进制位。例如在存放一个开关量时，只有 0 和 1 两种状态，用 1 位二进位即可。为了节省存储空间，并使处理简便，C 语言又提供了一种数据结构，称为"**位域**"或"位段"。
* 所谓"位域"是把一个字节中的二进位划分为几个不同的区域，并说明每个区域的位数。每个域有一个域名，允许在程序中按域名进行操作。这样就可以把几个不同的对象用一个字节的二进制位域来表示。
```c
struct 位域结构名 
{
    type member_name : width;
};
```
|     元素     |                                               描述                                               |
| ----------- | ----------------------------------------------------------------------------------------------- |
| type        | 只能为 int(整型)，unsigned int(无符号整型)，signed int(有符号整型) 三种类型，决定了如何解释位域的值。 |
| member_name | 位域的名称。                                                                                     |
| width       | 位域中位的数量。宽度必须小于或等于指定类型的位宽度。                                                 |
```c
int main(){
    struct bs{
        unsigned a:1;
        unsigned b:3;
        unsigned c:4;
    } bit,*pbit;
    bit.a=1;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    bit.b=7;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    bit.c=15;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
    printf("%d,%d,%d\n",bit.a,bit.b,bit.c);    /* 以整型量格式输出三个域的内容 */
    pbit=&bit;    /* 把位域变量 bit 的地址送给指针变量 pbit */
    pbit->a=0;    /* 用指针方式给位域 a 重新赋值，赋为 0 */
    pbit->b&=3;    /* 使用了复合的位运算符 "&="，相当于：pbit->b=pbit->b&3，位域 b 中原有值为 7，与 3 作按位与运算的结果为 3（111&011=011，十进制值为 3） */
    pbit->c|=1;    /* 使用了复合位运算符"|="，相当于：pbit->c=pbit->c|1，其结果为 15 */
    printf("%d,%d,%d\n",pbit->a,pbit->b,pbit->c);    /* 用指针方式输出了这三个域的值 */
}
```
# 第21章 标准库

# 第22章 输入/输出
## 22.1 三字母词
以??后面加一个字符的形式，在打印时会显示成另外的样子。MISRA-C里面规定不能使用三字母词
| 三字母词 | 输出 | 三字母词 | 输出 | 三字母词 | 输出 |
| :------- | :--- | :------- | :--- | :------- | :--- |
| ??=      | #    | ??(     | [    | ??)      | \]   |
| ??<      | {    | ??>      | {    | ??/      | \}   |
| ??!      | \|   | ??'      | ^    | ??-      | ~    |
## 22.2 按位运算

| 符号 |   含义   | 符号 |   含义   |
| :--: | :------: | :--: | :------: |
|  &   |  按位与  |  \|  |  按位或  |
|  ~   | 按位取反 |  ^   | 按位异或 |
|  <<  |   左移   |  >>  |   右移   |

* 按位与：让某一位或某些位为0，如用FE(11111110可以使最后一位为零)；取其中一个数的一段，用FF

* 按位或：让某一位或某些位为1；使两个拼接，如0x00FF | 0xFF00

* 使用这些符号后，所得是int型。如果打印，需要用%d等

* <img src="C:\Users\lsy\AppData\Roaming\Typora\typora-user-images\image-20220403174847061.png" alt="image-20220403174847061" style="zoom:50%;" />

* 按位异或：x\^y^y = x，即同一个变量用同一个值异或两次，等于本身。可用来加密

* 左移：左移后右边填入零，相当于乘2。所有小于int的类型，移位以int的方式来做，结果是int。

* 右移：相当于除以2。所有小于int的类型，移位以int的方式来做，结果是int。对于unsigned类型，左边填入0；对于signed类型，左边填入原来的最高位（保持符号不变）。

* 举例：

* ```c
  SID = (bit)(i_data & 0x80) /* 后面&号表示取出字节的最高位,后7位均为0，加上(bit)表示强制转换为一个位*/
  i_data &= 0xf0;		//取出字节的高4位
  i_data <<= 4;		//左移4位，即选出了字节的低4位
  ```
```c
int main()
{
    char c;
    do
    {
        printf("Uppercase character please: ");
        scanf("%c", &c);
    } while (c < 'A' || c > 'Z');
    
    printf("%c\n", c | 0x20);    /*ASCII码中大小写相差32，如a是97（01100001），A是65（01000001）*/
    //printf("%c\n", c & 0xDF);    /*这里是小写转大写的方法*/    
    return 0;
}
```
```c
void swap(int *a, int *b)    /*不用临时变量完成两个数的交换*/
{
    int *a, *b;
    *a = *a ^ *b;
    *b = *a ^ *b;
    *a = *a ^ *b;
}

swap(&x, &y);    /*调用*/
```

# 第23章 库对数值和字符数据的支持

# 第24章 错误处理
* segmentation fault： 内存段冲突
```c
void foo(void)

int main(int argc, char *argv[])
{
    foo();
}

void foo(void)
{
    foo(); 
}
```