---
title: c语言简要
date: 2017-11-13 14:02:34
tags: c语言
---
### 引子
故至诚无息。   

不息则久，久则征。   

征则悠远。悠远，则博厚。博厚，则高明。  

博厚，所以载物也。高明，所以覆物也。悠久，所以成物也。   

博厚，配地。高明配天。悠久，无疆。 

如此者，不见而章，不动而变，无为而成。  

天地之道，可一言而尽也。其为物不贰，则其生物不测。   

天地之道，博也，厚也，高也，明也，悠也，久也。   
今夫天斯昭昭之多，及其无穷也，日月星辰系焉，万物覆焉。今夫地一撮土之多，及其广厚载华岳而不重，振河海而不泄，万物载焉。今夫山一石之多，及其广大，草木生之，禽兽居之，宝藏兴焉。今夫水，一勺之多，及其不测，鼋，鼍，蛟，龙，鱼，鳖生焉，货财殖焉。   

诗云，【维天之命，于穆不已】。盖曰，天之所以为天也。【于乎不显，文王之德之纯】。盖曰，文王之所以为文也，纯亦不已。

--《中庸》

### 正文

#### 简介和环境设置

略

#### 程序结构

- 预处理器指令
- 函数
- 变量
- 语句 & 表达式
- 注释

示例：
```c
#include <stdio.h>
int main(){
  /* 我的第一个c程序 */
  printf("Hello, World! \n");
  return 0
}
```
#### 基本语法

*c 的令牌(Tokens)*

c程序由各种令牌组成，令牌可以是关键字，标志符，常量，字符串值，或者是一个符号。例如下面的c语句包括5个令牌：
```c
printf
(
  "Hello, World! \n"
)
;
```
*分号*

在c程序中，分号是语句结束符。也就是说，每个语句必须以分号结束。它表明一个逻辑实体的结束。
例如，下面是两个不同的语句：
```c
printf("Hello, World! \n");   
return 0
```

*注释*

例：
```c
/* 我的第一个c程序 */
```

*标识符*

c标识符是用来标识变量、函数，或任何其他用户自定义项目的名称。一个标识符以字母A-Z或a-z或下划线_开始，后跟零个或多个字母、下划线和数字（0-9）

c标识符内不允许出现标点字符，比如@、$和%。c是区分大小写的编程语言。例：
```
mkhd zara abc move_name a_123 myname50 _temp j a23b9 retVal
```

*关键字*

`auto` `else` `long` `switch` `break` `enum` `register` `typeof` `case` `extern` `return` `union` `char` `float` `short` `unsigned` `const` `for` `signed` `void` `continue` `goto` `sizeof` `volatile` `default` `if` `static` `while` `do` `int` `struct` `_Packed` `double`

*c中的空格*

只包含空格的行，被称为空白行，可能带有注释，c编译器会完全忽略它。

在c中，空格用于描述空白符、制表符、换行符和注释。空格分隔语句的各个部分，让编译器能识别语句中的某个元素在哪里结束，下一个元素在哪里开始。因此，在下面的语句中：
```c
int age;
```
在这里，int和age之间必须至少有一个空格字符，这样编译器才能够区分它们。另一方面，在下面的语句中：
```c
fruit = apples + oranges; // 获取水果的总数
```
fruit和=，或者=和apples之间的空格字符不是必须的，但是为了增强可读性，您可以根据需要适当增加一些空格。

#### 数据类型

数据类型指的是用于声明不同类型的变量或函数的一个广泛的系统。变量的类型决定了变量存储占用的空间，以及如何解释存储的位模式。

- 基本类型：整数类型和浮点类型
- 枚举类型：被用来定义在程序中只能赋予其一定的离散整数值的变量
- void类型：类型说明符void表明没有可用的值
- 派生类型：指针类型、数组类型、结构类型、共同体类型和函数类型

#### 变量

*变量的定义*

语法
```c
type variable_list
```
例子：
```c
int i,j,k;
char c,ch;
float f,salary;
double d;
```
变量可以在定义的时候初始化。例：
```c
extern int d = 3, f = 5;
int d = 3, f = 5;
byte z = 22;
char x = 'x';
```
不带初始化的定义：带有静态存储持续时间的变量会被隐式初始化为NULL（所有的字节都是0），其他所有变量的初始值是未定义的。

*变量的声明*
- 需要建立存储空间的。例如：int a在声明的时候已经建立了存储空间。
- 不需要建立存储空间的，通过使用extern关键字声明变量名而不定义它。例如：extern int a其中变量a是可以在    别的文件中定义的。
- 除非有extern关键字，否则都是变量的定义
```c
extern int i; //声明，不是定义
int i; //声明，也是定义
```
实例
```c
// 变量声明
extern int a,b;
extern int c;
extern float f;
int main(){
  /*变量的定义*/
  int a,b;
  int c;
  float f;
  /*初始化*/
  a = 10;
  b = 20;

  c = a + b;
  printf("value of f : %f \n", f);
  return 0
}
```

*C中左值（Lvalues）和右值（Rvalues）*

1. 左值(lvalue)：指向内存位置的表达式称为左值（lvalue）表达式。左值可以出现在赋值号的左边或右边。
2. 右值(rvalue)：术语右值(rvalue)指的是存储在内存中的某些地址的数值。右值是不能对其进行赋值的表达式，也就是说，右值可以出现在赋值号的右边，但不能出现在赋值号的左边。

变量是左值，因此可以出现在赋值号的左边。数值型的字面值是右值，因此不能被赋值，不能出现在赋值号的左边。例
```c
int g = 20;
```
错误：
```c
10 = 20
```
#### 常量
常量是固定值，在程序执行期间不会改变。这些固定的值，又叫做字面量。

常量可以是任何的基本数据类型

把常量定义为大写字母形式，是一个很好的编程实践。

#### 存储类

存储类定义c程序中变量／函数的范围（可见性）和生命周期。这些说明符放置在它们所修饰的类型之前。c程序中可用的存储类：
- auto
- register
- static
- extern

*auto 存储类* 

auto 存储类是所有局部变量默认的存储类。
```
{
  int mount;
  auto int month;
}
```
上面的实例定义了两个带有相同存储类的变量，auto只能用在函数内，即auto只能修饰局部变量。

*register 存储类*

register 存储类用于定义存储在寄存器中而不是RAM中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个词），且不能对它应用一元的'&'运算符（因为它没有内存位置）。
```c
{
  register int miles;
}
```
寄存器只用于需要快速访问的变量，比如计数器。定义为'register'并不意味着变量将被存储在寄存器中，它意味着变量可能存储在寄存器中，这取决于硬件和实现的限制。

*static 存储类*

static 存储类指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁。因此，使用static修饰局部变量可以在函数调用之间保持局部的值。

static修饰符也可以应用于全局变量，当static修饰全局变量时，会使变量的作用域限制在声明它的文件内。

static是全局变量的默认存储类

