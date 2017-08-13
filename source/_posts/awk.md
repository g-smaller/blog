---
title: awk
date: 2017-08-12 17:53:35
categories:
- linux
- awk
tags:
- awk
- linux
---


### awk命令的基本格式
> awk [options] 'program' file

- options 这个表示一些可选的参数选项
- program 这个表示 awk 的可执行脚本代码，这个是必须要有的
- file 这个表示 awk 需要处理的文件，注意是纯文本文件

例子
> awk '{print $0}' /etc/passwd

awk 命令的可执行脚本代码使用单引号括起来，紧接着里面是一对花括号,然后花括号里面就是一些可执行的脚本代码段,当 awk 每读取一行之后，它会依次执行双引号里面的每个脚本代码段，在上面这个例子中， $0 表示当前行;当你执行了上面的命令之后，它会依次将 /etc/passwd 文件的每一行内容打印输出，你一定在想：这有个毛用，用 cat 命令也能搞定。

<!--more-->

### awk 自定义分隔符

awk 默认的分割符为空格和制表符，我们可以使用 `-F` 参数来指定分隔符
> awk -F ':' '{print $1}' /etc/passwd

上面的命令将 /etc/passwd 文件中的每一行用冒号 : 分割成多个字段，然后用 print 将第 1 列字段的内容打印输出

### 如何同时指定多个分隔符

例如一个文件info.log的内容如下

```
Grape(100g)1980
raisins(500g)1990
plum(240g)1997
apricot(180g)2005
nectarine(200g)2008
```

现在我们想将上面的 some.log 文件中按照 “水果名称(重量)年份” 来进行分割

> awk -F '[()]' '{print $1, $2, $3}' info.log

在 -F 参数中使用一对方括号来指定多个分隔符，awk 处理 info.log 文件时就会使用 “(” 和 “)” 来对文件的每一行进行分割。

### awk 内置变量
- $0 表示文本处理时的当前行
- $1 表示文本行被分隔后的第 1 个字段列
- $2 表示文本行被分隔后的第 2 个字段列
- $3 表示文本行被分隔后的第 3 个字段列
- $n 表示文本行被分隔后的第 n 个字段列
- NR 表示文件中的行号，表示当前是第几行
- NF 表示文件中的当前行列的个数，类似于MySQL数据表里面每一行记录有多少个字段
- FS 表示awk的输入分隔符，默认分隔符为空格和制表符，你可以对其进行自定义设置
- OFS 表示awk的输出分隔符，默认为空格，你也可以对其进行自定义设置
- FILENAME 表示当前文件名称，如果同时处理多个文件，它也表示当前文件名称

比如我们有这么一个文本文件 fruit.txt 内容如下，我将用它来向你演示如何使用 awk 命令工具，顺便活跃一下此时尴尬的气氛。。
```
peach    100   Mar  1997   China
Lemon    150   Jan  1986   America
Pear     240   Mar  1990   Janpan
avocado  120   Feb  2008   china
```

#### 打印输出文件的每一行的第 1 列、第 2 列和第 3 列内容
> awk '{print $1, $2, $3}' fruit.txt

其中加入的逗号表示插入输出分隔符，也就是默认的空格。

#### 文件的每一行的每一列的内容除了可以用 print 命令打印输出以外，还可以对其进行赋值

> awk '{$2 = "***"; print $0}' fruit.txt

通过对 $2 变量进行重新赋值，来隐藏每一行的第 2 列内容，并且用星号 * 来代替其输出

#### 在参数列表中加入一些字符串或者转义字符之类的东东

> awk '{print $1 "\t" $2 "\t" $3}' fruit.txt

可以在 print的参数列表中加入一些字符串或者转义字符之类的东东，让输出的内容格式更漂亮，但一定要记住要使用双引号。

#### awk 内置 NR 变量表示每一行的行号

> awk '{print NR "\t" $0}' fruit.txt

**awk 内置 NF 变量表示每一行的列数**
> awk '{print NF "\t" $0}' fruit.txt

