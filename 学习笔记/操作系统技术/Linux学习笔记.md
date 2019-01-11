[TOC]
# Linux学习笔记
## 一、Linux快捷命令：
- 在vim中查看行号：   :set nu
- 查找文件 ：whereis  mongod
- 查看端口状况：      netstat -anl | grep 80
- 查看端口是否被监听：netstat -anp | grep 80
- 查看进程(常用)：          ps aux | grep tomcat
    - ps命令查找与进程相关的PID号：
    - ps a 显示现行终端机下的所有程序，包括其他用户的程序。
    - ps -A 显示所有程序。
    - ps c 列出程序时，显示每个程序真正的指令名称，而不包含路径，参数或常驻服务的标示。
    - ps -e 此参数的效果和指定"A"参数相同。
    - ps e 列出程序时，显示每个程序所使用的环境变量。
    - ps f 用ASCII字符显示树状结构，表达程序间的相互关系。
    - ps -H 显示树状结构，表示程序间的相互关系。
    - ps -N 显示所有的程序，除了执行ps指令终端机下的程序之外。
    - ps s 采用程序信号的格式显示程序状况。
    - ps S 列出程序时，包括已中断的子程序资料。
    - ps -t<终端机编号> 指定终端机编号，并列出属于该终端机的程序的状况。
    - ps u 以用户为主的格式来显示程序状况。
    - ps x 显示所有程序，不以终端机来区分。
- 创建文件：         touch XX.txt
- 查找指定文件：   find -name "*mm*"
- 查看是否存在某一个软件： which java
- 干掉进程：rm -f /var/run/yum.pid
- 显示Linux占用磁盘空间:  df -h 
- 显示隐藏文件： ls -a
- 删除文件夹：rm -rvf tomcat
    - f --- 表示强制
    - r --- 表示递归
    - v --- 表示显示详细信息
- file /xxx/xx   显示一个文件的详细信息
- Ctrl + C   终止当前运行
- Ctrl + D  退出，相当于exit
- Ctrl + L  清屏
- Ctrl + A  光标移动到最前端
- Ctrl + E  光标移动到最后端
- Ctrl + U  删除光标前的所有字符
- Ctrl + K  删除光标后面的所有字符


- ls --help  查看命令的使用

- ls [选项] [文件]   :  [] 中括号包括的东西表示为可选项，可以写，可以不写



## vi快捷操作
### 在esc状态下：
1.  /(需要所有的字符)    就能够查找到所有与之相同的字符
2.  n    表示next的意思
3.  shift + n  反向next
4.  yy  在需要被复制的那一行就会被复制
5.  pp  粘贴
6.  dd  删除某一行  
7.  xx  删除光标的这个字符
8.  shift + d  删除光标那一行的光标后面的所有字符
9. u    撤销
10. echo 输出值

### 在插入模式下：
1.  如果一个单词需要自动补全，可以按住 ctrl  + P 


### 可视块模式：
Ctrl + V 进入到可视块模式  

块前面添加元素 ： 选中块，按住 shift + i ，输入字符，按ESC，块前面就会添加你输入的字符。  
块替换 ： 选中块，按 r ，替换  
删除块 ： 选中块，按 d ，删除  
复制块 ： 选中块，按 y ，复制  


### 注意：
Linux与windows的（记事本）文件换行符不一致，需要转换一下：  
安装软件：
```
yum -y install dos2unix
```

然后转换在windows上面的文件为Linux的文件 ： dos2unix  t1.txt



## 二、Linux文件
###  注意 ： 
通过颜色判断文件类型是不一定正确的。Linux系统中文件是没有后缀名的。

### Linux文本判断：
**输入  ll  查看文件，再查看文件那一行的第一个字母：**  
​      -    普通文件                           
​      d   目录文件（蓝色）              
​      b   设备文件（存储设备）      
​      c   设备文件 （打印机，终端）  
​      s   套接制文件   
​      p    管道文件  
​      l     链接文件（淡蓝色）  
​      

### Linux用户用户与组
- （1)   vim /etc/passwd   存放的是用户信息  

- （2） vim /etc/shadow   存放的是密码信息
- （3） vim /etc/group       存放的是组的信息



### Linux防火墙：
**网站：**  
https://www.linuxidc.com/Linux/2016-12/138979.htm
- 关闭防火墙： systemctl stop firewalld.service  (一次性的)  