```c
static int Count ;
int Road;

main(){
  printf("%d\n", Count);
  printf("%d\n", Road);
}
```
实例：
```c
#include <stdio.h>

/*函数声明*/
void func1(void);

static int count=10; /*全局变量 - static是默认的*/
int main(){
  while(count--){
    func1();
  }
  return 0;
}

void func1(void){
  /* 'thingy'是 'func1' 的局部变量 - 只初始化一次
  *  每次调用函数 'func1' 'thingy' 值不会被重置。
  */
  static int thingy=5;
  thingy++;
  printf("thingy 为 %d, count 为 %d\n", thingy, count);
}
```
*extern存储类*

extern 存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的。当您使用‘extern’时，对于无法初始化的变量，会把变量名指向一个之前定义过的存储位置。

当您有多个文件且定义了一个可以在其他文件中使用的全局变量或函数时，可以在其他文件中使用extern来得到已定义的变量或函数的引用。可以这么理解，extern是用来在另一个文件中声明一个全局变量或函数。
extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候，如下所示：

第一个文件**main.c**

实例:
```c
#include <stdio.h>
int count;
extern void write_extern();
main(){
  count = 5;
  write_extern()
}
```
第二个文件**support.c**

实例：
```c
#include <stdio.h>
extern int count;

void write_extern(void){
  printf("count is %d\n", count);
}
```
#### 运算符
运算符是一种告诉编译器执行特定的数学或逻辑操作符号。c语言内置了丰富的运算符，并提供了以下类型的运算符：

- 算数运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 赋值运算符
- 杂项运算符

*算数运算符*

\+ \- \* / % ++ --

*逻辑运算符*

&& || ！

*位运算符*

& | ^ ~ << >>

*赋值运算符*

= += -= *= /= %= <<= >>= &= ^= |=

*杂项运算符*

- sizeof()：返回变量大小
- &：返回变量的地址
- *：指向一个变量
- ?:：条件表达式

*C 中的运算符优先级*

略
#### 判断
*判断语句*

- if语句
- if...else语句
- 嵌套if语句
- switch语句
- 嵌套switch语句

*?:运算符（三元运算符）*
```c
  Exp1? Exp2 : Exp3;
```
#### 循环

*循环类型*

- while循环
- for循环
- do...while循环
- 嵌套循环

*循环控制语句*

- break语句：中止循环或switch语句，程序流将继续执行紧接着循环或switch的下一条语句
- continue语句：告诉一个循环体立刻停止本次循环迭代，重新开始下次循环迭代
- goto语句：将控制转移到标记的语句。但不建议在程序中使用goto语句

*无限循环*
```c
#include <stdio.h>
int main(){
  for(;;){
    printf("该循环会永远执行下去！\n");
  }
  return 0;
}
```
当条件表达式不存在时，它被假设为真

#### 函数
函数声明告诉编译器函数的名称、返回类型和参数。函数定义提供了函数的实际主体。

*定义函数*
```c
return_type function_name(parameter list)
{
  body of function
}
```
函数所有组成部分：
- 返回类型
- 函数名称
- 参数
- 函数主体

*实例*
```c
int max(int num1, int num2){
  /*局部变量声明*／
  int result;

  if(num1 > num2)
    result = num1;
  else
    result = num2;
  return result;
}
```

*函数声明*

函数声明会告诉编译器函数的名称以及如何调用函数。函数的主体可以单独定义。

*函数调用*

实例：
```c
#include <stdio.h>
/*函数声明*/
int max(int num1, int num2);
int main(){
  /*局部变量定义*/
  int a = 100;
  int b = 200;
  int ret;
  /*调用函数获取最大值*/
  ret = max(a, b);

  printf("Max value is : %d\n", ret);

  return 0;
}

/*函数返回两个数中较大的那个数*/
int max(int num1, int num2){
  /*局部变量声明*/
  int result;

  if(num1 > num2)
    result = num1;
  else
    result = num2;

  return result;
}
```
*函数参数*

- 传值调用
- 引用调用

*作用域规则*

c语言在三个地方可以声明变量：

1. 在函数或块内部的**局部变量**
2. 在所有函数外部的**全局变量**
3. 在**形式**参数的函数的函数参数定义中

*局部变量*

在某个函数或块的内部声明的变量成为局部变量。它们只能被该函数或代码块内部的语句使用。局部变量在函数外部是不可知的。下面是使用局部变量的实例：
```c
#include <stdio.h>

int main(){
  /*局部变量声明*/
  int a, b;
  int c;
  /*实际初始化*/
  a = 10;
  b = 20;
  c = a + b;
  printf("value of a = %d, b = %d and c = %d\n", a, b, b);
  return 0;
}
```
*全局变量*

全局变量是定义在函数外部，通常是在程序的顶部。全局变量在整个程序生命周期内都是有效的，再任意的函数内部能访问全局变量。
```c
#include <stdio.h>

/* 全局变量声明 */

int g;

int main()
{
  /* 局部变量声明 */
  int a, b;

  /* 实际初始化 */
  a = 10;
  b = 20;
  g = a + b;

  printf("value of a = %d, b = %d and g = %d\n", a, b, g);
  return 0;
}

```
在程序中，局部变量和全局变量的名称可以相同，但是在函数内，局部变量的值会覆盖全局变量的值。实例：
```c
/* 全局变量声明 */
int g = 20;

int main(){
  /* 局部变量声明 */
  int g = 10;
  printf("value of g = %d\n", g);
  return 0;
}
```
*形式参数*

函数的参数，形式参数，被当作该函数内的局部变量，它们会优先覆盖全局变量。实例：
```c
#include <stdio.h>

/* 全局变量声明 */
int a = 20;

int main(){
  /* 在主函数中的局部变量声明 */
  int a = 10;
  int b = 20;
  int c = 0;
  int sum(int, int);
  
  printf("value of a in main() = %d\n", a);
  c = sum(a, b);
  printf("value of c in main() = %d\n", c);
  return 0;
}

/* 添加两个整数的函数 */
int sum(int a, int b){
  printf("value of a in sum() = %d\n", a);
  printf("value of b in sum() = %d\n", b);

  return a + b;
}
```
*初始化局部变量和全局变量*

局部变量被定义的时候，系统不会对其初始化，您必须自行对其初始化。定义全局变量的时候，系统会自动对其初始化。

- int: 0
- char: '\0'
- float: 0
- double: 0
- pointer: NULL

#### 数组
所有的数组都是由连续的内存位置组成，最低的地址对应第一个元素，最高的地址对应最后一个元素。

*声明数组*

在c中声明一个数组，需要指定元素的类型和元素的数量，如下所示：
```c
type arrayName [ arraySize ];
```
示例：
```c
  double balance[10];
```
现在balance是一个可用的数组，可以容纳10个类型为double的数字；

*初始化数组*

在c中，可以逐个初始化数组，也可以使用一个初始化语句：
```c
double balance[5] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```
如果省略了数组大小，数组的大小则为初始化时元素的个数：
```c
double balance[] = {1000.0, 2.0, 3.4, 7.0, 50.0};
```
*访问数组元素*

```c
  double salary = balance[9];
```
示例：
```c
#include <stdio.h>
int main(){
  int n[10]; /* n是一个包含10个整数的数组 */
  int i,j;
  /* 初始化数组元素 */
  for(i =0; i < 10; i++){
    n[i] = i + 100; /* 设置元素i为i + 100 */
  }
  /* 输出数组中每个元素的值 */
  for(j = 0; j < 10; j++){
    printf("Element[%d] = %d\n", j, n[j]);
  }
  return 0;
}
```
> 指针理解：*int *b = &a;*表示**变量b**的值是**变量a**的地址

