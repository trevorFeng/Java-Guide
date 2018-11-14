### 数据类型

数据类型 | 描述
---|---
String | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。
Integer | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位
Boolean | 布尔值。用于存储布尔值（真/假）。 
Double |双精度浮点值。用于存储浮点值。 
Min/Max keys |将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。 
Array |用于将数组或列表或多个值存储为一个键。
Timestamp |时间戳。记录文档修改或添加的具体时间。
Object |    用于内嵌文档
Null |用于创建空值
Symbol |符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。
Date |日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。
Object ID |对象 ID。用于创建文档的 ID。 
Binary Data |二进制数据。用于存储二进制数据。
Code |代码类型。用于在文档中存储 JavaScript 代码
Regular expression |则表达式类型。用于存储正则表达式。

##### ObjectId
ObjectId 类似唯一主键，可以很快的去生成和排序，包含 12 bytes，含义是：
- 前 4 个字节表示创建 unix时间戳,格林尼治时间 UTC 时间，比北京时间早了 8 个小时
- 接下来的 3 个字节是机器标识码
- 紧接的两个字节由进程 id 组成 PID
- 最后三个字节是随机数

注意：

- MongoDB 中存储的文档必须有一个 _id 键。这个键的值可以是任何类型的，默认是个 ObjectId 对象
- 由于 ObjectId 中保存了创建的时间戳，所以你不需要为你的文档保存时间戳字段，你可以通过 getTimestamp 函数来获取文档的创建时间:
- 一般由客户端驱动程序完成ObjectId的创建，因为生成的时候产生开销并且insert()方法可以返回生成的ObjectId

```
> var newObject = ObjectId()
> newObject.getTimestamp()
ISODate("2017-11-25T07:21:10Z")
```
- ObjectId 转为字符串

```
> newObject.str
5a1919e63df83ce79df8b38f
```
##### 字符串
BSON 字符串都是 UTF-8 编码。

##### 时间戳
BSON 有一个特殊的时间戳类型用于 MongoDB 内部使用，与普通的 日期 类型不相关。 时间戳值是一个 64 位的值。其中：      