- 关闭防火墙 ：systemctl disable firewalld.service (禁止开机启动的)
- 查看防火墙状态： systemctl status firewalld.service
- 开启防火墙：systemctl start firewalld.service


### Linux权限
#### 1.  文件权限
**-rw-r--r--. 1 root root 0 May 23 20:51 file1**      
文件的权限 -- 文件的所属对象 -- 文件所属组 -- 文件大小 -- 文件的更新时间 --  文件名


**-rw-r--r--    对应的关系**  
第一个位置（1） ： 表示文件或者文件夹  
前三个位置 （2.3.4）： 创建文件者的权限(相当于 u 这个人)                r   读的权限   
中三个位置 （5.6.7）： 对象的所属组的权限（相当于 g 这个人）        w  写的权限   
后三个位置 （8.9.10）： 其他人的权限（相当于 o 这个人）                 x  执行的权限    

**注意：** 通常就是 u g o 来形容这三个的对象

#### 2. 更改文件所有者：
- chown jack  file1     将file1这个文件的所有者改为 Jack   

- chown .hr  file1        将文件的所有组改变为 hr 组
- chgrp hr file1           将文件的所有组改变为 hr 组

#### 3. 更改权限
- chmod u+x file1      为文件所属者添加执行权限

- chmod a=rwx file1   为所有人的权限都赋值为 读写执行 的权限
- chmod u=rwx,g=rw,o=- file1   为所有者添加读写执行的权利，组员添加读写，其他人没权限    


## 三、Shell编程
### 1、Shell概述
#### 1）使用场景
- 在开发过程中，能够看懂运维人员的Shell程序。
- 使用Shell程序来实现集群的管理，提高开发效率。

#### 2）简介
Shell是一个**命令行解释器**，它接收应用程序/用户命令，然后调用操作系统内核。