#### 指针

学习c语言的指针既简单又有趣。通过指针，可以简化一些c编程任务的执行，还有一些任务，如动态内存分配，没有指针是无法执行的。所以想要成为一名优秀的c程序员，学习指针是很有必要的。

每一个变量都有一个内存位置，每一个内存位置都定义了可使用连字号（&）运算符访问的地址，它表示了在内存中的一个地址。请看下面的实例，它将输出定义的变量地址：

```c
#include <stdio.h>
int main(){
  int var1;
  char var2[10];

  printf("var1 变量的地址： %p\n", &var1);
  printf("var2 变量的地址： %p\n", &var2);
  return 0;
}
```
*什么是指针*

指针是一个变量，其值为另一个变量的地址，即，内存位置的直接地址。就像其他变量或常量一样，您必须在使用指针存储其他变量地址之前，对其进行声明。
```c
type *var_name;
```
在这里，type就是指针的基本类型，它必须是一个有效的c数据类型，var_name是指针变量的名称。用来声明指针的星号*和乘法中使用的星号是相同的。但是，在这个语句中，星号是用来指定一个变量是指针。以下是有效的指针声明：
```c
int *ip; /* 一个整数型的指针 */
double *dp; /* 一个double型的指针 */
float *fp; /* 一个浮点型的指针 */
char *ch; /* 一个字符型的指针 */
```
所有的指针的值的实际数据类型，不管是整型、浮点型、字符型，还是其他的数据类型，都是一样的，都是一个代表内存地址的长的十六进制数，不同数据类型的指针之间唯一的不同是，指针所指向的变量或常量的数据类型不同。

*如何使用指针*

使用指针时会频繁进行以下几个操作：定义一个指针变量、把变量地址赋值给指针、访问指针变量中可用地址的值。这些是通过使用一元运算符*来返回位于操作数所制定地址的变量的值。实例：
```c
#include <stdio.h>

int main(){
  int var = 20; /* 实际变量的声明 */
  int *ip; /* 指针变量的声明 */

  ip = &var; /* 在指针变量中存储var的地址 */

  printf("Address of var variable: %p\n", &var);
  
  /* 在指针变量中存储的地址 */
  printf("Address stored in ip variable: %p\n", ip);

  /* 使用指针访问值 */
  printf("Value of *ip variable: %d\n", *ip);
  return 0;
}
```
*C中的NULL指针*
在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个NULL值是一个良好的编程习惯。赋为NULL值的指针被称为空指针。
NULL指针是一个定义在标准库中的值为零的常量。
```c
#include <stdio.h>
int main(){
  int *ptr = NULL;
  printf("ptr 的值 %p\n", ptr);
  return 0;
}
```
检查一个空指针：
```c
if(ptr) /* 如果p非空，则完成 */
if(!ptr) /* 如果p为空，则完成 */
```
*指针详解*

略

> 每个变量都有自己的内存地址，同时拥有自己的值（内存地址中存储的值，如果没有初始化则是不可知的值）。指针也是变量，但它的值一般是另一个变量的地址。

#### 函数指针与回调函数

*函数指针*

函数指针是指向函数的指针变量。通常我们说的指针变量是指向一个整型、字符型、或数组等变量，而函数指针是指向函数。函数指针可以像一般函数一样，用于调用函数、传递参数。函数指针变量的声明：
```c
typedef int (*fun_ptr)(int, int); //声明一个指向同样参数，返回值的函数指针类型
```
实例：
```c
#include <stdio.h>
int max(int x, int y){
  return x > y ? x : y
}
int main(void){
  /* p是函数指针 */
  int (* p)(int, int) = & max; // &可以省略
  int a, b, c, d;
  printf("请输入三个数字：");
  scanf("%d %d %d", &a, &b, &c);
  /* 与直接调用函数等价，d = max(max(a,b), c) */
  d = p(p(a,b), c);
  
  printf("最大的数字是：%d\n", d);

  return 0;
}
```
*回调函数*

函数指针作为某个函数的参数

```c
#include <stdlib.h>
#include <stdio.h>

//回调函数
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
  for(size_t i=0; i<arraySize; i++)
    array[i] = getNextValue();
}
//获取随机值
int getNextRandomValue(void)
{
  return rand();
}

int main(void)
{
  int myarray[10];
  populate_array(myarray, 10, getNextRandomValue);
  for(int i = 0; i < 10; i++){
    printf("%d ", myarray[i]);
  }
  printf("\n");
  return 0;
}

```

*tips*
```c
#include <stdio.h>
void test(){
  printf("123456\n")
}
void main(void){
  printf("0x%x\n", test);
  printf("0x%x\n", &test);
}
```
按照&运算符本来的意义，它要求其操作数是一个对象，但函数名不是对象（函数是一个对象），本来&test是非法的，但很久以前有些编译器已经允许这样做， 
c/c++标准的制定者出于对象的概念已经有所发展的缘故，也承认了&test的合法性。 

因此，对于test和&test你应该这样理解，test是函数的首地址，它的类型是void ()，&test表示一个指向函数test这个对象的地址，它的类型是void (*)()，因此test和&test所代表的地址值是一样的，但类型不一样。test是一个函数，&test表达式的值是一个指针！ 

跟此问题类似的还有对一个数组名取地址。 
```c
int a[100]; 
printf("%p\n", a); 
printf("%p\n", &a[0]); 
```
打印值一样。 
但是数组名a，指向的是具有100个int类型的数组； 
&a[0]指向的是元素a[0]。 
即他们的值相同，但指向的类型不同。

#### 字符串

在c语言中，字符串实际上是使用null字符'\0'终止的一维字符数组。因此，一个以null结尾的字符串，包含了组成字符串的字符。

下面的声明初始化创建了一个“Hello”字符。由于在数组的末尾存储了空字符，所以字符数组的大小比单词“Hello”的字符数多一个。

```c
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
```
依据数组初始化规则，还可以写作：
```c
char greeting[] = "Hello";
```
其实，编译器在初始化数组时，自动把“\0”放在字符串的末尾：
```c
#include <stdio.h>

int main()
{
  char greeting[6] = {'H', 'e', 'l', 'l', '0', '\0'};

  printf("Greeting message: %s\n", greeting);

  return 0;
}
```

c语言操作字符串的函数：
- strcpy(s1, s2)
- strcat(s1, s2)
- strlen(s1)
- strcmp(s1, s2)
- strchr(s1, ch)
- strstr(s1, s2)

实例如下：
```c
#include <stdio.h>
#include <string.h>

int main()
{
  char str1[12] = "Hello";
  char str2[12] = "World";
  char str3[12];
  int len;
  /* 复制str1 到 str3 */
  strcpy(str3, str1);
  printf("strcpy(str3, str1) : %s\n", str3);

  /* 连接str1 和 str2 */
  strcat(str1, str2);
  printf("strcat( str1, str2) : %s\n", str1);

  /* 连接后， str1的总长度 */
  len = strlen(str1);
  printf("strlen( str1 ) : %d\n", len);

  return 0;
}
```
英文全称：
- strcmp: string compare 
- strcat: string catenate 
- strcpy: string copy 
- strlen: string length 
- strlwr: string lowercase 
- strupr: string upercase

