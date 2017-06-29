---
title: 在HTML中使用javascript
date: 2017-06-26 23:18:34
tags: js
---
### 引子

《康诰》曰：“克明德。”《太甲》曰：“顾諟天之明命。”《帝典》曰：“克明峻徳。”皆自明也。

--《大学》

### script元素

- async：表示应该立即下载脚本，但不阻塞页面其余caozuo
- charset：可选，指定src属性引入代码的字符集，大多数浏览器会忽略，所以很少人使用。
- defer：可选，表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本有效。IE7及之前版本，对嵌入脚本也生效。
- language：已废弃
- src：可选，表示包含要执行代码的外部文件
- type：可选，可看作language属性的替代属性，表示编写代码使用脚本语言的内容类型(也称为MIME类型)

#### 标签的位置

惯例是应该放在head元素中间，但这会导致script标签加载完之后，才会加载DOM内容，加载页面时会发生明显的延时。为了解决这一问题，一般把script标签防盗内容后面

#### 延迟脚本 

defer属性告诉浏览器立即加载，但延迟执行！  
如果有两个defer属性脚本，延迟脚本不一定会按顺序执行，也不一定会在DOMContentLoaded事件触发前执行，因此，最好

#### 异步脚本

async属性高诉浏览器立即下载，但不保证执行的先后顺序。一定会在页面的load事件之前执行，但可能在DOMContentLoaded事件之前或者之后执行

#### 在XHTML中的使用

```
<script type='text/javascript'>
  // <![CDATA[
    if(a < b){
      alert('a is less than b')
    }
  // ]]>
</script>
这样是为了保证所有现代浏览器都可以正常使用
```

#### 不推荐使用的方法

不要用html注释注释脚本！

#### 嵌入代码与外部文件

- 可维护性：所有js文件放到同一个文件夹中
- 可缓存：浏览器根据设置缓存链接的所有js文件，也就是说如果两个页面使用同一个文件，那么这个文件只需下载一次。因此最终结果就是能够加快页面加载的速度。
- 适应未来：通过外部文件来包含的javascript无须使用前面说过的hack技术，html和xhtml包含外部文件的语法是想通的

#### 文档模式

混杂模式：混杂模式会让IE的行为和ie5一致
标准模式：标准模式会让IE的行为更接近标准行为
准标准模式：很多行为符合标准，不标准的地方体现在处理突变间隙的时候

标准模式开启方法：
```
html4.01 严格型
<!DOCTYPE HTML PUBLIC "-//w3c//DTD HTML 4.01//EN"
"http://www.w3.org/TR/html4/strict.dtd">

xhtml1.0 严格型
<!DOCTYPE HTML PUBLIC "-//w3c//DTD XHTML 1.0 Strict//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```
准标准模式开启可以使用过渡型(transitional)或框架集型(framset)文档类型来触发

#### noscript元素

noscript包裹的元素只有在下列条件下才会显示

- 浏览器不支持脚本
- 浏览器支持脚本，但脚本被禁用



