### 文档类似于mysql中的行
- 文档中的键/值对是有序的
- 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)
- MongoDB区分类型和大小写
- MongoDB的文档不能有重复的键
- 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符
##### 键的命名规范：
- 键不能含有\0 (空字符)。这个字符用来表示键的结尾。
- .和$有特别的意义，只有在特定环境下才能使用。
- 以下划线"_"开头的键是保留的(不是严格要求的)。

##### 插入文档
- MongoDB 使用 insert() 或 save() 方法向集合中插入文档
```
db.COLLECTION_NAME.insert(document)
```
- 实例

以下文档可以存储在 MongoDB 的 runoob 数据库 的 col 集合中

```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: 'liubing',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
- save()方法

只有一个参数--文档

文档存在时更新，不存在插入



---
##### 查询文档

```
> db.col.find(query,projection)
```
- query:可选，指定查询条件
- projection:可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```
>db.col.find().pretty()
```
pretty() 方法以格式化的方式来显示所有文档。

MongoDB 与 RDBMS Where 语句比较:

范例 | RDBMS中类似语句
---|---
db.col.find({"by":"liubing"}).pretty() | where by = 'liubing'
db.col.find({"likes":{$lt:50}}).pretty() | where likes < 50
db.col.find({"likes":{$lte:50}}).pretty() | where likes <= 50
db.col.find({"likes":{$gt:50}}).pretty() | where likes > 50
db.col.find({"likes":{$gte:50}}).pretty() | where likes >= 50
db.col.find({"likes":{$ne:50}}).pretty() | where likes != 50

- and条件

```
>db.col.find({key1:value1, key2:value2}).pretty()
```
- or条件

```
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```

---



##### 删除文档
- 删除集合中所有的文档，但不会删除集合本身，原有的索引也会保存
```
> db.blog.remove()
```
- 接受参数删除

```
> db.blog.remove({"tiltle":"my blog title"})
```
---
##### 更新文档
```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

参数说明：
- query : update的查询条件，类似sql update查询内where后面的。
- update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- upsert : 可选，这个参数的意思是，如果不存在满足条件的记录，是否插入objNew,true为插入，默认是false，不插入。
- multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- writeConcern :可选，抛出异常的级别。

save()方法：save() 方法通过传入的文档来替换已有文档

```
db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```

##### $set修改器
此修改器来指定一个键的值，若这个键不存在，则创建
- 增加键值对

```
> db.runoob.update({"x":10}, {"$set":{"favorite book" : "war and peace"}})
```
- 若键存在，则修改值(可以修改值的数据类型)

```
> db.runoob.update({"x":10}, {"$set":{"favorite book" : "green eggs and ham"}})
```
- 修改内嵌文档

```
 db.runoob.update({"x":10}, {"$set":{"author.name" : "lisi"}})
```

- $unset删除键

```
> db.runoob.update({"x":10}, {"$set":{"favorite book" : 1}})
```

##### $inc修改器
此修改器与$set修改器类似，不过$inc修改器专门用来修改数字的，只能用于整数，长整数，双精度浮点数，用于加操作
- 键不存在

```
>db.games.insert({"game":"pinball","user":"joe"})
>db.games.update({""game":"pinball","user":"joe""},{$inc:{"score":50}})
>db.games.findOne()
{
    "_id":ObjectId("4234"),
    "game":"pinball",
    "user":"joe",
    "score":50
}
```
- 键存在

```
>db.games.update({""game":"pinball","user":"joe""},{$inc:{"score":1000}})
>db.games.findOne()
{
    "_id":ObjectId("4234"),
    "game":"pinball",
    "user":"joe",
    "score":1050
}
```



##### 数组修改器
- $push 只能用于数组，如果指定的键存在，就向已有的数组末尾添加一个元素，要是没有就创建一个数组

```
> db.runoob.post.update({"x":10}, {"$push":{"favorite book" : {"name":"joe"}}})
```
- $addToSet 如果一个值不在数组里就加进去，否则就不加
- $addToSet和$each组合先数组添加多个值（数字或字符串或文档）

```
> db.runoob.post.update({"x":10}, {"$addToSet":{"emails":{"$each":["joe.java","joe.python"]}}}})
```
- $pop 修改器

```
#从数组尾部删除
{$pop : {key:1}}
#从数组头部删除
{$pop : {key:-1}}
```
- $pull 修改器删除特定元素
对于值为数组[1,1,2,1],pull 1 就是只有一个元素2的数组
```
>db.lists.update({"x":20},{"$pull":{"nums" : 1}})
```
- $数组定位符

将名字换位jim,只会匹配查找到的第一个，$表示数组下标

```
>db.blog.update({"comments.author":John},{"$set" : {"comments.$.author":"jim"}})
```
##### upsert