#### 结构体

c数组允许定义可存储相同类型数据项的变量，结构是c编程中另一种用户自定义的可用的数据类型，它允许您存储不同类型的数据项。

结构用于表示一条记录，假设你想要跟踪图书馆中书本的动态，您可能需要跟踪每本书的下列属性：
- Title
- Author
- Subject
- Book ID

*定义结构*

为了定义结构，您必须使用struct语句。struct语句定义了一个包含多个成员的数据类型，struct语句格式如下：
```c
struct [structure tag]
{
  member definition;
  member definition;
  ...
  member definition;
} [one or more structure variables];
```
structure tag是可选的，每个member definition是标准的变量定义，例：
```c
struct Books
{
  char title[50];
  char author[50];
  char subject[100];
  int book_id;
} book;
```

*访问结构成员*

为了访问结构的成员，我们使用成员访问运算符(.)。成员访问运算符是结构变量名称和我们要访问的结构成员之间的一个点号。可以使用struct关键字定义结构类型的变量。实例：
```c
#include <stdio.h>
#include <string.h>

struct Books
{
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
};

int main()
{
    struct Books Book1; /* 声明Book1，类型为Books */
    struct Books Book2; /* 声明Book2，类型为Books */
    
    /* Book1 详述 */
    strcpy( Book1.title, "C Programming");
    strcpy( Book1.author, "Nuha Ali");
    strcpy( Book1.subject, "C Programming Tutorial");
    Book1.book_id = 6495407;
    
    /* Book2 详述 */
    strcpy( Book2.title, "Telecom Billing");
    strcpy( Book2.author, "Zara Ali");
    strcpy( Book2.subject, "Telecom Billing Tutorial");
    Book2.book_id = 6495700;
    
    /* 输出Book1的信息 */
    printf("Book 1 title: %s\n", Book1.title);
    printf("Book 1 author: %s\n", Book1.author);
    printf("Book 1 subject: %s\n", Book1.subject);
    printf("Book 1 book_id: %d\n", Book1.book_id);
    
    /* 输出Book2信息 */
    printf("Book 2 title: %s\n", Book2.title);
    printf("Book 2 author: %s\n", Book2.author);
    printf("Book 2 subject: %s\n", Book2.subject);
    printf("Book 2 book_id: %d\n", Book2.book_id);
    
    return 0;
}
```
*结构作为函数参数*

实例：
```c
#include <stdio.h>
#include <string.h>

struct Books
{
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
};

/* 函数声明 */
void printBook( struct Books book);

int main()
{
    struct Books Book1; /* 声明Book1，类型为Books */
    struct Books Book2; /* 声明Book2，类型为Books */
    
    /* Book1 详述 */
    strcpy( Book1.title, "C Programming");
    strcpy( Book1.author, "Nuha Ali");
    strcpy( Book1.subject, "C Programming Tutorial");
    Book1.book_id = 6495407;
    
    /* Book2 详述 */
    strcpy( Book2.title, "Telecom Billing");
    strcpy( Book2.author, "Zara Ali");
    strcpy( Book2.subject, "Telecom Billing Tutorial");
    Book2.book_id = 6495700;
    
    /* 输出Book1的信息 */
    printBook(Book1);
    
    /* 输出Book2信息 */
    printBook(Book2);
    
    return 0;
}

void printBook(struct Books book)
{
    printf("Book title: %s\n", book.title);
    printf("Book author: %s\n", book.author);
    printf("Book subject: %s\n", book.subject);
    printf("Book book_id: %d\n", book.book_id);
}
```
*指向结构的指针*

同其他类型变量的指针
```c
struct Books *struct_pointer;
struct_pointer = &Book1;
```
为了使用指向该结构的指针访问结构的成员，必须使用->运算符：
```c
struct_pointer->title;
```
实例：
```c
#include <stdio.h>
#include <string.h>

struct Books
{
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
};

/* 函数声明 */
void printBook( struct Books *book);

int main()
{
    struct Books Book1; /* 声明Book1，类型为Books */
    struct Books Book2; /* 声明Book2，类型为Books */
    
    /* Book1 详述 */
    strcpy( Book1.title, "C Programming");
    strcpy( Book1.author, "Nuha Ali");
    strcpy( Book1.subject, "C Programming Tutorial");
    Book1.book_id = 6495407;
    
    /* Book2 详述 */
    strcpy( Book2.title, "Telecom Billing");
    strcpy( Book2.author, "Zara Ali");
    strcpy( Book2.subject, "Telecom Billing Tutorial");
    Book2.book_id = 6495700;
    
    /* 输出Book1的信息 */
    printBook(&Book1);
    
    /* 输出Book2信息 */
    printBook(&Book2);
    
    return 0;
}

void printBook(struct Books *book)
{
    printf("Book title: %s\n", book->title);
    printf("Book author: %s\n", book->author);
    printf("Book subject: %s\n", book->subject);
    printf("Book book_id: %d\n", book->book_id);
}
```
*位域*

有些信息在存储时，并不需要占用一个完整的字节，而只需要占几个或一个二进制位。例如在存放一个开关变量时，只有0和1两种状态，用一位二进制位即可。为了节省空间，并使处理方便，c语言又提供了一种数据结构，称为“位域”或“位段”。

所谓“位域”是把一个字节中的二进位划分为几个不同的区域，并说明每个区域的位数。每个域有一个域名，允许在程序中按域名进行操作。这样就可以把几个不同的对象用一个字节的二进制位域来表示。
典型的实例：
- 用1位二进位存放一个开关量，只有0和1两种状态
- 读取外部文件格式 - 可以读取非标准的文件格式。例如：9位的整数。

*位域的定义和位域变量的说明*

定义：
```c
struct 位域结构名
{
  位域列表
};
```
位域列表的形式：
```c
类型说明符 位域名：位域长度
```
例如：
```c
struct bs{
  int a: 8;
  int b: 2;
  int c: 6;
} data;
struct packed_struct {
  unsigned int f1:1;
  unsigned int f2:1;
  unsigned int f3:1;
  unsigned int f4:1;
  unsigned int type:4;
  unsigned int my_int:9;
} pack;
```
- 一个位域必须存储在同一个字节中，不能跨两个字节。如一个字节所剩空间不够存放另一位域时，应从下一单元起存放   该位域。也可以使某位域从下一单元开始。例：
  ```c
  struct bs{
    unsigned a:4;
    unsigned  :4; /* 空域 */
    unsigned b:4; /* 从下一单员开始存放 */
    unsigned c:4;
  }
  ```
- 位域的长度不能超过8位二进制位
- 可以使用无名位域
所以位域在本质上就是一种结构类型，只不过成员是按二进位分配的。

*位域的使用*

