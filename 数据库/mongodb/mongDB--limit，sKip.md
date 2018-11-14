### limit方法
如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数

```
>db.COLLECTION_NAME.find().limit(NUMBER)
```
### skip方法
我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。

```
>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```
### sort方法
在MongoDB中使用使用sort()方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用 1 和 -1 来指定排序的方式，其中 1 为升序排列，而-1是用于降序排列。
```
>db.COLLECTION_NAME.find().sort({KEY:1})
```

### 实例：以下实例只会显示第二条文档数据

```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "Java 教程" }
```
- 第一个{}表示where条件，为空就不加任何条件
- 第二个{}指定指定显示哪些列，0不显示，1显示

### 补充说明
- skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()。