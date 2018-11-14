### shll支持的运算符
- 算数运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

如：两个数相加(注意使用的是反引号 ` 而不是单引号 )
~~~
val=`expr 2 + 2`
~~~
注意：
- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2
- 完整的表达式要被 \` \` 包含
---
### 算数运算符
##### 实例
~~~
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
~~~

---
### 关系运算符
- -eq 检测两个数是否相等，相等返回 true。
- -ne 检测两个数是否不相等，不相等返回 true。
- -gt 检测左边的数是否大于右边的，如果是，则返回 true
- -lt 检测左边的数是否小于右边的，如果是，则返回 true。
- -ge 检测左边的数是否大于等于右边的，如果是，则返回 true。
- -le 检测左边的数是否小于等于右边的，如果是，则返回 true。
##### 实例
~~~
a=10
b=20

if [ $a -eq $b ]
then
   
   echo "$a -eq $b : a 等于 b"
else
   echo "$a -eq $b: a 不等于 b"
fi
if [ $a -ne $b ]
then
   echo "$a -ne $b: a 不等于 b"
else
   echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
   echo "$a -gt $b: a 大于 b"
else
   echo "$a -gt $b: a 不大于 b"
fi
if [ $a -lt $b ]
then
   echo "$a -lt $b: a 小于 b"
else
   echo "$a -lt $b: a 不小于 b"
fi
if [ $a -ge $b ]
then
   echo "$a -ge $b: a 大于或等于 b"
else
   echo "$a -ge $b: a 小于 b"
fi
if [ $a -le $b ]
then
   echo "$a -le $b: a 小于或等于 b"
else
   echo "$a -le $b: a 大于 b"
fi
~~~
---
### 布尔运算符
- ！非运算，表达式为 true 则返回 false，否则返回 true。
- -o 或运算，有一个表达式为 true 则返回 true。
- -a 与运算，两个表达式都为 true 才返回 true。
##### 实例
~~~
a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
~~~

---
### 逻辑运算符
- && 逻辑的 AND 
- || 逻辑的 OR
##### 实例
~~~
a=10
b=20

if [[ $a -lt 100 && $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi

if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
~~~
---
### 字符串运算符
- = 检测两个字符串是否相等，相等返回 true。
- !=  检测两个字符串是否相等，不相等返回 true 
- -z 检测字符串长度是否为0，为0返回 true。
- -n 检测字符串长度是否为0，不为0返回 true。
- str 检测字符串是否为空，不为空返回 true。
##### 实例
~~~
a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n $a ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
~~~
---
### 文件测试运算符
文件测试运算符用于检测 Unix 文件的各种属性
- -b file 检测文件是否是块设备文件，如果是，则返回 true。
- -c file 检测文件是否是字符设备文件，如果是，则返回 true。
- -d file 检测文件是否是目录，如果是，则返回 true。 
- -f file 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。
- -g file 检测文件是否设置了 SGID 位，如果是，则返回 true。
- -k file 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。
- -p file 检测文件是否是有名管道，如果是，则返回 true。
- -u file 检测文件是否设置了 SUID 位，如果是，则返回 true。
- -r file 检测文件是否可读，如果是，则返回 true。
- -w file 检测文件是否可写，如果是，则返回 true。
- -x file 检测文件是否可执行，如果是，则返回 true。
- -s file 检测文件是否为空（文件大小是否大于0），不为空返回 true。
- -e file 检测文件（包括目录）是否存在，如果是，则返回 true。
##### 实例
~~~
file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
~~~