#### 3）Shell的使用环境
![image](https://note.youdao.com/yws/api/personal/file/FD861F4D30C94B22AA52F7D0748C6295?method=download&shareKey=4a3264fe8eb8de106aec6db36041ce5c)

#### 4）Shell的特点
Shell还是一个功能相当强大的编程语言，易编写、易调试、灵活性强。

#### 5）Linux中的Shell解析器
```
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
```

#### 6）Linux默认的解析器
```
/bin/bash
```

#### 7）sh 与 bash 的区别
在Linux中，sh 与 bash 建立起了的软连接，所以，使用 sh 与 bash 的效果一样。


### 2、Shell脚本入门一
#### 案例一：永远的Hello World
#### 1）脚本格式
在编写Shell的时候，需要在开头指定解析器的类型，如果不写，默认为 /bin/bash 解析器。

例如：先创建一个文件helloWorld.sh ，然后编辑内容为：
```
#!/bin/bash

echo "hello woeld!"

```

#### 2）Shell脚本的执行
输入命令：
```
//方式一：使用 sh 解析器来执行Shell脚本
sh helloWorld.sh

//方式二：使用 bash 解析器来执行Shell脚本
bash helloWorld.sh

//方式三：为这个文件添加可执行权限以后，来执行Shell脚本
./helloWorld.sh
```
shell脚本输出的结果为：
```
hello woeld!
```

#### 案例二：多条命令的Shell脚本
#### 1）需求
在/usr/local/目录下创建一个cls.txt,在cls.txt文件中增加“I love cls”。

#### 2）编写Shell脚本
```
#!/bin/bash
cd /usr/local
touch cls.txt
echo "i love cls" >>cls.txt

```

#### 3）执行Shell脚本
```
bash Shell1.sh
```

### 3、Shell变量
#### 1）常用系统变量


变量 | 介绍
---|---
$HOME  | 当前用户的家目录
$PWD   | 当前的目录
$SHELL | 当前默认的Shell解析器
$USER  | 当前的用户
...    | ...

#### 2）查看系统的所有变量
输入命令： **set** 即可。  
例如：
```
[root@localhost shell]# set
```
#### 3）自定义变量
##### 定义变量
基本语法： 变量名=变量值   
注意：等号两边不能够有空格。   
例如：
```
[root@localhost shell]# A=1
```

##### 查看变量的值
基本语法： echo $变量名

例如：  
```
echo $A

//输出结果为：
3
```

##### 修改变量
基本语法： 变量名=变量值   
注意：修改变量与定义变量的方式一致，注意等号两边不能够有空格。  
例如：
```
[root@localhost shell]# A=2
//查看变量A的值
[root@localhost shell]# echo $A
A
[root@localhost shell]# A=4
[root@localhost shell]# echo $A
A

```

##### 撤销变量
基本语法： unset 变量名  
例如：
```
[root@localhost shell]# unset A

```

##### 声明静态变量
基本语法： readonly 变量名=变量值
注意：静态变量不能够进行unset的操作。等号两边不能够有空格。
例如：
```
[root@localhost shell]# readonly D=3

```

#### 4）变量定义规则
- 变量名可以有字幕、数字以及下划线组成，但是不能够以数字开头，环境变量名建议大写。
- <font color=red>**等号两边不能够有空格。**</font>
- 在bash中，变量默认类型都是字符串类型，无法直接进行数值运算。

- 变量的值中如果有空格，需要使用双引号或者单引号来括起来。例如：
```
[root@localhost shell]# PP="asdf asdfgg gre"
```

#### 5）局部变量转换为全局变量
```
//直接将A转换为全局变量
export  A

//直接将B进行赋值以后，又转化为全局变量
export B=2

```

#### 6）特殊变量
##### 第一个特殊变量 $n
**基本语法：**   
$n ,这个 n 为数字，$0代表的是该脚本名称，$1-$9代表第一到第九个参数，十以上的参数，十以上的参数需要用大括号包含，如${10}。

**代码演示：**  
```
//创建一个新文件，输入内容为：
#!/bin/bash
echo "$0  $1   $2"

//执行这个文件，并胃里面的参数赋值
[atguigu@hadoop101 datas]$ chmod 777 parameter.sh

[atguigu@hadoop101 datas]$ ./parameter.sh zsl  wx ysp
./parameter.sh  cls   xz

```

##### 第二个特殊变量 $#
**基本语法：**   
获取所有输入参数个数，常用于循环。让我们的Shell文件有多个 $n的时候，我们可以使用 $# 来查看输入了多少个参数。

**代码演示：**  
```
//编写Shell脚本，内容如下：

#!/bin/bash
echo "$0 $1 $2 $3"

//执行Shell脚本
[root@localhost shell]# bash parameter.sh zsl tsl
//输出的结果为：
parameter.sh zsl tsl 
2
```

##### 第三个特殊变量 $*、$@
**基本语法：**   
$*	功能描述：这个变量代表命令行中所有的参数，$*把所有的参数看成一个整体。

$@	功能描述：这个变量也代表命令行中所有的参数，不过$@把每个参数区分对待。

**代码演示：**  
```
//编写Shell脚本，内容如下：

#!/bin/bash
echo "$0 $1 $2 $3"
echo $#
echo $*
echo $@

//执行Shell脚本

[root@localhost shell]# bash parameter.sh zsl tsl
//结果如下
parameter.sh zsl tsl 
2
zsl tsl
zsl tsl
```

##### 第四个特殊变量 $?
**基本语法：**  
**判断上一条语句是否正确执行。**最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，由命令自己来决定），则证明上一个命令执行不正确了。

**代码演示：**  
```
//当我上一条命令正常执行，那么下面使用 $? 来进行判断的话，返回值为0

[root@localhost shell]# bash parameter.sh zsl tsl
parameter.sh zsl tsl 
2
zsl tsl
zsl tsl

[root@localhost shell]# echo $?
0


//当我上一条命令执行失败，在使用 $? 来判断上一条命令执行的装填的话，返回值不为0

[root@localhost shell]# $?
-bash: 0: 未找到命令
[root@localhost shell]# echo $?
127

```

### 4、Shell运算符
#### 1）基本语法
##### 方式一：
"$((运算式))" 或者 "$[运算式]"

##### 方式二：
expr + , - , \\*或者 * , / , %  //对应着 加，减，乘，除，取余。

#### 2）注意事项
- expr 的乘号，必须要加上一个反斜杠 \\*，而方式一不需要加上反斜杠 \*。
- expr 运算符之间必须要有空格。也就是说运算符的两边必须要有空格。

#### 3）代码演示
简单运算操作：2 + 3
```
//使用方式一进行运算
[root@localhost shell]# echo $((2+3))
5
[root@localhost shell]# echo $[2+3]
5

//使用方式二进行运算
[root@localhost shell]# expr 2 + 3
5

```
复杂运算操作：计算 (2 + 3) * 4 的值
```
//使用方式一进行运算
[root@localhost shell]# echo $[(2 + 3) * 4]
20
[root@localhost shell]# echo $(((2 + 3) * 4))
20

//使用方式二进行运算
[root@localhost shell]# expr `expr 2 + 3` \* 4
20

```

### 5、条件判断
#### 1）基本语法
[ 条件表达式 ]

#### 2）注意事项
- **条件表达式的两边必须要有空格。**
- 条件非空即为true，[ atguigu ]返回true，[] 返回false。

#### 3）常用判断条件
##### ① 两个整数之间的比较：
符号 | 介绍 | 结果
---|---|---
-lt | 小于（less than）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-le | 小于等于（less equal）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-eq | 等于（equals）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-gt | 大于（greater than）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-ge | 大于等于（greater equals）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-ne | 不等于（not equal）| 返回结果为0，则证明正确；返回结果为1，则证明错误


##### ② 文件权限的判断
符号 | 介绍 | 结果
---|---|---
-r | 有读的权限（read） | 返回结果为0，则证明正确；返回结果为1，则证明错误
-w | 有写的权限（write） | 返回结果为0，则证明正确；返回结果为1，则证明错误
-x | 有执行的权限（execute） | 返回结果为0，则证明正确；返回结果为1，则证明错误

##### ③ 按照文件类型进行判断
符号 | 介绍 | 结果
---|---|---
-f | 文件存在并且是一个常规的文件（file）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-e | 文件存在（exist）| 返回结果为0，则证明正确；返回结果为1，则证明错误
-d | 文件存在并且是一个目录（directory）| 返回结果为0，则证明正确；返回结果为1，则证明错误

#### 4）代码演示：

```
//判断 23>=22 ? 
[root@localhost shell]# [ 23 -ge 22 ]

//查看结果，结果为0，证明条件判断正确。
[root@localhost shell]# echo $?
0

```

```
//判断文件是否有执行的权限
[root@localhost shell]# [ -x Shell1.sh  ]
//输出结果，值为1，证明没有写的权限。
[root@localhost shell]# echo $?
1

```

```
//判断文件是否存在
[root@localhost shell]# [ -e Shell1.sh ]
//输出结果：值为0 ，证明文件存在。
[root@localhost shell]# echo $?
0

```

#### 5）多条件判断
##### 基本语法：
使用 && 与 || 符号来进行条件的判断。
##### 注意事项：
&& 表示前一条命令执行成功时，才执行后一条命令，|| 表示上一条命令执行失败后，才执行下一条命令。

##### 代码演示：
```
//执行一个复杂的表达式
[root@localhost shell]# [ 2 -le 3 ] && [ 4 -ge 5 ] || [ -e Shell1.sh ]

//输出结果，值为0，证明结果为真。
[root@localhost shell]# echo $?
0

```

### 6、流程控制
#### 1）if 判断
##### 基本语法一： 
```
if [ 条件表达式 ];then

    程序

fi
```

##### 基本语法二：
```
if [ 条件表达式 ]
    then
    
    程序
fi
    
```

##### 使用demo：
```
#!/bin/bash

if [ $1 -eq "1" ]
then
        echo "数字小于1"
elif [ $1 -eq "2" ]
then
        echo "数字等于2"
fi

```

##### 注意事项：
- [ 条件表达式 ]，**中括号与条件判断式之间必须有空格**。

- **if 标签后面必须要有空格**。


#### 2）case 语句
##### 基本语法：
```
case $变量名 in 
  "值1"） 
    如果变量的值等于值1，则执行程序1 
    ;; 
  "值2"） 
    如果变量的值等于值2，则执行程序2 
    ;; 
  …省略其他分支… 
  *） 
    如果变量的值都不是以上的值，则执行此程序 
    ;; 
esac

```

##### 使用Demo：
```
//编写Shell文件，来实现case语句的编写。

#!/bin/bash
case $1 in
1)
    echo "结果为1"
;;
2)
    echo "结果为2"
;;
*)
    echo "结果为其他数据"
;;
esac

//执行 Shell文件
[root@localhost shell]# bash Shell3.sh 2

//输出的结果：
结果为2

```

##### 注意事项：
- case 行尾必须为单词 in，每一个模式匹配必须以右括号 ) 结束。
- 双分号 ;; 表示命令序列结束，相当于java中的 break 。

