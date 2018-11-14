### mongDB是面向文档的非关系型数据库
- 数据结构有key-value组成
- 基于json
- 数据存贮在硬盘和内存中，在内存中存储热数据
- 有多个应用或用户--》多个数据库--》多个集合--》多个文档--》每个文档一个json

##### 路径添加到path
MongoDB 的可执行文件位于 bin 目录下，所以可以将其添加到 PATH 路径中

```
export PATH=<mongodb-install-directory>/bin:$PATH
```
##### 创建数据库目录
- MongoDB的数据存储默认在/data/db下，-p循环创建
```
mkdir -p /data/db
```
- 若要指定目录，也需先创建出来，并且启动时指定目录，如：

```
./mongod --dbpath /usr/data/db
```
##### 启动并到后台运行
- 指定配置文件运行
~~~
./mongod --config /root/server/mongodb/mongodb-linux-x86_64-rhel70-3.6.3/mongodb.conf 
~~~
&表示到后台运行

```
./mongod --dbpath /usr/data/db &
```
##### MongoDB后台管理 Shell
（确保已启动moingDB），如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo命令文件。
MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。
当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）  
开启的时候shell会连接到mongDB服务器的test数据库，并将这个数据库连接赋值给全局变量db，这个变量是shell访问数据库的主要入口点
- 运行shell,shell具有完备的javascript解释器 
```
./mongo
```
- 显示所有数据的列表
~~~
show dbs
~~~
- 查看当前所在数据库
~~~
db
~~~
- 连接到指定数据库
~~~
use local
~~~
有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

库名 | 作用
---|---
admin | 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器
local | 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合 
config   | 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息





