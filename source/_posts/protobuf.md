---
title: Protocol Buffers 基础介绍
date: 2017-07-08 18:17:17
tags: 微服务
---

### 引子

《诗》云：“穆穆文王，於缉熙敬止！”为人君，止于仁；为人臣，止于敬；为人子，止于孝；为人父，止于慈；与国人交，止于信。

-- 《大学》

### Protocol Buffers概览

#### 什么是Protocol Buffers

Protocol Buffers是谷歌开发的，跨语言跨平台的一种机制，目的是序列化结构化的数据(数据比xml更小，更快，更简单)。一旦你定义好了你想要的数据结构，那么就可以通过生成的代码方便的读和写你的结构化数据。这些结构化的数据可以从不同的数据流中读和写，并且支持使用不同的语言。你甚至可以在保证现有程序不被破坏的情况下，更新数据结构。

#### Protocol Buffers是怎么工作的

你具体在.proto文件中定义你想要的序列化的数据结构。每个proto buffer message 都是一条小而富有逻辑的信息记录，包含一些列的名值对。下面是一个示例：
```
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WORK = 2;
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}
```
一旦你定义好了你的messages，运行相应语言编译器，生成messages对应的classes。这些类为每一个字段提供一些基本的入口（就像name(),set_name()）并且同时提供提供序列化／反序列化整个结构原生字节的方法。假设我们现在用的语言是C++，运行了编译器之后，会生成一个名为Person的类。你之后可以在你的应用中使用这些类来设置，序列化，和取回Person protocol buffer messages。然后你可以写类似如下的代码：
```
Person person;
person.set_name("John Doe");
person.set_id(1234);
person.set_email("jdoe@example.com");
fstream output("myfile", ios::out | ios::binary);
person.SerializeToOstream(&output);
```
之后，你可以取回你的信息：
```
fstream input("myfile", ios::in | ios::binary);
Person person;
person.ParseFromIstream(&input);
cout << "Name: " << person.name() << endl;
cout << "E-mail: " << person.email() << endl;
```
*你可以在message模式上增加字段，这样并不会破坏背后的兼容性，旧的二进制仅仅会在转化的时候忽略掉新的字段，你可以拓展的协议，并且不用担心破坏现存代码。*

注：完整的使用protocol buffer code的参考资料，protocol编码的原理

#### 为什么不用xml
Protocol buffers在序列化结构数据上比xml有很多优势：
- 更兼点
- 大小是xml的1/10到1/3
- 速度比xml快20到100倍
- 歧义更少
- 生成数据对应的类，方便程序调用
举例：假设我们需要的一个拥有email和name的person模型。在XML中，你需要这样定义：
```
  <person>
    <name>John Doe</name>
    <email>jdoe@example.com</email>
  </person>
```
对应的protocol buffer message（protocol buffer 文本模式）：
```
# Textual representation of a protocol buffer.
# This is *not* the binary format used on the wire.
person {
  name: "John Doe"
  email: "jdoe@example.com"
}
```
当这个message被编码为protocol buffer 二进制模式时（文本模式只是方便人们调试，编写和阅读的描述）后，仅仅只有28个字节并且转换只需要100到200纳秒。xml本本至少有69个字节，并且转换需要5000到10000纳秒。

操作一个protocol buffer更简便：
```
cout << "Name: " << person.name() << endl;
cout << "E-mail: " << person.email() << endl;
```
然而在xml中你要这样操作：
```
cout << "Name: "
      << person.getElementsByTagName("name")->item(0)->innerText()
      << endl;
cout << "E-mail: "
      << person.getElementsByTagName("email")->item(0)->innerText()
      << endl;
```
然而，protocol buffers并不总是强于xml：
- protocol buffers并不适合描述一个文档模型（比如html），因为你不能直接插入带有文本的结构
- xml人类可读且可编辑，protcol buffers原生模式下并不是
- xml在某种程度是自解释的，protocol buffer只有在拥有了message定义（.proto 文件）之后才是具有意义的。
#### 如何开始

*下载包，跟随指导*

#### proto3介绍
proto3 简化了protocol buffer语言，不仅变得更好用，而且支持了更多的语言： Java, C++, Python, Java Lite, Ruby, JavaScript, Objective-C, and C#。同时你可以使用最新的Go protoc插件生成go的proto3代码，可以在*golang/protobuf*仓库中获取。

两个版本api并不兼容。

可以在*release notes*中查看两者不同之处。可以在*proto3 指引*中学习proto3语法。

注：谷歌开源的时候已经是第二版本了，所以默认为proto2

#### 一点历史

Protocol buffers最开始是谷歌要处理request/response协议而研发的。在protocol buffer之前，存在一个requests和responses模式用来手工处理解析和反解析requests和responses，并且支持模式的不同版本。结果导致了丑陋的代码如下：
```
 if (version == 3) {
   ...
 } else if (version > 4) {
   if (version == 5) {
     ...
   }
   ...
 }
```
明确格式化的协议会使新的协议版本首次使用变得很复杂，因为开发者必须确认在请求的发起者到处理请求的服务者之前所有的服务方都要可以迅速地切换到新的协议。

Protocol buffers被用来解决这些问题：
- 新的字段可以很容易地被引入，中间服务层不会检查数据，可以简单的转换它和传递它，不需要知道所有的字段。
- 这些格式完全是自解释的，可以被多种语言处理。

然而，用户依然要手写他们的转化代码。

随着系统的进化，它增加了一系列的特性和用途。

- 自动生成序列化和反序列化的代码，避免了手工转换
- 为了在短生命周期的rpc请求上使用，人们开始使用protocol buffers作为一个持久储存数据的好用的自解释格式。
- 服务端的rpc接口开始成为protocol文件声明的一部分，用户可以使用服务接口的真实实现覆盖协议编译生成的根类。

Protocol buffers 现在已经成为书写数据的通用语言，目前有48,162个不同的message类型定义于12,183个不同的.proto文件中。它们不仅仅用于rpc系统，同时也用于在不同的存储系统上持续的存储数据。
