---
title: javascript简介 - 第一章
date: 2017-06-26 19:16:59
tags: js
---
### 引子

知止而后有定，定而后能静，静而后能安，安而后能虑，虑而后能得。物有本末，事有终始。知所先后，则近道矣！

-- 大学

### javascript简史

诞生于1995年，1997年ECMA（欧洲计算机制造商协会）制定ECMA-262标准

### javascript实现
- 核心：ECMAScript
- 文档对象模型：DOM
- 浏览器对象模型：BOM

#### ECMAScript

- 语法
- 类型
- 语句
- 关键字
- 保留字
- 操作符
- 对象

#### 文档对象模型(DOM)

DOM1: DOM核型(DOMcore)和DOM HTML。DOM核心规定的是如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作；DOM HTML则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法。

DOM2: DOM2级在原来的DOM的基础上又扩充了(DHTML一直都支持)鼠标和用户界面事件，范围，遍历（迭代DOM文档的方法）等细分模块，而且通过对象接口增加了对css的支持。

##### DOM2新增模块
- DOM视图(DOM Views)：定义了跟踪不同文档（定义了跟踪不同文档，应用css之前，应用css之后）视图的接口
- DOM事件(DOM Events)：定义了事件和事件处理的接口
- DOM样式(DOM Style)：定义了基于css为元素应用样式的接口
- DOM范围和遍历(DOM Travalsal and Range)：定义了遍历和操作文档的接口

##### DOM3模块
- DOM加载和保存模块(DOM Load and Save)：统一方式加载和保存文档的方法
- DOM验证模块(DOM Validation)：验证文档的方法

##### 其他DOM标准

- SVG1.0
- MathML1.0
- SMIL

#### 浏览器对象模型(BOM)

- 弹出新浏览器窗口的功能
- 移动、缩放和关闭浏览器窗口的功能
- 提供浏览器详细信息的navigator对象
- 提供浏览器所加载页面详细信息的location对象
- 提供用户分辨率详细信息的screen对象
- 对cookie的支持
- 像XMLHttpRequest和ActionXObject这样的自定义对象