规则：
```c
位域变量名.位域名
位域变量名->位域名
```
位域允许使用各种格式输出，实例：
```c
main(){
    struct bs{
        unsigned a:1;
        unsigned b:3;
        unsigned c:4;
    } bit, *pbit;
    bit.a = 1; /* 给位域赋值（赋值不应超过该位域允许范围）*/
    bit.b = 7;
    bit.c = 15;
    printf("%d,%d,%d\n", bit.a, bit.b, bit.c); /*以整型格式输出三个域的内容*/
    pbit = &bit;
    pbit->a = 0;
    pbit->b &= 3;
    pbit->c |= 1;
    printf("%d,%d,%d\n", pbit->a, pbit->b, pbit->c);
}
```
所以位域也可以使用指针

*其他*

[内存对齐](http://blog.csdn.net/andy572633/article/details/7213465)

#### 共用体
**共用体**是一种特殊的数据类型，允许您在相同的内存位置储存不同的数据类型。您可以定义一个带有多个成员的共用体，到那时任何时候只能有一个成员的值。共用体提供了一种使用相同的内存位置的有效方式。

*定义共用体*

使用union语句：
```c
union [union tag]
{
  member definition;
  member definition;
  ...
  member definition;
} [one or more union variables];
```
**union tag**是可选的。实例：
```c
union data
{
  int i;
  float f;
  char str[20];
} data;
```
**Data**类型的变量可以存储多种类型的数据。共用体占用内存应足够存储共用体中最大的成员，实例：
```c
#include <stdio.h>
#include <string.h>

union Data {
    int i;
    float f;
    char str[20];
};
int main()
{
    union Data data;
    printf("Memory size occupied by data : %d\n", sizeof(data));
    return 0;
}
```
*访问共用体成员*

为了访问共用体的成员，我们使用**成员访问运算符（.）**。
实例：
```c
#include <stdio.h>
#include <string.h>

union Data {
    int i;
    float f;
    char str[20];
};
int main()
{
    union Data data;
    data.i = 10;
    data.f = 220.5;
    strcpy( data.str, "C Programming");
    
    printf("data.i : %d\n", data.i);
    printf("data.f : %f\n", data.f);
    printf("data.str : %s\n", data.str);
    return 0;
}
```
可以看到i和f的值有损坏，因为最后赋值的变量占用了内存位置。再看一个实例，这次我们在同一时间只是用一个变量，这也演示了共同体的主要目的：
```c
#include <stdio.h>
#include <string.h>

union Data {
    int i;
    float f;
    char str[20];
};
int main()
{
    union Data data;
    data.i = 10;
    printf("data.i : %d\n", data.i);
    
    data.f = 220.5;
    printf("data.f : %f\n", data.f);
    
    strcpy( data.str, "C Programming");
    printf("data.str : %s\n", data.str);
    
    return 0;
}
```
#### typedef
c语言提供了typedef关键字，您可以使用它为类型取一个新的名字。下面的实例位单字节数字定义了一个术语**BYTE**:
```c
typedef unsigned char BYTE;
```
在这个类型定义后，标识符BYTE可作为类型**unsigned char**的缩写，例如：
```c
BYTE b1, b2;
```
按照惯例，定义使用大写字母。

还可以给自定义数据类型取一个新的名字。实例：
```c
#include <stdio.h>
#include <string.h>

typedef struct Books
{
    char title[50];
    char author[50];
    char subject[100];
    int book_id;
} Book;

int main()
{
    Book book;
    
    strcpy( book.title, "C 教程");
    strcpy( book.author, "Runoob");
    strcpy( book.subject, "编程语言");
    book.book_id = 12345;
    
    printf("书标题 ：%s\n", book.title);
    printf("书作者 ：%s\n", book.author);
    printf("书类目 ：%s\n", book.subject);
    printf("书ID ：%d\n", book.book_id);
    
    return 0;
}
```
*typedef vs #define*

\#define 是c指令，用于位各类数据类型定义别名，与typedef类似，但是有以下几点不同：
- typedef 仅限于为类型定义符号名称，#define 不仅可以为类型定义别名，也能为数值定义别名，比如您可以定义1   为ONE
- typedef是由编译器执行解释的，#define语句是由预编译器进行处理的。
实例：
```c
#include <stdio.h>

#define TRUE 1
#define FALSE 0

int main(){
    printf("TRUE 的值为：%d\n", TRUE);
    printf("FALSE 的值为：%d\n", FALSE);
    
    return 0;
}
```
#### 输入&输出
*标准文件*
c语言把所有的设备当作文件，所以设备被处理的方式与文件相同。以下三个文件会在程序执行时自动打开，以便访问键盘和屏幕。
  标准文件 文件指针 设备
- 标准输入 stdin 键盘
- 标准输出 stdout 屏幕
- 标准错误 stderr 您的屏幕
实例：
```c
#include <stdio.h>
int main()
{
    printf("Hello world!");
    return 0;
}
```
’%d‘来匹配整型变量，‘%f’格式化输出浮点型数据
```c
#include <stdio.h>
int main()
{
    float f;
    printf("Enter a number:");
    scanf("%f", &f);
    printf("Value = %f", f);
    return 0;
}
```
*getchar()&putchar()函数*
int getchar(void): 函数从屏幕读取下一下可用的字符，并把它返回为一个整数。这个函数在同一个时间内只会读取一个单一的字符。可以在循环内使用这个方法，以便从屏幕中读取多个字符。
int putchar(void): 函数把字符输出到屏幕上，并返回相同的字符。这个函数在同一个时间内只会输出一个单一的字符。可以在循环中使用这个方法，在屏幕上输出多个字符。
实例：
```c
#include <stdio.h>
int main()
{
    int c;
    printf("Enter a value:");
    c = getchar();
    
    printf("\nYou entered:");
    putchar(c);
    printf("\n");
    return 0;
}
```
*scanf()和printf()*

int scanf(const char *format, ...) 函数从标准输入流stdin读取输入，并根据提供的format来浏览输入。
int printf(const chat *format, ...) 函数把输出写入到标准输出流stdout，并根据提供的格式产生输出。

format可以是一个简单的常量字符串，到那时您可以分别指定%s %d %c %f等来输出或读取字符串、整数、字符或浮点数。还有许多其它可用的格式选项，可以根据需要使用。如需了解完整的细节，可以查看这些函数的参考手册。
```c
#include <stdio.h>
int main()
{
    char str[100];
    int i;
    
    printf("Enter a value:");
    scanf("%s %d", str, &i);
    
    printf("\nYou entered: %s %d", str, i);
    printf("\n");
    return 0;
}
```
读取字符串的时候，只要遇到空格就会停止读取。

#### 文件读写

*打开文件*

可以使用**fopen()**函数来创建一个新的文件或者打开一个已有的文件，这个调用会初始化FILE的一个对象，类型FILE包含了所有用来控制流的必要的信息。函数调用原型如下：
```c
FILE *fopen(const char * filename, const char *mode);
```
在这里，filename是字符串，用来命名文件，访问模式mode的值可以是下列值中的一个：
- r：打开一个已有的文本文件，允许读取文件
- w：打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件，从开头写入内容
- a：打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件
- r+：打开一个文本文件，允许读写文件
- w+：打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则创建
- a+：打开一个文本文件，允许读写文件。如果文件不存在，则创建一个新文件。读取从开头开始，写入则只能是追加模式
如果处理的是二进制文件，则需要使用下面的访问方式来取代上面的访问模式：
```c
"rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"
```
*关闭文件*
```c
int fclose(FILE *fp)
```
如果成功关闭文件，fclose()函数返回零，若关闭文件发生错误，函数返回EOF。这个函数实际上，会清空缓冲区中的数据，关闭文件，并释放用于该文件的所有内存。EOF是一个定义在文件stdio.h中的常量。
c标准库提供了各种函数来按字符或者以固定长度字符串的形式读写文件。

*写入文件*
```c
int fputc(int c, FILE *fp);
```
函数fputc()把参数c的字符值写入到fp所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回EOF。可以使用下面的函数来把一个以null结尾的字符串写入到流中：
```c
int fputs( const char *s, FILE *fp );
```
函数*fputs*把字符串s写入到fp所指向的输出流中。如果写入成功，返回一个非负值，如果发生错误，则会返回EOF。您也可以使用int fprintf(FILE *fp, const char *format, ...)函数来把一个字符串写入到文件中。实例：
```c
#include <stdio.h>
int main()
{
    FILE *fp = NULL;
    
    fp = fopen("/tmp/test.txt", "w+");
    fprintf(fp, "This is testing for fprintf...\n");
    fputs("This is testing for fputs...\n", fp);
    fclose(fp);
}
```
*读取文件*

读取单个字符：
```c
int fgetc( FILE *fp);
```
**fgetc()**函数从fp所指向的输入文件中读取一个字符。返回值是读取的字符，如果发生错误返回EOF。下面的函数允许从流中读取一个字符：
```c
char *fgets(char *buf, int n, FILE *fp);
```
函数**fgets()**从fp所指向的输入流中读取n-1个字符。他会把读取的字符串复制到缓冲区buf，并在最后加上一个null字符来终止字符串。

如果这个函数在读取最后一个字符之前就遇到一个换行符"\n"或文件末尾EOF，则只会返回读取到的字符，包括换行符。所以可以使用int fscanf(FILE *fp, const char *format, ...)函数来从文件中读取字符串，但是在遇到第一个空格字符时，它会停止读取。实例：
```c
#include <stdio.h>
int main()
{
    FILE *fp = NULL;
    char buff[255];
    
    fp = fopen("/tmp/test.txt", "r");
    fscanf(fp, "%s", buff);
    printf("1 : %s\n", buff);
    
    fgets(buff, 255, (FILE *)fp);
    printf("2 : %s\n", buff);
    
    fgets(buff, 255, (FILE *)fp);
    printf("3 : %s\n", buff);
    fclose(fp);
}
```
*二进制I／O函数*
下面两个函数用于二进制的输入和输出：
```c
size_t fread(void *ptr, size_t size_elements, size_t number_of_elements, FILE *a_file);

size_t fwrite(const void *ptr, size_t size_of_elements, size_t number_of_elements, FILE *a_file);
```
这两个函数都是用于存储块的书写-通常是数组或结构体

#### 预处理器

**c预处理器**不是编译器的组成部分，但它是编译过程中的一个单独步骤，c预处理器只不过是一个文本替换工具而已，它们会指示编译器在实际编译之前完成所需的预处理。我们把c预处理器简写为CPP

所有的预处理器命令都是以#开头。它必须是第一个非空字符，为了增强可读性，预处理指令应从第一列开始。所有重要的预处理器指令：
- #define 定义宏
- #include 包含一个源代码文件
- #undef 取消已定义的宏
- #ifdef 如果宏已经定义，则返回真
- #if 如果给定条件为真，则编译下面代码
- #else #if的替代方案
- #endif 结束一个#if...#else条件编译块
- #error 当遇到标准错误时，输出错误信息
- #prama 使用标准化方法，向编译器发布特殊的命令到编译器中
实例：
```c
#define MAX_ARRAY_LENGTH 20
```
这个指令会告诉cpp把所有的MAX\_ARRAY\_LENGTH替换为20。使用#define定义常量增加可读性
```c
#include <stdio.h>
#include "myheader.h"
```
这些指令告诉cpp从系统库中获取stdio.h，并添加文本到当前的源文件中。下一行告诉cpp从本地目录中获取myheader.h，并添加内容到当前的源文件中。
```c
#undef FILE_SIZE
#define FILE_SIZE 42
```
取消原来的FILE_SIZE定义，并重新定义
```c
#ifndef MESSAGE
  #define MESSAGE "you wish"
#endif
```
这个指令告诉cpp只有当MESSAGE未定义时，才定义
```c
#ifdef DEBUG
  /* debugging statements */
#endif
```
这个指令告诉cpp如果定义了DEBUG，则执行处理语句。如果向gcc编译器传递了DEBUG开关量，这个指令就很有用，可以在编译期间随时开关调试。
*预定义宏*
ANSI C 定义了许多宏。在编译中可以直接使用这些宏，但不能直接修改这些预定义的宏。
- \_DATE\_ 当前日期，一个以“MMM DD YYYY”格式表示的字符常量。
- \_TIME\_ 当前时间，一个以"HH:MM:SS"格式表示的字符变量。
- \_FILE\_ 这会包含当前文件名，一个字符串变量
- \_line\_ 这会包含当前行号，一个十进制常量。
- \_STDC\_ 当编译器以ANSI标准编译时，则定义为1.
实例：
```c
#include <stdio.h>

main(){
    printf("File : %s\n", __FILE__);
    printf("Date : %s\n", __DATE__);
    printf("Time : %s\n", __TIME__);
    printf("Line : %d\n", __LINE__);
    printf("ANSI : %d\n", __STDC__);
}
```
*预处理齐运算符*

c预处理器提供了下列的运算符帮助您创建宏：

宏延续运算符(\)

一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下，则使用宏延续运算符(\)，例：
```c
#define message_for(a, b)\
  printf(#a " and " #b ": We love you!\n")
```
字符串常量化运算符#

在宏定义中，当需要把一个宏的参数转换为字符串常量时，则使用字符串常量化运算符（#）。在宏中使用的该运算符有一个特定的参数或参数列表。例：
```c
#include <stdio.h>
#define message_for(a, b)  \
printf(#a " and " #b ": We love you!\n")

int main(void){
    message_for(Carole, Debra);
    return 0;
}
```
标记粘贴运算符（##）

宏定义内的标记粘贴运算符（##）会合并两个参数。它允许在宏定义重两个独立的标记被合并为一个标记。例：
```c
#include <stdio.h>
#define tokenpaster(n) printf("token" #n " = %d", token##n)

int main(void){
  int token34 = 40;
  tokenpaster(34);
  return 0;
}
```
defined()运算符

预处理器defined运算符是用在常量表达式中的，用来确定一个标识符是否已使用#define定义过。如果指定的标识符已定义，则值为真（非零）。如果指定的标识符未定义，则值为假（零）。实例:
```c
#include <stdio.h>

#if !defined(MESSAGE)
#define MESSAGE "You wish!"
#endif

int main(void){
    printf("Here is the message: %s\n", MESSAGE);
    return 0;
}
```
*参数化的宏*

cpp一个强大的功能时可以使用参数化的宏来模拟函数。例：
```c
  int square(int x){
    return x*x;
  }
```
可重写为
```c
#define square(x) ((x) * (x))
```
在使用带有参数的宏之前，必须使用#define指令定义。参数列表时括在圆括号内，且必须紧跟在宏名称后面。宏名称和左圆括号之间不允许有空格，例：
```c
#include <stdio.h>

#define MAX(x,y) ((x) > (y) ? (x) : (y))
int main(void){
    printf("Max between 20 and 10 is %d\n", MAX(10, 20));
    return 0;
}
```

#### 头文件

头文件是扩展名为.h的文件，包含了c函数声明和宏定义，被多个源文件引用共享。有两种头文件：程序员编写的头文件和编译器自带的头文件。

在程序中要使用头文件，需要使用c预处理指令#include来引用她。前面我们已经看过stdio.h头文件，它是编译器自带的头文件。

建议把所有的常量、宏、系统全局变量和函数原型写在头文件中，在需要的时候随时引用这些头文件。

*引用头文件的语法*

第一种：
```c
#include <file>
```
这种模式适用于引用系统文件。它在系统目录的标准列表里搜索名为file的文件。在编译源代码的时候，可以通过-l选项把目录前置在该列表前。第二种：
```c
#include "file"
```
这种形式适用于引用用户头文件。它在包涵当前文件爱的呢目录中搜索名为file的文件。在编译的时候，可以通过-l选项前置在该列表前。

*引用头文件的操作*

\#include指令会指示c预处理器浏览指定的文件作为输入。预处理器的输出包含了已经生成的输出，被引用文件生成的输出以及#include指令之后的文本输出。例如：
```c
char *test (void);
```
使用了头文件的*program.c*，如下：
```c
int x;
#include "header.h"

int main(void)
{
  puts(test());
}
```
编译器会看到如下的令牌流：
```c
int x;
char *test(void);

int main(void)
{
  puts(test());
}
```
*只引用一次头文件*

如果一个头文件被引用两次，编译器会处理两次头文件的内容，这将产生错误。为了防止这种错误，标准的做法是把整个文件内容放在条件编译语句中，如下：
```c
#ifdef HEADER_FILE
#define HEADER_FILE
header file
#endif
```
*有条件引用*

多个头文件中选择一个引用到程序中，例：
```c
#if SYSTEM_1
  #include "system_1.h"
#elif SYSTEM_3
  #include "system_2.h"
#elif SYSTEM_3
  ...
#endif
```
头文件比较多的时候，这么做是很麻烦的，预处理器使用宏来定义头文件的名称。这就是所谓的有条件饮用。它不是使用头文件的名称作为#include的直接参数，您只需要使用宏名称代替即可：
```c
#define SYSTEM_H "system_1.h"
...
#include SYSTEM_H
```
SYSTEM\_H会扩展，预处理器会查找system\_1.h。SYSTEM\_H可通过-D选项被Makefile定义。

#### 强制类型转换
强制类型转换符可以显式地从一种类型转换为另一种类型，如下所示：
```c
(type_name) expression
```
实例：
```c
#include <stdio.h>

int main(void){
    int sum = 17, count = 5;
    double mean;
    
    mean = (double) sum / count;
    printf("Value of mean: %f\n", mean);
    return 0;
}
```
强制类型转换符的优先级大于除法。类型转换可以是隐式的，也可以是显式的，通过使用强制类型转换符指定。在编程时，有需要类型转换的时候都用上强制类型转换运算符，是一种良好的编程习惯。

*整数提升*

整数提升是指把小于int或unsigned int的整数类型转换为int或unsigned int的过程。请看下面的实例，在int中添加一个字符：
```c
#include <stdio.h>

int main(void){
    int i = 17;
    char c = 'c';/* ascii值为99 */
    int sum;
    
    sum = i + c;
    printf("Value of sum : %d\n", sum);
}
```
*常用的算术转换*

常用的算术转换是隐式地把值强制转换为相同的类型。编译器首先进行整数提升，如果操作数类型不同，则它们会被转换为下列层次中出现的最高层次的类型，从右往左依次升高

int -> unsigned int -> long -> long long -> unsigned long -> float -> double -> long double

常用的算术转换不适用于赋值运算符、逻辑运算符&&和||。实例：
```c
#include <stdio.h>

int main(void){
    int i = 17;
    char c = 'c';/* ascii值为99 */
    float sum;
    
    sum = i + c;
    printf("Value of sum : %f\n", sum);
}
```
在这里c首先被转换为整数，但是最后的值是double型的，编译器会把i和c转换为浮点型，然后相加。

#### 错误处理

c语言不提供对错误处理的直接支持，但是作为一种系统编程语言，它以返回值的形式允许您访问底层数据。在发生错误时，大多数的c或UNIX函数调用返回1或NULL，同时会设置一个错误代码errno，该错误代码是全局变量，表示在函数调用期间发生了错误。可以在<error.h>头文件中找到各种各样的错误代码。

所以，c程序员可以通过检查返回值，然后根据返回值决定采取哪种适当的动作。开发人员应该在程序初始化时，把errno设置为0，这是一种良好的编程习惯，0表示程序中没有错误。

*errno、perror()和strerror()*

c语言提供了perror()和strerror()函数来显示与errno相关的文本消息。
- perror()函数显示传给它的字符串，后跟一个冒号、一个空格和当前errno值的文本表示形式。
- strerror()函数，返回一个指针，指针指向当前errno值的文本表示形式

模拟一种情况，尝试打开一个不存在的文件。这里我们使用函数来演示用法。另外一点需要注意，应该使用stderr文件流来输出所有的错误。例：
```c
#include <stdio.h>
#include <errno.h>
#include <string.h>

extern int errno;

int main(){
    FILE *pf;
    int errnum;
    pf = fopen("unexist.txt", "rb");
    if(pf == NULL)
    {
        errnum = errno;
        fprintf(stderr, "错误号：%d\n", errno);
        perror("通过perror输出错误");
        fprintf(stderr, "打开文件错误：%s\n", strerror(errnum));
    }
    else
    {
        fclose(pf);
    }
    return 0;
}
```
*被零除的错误*

在进行除法运算时，如果不检查除数是否为零，则会导致一个运行时错误。

为了避免这种情况发生，下面的代码在进行除法运算前会先检查除数是否为零：
```c
#include <stdio.h>
#include <stdlib.h>

main(){
    int dividend = 20;
    int divisor = 0;
    int quotient;
    
    if( divisor == 0){
        fprintf(stderr, "除数为 0 退出运行...\n");
        exit(-1);
    }
    quotient = dividend / divisor;
    fprintf(stderr, "quotient 变量的值为：%d\n", quotient);
    
    exit(0);
}
```
*程序退出状态*

通常情况下，程序成功执行完一个操作正常退出的时候会带有值EXIT\_SUCCESS。在这里，EXIT\_SUCCESS是宏，他被定义为0。

如果程序中存在一种错误情况，当您退出程序时，会带有状态值EXIT_FAILURE，被定义为-1。所以，上面的程序可以写为：
```c
#include <stdio.h>
#include <stdlib.h>

main(){
    int dividend = 20;
    int divisor = 5;
    int quotient;
    
    if( divisor == 0){
        fprintf(stderr, "除数为 0 退出运行...\n");
        exit(EXIT_FAILURE);
    }
    quotient = dividend / divisor;
    fprintf(stderr, "quotient 变量的值为：%d\n", quotient);
    
    exit(EXIT_SUCCESS);
}
```
#### 递归
递归指的在函数的定义中使用函数自身的方法。
语法格式如下：
```c
void recursion()
{
  recursion(); /* 函数调用自身 */
}
int main(){
  recursion();
}
```
c语言支持递归，即一个函数可以调用其自身。但在使用递归时，程序员需要注意定义一个从函数退出的条件，否则会进入死循环。

递归在解决许多数学问题上起了至关重要的作用，比如计算一个数的阶乘、生成斐波那契数列，等等。

*数的阶乘*
```c
#include <stdio.h>

double factorial(unsigned int i){
    if(i <= 1){
        return  1;
    }
    return  i*factorial(i-1);
}
int main(){
    int i = 15;
    printf("%d 的阶乘为 %f\n", i, factorial(i));
    return 0;
}
```
*斐波那契数列*
```c
#include <stdio.h>

int fibonaci(int i){
    if(i == 0){
        return 0;
    }
    if(i == 1){
        return 1;
    }
    return fibonaci(i-1) + fibonaci(i-2);
}
int main(){
    int i;
    for (i = 0; i < 10; i++) {
        printf("%d\t\n", fibonaci(i));
    }
    return 0;
}
```
> 能用数学归纳法解决的问题理论上都能用递归进行表达，递归能解决的问题也可以用循环解决，不过表达要麻烦一点

*可变参数*

有时，您可能会碰到这样的情况，您希望函数带有可变数量的参数，而不是预定义数量的参数。c语言为这种情况提供了一个解决方案，它允许您定义一个函数，能根据具体的需求接受可变数量的参数。实例：
```c
int func(int, ...){
  .
  .
  .
}
int main(){
  func(1,2,3);
  func(1,2,3,4);
}
```
请注意，函数func()最后一个参数写成省略号，即三个点号(...)，省略号之前的那个参数时int，代表了要传递的可变参数的总数。为了使用这个功能，，您需要使用stdarg.h头文件，该文件提供了实现可变参数功能的函数和宏。具体步骤：
- 定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数
- 在函数定义中创建一个va_list类型变量，该类型是在stdarg.h头文件中定义的
- 使用int参数和va\_start宏来初始化va\_list变量为一个参数列表。宏va_start是在stdarg.h头文件中定义的
- 使用va\_arg宏和va\_list变量来访问参数列表中国年的每个项
- 使用va\_end来清理赋予va\_list变量的内存
实例：
```c
#include <stdio.h>
#include <stdarg.h>

double average(int num,...)
{
    
    va_list valist;
    double sum = 0.0;
    int i;
    
    /* 为 num 个参数初始化 valist */
    va_start(valist, num);
    
    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
        sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);
    
    return sum/num;
}

int main()
{
    printf("Average of 2, 3, 4, 5 = %f\n", average(4, 2,3,4,5));
    printf("Average of 5, 10, 15 = %f\n", average(3, 5,10,15));
}
```
#### 内存管理
c语言为内存的分配和管理提供了几个函数。这些函数可以在<stdlib.h>头文件中找到。
1. void *calloc(int num, int size): 在内存中动态的分配num个长度为size的连续空间，并将每个字节都初始化为0。所以结果是分配了num\*size个字节长度的内存空间，并且每个字节的值都是0。
2. void free(void *address): 该函数释放address所指向的内存块，释放的是动态分配的内存空间
3. void *malloc(int num): 在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后并不会被初始化，它们的值是未知的。
4. void *realloc(void *address, int newsize): 该函数重新分配内存，把内存扩展到newsize。

*动态分配内存*

编程的时候，如果预先知道数组的大小，那么定义数组就比较容易。例：
```c
char name[100]
```
但是，如果您预先不知道需要存储的文本长度，例如存储一个主题的详细描述。在这里，我们需要定义一个指针，该指针指向未定义所需内存大小的字符，后续再根据需求来分配内存，如下所示：
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    char name[100];
    char *description;
    
    strcpy(name, "Zara Ali");
    
    /* 动态分配内存 */
    description = malloc( 200*sizeof(char));
    if(description == NULL){
        fprintf(stderr, "Error - unable to allocate required memory\n");
    }
    else {
        strcpy(description, "Zara ali a DPS student in class 10th");
    }
    printf("Name = %s\n", name);
    printf("Description: %s\n", description);
}
```
用calloc()来编写：
```c
calloc(200, sizeof(char));
```
动态分配内存的时候，我们有完全的控制权，可以传递任何大小的值，但是预定义了大小的数组，一旦定义则无法改变大小

*重新调整内存的大小和释放内存*

当程序退出时，操作系统会自动释放所有分配给程序的内存，但是，最好在不需要内存的时候，调用free()函数来释放内存。或者调用realloc()来增加或减少已分配的内存大小。实例：
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main()
{
    char name[100];
    char *description;
    
    strcpy(name, "Zara Ali");
    
    /* 动态分配内存 */
    description = malloc( 30*sizeof(char));
    if(description == NULL){
        fprintf(stderr, "Error - unable to allocate required memory\n");
    }
    else {
        strcpy(description, "Zara ali a DPS student.");
    }
    /* 假设想要存储更大的描述信息 */
    description = realloc(description, 100*sizeof(char));
    if(description == NULL){
        fprintf(stderr, "Error - unable to allocate required memory\n");
    } else {
        strcat(description, "She is in class 10th");
    }
    printf("Name = %s\n", name);
    printf("Description: %s\n", description);
    /* 使用free()函数释放内存 */
    free(description);
}
```
> 理论上不重新分配额外的内存，strcat() 函数会生成一个错误，因为存储 description 时可用的内存不足。但是实际操作的时候，即使不够，也不报错

