##### 集合类似于mysql的表
集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。
例如：将一下不同数据结构的文档插入集合

```
{"site":"www.baidu.com"}
{"site":"www.google.com","name":"Google"}
{"site":"www.runoob.com","name":"菜鸟教程","num":5}
```
当第一个文档插入时，集合就会被创建。

##### 查看当前数据库

```
> db
```

##### 创建
- 使用insert函数，将保存到blog集合中

```
> post={"title":"my blog title","content":"sssss","date":new Date()}
> db.blog.insert(post)
```
##### 读取
- 使用find函数读取指定集合

```
> db.blog.find()
```
##### 更新
- update函数有两个参数，一个是查找到文档的限定条件，另一个是新的文档，注意如果找到文档有多个则会报错，最好指定以“_id”这样的键来匹配

```
> post.commments=[]
> db.blog.update({"tiltle":"my blog title"},post)

```
##### 删除文档
- remove函数删除文档

```
> db.blog.remove({"tiltle":"my blog title"})
```
##### 查看集合
```
> > show collections
```
##### 删除集合
```
> db.collectionName.drop()
```
##### 创建集合
```
> db.createCollection(name,options)
```
参数说明：
- name:创建的集合名
- options：可选参数

字段 | 类型 | 描述
---|--- |---
capped | 布尔 | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。当该值为 true 时，必须指定 size 参数
autoIndexId | 布尔 | （可选）如为 true，自动在 _id 字段创建索引。默认为 false。
size | 数值 | 可选）为固定集合指定一个最大值（以字节计）。如果 capped 为 true，也需要指定该字段。
max|数值|可选）为固定集合指定一个最大值（以字节计）。如果 capped 为 true，也需要指定该字段。

在插入文档时，MongoDB 首先检查固定集合的 size 字段，然后检查 max 字段。

实例：

创建固定集合 mycol，整个集合空间大小 6142800 KB, 文档最大个数为 10000 个

```
> db.createCollection("mycol", { capped : true,autoIndexId : true, size :  6142800, max : 10000 } )
```

在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。
```
> db.mycol2.insert({"name" : "liubing"})
> show collections
mycol2
...
```