- 最后的 *) 表示默认模式，相当于Java中的 default 。


#### 3）for循环
##### 基本语法一：
```
for (( 初始值;循环控制条件;变量变化 )) 
  do 
    程序 
  done

```

##### 语法一代码Demo：
```
//编写Shell文件
#!/bin/bash
s=0
for (( i=0;i<=100;i++ ))
do
    s=$[$s+$i]
done
echo $s

//执行Shell文件
[root@localhost shell]# bash Shell4.sh 
5050

```

##### 基本语法二：
```
for 变量 in 值1 值2 值3… 
  do 
    程序 
  done

```

##### 语法二代码Demo：
```
//编写Shell文件
#!/bin/bash
#打印数字
# 使用 $* 来将所有哦的变量整合成一个整体
for i in  "$*"
    do
      echo "ban zhang love $i "
    done
# 使用 $@ 来将数据分开输出
for j in $@
do
        echo "ban zhang love $j"
done


    
//执行Shell文件
[root@localhost shell]# bash Shell5.sh a b c d
ban zhang love a b c d 
ban zhang love a
ban zhang love b
ban zhang love c
ban zhang love d

```

#### 4）while循环
##### 基本语法：
```
while [ 条件判断式 ] 
  do 
    程序
  done

```

##### 代码Demo：
```
#!/bin/bash
s=0
i=0
while [ $i -le 100 ]
do
        s=$[$s+$i]
        i=$[$i+1]
done

echo $s

//
```

