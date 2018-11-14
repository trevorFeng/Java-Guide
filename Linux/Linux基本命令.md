### 文件管理指令

指令 | 涵义
---|---
scp -r   ./util   用户名@192.168.1.65:/home/wwwroot/limesurvey_back/scp | 传文件至远端
su | 切换用户
netstat -anop \ grep 端口号 |查看进程 
ln -s source dist        |创建符号连接
sudo 命令|以管理员身份运行
tar -xzvf source dist | 解压
ls | 显示文件或目录
ls -l | 列出文件详细信息
ls -a | 列出当前文件目录下所有文件或目录，包括隐藏的
ll |显示文件或目录（蕴含权限信息）
mkdir |创建目录
mkdir -p | 创建目录，若无目录，则创建p(parent)
cd  | 切换目录
cat | 查看文件内容
cp 源文件 目标目录| 拷贝
mv  | 移动或重命名
rm | 删除文件
rm -r | 递归删除，刻删除子目录及文件
rm -f | 强制删除
find | 在文件系统中搜索某文件
stat | 显示指定文件的详细信息，比ls更详细
cat data.txt >> data1.txt | 将data.txt内容追加到data1.txt文件之后
vi aa.txt | 当前是编辑模式
ESC | 编辑模式
编辑模式 | 
/apache | 在文档中查找apaceh，按n跳到下一个，shift+n上一个
yy | 复制光标行
yyp | 复制光标所在行，并粘贴
p | 粘贴当前复制内容
： | 进入命令模式
命令模式 |
:q |命令模式下推出
:q! | 强制退出
:wq | 保存并退出
:set number | 显示行号
:set nonumber | 隐藏行号
:u | 撤销刚才的操作
v | 可视模式
可视模式 | 选择内容，复制，剪切，粘贴，删除内容
（1）|选定文本块，使用v进入可视模式；移动光标选定内容
(2)|复制选定块到缓冲区，用y；复制整行，用yy
（3）|剪切选定块到缓冲区，用d；剪切整行用dd
(4)|粘贴缓冲区的内容，用p
i | 插入模式
service network start |开启网络
service network stop |禁用网络
service iptables start |开启防火墙
service iptables stop| 禁用防火墙（开机重启）
chkconfig iptables off |永久关闭防火墙
jps|查看java程序
kill -9 进程id | 强制杀死进程，先用jps或jps top查看进程id
killall java |杀死所有java程序
top| 任务资源管理器
free m | 显示内存使用情况
echo 1 > /proc/sys/vm/drop_caches | 清理不使用的内存
netstat -lanp \ grep "27017" | 查看端口号
 nohup java -jar evaluation_engine-1.0-SNAPSHOT.jar & |java后台启动
ps -ef \ grep java | 显示java进程id 
---

### 指令说明
##### touch指令
创建文件
~~~
touch a.tx
~~~
##### cat指令
查看文件
~~~
cat a.txt
~~~
##### cp指令
cp [options] source dest
参数说明
- -a 此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
- -p 除复制文件的内容外，还把修改时间和访问权限也复制到新文件中
- -r 若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。 

实例：使用指令"cp"将当前目录"test/"下的所有文件复制到新目录"newtest"下

```
cp –r test/ newtest
```






