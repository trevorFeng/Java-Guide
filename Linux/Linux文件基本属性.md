### Linux  文件基本属性
Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。 

在Linux中我们可以使用ll或者ls –l命令来显示一个文件的属性以及文件所属的用户和组，如：
~~~
[root@www /]# ls -l
total 64
dr-xr-xr-x   2 root root 4096 Dec 14  2012 bin
dr-xr-xr-x   4 root root 4096 Apr 19  2012 boot
~~~
##### 第一个字符说明
在Linux中第一个字符代表这个文件是目录、文件或链接文件等等
- 当为[ d ]则是目录
- 当为[ - ]则是文件
- 若是[ l ]则表示为链接文档(link file)
- 若是[ b ]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)
- 若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)
##### 接下来三组字符说明
接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。其中，[ r ]代表可读(read)、[ w ]代表可写(write)、[ x ]代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已

每个文件的属性由左边第一部分的10个字符来确定

文件类型 | 属主权限 | 属组权限 | 其他用户权限
---|---|---|---
0 | 1 2 3 |  1 2 3  |1 2 3
d：目录文件 -:文件 | r:读 w:写 x:可执行|r:读 w:写 x:可执行|r:读 w:写 x:可执行

- 第0位确定文件类型，第1-3位确定属主（该文件的所有者）拥有该文件的权限
- 第4-6位确定属组（所有者的同组用户）拥有该文件的权限，第7-9位确定其他用户拥有该文件的权限

对于 root 用户来说，一般情况下，文件的权限对其不起作用

---
### 更改文件属性
##### chgrp：更改文件属组
~~~
chgrp [-R] 属组名 文件名
~~~
- -R 递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。 
##### chown：更改文件属主，也可以同时更改文件属组
~~~
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
~~~
实例：进入 /root 目录（~）将install.log的拥有者改为bin这个账号
~~~
[root@www ~] cd ~
[root@www ~]# chown bin install.log
[root@www ~]# ls -l
-rw-r--r--  1 bin  users 68495 Jun 25 08:53 install.log
~~~
将install.log的拥有者与群组改回为root：
~~~
[root@www ~]# chown root:root install.log
[root@www ~]# ls -l
-rw-r--r--  1 root root 68495 Jun 25 08:53 install.log
~~~
##### chmod:更改文件9个属性
- 数字更改权限
各权限分数对照表

r | w | x
---|---|---
4 | 2 | 1
更改权限语法

```
chmod [-R]  xyz
```
实例：如果要将.bashrc这个文件所有的权限都设定启用

```
[root@www ~]# ls -al .bashrc
-rw-r--r--  1 root root 395 Jul  4 11:45 .bashrc
[root@www ~]# chmod 777 .bashrc
[root@www ~]# ls -al .bashrc
-rwxrwxrwx  1 root root 395 Jul  4 11:45 .bashrc
```
- 符号类型改变文件权限

如果我们需要将文件权限设置为 -rwxr-xr-- ，可以使用 chmod u=rwx,g=rx,o=r 文件名 来设定:

```
#  touch test1    // 创建 test1 文件
# ls -al test1    // 查看 test1 默认权限
-rw-r--r-- 1 root root 0 Nov 15 10:32 test1
# chmod u=rwx,g=rx,o=r  test1    // 修改 test1 权限
# ls -al test1
-rwxr-xr-- 1 root root 0 Nov 15 10:32 test1
```




