---
title: java_note_01
date: 2017-08-23 17:35:35
tags: java
---
### 引子

所谓修身在正其心者，身有所忿懥，则不得其正，有所恐惧，则不得其正，有所好乐，则不得其正，有所忧患，则不得其正。心不在焉，视而不见，听而不闻，食而不知其味。此谓修身在正其心。

--《大学》

### 基本语法

- 大小写敏感
- 类名：首写字母应该大写，驼峰命名
- 方法名：所有方法名都应该小写开头，驼峰命名
- 源文件名：源文件名必须和类名相同。当保存文件的时候，你应该使用类名作为文件保存（文件名和类名不同会导致编译错误！）
- 主方法入口：所有的java程序由public static void main(String []args)方法开始执行。

### java标识符

- 字母，美元符号，下划线开头
- 首字符之后可以是字母，美元符号，下划线或数字的任何字符组合
- 关键字不能用作标志符

### java修饰符

- 访问控制修饰符： default，public，protected，private
- 非访问控制修饰符：final，abstract，strictfp

### java变量

- 局部变量
- 类变量
- 成员变量

### java数组

数组是储存在堆上的对象，可以保存多个同类型变量

### java枚举

枚举类型限制变量只能是预先设定好的值。使用枚举可以减少代码中的bug。

### java关键字

abstract, assert, boolean, break, byte, case, catch, char, class, const, continue, default, do, double, else, enum, extends, final, finally, float, for, goto, if, implements, import, instanceof, int, interface, long, native, new, package, private, protected, public, return, short, static, strictfp, super, switch, synchronized, this, throw, throws, transient, try, void, volatile, while

### java注释

同时支持单行和多行注释

### 继承和接口

继承：一个类可以由其他类派神。如果你要创建一个类，而且已经存在一个类具有你所需要的属性或方法，那么你可以将新创建的类继承该类。

接口：接口可以理解为对象间相互通信的协议。接口在继承中扮演很重要的角色。接口只定义派生要用到的方法，但是方法的具体实现完全取决于派生类。