### 7、read控制台输入
#### 1）基本语法
```
read(选项)(参数)
```
语法的介绍：  
- 选项：
    - -p : 指定读取值时的提示符。
    - -t : 指定读取值时等待的时间。

- 参数：
    - 变量 : 指定读取值的变量名。

#### 2）代码演示
```
//编写Shell脚本，让外界在七秒之内输入一个数据，赋值给 NAME 变量。

#!/bin/bash

read -t 7 -p "请在七秒钟之内输入数据：" NAME
echo $NAME

//执行Shell脚本
[root@localhost shell]# bash Shell7.sh 
请在七秒钟之内输入数据：sdf
sdf

```

### 8、函数
#### 1）系统函数
##### basename 函数：
**基本语法：** basename[string / pathname][suffix]

**功能描述：** basename函数会删除所有浅醉，包括最后一个 /，然后将字符串提取出来。如果有suffix，那么也会将字符串中的suffix删除掉。

**功能特点：** 能够将一个文件的名称接取出来，可以选择是否删除文件后缀。

**代码演示：**
```
//获取文件名称以及文件后缀
[root@localhost shell]# basename /usr/local/shell/Shell7.sh
Shell7.sh

//获取文件名称，删除文件名称的后缀
[root@localhost shell]# basename /usr/local/shell/Shell7.sh .sh
Shell7

```

##### dirname 函数：
**基本语法：** dirname 文件的绝对路径

**功能描述：** 从给定的包含绝对路径的文件名中去除掉文件名，返回文件路径。

**功能特点：** 返回的是一个文件的路径。不包括文件名。

**代码演示：**
```
[root@localhost shell]# dirname /usr/local/shell/Shell7.sh 
/usr/local/shell

```

#### 2）自定义函数
##### 基本语法：
```
[ function ] funname[()]
{
	Action;
	[return int;]
}

```

##### 注意事项：
- 必须在调用函数地方之前，先声明函数，shell脚本是逐行运行。
- 函数的返回值，只能通过 $? 系统变量来获得，可以显示地添加： **return 返回值**  方式进行添加。

- 如果不添加return返回值，那么就会以最后一条命令的运行结果，作为返回值。

##### 代码演示：
```
//编写Shell函数
#!/bin/bash

function sum()
{
        s=0;
        s=$[$1+$2]
        echo $s
}

read -t 5 -p "输入第一个参数：" p1
read -t 5 -p "输入第二个参数：" p2

sum $p1 $p2

//执行Shell函数
[root@localhost shell]# bash Shell8.sh 
输入第一个参数：1
输入第二个参数：2
3

```

### 9、Shell 工具
#### 1）cut 工具
##### 简介：
cut 的工作就是 “剪”，具体说的就是在文件中赋值剪切数据用的。cut命令从文件的每一行剪切字节、字符和字段并将这些字节、字符和字段进行输出。

##### 基本用法：
cut [选项参数] 文件名

##### 选项参数说明：

选项参数 | 功能
---|---
-f | 列号，提取第几列
-d | 分隔符，按照分隔符分割列