#### 命令行参数
执行程序时，可以从命令行传值给c程序。这些值被称为命令行参数，它们对程序很重要，特别是当您想从外部控制程序，而不是在代码内对这些值进行硬编码时，就显得尤为重要了。

命令行参数是使用main()函数参数来处理的，其中，argc是指传入参数的个数，argv[]是一个指针数组，指向传递给程序的每个参数。实例：
```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    if(argc == 2){
        printf("The argument supplied is %s\n", argv[1]);
    }
    else if(argc > 2){
        printf("Too many arguments supplied.\n");
    }
    else {
        printf("One argument expected.\n");
    }
}
```
应当指出的是，argv[0]存储程序的名称，argv[1]是一个指向第一个命令行参数的指针，*agrv[n]是最后一个参数。如果没有提供任何参数，argc将为1，否则，如果传递了一个参数，argc将被设置为2。

多个命令行参数之间用空格分隔，但是如果参数本身带有空格，那么传递参数的时候应把参数放置在双引号或单引号内部。实例：
```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    printf("Program name %s\n", argv[0]);
    if(argc == 2){
        printf("The argument supplied is %s\n", argv[1]);
    }
    else if(argc > 2){
        printf("Too many arguments supplied.\n");
    }
    else {
        printf("One argument expected.\n");
    }
}
```

#### 体会

- 字符数组名是一个常量指针，不可操作
- 字符串指针存放的是字符串常量的地址，所以可以进行操作
- 指针的+1并非是值+1而是指向内存的位置+1