---
title: js语法解析探索
date: 2017-10-29 13:53:10
tags: js
---
### 引子

自诚明，谓之性；自明诚，谓之教。诚则明矣，明则诚矣。

--《中庸》

### 大纲

现在流行的es6 甚至 es7等特性的写法，通常并不被浏览器所认识，那么就需要一定的parser（转换器）将其转换为浏览器兼容性更好的的es5深知es3。

一种很自然而然的想法是将es6或者es7语法下写成的js代码解析为语法树，让后对应生成相应es5（es3）的代码。

eslint等语法检查就是基于这个原理做的

另外，很多操作，比如压缩，模块化等都需要解析原来的代码，从而进行相应的操作。还有，js并没有反射机制，如果要获取变量名本身字符串，还是需要解析源代码。

解析js最开始是 [esprima](https://github.com/jquery/esprima)

还有 [acorn](https://github.com/ternjs/acorn)

由于eslint用的是[espree](https://github.com/eslint/espree)，所以我们探索的大纲是：esprima --> acorn --> espree

#### esprima

esprima，bsd协议，是使用ECMAscript编写的高可用，符合标准的转换器，由Ariya Hidayat同其它贡献者共同完成。

特性：
- 对 ECMAScript 2017 完全支持（[ecma-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)）
- 由[ESTree project](https://github.com/estree/estree)标准化的[语法树模式](https://github.com/estree/estree/blob/master/es5.md#node-objects)
- 对jsx实验性的支持，React的语法扩展
- 可选的语法节点位置的记录（序号和行列）
- 已经被严格的测试（1500左右的[标准测试](https://github.com/jquery/esprima/tree/master/test/fixtures)）

API:

暂时不表，有点多，因为非常底层