##### 代码演示：
```
（0）数据准备
[atguigu@hadoop101 datas]$ touch cut.txt
[atguigu@hadoop101 datas]$ vim cut.txt
dong shen
guan zhen
wo  wo
lai  lai
le  le

（1）切割cut.txt第一列
[atguigu@hadoop101 datas]$ cut -d " " -f 1 cut.txt 
dong
guan
wo
lai
le

（2）切割cut.txt第二、三列
[atguigu@hadoop101 datas]$ cut -d " " -f 2,3 cut.txt 
shen
zhen
 wo
 lai
 le
 
（3）在cut.txt文件中切割出guan
[atguigu@hadoop101 datas]$ cat cut.txt | grep "guan" | cut -d " " -f 1
guan

（4）选取系统PATH变量值，第2个“：”开始后的所有路径：
[atguigu@hadoop101 datas]$ echo $PATH
/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/atguigu/bin

[atguigu@hadoop102 datas]$ echo $PATH | cut -d: -f 2-
/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/atguigu/bin

（5）切割ifconfig 后打印的IP地址
[atguigu@hadoop101 datas]$ ifconfig eth0 | grep "inet addr" | cut -d: -f 2 | cut -d" " -f1
192.168.1.102

```

#### 2）sed 工具
##### 基本介绍：
sed是一种流编辑器，它**一次处理一行内容**。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”，接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有改变，除非你使用重定向存储输出。

##### 基本用法：
sed [选项参数]  'command' 文件名

##### 选项参数说明:
选项参数 | 功能
---|---
-e | 直接在指令列模式上进行 sed 操作。

##### 命令功能描述：

命令 | 功能描述
---|---
a | 新增，a的后面能够添加字符串，在下一行后面出现
d | 删除
s | 查找并替换

##### 代码演示：
```
（0）数据准备
[atguigu@hadoop102 datas]$ touch sed.txt
[atguigu@hadoop102 datas]$ vim sed.txt
dong shen
guan zhen
wo  wo
lai  lai

le  le

（1）将“mei nv”这个单词插入到sed.txt第二行下，打印。
[atguigu@hadoop102 datas]$ sed '2a mei nv' sed.txt 
dong shen
guan zhen
mei nv
wo  wo
lai  lai

le  le

[atguigu@hadoop102 datas]$ cat sed.txt 
dong shen
guan zhen
wo  wo
lai  lai

le  le
注意：文件并没有改变
（2）删除sed.txt文件所有包含wo的行
[atguigu@hadoop102 datas]$ sed '/wo/d' sed.txt
dong shen
guan zhen
lai  lai

le  le
（3）将sed.txt文件中wo替换为ni
[atguigu@hadoop102 datas]$ sed 's/wo/ni/g' sed.txt 
dong shen
guan zhen
ni  ni
lai  lai

le  le
注意：‘g’表示global，全部替换
（4）将sed.txt文件中的第二行删除并将wo替换为ni
[atguigu@hadoop102 datas]$ sed -e '2d' -e 's/wo/ni/g' sed.txt 
dong shen
ni  ni
lai  lai

le  le

```

#### 3）awk 工具
##### 基本介绍：
一个枪法的文本分析工具，把文件逐行的读入，一空格符为默认分隔符将每行切片，切开的部分再进行分析处理。

##### 基本语法：
awk [选项参数] ‘pattern1{action1}  pattern2{action2}...’ filename

pattern：表示AWK在数据中查找的内容，就是匹配模式

action：在找到匹配内容时所执行的一系列命令

意思是匹配pattern1这个表达式的话，就执行action1这个功能；匹配了pattern2表达式的话，使用action2这个表达式。

##### 选项参数介绍：

选项参数 | 功能
---|---
-F | 指定输入文件拆分隔符
-v | 赋值一个用户定义变量

##### 代码演示：
```
（0）数据准备
[atguigu@hadoop102 datas]$ sudo cp /etc/passwd ./
（1）搜索passwd文件以root关键字开头的所有行，并输出该行的第7列。


[atguigu@hadoop102 datas]$ awk -F: '/^root/{print $7}' passwd 
/bin/bash


（2）搜索passwd文件以root关键字开头的所有行，并输出该行的第1列和第7列，中间以“，”号分割。
[atguigu@hadoop102 datas]$ awk -F: '/^root/{print $1","$7}' passwd 
root,/bin/bash

注意：只有匹配了pattern的行才会执行action


（3）只显示/etc/passwd的第一列和第七列，以逗号分割，且在所有行前面添加列名user，shell在最后一行添加"dahaige，/bin/zuishuai"。
[atguigu@hadoop102 datas]$ awk -F : 'BEGIN{print "user, shell"} {print $1","$7} END{print "dahaige,/bin/zuishuai"}' passwd
user, shell
root,/bin/bash
bin,/sbin/nologin
。。。
atguigu,/bin/bash
dahaige,/bin/zuishuai

注意：BEGIN 在所有数据读取行之前执行；END 在所有数据执行之后执行。


（4）将passwd文件中的用户id增加数值1并输出
[atguigu@hadoop102 datas]$ awk -v i=1 -F: '{print $3+i}' passwd
1
2
3
4
```

