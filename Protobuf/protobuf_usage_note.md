# Protobuf 使用说明

## 简介

Google Protocol Buffer (简称 Protobuf)是 Google 公司内部的混合语言数据标准，目前已经正在使用的有超过 48,162 种报文格式定义和超过 12,183 个 .proto 文件。他们用于 RPC 系统和持续数据存储系统。

RPC指远程过程调用，也就是说两台服务器A、B，一个应用部署在 A 服务器上，想要调用 B 服务器上应用提供的函数或方法，由于不再一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。

Protocol Buffers 是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。目前支持 C++、Java、Python、C#、Go 。

存储和交换正是 Protobuf 最有效的应用领域。

### 优点

Protobuf 的主要优点就是：

* 定义简单
* 速度快 （速度明显快速XML、JSON等其他类似技术）
* 语义更清晰

### 缺点

Protbuf 与 XML 相比也有不足之处：

* 功能简单，无法用来表示复杂的概念
* XML 已经成为多种行业标准的编写工具，Protobuf 只是 Google 公司内部使用的工具，在通用性上还差很多
* 不能直接手动编辑

关键词：序列化、反序列化

## protoc 编译

``` shell
protoc -I=$SRC_DIR --cpp_out=$DST_DIR $SRC_DIR/addressbook.proto
```

* `$SRC_DIR` 为 `.proto` 文件存放路径
* `$DST_DIR` 为输出文件存放路径

## protoc2 语法

### 简单例子引入

``` proto
syntax = "proto2";  // 指明正在使用proto2语法

message SearchRequest {
  required string query = 1;
  optional int32 page_number = 2;
  optional int32 result_per_page = 3;
}
```

* 文件的第一行指定了你正在使用proto2语法：如果你没有指定这个，编译器提示警告，除此之外还有proto3语法。这个指定语法行必须是文件的非空非注释的第一个行
* 每一个报文结构由 `message` 进行声明，类似于C/C++中的 struct
* 数据格式 SearchRequest 中的每一个字段分别对应一个类型和名称还有一个特定字段（ `required` 、`optional` 和 `repeated` ）
* 后面的数字表示分配标签，消息中的每一个标签定义一个独一无二的数字，一旦消息被使用，这些数字不能再更改，数字1到15决定了编码时占用1个字节，而16-2047占用2个字节。因此宜将小数字用于经常使用的字段。数字的范围为[1，2^29-1]，但19000-19999不可使用，为编译预留数字标签
* protoc在编码时采用键值对的方式编码
* 一个文件 `.proto` 可定义多个 message

### 字段修饰符

每一个消息字段需属于下列之一：

* `required` 格式正确的消息必须有一个这个字段，可在字段最后加 `[packet = true]` 获取更高的编码效率，例如

``` proto
repeated int32 samples = 4 [packet = true]
```

* `optional` 格式正确的消息可以有一个或者零个这样的消息，可在地段最后加入 `[default = 10]` 指定字段的默认值

``` proto
optional int32 result_per_page = 3 [default = 10];
```

* `repeated` 这个字段可以有任意多个，且字段值的顺序被保留

* `reseved` 表示消息中不能使用该声明的数字标签和字段名称，例如

``` proto
message Foo {
  reserved 2, 15, 9 to 11;
  reserved "foo", "bar";
}
```

* `enum` 枚举，如下，也可以在 message 外部定义 enum 结构，这样其他 message 也可以定义 enum 类型的字段，如果直接在其他 message 中使用某一 message 中定义的 enum，可通过 A.enumB 的方式进行访问

``` proto
message SearchRequest {
  required string query = 1;
  optional int32 page_number = 2;
  optional int32 result_per_page = 3 [default = 10];
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  optional Corpus corpus = 4 [default = UNIVERSAL];
}
```

### 字段类型

![protoc type](/_Resource/protobuf_01.png)

### 复合类型

* 在其他 message 中使用其他的 message，例如

``` proto
message SearchResponse {
  repeated Result result = 1;  // 使用下面的 Result 作为字段的类型
}

message Result {
  required string url = 1;
  optional string title = 2;
  repeated string snippets = 3;
}
```

另外还可以使用 `import` 导入其他 `.proto` 文件中的 message，例如

``` proto
import "myproject/other_protos.proto";
```

### `.proto` 文件

使用 `protoc` 编译器编译一个 `.proto` 文件的时候，编译器将生成相应语言的接口文件。
对于C++，将生成一个 `.h` 头文件和一个 `.cc` 源文件

## 参考

[1] [Google Protocol Buffer 的使用和原理](https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/index.html)
[2] [Protobuf 语言指南 （ proto 2 ）](http://blog.csdn.net/cchd0001/article/details/50669079)