#### awk 中 $NF 变量的使用
> awk '{print $NF}' fruit.txt

上面这个 $NF 就表示每一行的最后一列，因为 NF 表示一行的总列数，在这个文件里表示有 5 列，然后在其前面加上 $ 符号，就变成了 $5 ，表示第 5 列

**我们用 fruit.txt 和 company.txt 两个文件来向你演示 awk 同时处理多个文件的时候有什么效果**

> awk '{print FILENAME "\t" $0}' fruit.txt company.txt

> fruit.tx peach    100   Mar  1997   China

> company.txt fji


### awk 其它操作

#### BEGIN 关键字的使用
如果在脚步代码段前面使用BEGIN关键字，它会在开始读取一个文件之前，运行一次BEGIN关键字后面的脚步代码，而且只会执行一次；
> awk 'BEGIN {print "Start read file"}' /etc/passwd

**awk 脚本中可以用多个花括号来执行多个脚本代码，就像下面这样**

> awk 'BEGIN {print "Start read file"} {print $0}' /etc/passwd

#### END 关键字使用方法**

awk 的 END 指令和 BEGIN 恰好相反，在 awk 读取并且处理完文件的所有内容行之后，才会执行 END 后面的脚本代码段

> awk 'END {print "End file"}' /etc/passwd

> $ awk 'BEGIN {print "Start read file"} {print $0} END {print "End file"}' /etc/passwd

#### 在 awk 中使用变量

可以在awk脚步中声明和使用变量
> awk '{msg="Hello Word"; print msg}' /etc/passwd

awk 声明的变量可以在任何多个花括号脚本中使用
> awk 'BEGIN {msg="hello world"} {print msg}' /etc/passwd

#### 在 awk 中使用数学运算
> awk '{a = 12; b = 24; print a + b}' company.txt

**awk 还支持其他的数学运算符**

- `+` 加法运算符
- `-` 减法运算符
- `*` 乘法运算符
- `/` 除法运算符
- `%` 取余运算符

#### 在 awk 中使用条件判断
比如有一个文件 company.txt 内容如下
```
yahoo   100 4500
google  150 7500
apple   180 8000
twitter 120 5000
```
我们要判断文件的第 3 列数据，也就是平均工资小于 5500 的公司，然后将其打印输出

> awk '$3 < 5500 {print $0}' company.txt

**awk 还有一些其他的条件操作符如下**

- `<` 小于
- `<=` 小于或等于
- `==` 等于
- `!=` 不等于
- `>` 大于
- `>=` 大于或等于
- `~` 匹配正则表达式
- `!~` 不匹配正则表达式

使用 if 指令判断来实现上面同样的效果

> awk '{if ($3 < 5500) print $0}' company.txt

#### 在 awk 中使用正则表达式

使用正则表达式匹配字符串 “There” ，将包含这个字符串的行打印并输出

> awk '/There/{print $0}' poetry.txt

使用正则表达式配一个包含字母 t 和字母 e ，并且 t 和 e 中间只能有任意单个字符的行

> awk '/t.e/{print $0}' poetry.txt

如果只想匹配单纯的字符串 “t.e”， 那正则表达式就是这样的 /t\.e/ ，用反斜杠来转义 . 符号 因为 . 在正则表达式里面表示任意单个字符。

使用正则表达式来匹配所有以 “The” 字符串开头的行

> awk '/^The/{print $0}' poetry.txt

在正则表达式中 ^ 表示以某某字符或者字符串开头。

使用正则表达式来匹配所有以 “true” 字符串结尾的行

> awk '/true$/{print $0}' poetry.txt

在正则表达式中 $ 表示以某某字符或者字符串结尾。


正则表达式中的圆括号的用法

> awk '/th(in){1}king/{print $0}' poetry.txt

正则表达式中的圆括号表示将多个字符当成一个完整的对象来看待。比如 /th(in){1}king/ 就表示其中字符串 “in” 必须出现 1 次。而如果不加圆括号就变成了 /thin{1}king/ 这个就表示其中字符 “n” 必须出现 1 次。