##### awk的内置变量

变量 |	说明
---|---
FILENAME |	文件名
NR |	已读的记录数
NF |	浏览记录的域的个数（切割后，列的个数）

##### 案例实操

```
（1）统计passwd文件名，每行的行号，每行的列数
[atguigu@hadoop102 datas]$ awk -F: '{print "filename:"  FILENAME ", linenumber:" NR  ",columns:" NF}' passwd 
filename:passwd, linenumber:1,columns:7
filename:passwd, linenumber:2,columns:7
filename:passwd, linenumber:3,columns:7
	  （2）切割IP
[atguigu@hadoop102 datas]$ ifconfig eth0 | grep "inet addr" | awk -F: '{print $2}' | awk -F " " '{print $1}' 
192.168.1.102
	  （3）查询sed.txt中空行所在的行号
[atguigu@hadoop102 datas]$ awk '/^$/{print NR}' sed.txt 
5
```

#### 4）sort 工具
##### 基本介绍：
sort命令是在Linux里非常有用，它将文件进行排序，并将排序结果标准输出。

##### 基本语法：
sort(选项)(参数)


选项 | 说明
---|---
-n |	依照数值的大小排序
-r	| 以相反的顺序来排序
-t	| 设置排序时所用的分隔字符
-k	| 指定需要排序的列

##### 代码演示：
```
（0）数据准备
[atguigu@hadoop102 datas]$ touch sort.sh
[atguigu@hadoop102 datas]$ vim sort.sh 
bb:40:5.4
bd:20:4.2
xz:50:2.3
cls:10:3.5
ss:30:1.6
（1）按照“：”分割后的第三列倒序排序。
[atguigu@hadoop102 datas]$ sort -t : -nrk 3  sort.sh 
bb:40:5.4
bd:20:4.2
cls:10:3.5
xz:50:2.3
ss:30:1.6

```

## 卸载jdk
用 rpm -qa | grep Java 命令来查询之前的jdk， 删除带箭头的。
![image](https://note.youdao.com/yws/api/personal/file/985A6C2776EF41B2A705413E3E1E6B99?method=download&shareKey=0ce42667d990e450cff39bf3c5389131)


## 企业真实面试题（重点）
### 1 京东
#### 问题1：使用Linux命令查询file1中空行所在的行号
答案：


```
[atguigu@hadoop102 datas]$ awk '/^$/{print NR}' sed.txt 
5
```

#### 问题2：有文件chengji.txt内容如下:

```
张三 40
李四 50
王五 60
使用Linux命令计算第二列的和并输出
```


```
[atguigu@hadoop102 datas]$ cat chengji.txt | awk -F " " '{sum+=$2} END{print sum}'
150
```

### 2 搜狐&和讯网
#### 问题1：Shell脚本里如何检查一个文件是否存在？如果不存在该如何处理？

```
#!/bin/bash

if [ -f file.txt ]; then
   echo "文件存在!"
else
   echo "文件不存在!"
fi
```

### 3 新浪
#### 问题1：用shell写一个脚本，对文本中无序的一列数字排序

```
[root@CentOS6-2 ~]# cat test.txt
9
8
7
6
5
4
3
2
10
1
```


```
[root@CentOS6-2 ~]# sort -n test.txt|awk '{a+=$0;print $0}END{print "SUM="a}'
1
2
3
4
5
6
7
8
9
10
SUM=55
```

### 3 金和网络
#### 问题1：请用shell脚本写出查找当前文件夹（/home）下所有的文本文件内容中包含有字符”shen”的文件名称

```
[atguigu@hadoop102 datas]$ grep -r "shen" /home | cut -d ":" -f 1
/home/atguigu/datas/sed.txt
/home/atguigu/datas/cut.txt
```
