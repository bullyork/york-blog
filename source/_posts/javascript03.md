---
title: 基本概念
date: 2017-06-27 16:50:20
tags: js
---
### 引子

汤之盘铭曰：“苟日新，日日新，又日新。”《康诰》曰：“作新民。”《诗》曰：“周虽旧邦，其命维新。”是故君子无所不用其极。

--《大学》

### 语法

- 区分大小写：变量名区分大小写
- 标识符
  + 第一个字母必须是：字母，下划线，或美元符号($)
  + 其他字符可以是字母、下划线、美元符号或数字
  + 标识符中也可以包含扩展的ACII字符或Unicode字符，但我们不推荐这么做，惯例上ECMAScript标识符采用驼峰大小写格式
- 注释：采用c风格注释
  + 单行：// 注释内容
  + 多行：/* 注释内容 */
- 严格模式：严格模式为javascript定义了一种不同的解析和执行模型。在严格模式下，ECMAScript3中的一些不确定行为将得到处理，而且某些不安全的操作也会抛出错误。整个脚本启用严格模式，可以在顶部加上代码：'use strict';也可指定某函数：function doSomething(){'use strict' //函数体}
- 语句：ECMAScript中语句以一个分号为结尾；省略分号，则由解析器确定语句的末尾。

### 关键字和保留字
ECMA-262关键字：

break,do,instanceof,typeof,case,else,new,var,catch,finally,return,void,continue,for,switch,while,debugger,
function,this,with,default,if,throw,delete,in,try

ECMA-262保留字：

abstract,enum,int,short,boolean,export,interface,static,byte,extends,long,super,char,final,native,synchronized,class,float,package,throws,const,goto,private,transient,implements,protected,volatile,double,import,public

第五版非严格模式保留字：

class,enum,extends,super,const,export,import

第五版严格模式下增加的保留字：

implements,package,public,interface,private,static,let,protected,yield

关键字和保留字虽然不能用做标识符，但是可以作为属性名使用，但不推荐。

除上述关键字和保留字，第五版还对eval和arguments做了限制，在严格模式下，这两个名字也不能作为标识符或属性名，否则会抛出错误。

### 变量
- 定义：var message
- 松散类型
- 使用var定义的变量将成为定义该变量作用域内的局部变量
- 省去var可以定义全局变量

### 数据类型

5种简单数据类型
- undefined
- null
- Boolean
- Number
- String

一种复杂数据类型
- Object

#### typeof操作符

- undefined 此值未定义
- boolean 此值为bool值
- string 此值为字符串
- number 此值为数值
- object 此值为对象
- function 此值为函数
- typeof null == 'object'

#### Undefined类型

Undefined类型只有一个值，undefined

包含undefined值的变量与尚未定义的变量是不一样的，尚未定义的变量直接调用会报错。

对尚未声明的变量只能执行一项操作，即使用typeof操作符检测其数据类型(对未经声明的变量调用delete不会导致错误，但这样做没有什么意义，而且严格模式下确实会导致错误)

尚未声明的变量调用typeof返回undefined

#### Null类型

Null类型只有一个值，null

从逻辑上讲，null值表示一个空对象指针

如果变量待保存的是对象，初始化的时候就应该明确的让变量保存null值

#### Boolean类型

Boolean类型只有两个值，true和false

要将一个值转换为其对应的Boolean值，可以调用函数Boolean()

转换规则(除Boolean类型本身):
- String类型，true - 任何非空字符串；false - 空字符串；
- Number类型，true - 任何非0数字值；false - 0和NaN；
- Object类型，true - 任何对象；false - null；
- Undefined类型，undefined - false

#### Number类型

数值字面量格式有十进制，八进制和十六进制

八进制第一位必须是0，然后是八进制数字序列(0-7)。如果数字序列中的值超出范围，那么前导0将会被忽略，后面数字将被当作十进制解析，八进制在严格模式下是不生效的，会导致javascript引擎抛除错误。

十六进制字面值的前两位必须是0x，后跟任何十六进制数字(0-9,A-F)，字母可小写。

进行算数计算的时候，所有八进制和十六进制数字都将被转换为十进制。

1. 浮点数值：所谓浮点数值，就是该数值中必须包含一个小数点，并且小数点后面至少要有一位数字。虽然小数点前面可以没有整数，但是不推荐。
    - 由于保存浮点数值需要的内存空间是保存整数数值的两倍，ECMAScript会不失时机地将浮点数转换为整数
    - 极大极小值，使用科学计数法，也叫e表示法。用e表示法表示的数值等于e前面的数值乘以10的指数次幂
    - 浮点数的最高精度为17位小数，但在进行算术计算时其精确度远远不如整数。0.1 + 0.2 = 0.30000000000000004(中间是15个0)

2. 数值范围：由于内存限制，ECMAScript并不能保存世界上的所有数值。
    - 最小值：Number.MIN_VALUE == 5e-324
    - 最大值：Number.MAX_VALUE == 1.7976931348623157e+308
    - 正无穷：超出最大值，Number.POSITIVE_INFINITY == Infinity
    - 负无穷：超出最小值，Number.NEGATIVE_INFINITY == -Infinity
    - isFinite：确定数值是否是无穷的，无穷返回false，有穷返回true
3. NaN：这个数值表示一个本来要返回数值的操作未返回数值的情况
    - 任何数值除以0返回NaN
    - 任何涉及NaN的操作返回NaN
    - NaN与任何值不等，包括自身
    - isNaN函数：函数接收一个值会尝试将这个值直接转换为数值，任何不能转换为数值的值都会导致这个函数返回true
    - isNaN也适用于对象，在基于对象调用isNaN函数时，会先调用对象的valueof方法，看此返回值能否转化为数值，如果不能，则基于这个返回值再调用toString方法，再测试返回值。这个过程同时也是ECMAScript中内置函数和操作符的一般执行流程。

4. 数值转换：Number(), parseInt(), parseFLoat()
    - Number：用于任何数据类型
        - Boolean：true&false - 1&0
        - Number：返回本身
        - Null：返回0
        - undefine：返回NaN
        - 如果是字符串，遵循规则：
            + 如果字符串中只包含数字(包含符号)，将其转换为十进制数字值
            + 如果字符串中包含有效的浮点格式，则将其转换为对应的浮点数值
            + 如果字符串中包含有效的十六进制格式数值，则将其转换为相同大小的十进制数值
            + 如果字符串是空的，则将其转换为0
            + 如果字符串中包含除上述格式之外的字符，则将其转换为NaN
        - 如果是对象，则调用其valueOf()方法，然后依照之前规则转换。如果转换为NaN，则调用对象toString()方法，然后再依照前面得规则转换
    - parseInt：更多的看其是否符合数值模式
        - 忽略字符串前面空格
        - 第一个字符不是数字或者正负号，返回NaN
        - 解析在遇到非数字字符结束
        - 可以识别不同格式(5中八进制被忽略)
        - 第二个参数可以传基数
    - parseFloat：主要特点是第一个小数点有效
        - 第一个小数点有效
        - 始终忽略前导0，没有第二个参数传基数的用法，始终只解析十进制
        - 如果能解析为整数，返回整数


