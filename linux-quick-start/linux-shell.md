
# Linux Shell 

$ vi hello.sh

```
#!/bin/bash
echo "Hello World !"
```

执行脚本: 


$ sh hello.sh
Hello World !

$ chmod +x hello.sh
$ demo ./hello.sh
Hello World !


定义变量时，变量名不加美元符号（$），变量名和等号之间不能有空格, 如：

```
Name="WANGYT"
```

使用一个定义过的变量，只要在变量名前面加美元符号（$）即可，如：

```
Name="WANGYT"
echo $Name
echo ${Name}
```

字符串

字符串可以用单引号，也可以用双引号，也可以不用引号。

单引号: 

单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
单引号字串中不能出现单引号（对单引号使用转义符后也不行）。

双引号：

双引号里可以有变量
双引号里可以出现转义字符


拼接字符串

```
# hello.sh 
Name="Linux Shell"
str1='str1: hello, $Name !'
str2="str2: hello, $Name !"
str3="str3: hello, "$Name" !"
echo $str1;
echo $str2;
echo $str3;
```

```
$ sh hello.sh
str1: hello, $Name !
str2: hello, Linux Shell !
str3: hello, Linux Shell !
```

获取字符串长度

```
# 输出结果: 5
string="nihao"
echo ${#string}
```

| 运算符  | 说明                            | 举例                      |
| ---- | ----------------------------- | ----------------------- |
| -eq  | 检测两个数是否相等，相等返回 true。          | [ $a -eq $b ] 返回 true。  |
| -ne  | 检测两个数是否相等，不相等返回 true。         | [ $a -ne $b ] 返回 true。  |
| -gt  | 检测左边的数是否大于右边的，如果是，则返回 true。   | [ $a -gt $b ] 返回 false。 |
| -lt  | 检测左边的数是否小于右边的，如果是，则返回 true。   | [ $a -lt $b ] 返回 true。  |
| -ge  | 检测左边的数是否大等于右边的，如果是，则返回 true。  | [ $a -ge $b ] 返回 false。 |
| -le  | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

| 操作符     | 说明                                       | 举例                     |
| ------- | ---------------------------------------- | ---------------------- |
| -b file | 检测是否是块设备文件，如果是，则返回 true。               | [ -b $file ] 返回 false。 |
| -c file | 检测是否是字符设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -d file | 检测是否是目录，如果是，则返回 true。                  | [ -d $file ] 返回 false。 |
| -f file | 检测是否是普通文件（非目录、非设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测是否设置了 SGID 位，如果是，则返回 true。           | [ -g $file ] 返回 false。 |
| -k file | 检测是否设置了粘着位(Sticky Bit)，如果是，则返回 true。   | [ -k $file ] 返回 false。 |
| -p file | 检测是否是具名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测是否设置了 SUID 位，如果是，则返回 true。           | [ -u $file ] 返回 false。 |
| -r file | 检测是否可读，如果是，则返回 true。                   | [ -r $file ] 返回 true。  |
| -w file | 检测是否可写，如果是，则返回 true。                   | [ -w $file ] 返回 true。  |
| -x file | 检测是否可执行，如果是，则返回 true。                  | [ -x $file ] 返回 true。  |
| -s file | 检测是否为空（文件大小是否大于0），不为空返回 true。          | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。             | [ -e $file ] 返回 true。  |



if 分支

Shell 有三种 if ... else 语句：

if ... fi 语句
if ... else ... fi 语句
if ... elif ... else ... fi 语句

```
#!/bin/sh
a=10
b=20

if [ $a == $b ]
then
   echo "=> a is equal to b"
fi

if [ $a != $b ]
then
   echo "=> a is not equal to b"
fi
```

```
#!/bin/sh
a=10
b=20
if [ $a == $b ]
then
   echo "=> a is equal to b"
else
   echo "=> a is not equal to b"
fi
```

```
#!/bin/sh
a=10
b=20
if [ $a == $b ]
then
   echo "a is equal to b"
elif [ $a -gt $b ]
then
   echo "a is greater than b"
elif [ $a -lt $b ]
then
   echo "a is less than b"
else
   echo "None of the condition met"
fi
```

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

DATE=`date "+%Y-%m-%d %T"`

DATE=`date "+%Y-%m-%d %H:%M:%S"`;

