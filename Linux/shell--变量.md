### 一.变量类型
- 局部变量：仅在当前脚本中生效
- 环境变量：所有的程序、shell都能访问
- shell变量：由shell设置的特殊变量，有一部分是局部变量，有一部分是环境变量

##### 1.变量定义及使用
```
#shell脚本只能用此开头
#!/bin/sh
your_name="liubing"
#量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界
echo ${your_name}
#重新定义
your_name="xufei"
#设置只读变量
my_name="zhang"
readonly my_name
#删除变量
she="sb"
unSet she
```

##### 2.shell字符串
~~~
#定义
str="i am str"
str1="i am str1"
#转义
str2="i am \"${str}\" ! \n"
#拼接字符串
your_name="qinjx"
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting $greeting_1
#获取字符串长度 
string="abcd"
echo ${#string} #输出 4
#提取子字符串 以下实例从字符串第 2 个字符开始截取 4 个字符：
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
#查找子字符串 查找字符 "i 或 s" 的位置 注意：脚本中 "`" 是反引号，而不是单引号 "'"，
string="runoob is a great company"
echo `expr index "$string" is`  # 输出 8
~~~
##### 3.shell数组
~~~
#shell只支持一维数组
#定义
array_name=("aa" "bb" "cc" "dd")
array_name_1=(
value0
value1
value2
value3
)
#可以不使用连续的下标，而且下标的范围没有限制。
array_name_2[0]=value0
array_name_2[1]=value1
array_name_2[100]=value100

#读取数组
echo ${array_name[0]}
#读取数组中所有元素
echo ${array_name[@]}
#获取数组的个数
length=${#array_name[@]}
length1=${#array_name[*]}
#获取数组单个元素的长度
length_0=${#array_name[0]}
~~~
