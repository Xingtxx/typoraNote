

### **Vim 编辑器与shell 命令脚本**

##### VIm 文本编辑器

###### VIM编辑切换的方法

命令模式：控制光标移动，可对文本进行复制、粘贴、删除和查找等工作。

输入模式：正常的文本录入。

末行模式：保存或退出文档，以及设置编辑环境。

![vim不同模式间的切换](https://static.bookstack.cn/projects/linuxprobe/af2bfb18a24d7ca26dd6ff21405a9d89.png)

```sequence
输入模式-->命令模式:Esc健
命令模式->输入模式:a、i、o 等健
末行模式-->命令模式: Esc健
命令模式->末行模式: : 健
```



###### VIm中常用的命令

| 命令 | 作用                               |
| ---- | ---------------------------------- |
| dd   | 删除剪贴（整行）                   |
| yy   | 复制光标所在行                     |
| n    | 显示搜索命令定位到的下一个字符     |
| N    | 显示搜索命令定位到的上一个字符     |
| u    | 撤销上一步的操作                   |
| p    | 将删除dd yy 过的数据粘贴到光标后面 |



###### 末行模式中可用的命令

| 命令          | 作用                                 |
| ------------- | ------------------------------------ |
| :w            | 保存                                 |
| :q            | 退出                                 |
| :q!           | 强制退出（放弃对文档的修改内容）     |
| :wq!          | 强制保存退出                         |
| :set nu       | 显示行号                             |
| :set nonu     | 不显示行号                           |
| :命令         | 执行该命令                           |
| :整数         | 跳转到该行                           |
| :s/one/two    | 将当前光标所在行的第一个one替换成two |
| :s/one/two/g  | 将当前光标所在行的所有one替换成two   |
| :%s/one/two/g | 将全文中的所有one替换成two           |
| ?字符串       | 在文本中从下至上搜索该字符串         |
| /字符串       | 在文本中从上至下搜索该字符串         |

##### 编写Shell 脚本

###### shell 简单介绍

1. ​	交互式（Interactive）：用户每输入一条命令就立即执行。
2. ​	批处理（Batch）：由用户事先编写好一个完整的Shell脚本，Shell会一次性执行脚本中诸多的命令。
3. 查看SHELL变量可以发现当前系统已经默认使用Bash作为命令行终端解释器了：

```shell
[root@linuxprobe ~]# echo $SHELL/bin/bash
```

###### 编写简单的脚本

1. 实现查看当前路径，并显示目录下的所有文件及属性信息。


```shell
[root@linuxprobe ~]# vim example.sh
#!/bin/bash 
#For Example BY linuxprobe.com 
pwd 
ls -al
```

解析：

1. 文件名随意，但是需要以.sh 结尾，分辨文件。
2. "#!" 声明shell 解释器执行脚本
3. "#" 是注释：对该脚本说明
4. “剩下就是系统执行命令”

注意：权限问题

```shell
[root@linuxprobe ~]# ./example.sh
bash: ./Example.sh: Permission denied
[root@linuxprobe ~]# chmod u+x example.sh
[root@linuxprobe ~]# ./example.sh
/root/Desktop
total 8
drwxr-xr-x. 2 root root 23 Jul 23 17:31 .
dr-xr-x---. 14 root root 4096 Jul 23 17:31 ..
-rwxr--r--. 1 root root 55 Jul 23 17:31 example.sh
```

###### 接收用户的参数

1. Linux 系统中的shell ，已经内设用于接收参数变量，变量之间使用空格间隔。

2. 接受参数介绍

   1. "$#" 总共有几个参数

   2. "$*" 对应所有的位置参数(变量参数)

   3. ”$?“ 对应上一次的返回值（上一次命令返回的结果）

   4. ”$0“ 对应当前执行脚本名称

   5. ”$1,$2,$3“ $n 对应相应数字的位置参数

      ![4.2 编写Shell脚本 - 图1](https://static.bookstack.cn/projects/linuxprobe/af08146e62405d151354861067f656cb.png)

3.练习

```shell
[root@linuxprobe ~]# vim example.sh
#!/bin/bash
echo "当前脚本名称为$0"
echo "总共有$#个参数，分别是$*。"
echo "第1个参数为$1，第5个为$5。"
[root@linuxprobe ~]# sh example.sh one two three four five six
当前脚本名称为example.sh
总共有6个参数，分别是one two three four five six。
第1个参数为one，第5个为five。
```

###### 判断用户参数

1. shell 条件测试语法是否成立，成立返回数字0，不成立返回其他随机数字。

2. 注意：条件表达式两边均有一个空格!

   ![测试语句格式](https://static.bookstack.cn/projects/linuxprobe/5d33b775522c2427345499a7015da65f.png)

3. 测试对象划分

   ```
   1、文件测试语句
   2、逻辑测试语句
   3、整数值比较语句
   4、字符串比较语句
   ```

   

4. 文件测试语句

   | 操作符 | 作用                        |
   | ------ | --------------------------- |
   | -d     | 测试文件是否为目录类型      |
   | -e     | 测试文件是否存在            |
   | -f     | 判断是为一般文件            |
   | -r     | 测试当前用户是否有 可读权限 |
   | -w     | 测试当前用户是否有权限写入  |
   | -x     | 测试当前用户是否有权限执行  |

   下面使用文件测试语句来判断/etc/fstab是否为一个目录类型的文件

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ -d /etc/fstab ]
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
   1
   ```

   再使用文件测试语句来判断/etc/fstab是否为一般文件

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ -f /etc/fstab ]
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
   0
   ```

5.  逻辑测试语句

   1. "与"  运算符号 &&，表示当前命令执行成功后才会执行它后面的命令。 

      ```shell
      判断/dev/cdrom 文件是否存在，若纯在则输出Exist 字样
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ -e /dev/cdrom ] && echo "Exist"
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
      1
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ -e /dev ] && echo "Exist"
      Exist
      
      ```

   2. “或” 运算符号 || ，表示当前命令执行失败后才会执行它后面的命令

      ```shll
      判断当前用户是否是管理员身份
      [tang@iZwz9cn05w4ymx9od6osvkZ root]$ [ $USER = root ] || echo "user"
      user
      ```

   3. "非"运算符号(! )，把条件测试中的判断结果取相反，如果结果是正确的，则将其变成错误的，测试错误的结果则将其变成正确的。

   ```shell
   判断当前用户是否为一个非管理员的用户，由于判断结果因为两次否定而变成正确，因此会正常的输出预设信息
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ $USER != root ] || echo "administrator"
   administrator
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
   0
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# su tang
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ [ $USER != root ] || echo "administrator"
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ echo $?
   0
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ [ $USER != root ] || echo "administrator"
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ echo $?
   0
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ exit
   exit
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ $USER != root ] || echo "administrator"
   administrator
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
   0
   ```

   ```shell
   先判断当前登陆用户的USER变量名称是否等于root,然后用逻辑运算符“非”，进行取反操作，效果就变成了判断当前登陆的用户是否为非管理员用户了。若最后的条件成立则会根据逻辑“与”，运算符输出user 字样，或条件不满足则会通过逻辑“或”，运算符输出root 字样，而如果前面的&&不成立才会执行后面||的符号。
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ $USER != root ] && echo "user" || echo "root"
   root
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# su tang
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ [ $USER != root ] && echo "user" || echo "root"
   user
   [tang@iZwz9cn05w4ymx9od6osvkZ root]$ 
   
   ```

   

6. 可用的整数比较运算符

   1. 整数比较运算符仅对数字操作

      | 操作符 | 作用           |
      | ------ | -------------- |
      | -eq    | 是否等于       |
      | -ne    | 是否不等于     |
      | -gt    | 是否大于       |
      | -lt    | 是否小于       |
      | -le    | 是否等于或小于 |
      | -ge    | 是否大于或等于 |

      ```shell
      是否大于
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ 10 -gt 10 ]
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
      1
      是否等于
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ 10 -eq 10 ]
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
      0
      
      ```

      

7. free 

   1. 使用

      1. 获取当前系统正在使用的内存量信息.正在使用，及可用的内存信息量。
      2. free -m 查看内存使用量 单位(mb) 
      3. 通过grep Men ：过滤剩余内存量的行.
      4. 使用 awk "{print $4}" 命令值保留第4列
      5. 最后使用FreeMem =  语句 的方式把语句内执行的结果赋值给变量。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m
                    total        used        free      shared  buff/cache   available
      Mem:           7821        5222         183          13        2414        2286
      Swap:             0           0           0
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m |grep  Mem:
      Mem:           7821        5247         154          13        2420        2262
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m | grep Mem: | awk "{print $4}"
      Mem:           7821        5249         151          13        2420        2259
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m | grep Mem: | awk '{print $4}'
      155
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m | grep Mem: | awk '{print $4}'
      155
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# FreeMen="free -m | grep Men: | awk '{print $4}'"
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# FreeMen="free -m |grep Men: | awk '{print $4}'"
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $FreeMem 
      
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $FreeMem
      
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# FreeMem=`free -m | grep Mem: | awk '{print $4}'`
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $FreeMem
      141
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# 
      ```

      

   2. free 参数详解

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# free -m
                    total        used        free      shared  buff/cache   available
      Mem:           7821        5231         188          13        2401        2277
      Swap:             0           0           0
      ```

      | 参数       | 说明                                |
      | ---------- | ----------------------------------- |
      | Mem        | 内存使用信息                        |
      | Swap       | 交换空间的使用信息                  |
      | total      | 系统总的可用物理内存大小            |
      | used       | 已被使用的物理内存大小              |
      | free       | 被共享使用的物理可用                |
      | shared     | 被共享使用的物理内存大小            |
      | buff/cache | 被buffer 和cache 使用的物理内存大小 |
      | available  | 还可以被应用程序使用的物理内存大小  |

   3. 判断内存可用量是否小于1024 若小于提示 “Insufficient Memory”

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ $FreeMem -lt 1024 ] && echo "Insufficient Memory"
      Insufficient Memory
      [root@iZwz9cn05w4ymx9od6osvkZ ~]#
      ```

      

8. 字符串比较运算符

   1. 字符串比较语句用来判断字符串是否为空值。，或两个字符串是否相同。判断某个变量是否未被定义。

   2. 操作符的作用介绍。

      | 操作符 | 作用                   |
      | ------ | ---------------------- |
      | =      | 比较字符串内容是否相同 |
      | !=     | 比较字符串内容是否不同 |
      | -z     | 判断字符串内容是否为空 |

      判断sting 是否为空值

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [-z $string]
      -bash: [-z: command not found
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ -z $string]
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $?
      0
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# 
      ```

      保存当前语系环境变量值LANG 不是英语。满足条件输出NOT en.US

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo $LANG
      en_US.UTF-8
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# [ $LANG != 'en.US' ] && echo "Net en.US"
      Net en.US
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# 
      ```

###### 单引号双引号

1. 单引号属于强引用，它会忽略所有被引用起来的字符的特殊处理，被引用起来的字符会被原封不动的被使用，唯一需要注意点是不允许引用自身。

2. 双引号属于弱引用，它会对一些被引起来的字符进行特殊处理，主要包括以下情况

   1. $加变量 名可以获取变量值

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo "$PWD"
      /root
      ```

   2. 反引号和$() 引起来的字符会被当做命令执行后替换原来字符

      1. ```shell
         [root@iZwz9cn05w4ymx9od6osvkZ ~]#  echo '$PWD'
         $PWD
         [root@iZwz9cn05w4ymx9od6osvkZ ~]#  echo "$PWD"
         /root
         ```

      2. ```shell
         [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo '$(echo hello world)'
         $(echo hello world)
         [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo "$(echo hello world)"
         hello world
         [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo '`echo hello world`'
         `echo hello world`
         [root@iZwz9cn05w4ymx9od6osvkZ ~]#  echo "`echo hello world`"
         hello world
         ```



##### 流程控制语句

###### if 条件测试语句

1. if 语句分为单只结构、双只结构、多分支结构
2. 单分支语句 组成 关键字if;then;fi ,在条件成立后才执行的命令,相当于口语 “如果....那么....”

![%e5%8d%95%e5%88%86%e6%94%af%e7%bb%93%e6%9e%84](https://static.bookstack.cn/projects/linuxprobe/3e78c7cc8ebe49137621237b4623ff51.png)

单分支if 语句

if 语句判断/media/cdrom 文件是否存在，存在判断结束，不存在创建目录

```shell
#/bin/bash
DIR="/media/cdrom"

if [ ! -e $DIR ]
then
mkdir -p $DIR
fi
~                                                                                                                                                                                                         
~ 

[root@iZwz9cn05w4ymx9od6osvkZ ~]# rm -fr media/
[root@iZwz9cn05w4ymx9od6osvkZ ~]# vim mkcdrom.sh 
[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash mkcdrom.sh 
[root@iZwz9cn05w4ymx9od6osvkZ ~]# ls -d /media/cdrom/
/media/cdrom/
```

出现的问题

```shell
[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash mkcdrom.sh 
mkcdrom.sh: line 2: DIR: command not found

#/bin/bash
DIR = "/media/cdrom"
#原因：在定义变量时不能有空格
```

3. ​	双分支语句结构：if;then;else;fi 关键字组成，相当于口语“如果......那么......或者.......那么......”。

   ![%e5%8f%8c%e5%88%86%e6%94%af%e7%bb%93%e6%9e%84](https://static.bookstack.cn/projects/linuxprobe/b10d70a37bc6bffdbee5b18ac7d4694c.png)

   验证某台主机是否在线，根据返回值，打印信息，要么在线要么不在线，使用ping 来测试助教的网络联通性，需要通过-C 参数规定尝试次数，并使用-i 参数定义每个数据包的发送间隔。

   ```shell
   #!/bin/bash
   ping -c 3 -i 0.2 -W 3 $1 &> /dev/null
   
   if [ $? -eq 0 ]
   then
   echo "HOST $1 IS ON-LINE,"
   else
   echo "HOST $1 IS OFF-LINE "
   fi
   
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# bash chkhost.sh www.baidu.com
   HOST www.baidu.com IS ON-LINE,
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# bash chkhost.sh 192.168.10.20
   HOST 192.168.10.20 IS OFF-LINE 
   ```

   

4. 多分支结构 if 、then 、else 、elif 、fi 关键字。相当于语法 “如果......那么......如果......那么......”.

   ![%e5%a4%9a%e5%88%86%e6%94%af%e7%bb%93%e6%9e%84](https://static.bookstack.cn/projects/linuxprobe/db1fb0ca2fee1cd9e30653feaf6805fc.png)

   

   read 读取用户输入的信息命令,能够把接受到的变量赋值给后面指定变量，-p 参数用于显示一定的提示信息，number<85 and number>100 输出Excekkent ;number >70<=84 输出 Pass; 若两次为空就失败，输出Fail

   ```shell
   
   #!/bin/bash
   read -p "Enter your score (0-100):" GRADE
   if [ $GRADE -ge 85 ] && [ $GRADE -le 100 ] ; then
   echo "$GRADE is Excellent"
   elif [ $GRADE -ge 70 ] && [ $GRADE  -le 84 ] ; then
   echo "$GRADE is Pass"
   else
   echo "$GRADE is fall"
   fi
   
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# vim chkscore.sh 
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# bash chkscore.sh 
   Enter your score (0-100):30
   30 is fall
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# bash chkscore.sh 
   Enter your score (0-100):80
   80 is Pass
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# bash chkscore.sh 
   Enter your score (0-100):
   chkscore.sh: line 3: [: -ge: unary operator expected
   chkscore.sh: line 5: [: -ge: unary operator expected
    is fall
   运行报错：
   chkscore.sh: line 5: [: -ge: unary operator expected
   错误原因：
   
   由于变量rate初始化赋值为空，那么就成了 [  -ge "10"] 了，显然 [ 和 "10" 不相比较并且缺少了 [ 符号，所以报了这样的错误。
   
   解决办法：
   
   1、检查是否因为赋值语句写错而导致赋值为空
   
   2、赋值前加declare -i rate=0
   
   3、改成 if [[ $rate -ge 10 ]]  再加一对 []
   ```


###### for 循环语句的语法格式

1、一次性读取信息，然后逐一进行操作处理。处理数据有范围时，采用for 循环。

![for%e6%9d%a1%e4%bb%b6%e8%af%ad%e5%8f%a5](https://static.bookstack.cn/projects/linuxprobe/b771c587ae6d52e6c1019a97b678890e.png)

创建用户名列表文件，每个用户名单独一行。使用脚本读取，逐一创建。

```shell
[root@iZwz9cn05w4ymx9od6osvkZ ~]# vim users.txt
tang
adny
car1
duke
tang1

```

/dev/null 是一个成为Linux 的黑洞文件，包输出信息重定向到这个文件等于删除数据（类似于没有回收功能的垃圾想）

编写 Exanole.sh 使用read 命令读取用户输入密码值，然后赋值给passwd 变量，并通过-p 参数向用户展示提示信息。然后逐一使用id 命令查看用户信息，并使用$? 判断这条命令是否执行成功，也判断该用户是否存在。

```shell

#!/bin/bash
read -p "Enter Ther Users Password :" PASSWD

for UNAME in `cat users.txt`
do
id $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "$UNAME Already exists"
else
useradd $UNAME &> /dev/null
echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "$UNAME , Create success"
else
echo "$UNAME , Create failure"
fi
fi
done

[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash Example.sh 
Enter Ther Users Password :11111
dd , Create success
da , Create success
dc , Create success

[root@iZwz9cn05w4ymx9od6osvkZ ~]# tail -10 /etc/passwd
tang1:x:8892:8892::/home/tang1:/bin/bash
aa:x:8893:8893::/home/aa:/bin/bash
bb:x:8894:8894::/home/bb:/bin/bash
cc:x:8895:8895::/home/cc:/bin/bash
ac:x:8896:8896::/home/ac:/bin/bash
ae:x:8897:8897::/home/ae:/bin/bash
aw:x:8898:8898::/home/aw:/bin/bash
oo:x:8899:8899::/home/oo:/bin/bash
oa:x:8900:8900::/home/oa:/bin/bash
oq:x:8901:8901::/home/oq:/bin/bash

```

读取主机列表通过命令检测主机是否在线，脚本命令 $() 是一种类似于转义字符中反引号命令的shll 操作符。

```shell
[root@iZwz9cn05w4ymx9od6osvkZ ~]# vim ipadds.txt
www.baidu.com
www.github.com
www.google.com
~  
#!/bin/bash
HLIST=$(cat ~/ipadds.txt)
for IP in $HLIST
do
ping -c 3 -i 0.2 -W 3 $IP &> /dev/null
if [ $? -eq 0 ]; then
echo "Host $IP is On-line"
else
echo "Host $IP is OFF-line"
fi
done
```

###### while 条件循环语句

while 条件循环语句，是让脚本根据某些条件来重复执行命令的语句，while 根据判断条件来决定是否为真假来决定是否继续执行命令，若条件为真继续执行，若为假结束循环。

![while%e6%9d%a1%e4%bb%b6%e8%af%ad%e5%8f%a5](https://static.bookstack.cn/projects/linuxprobe/84c9881e54753d4c6b4a8931a13449bc.png)



编写搅拌猜测数值大小Guess.sh $RANDOM 变量来调取出一个随机数（0~32767），随机数对1000进行取余操作，并使用expr 命令取得结果，

```shell
#!/bin/bash
PRICE=$(expr $RANDOM % 1000)
ITMES=0
echo "0-999 之间"
while true
do
read -p "input number:" INT
let ITMES++
if [ $INT -eq $PRICE ] ; then
echo "success $PRICE"
echo "count $ITMES"
exit 0
elif [ $INT -gt $PRICE ] ; then
echo "太高了"
else
echo "太低了"
fi
done
~     

[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash Guess.sh 
0-999 之间
input number:100
太低了
input number:200
太低了
input number:300
太高了
input number:250
太高了
input number:240
太高了
input number:230
太高了
input number:220
太高了
input number:210
太低了
input number:215
太低了
input number:216
太低了
input number:217
太低了
input number:218
success 218
count 12

```

```shell
错误统计：
[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash Guess.sh 
expr: syntax error
0-999 之间
input number:222
Guess.sh: line 9: [: 222: unary operator expected
Guess.sh: line 13: [: 222: unary operator expected
太低了

在第9行 和第13行$获取的变量为空，无法做运算。
expr: syntax error  这一行语法错误。注意报错
```

###### case 条件测试语句

case 条件测试语句，多个范围内匹配数据，若匹配成功则执行相关命令并结束整个条件测试语句，如果不在所列出的范围内，则会执行星号。

通过checkkeys.sh 检测用户输入的值是字母还是数字还是其它字符

```shell
[root@iZwz9cn05w4ymx9od6osvkZ ~]# vim checkkeys.sh
#!/bin/bash
read -p "请输入字母，Enter 确认：" KEY

case "$KEY" in
[a-z]|[A-Z])
echo "输入是字母 $KEY"
;;
[0-9])
echo "输入是数字 $KEY"
;;
*)
echo "输入的是其他 $KEY"
esac

[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash checkkeys.sh 
请输入字母，Enter 确认：a
输入是字母 a
[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash checkkeys.sh 
请输入字母，Enter 确认：2
输入是数字 2
[root@iZwz9cn05w4ymx9od6osvkZ ~]# bash checkkeys.sh 
请输入字母，Enter 确认：///
输入的是其他 ///
[root@iZwz9cn05w4ymx9od6osvkZ ~]# 


```

###### 计划任务程序

1. 一次性计划任务

   1. 一次性任务

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at 23:30
   at> systemctl restart httpd
   at> <EOT>   
   job 6 at Wed Jun  3 23:30:00 2020
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# 
   # 使用ctrl+d 结束任务
   ```

   2. 使用任意门创建任务（一次性任务）

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# echo "systemctl restart httpd" | at 23:30
   job 7 at Wed Jun  3 23:30:00 2020
   
   ```

   3. 查看一次性计划任务

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at -l
   6	Wed Jun  3 23:30:00 2020 a root
   7	Wed Jun  3 23:30:00 2020 a root
   ```

   4.  如果设置多条，可以使用atrm 序号 删除任务

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at -l
   5	Wed Jun  3 23:30:00 2020 a root
   4	Wed Jun  3 23:30:00 2020 a root
   3	Wed Jun  3 23:30:00 2020 a root
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# atrm 3
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at -l
   5	Wed Jun  3 23:30:00 2020 a root
   4	Wed Jun  3 23:30:00 2020 a root
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# atrm 4
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at -l
   5	Wed Jun  3 23:30:00 2020 a root
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# atrm 5
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# at -l
   ```

2. 长期性计划任务（定时任务）

   1. 创建编辑 crontab -e , 查看当前计划任务 crontab -l  , 删除任务命令  crontab -r ,crontab命令中加上-u参数来编辑他人的计划任务。

   2. crond 服务得参数格式 “分，时，日，月，星期”

      ![cron计划任务的参数](https://static.bookstack.cn/projects/linuxprobe/673f94ef1f5fa82b8af6e0cf3afc75f0.png)

   3. 参数说明

      | 字段 | 说明                                     |
      | ---- | ---------------------------------------- |
      | 分钟 | 取值为0～59的整数                        |
      | 小时 | 取值为0～23的任意整数                    |
      | 日期 | 取值为1～31的任意整数                    |
      | 月份 | 取值为1～12的任意整数                    |
      | 星期 | 取值为0～7的任意整数，其中0与7均为星期日 |
      | 命令 | 要执行的命令或程序脚本                   |

   4. 符号参数说明

      | 符号      | 作用                                                         | 示例            |
      | --------- | ------------------------------------------------------------ | --------------- |
      | ，(逗号)  | 间隔多个时间段，表示多个时间段执行（例如“8,9,12”表示8月、9月和12月） | * *  * 8,9,12 * |
      | -  (减号) | 表示一段连续的时间周期（例如字段“日”的取值为“12-15”，则表示每月的12～15日） | * * 12-15 * *   |
      | /  (除号) | 表示执行任务的间隔时间（例如“*/2”表示每隔2分钟执行一次任务） | */2 * *  *  *   |

      

   5. 设置一个打包文件夹定时 每个周 1，3，5  的 16点26分打包一个目录。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# crontab -e
      no crontab for root - using an empty one
      crontab: installing new crontab
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# crontab -l
      25 3 * * 1,2,5  /usr/bin/tar -czvf backup.tar.gz /home/root
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# ls
      backup.tar.gz  CheckHosts.sh  checkkeys.sh  chkhost.sh  chkscore.sh  Example.sh  Guess.sh  ipadds.txt  mkcdrom.sh  users.txt
      ```

      ```shell
      26 16 * * 1,3,5  /usr/bin/tar -czvf /root/backup.tar.gz /root
      ```

   6. 计划每周一至走五凌晨一点自动清空 /root/backup.tar.gz 文件

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# whereis rm
      rm: /usr/bin/rm /usr/share/man/man1/rm.1.gz
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# crontab -e
      crontab: installing new crontab
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# crontab -l
      26 16 * * 1,3,5  /usr/bin/tar -czvf /root/backup.tar.gz /root
      0  1  * * 1-5    /usr/bin/rm -fr /root/backup.tar.gz 
      ```

      ```shell
      0  1  * * 1-5    /usr/bin/rm -fr /root/backup.tar.gz
      ```

   7. 小提示

      1. 在crond 服务计划所有的命令一定要用绝对路径方式来写
      2. 查询命令绝对路径 whereis  进行查询。

   8. 注意事项

      ​	1、在crond服务的配置参数中，可以像Shell脚本那样以#号开头写上注释信息，这样在日后回顾这段命令代码时可以快速了解其功能、需求以及编写人员等重要信息。

      ​	2、计划任务中的“分”字段必须有数值，绝对不能为空或是*号，而“日”和“星期”字段不能同时使用，否则就会发生冲突

##### 用户身份与文件权限

###### 用户身份与能力

1. 用户身份介绍

   1. 管理员UID为0 ：系统管理员用户。
   2. 系统用户UID为1~999：Linux 系统因避免某个服务程序出现咯偶东而被黑客提权至整台服务器，默认服务程序会有独立的系统用户负责运行，进而有效控制被破坏的有效范围。
   3. 普通用户UID从1000开始：由管理员创建的用于日常工作的用户。
   4. UID 是不能冲突的。
   5. GID （greup identification ）用户组，可以方便用户组中的用户统一规划权限或指定任务。
   6. 创建用户的同时会出现一个同名的用户组（叫做基本用户组），一个用户只有一个基本用户组。
   7. 扩展组 ，把该用户归纳到其它用户组（扩展组），一个用户可以有多个扩展用户组。

2. useradd 命令

   1. useradd 用于创建用户，格式 “useradd [选项] 用户名”

   2.  可以使用useradd命令创建用户账户。使用该命令创建用户账户时，默认的用户家目录会被存放在/home目录中，默认的[Shell](https://www.linuxcool.com/)解释器为/bin/bash，而且默认会创建一个与该用户同名的基本用户组

   3.   useradd 命令中的用户参数及作用

      | 参数 | 作用                                   |
      | ---- | -------------------------------------- |
      | -d   | 指定用户的家目录(默认为/home/username) |
      | -e   | 账户的到期时间 格式：YYYY-MM-DD        |
      | -u   | 指定该用户的默认UID                    |
      | -g   | 指定一个出事的用户基本组（必须已存在） |
      | -G   | 指定一个或多个扩展用户组               |
      | -N   | 不创建与用户同名的基本用户组           |
      | -s   | 指定该用户的默认Shll 解释器            |

3. 创建普通用户指定家目录，用户的UID 以及shell解释器

   ```shell
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# useradd -d /home/tang -u 8888 -s /sbin/nologin tang
   useradd: user 'tang' already exists
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# useradd -d /home/tangxx -u 8888 -s /sbin/nologin tangxx
   useradd: UID 8888 is not unique
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# useradd -d /home/tangxx -u 8889 -s /sbin/nologin tangxx
   useradd: UID 8889 is not unique
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# useradd -d /home/tangxx -u 9999 -s /sbin/nologin tangxx
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# id 9999
   uid=9999(tangxx) gid=9999(tangxx) groups=9999(tangxx)
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# id 8888
   uid=8888(tang) gid=8888(tang) groups=8888(tang)
   [root@iZwz9cn05w4ymx9od6osvkZ ~]# id 8889
   uid=8889(adny) gid=8889(adny) groups=8889(adny)
   ```

   

4. groupadd 命令

   1. groupadd 命令用户创建用户组， 格式为 “groupadd [选项] 群组名”

   2. 可以更加高效指派系统中的用户权限，把用户加到一个组统一管理。安排权限。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# groupadd ronny
      ```

5. usermod 命令 

   1. 1. usermod 命令用户修改用户的属性，格式为 “usermod [选项] 用户名”

      2. Linux系统中一切都是文件，因此在系统中创建用户也是修改配置文件。用户信息 保存在/etc/passwd 中。

      3. usermod 命令中的参数及作用

         | 参数  | 作用                                                         |
         | ----- | ------------------------------------------------------------ |
         | -c    | 填写用户账户的备注信息                                       |
         | -d -m | -d 和-m 联合使用，可以重新指定用户的家目录，并自动转移转移旧目录数据 |
         | -e    | 账户的到期时间  YYYY-MM-DD                                   |
         | -g    | 变更所属用户组                                               |
         | -G    | 变更扩展用户组                                               |
         | -L    | 锁定用户禁止其登陆系统                                       |
         | -U    | 解锁用户，允许其登录系统                                     |
         | -s    | 变更默认终端                                                 |
         | -u    | 修改用户UID                                                  |

         

   2. 把tang 用户加到root 组

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tang
      uid=8888(tang) gid=8888(tang) groups=8888(tang)
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# usermod -G root tang
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tang
      uid=8888(tang) gid=8888(tang) groups=8888(tang),0(root)
      
      ```

      

   3. 修改用户的UID 号码值

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# usermod -u 1000 tang
      usermod: UID '1000' already exists
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# usermod -u 1001 tang
      usermod: UID '1001' already exists
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# usermod -u 7777 tang
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tang
      uid=7777(tang) gid=8888(tang) groups=8888(tang),0(root)
      ```

6. passwd 命令

   1. passwd 用户修改用户密码，过期时间,认证信息等，格式 ：“passwd [选项] [用户名]”

   2. 参数及作用

      | 参数     | 作用                                                         |
      | -------- | ------------------------------------------------------------ |
      | -l       | 锁定用户，禁止登陆                                           |
      | -u       | 解锁用户，允许用户登录                                       |
      | -- stdin | 允许通过标准输入修改用户密码   示例：echo  “NewPassWord” \| passwd --stdin Username |
      | -d       | 使该用户可用空密码登陆系统                                   |
      | -e       | 强制用户下次登陆时修改密码                                   |
      | -S       | 显示用户的密码是否被锁定以及密码所采用的加密算法名称。       |

   3. 修改自己密码，及其他人的密码。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd 
      Changing password for user root.
      New password: 
      BAD PASSWORD: The password is shorter than 8 characters
      Retype new password: 
      passwd: all authentication tokens updated successfully.
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd tang
      Changing password for user tang.
      New password: 
      BAD PASSWORD: The password is shorter than 8 characters
      Retype new password: 
      passwd: all authentication tokens updated successfully.
      ```

   4. 使用passwd 禁止用户远程登陆

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd -l tang
      Locking password for user tang.
      passwd: Success
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd -S tang
      tang LK 2020-06-04 0 99999 7 -1 (Password locked.)
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd -u tang
      Unlocking password for user tang.
      passwd: Success
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# passwd -S tang
      tang PS 2020-06-04 0 99999 7 -1 (Password set, SHA512 crypt.)
      ```

      

   

7. userdel 命令

   1. userdel命令用于删除用户，格式为“userdel [选项] 用户名”。

   2. 可以删除用户保留家目录，可以删除用户所有信息。

   3. 参数及作用

      | 参数 | 作用                     |
      | ---- | ------------------------ |
      | -f   | 强制删除用户             |
      | -r   | 同时删除用户及用户家目录 |

   4. 删除用户 

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tangxx
      uid=9999(tangxx) gid=9999(tangxx) groups=9999(tangxx)
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tang
      uid=7777(tang) gid=8888(tang) groups=8888(tang),0(root)
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# userdel -r tangxx
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# userdel -r tang
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# id tang
      id: tang: no such user
      
      ```

###### 文件权限与归属

1. 文件类型介绍
   1. -：普通文件
   2. d: 目录文件
   3. l : 链接文件
   4. b:块设备文件
   5. c: 字符设备文件
   6. p: 管道文件

2. 文件权限介绍

   1. 文件都有所属的所有者和所有组

   2. 文件都规定了 所有者和 所有组及其他人，对其 读（r）写（w）执行（x）.

   3. 文件权限字符与字数的表示。

      | 权限项   | 读     | 写     | 执行   | 读     | 写     | 执行   | 读   | 写   | 执行 |
      | -------- | ------ | ------ | ------ | ------ | ------ | ------ | ---- | ---- | ---- |
      | 字符表示 | r      | w      | x      | r      | w      | x      | r    | w    | x    |
      | 数字表示 | 4      | 2      | 1      | 4      | 2      | 1      | 4    | 2    | 1    |
      | 权限分配 | 所有者 | 所有者 | 所有者 | 所属组 | 所属组 | 所属组 | 其它 | 其它 | 其它 |

      ![文件权限](https://static.bookstack.cn/projects/linuxprobe/7dea36c5f2d831201007da64ec35e3e7.png)
      
      
      
      

在图中，包含了文件的类型、访问权限、所有者（属主）、所属组（属组）、占用的磁盘大小、修改时间和文件名称等信息。通过分析可知，该文件的类型为普通文件，所有者权限为可读、可写（rw-），所属组权限为可读（r—），除此以外的其他人也只有可读权限（r—），文件的磁盘占用大小是34298字节，最近一次的修改时间为4月2日的凌晨23分，文件的名称为install.log。

###### 文件的特殊权限

1. SUID

   1. 是一种对二进制程序进行的特殊权限，可以让二进制的执行者临时拥有属主的权限。

   2. 文件授予临时权限时 ，正常的rwx 权限没有x ,会比纳城rwS 变成大写的S。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# ls -l /etc/shadow
      ---------- 1 root root 2778 Jun  4 11:07 /etc/shadow
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# ls -l /bin/passwd
      -rwsr-xr-x. 1 root root 27856 Aug  9  2019 /bin/passwd
      ```

2. SGID

   1. SGID 主要实现如下两种功能。

      1. 让执行者临时拥有属组的权限（对拥有执行权限的二进制程序进行设置）

      2. 在某个目录中创建的文件自动继承该目录的用户组（只可以对目录进行设置）

      3. 在早期linunx 系统中 /dev/kmem 是一个字符设备文件，只有system用户能访问，及root 组才有权限

         ```shell
         cr—r——- 1 root system 2, 1 Feb 11 2017 kmem
         ```

      4.  除了root 及root 组的 成员外，所有用户没有读取该文件的权限，由于平时需要查看系统进程状态，未来获取到进程状态的信息，可以在ps命令文件上增加SGID 特殊权限位。

         ```shell
         [root@iZwz9cn05w4ymx9od6osvkZ usr]# ls -la /usr/bin/ps
         -rwxr-xr-x 1 root root 100112 Oct 19  2019 /usr/bin/ps
         ```

         ```shell
         -r-xr-sr-x 1 bin root 59346 Feb 11 2017 ps
         ```

      5. 每个文件都有归属的所有者所有组，在创建的时候都会归属与创建人员。如果需要创建一个公共目录，所有人创建的时候，自动继承目录用户组权限，而不是自己的基本用户组,这就用到SGID第二个功能。

         ```shell
         [root@iZwz9cn05w4ymx9od6osvkZ /]# cd /tmp/
         [root@iZwz9cn05w4ymx9od6osvkZ tmp]# mkdir testdir
         [root@iZwz9cn05w4ymx9od6osvkZ tmp]# ls -ald testdir/
         drwxr-xr-x 2 root root 4096 Jun  5 17:09 testdir/
         [root@iZwz9cn05w4ymx9od6osvkZ tmp]# chmod -Rf 777 testdir/
         [root@iZwz9cn05w4ymx9od6osvkZ tmp]# chmod -Rf g+s testdir/
         [root@iZwz9cn05w4ymx9od6osvkZ tmp]# ls -ald testdir/
         drwxrwsrwx 2 root root 4096 Jun  5 17:09 testdir/
         ```

         上诉命令设置目录777 权限 （确保普通用户能够写入文件）并设置SGID特殊权限位后，就可以切换至一个普通用户，然后尝试在目录创建文件，查看是否继承文件所在目录所属组权限。

         ```shell
         [root@iZwz9cn05w4ymx9od6osvkZ ~]# su - tang
         Last login: Fri Jun  5 17:38:27 CST 2020 on pts/0
         [tang@iZwz9cn05w4ymx9od6osvkZ ~]$ cd /tmp/testdir/
         [tang@iZwz9cn05w4ymx9od6osvkZ testdir]$ echo "tang" > test.txt
         [tang@iZwz9cn05w4ymx9od6osvkZ testdir]$ ls -al test.txt 
         -rw-rw-r-- 1 tang root 5 Jun  5 17:40 test.txt
         ```

      6. chmod [参数] 所有者:所属组 文件或名称 ，能够用来设置文件或目录权限。如果要把文件的权限设置成所有者可读可写可执行，所属组可读可写，其它人没有任何权限

         1. 字母对应 rwxrw----
         2. 数字法 750

         ```shell
         [tang@iZwz9cn05w4ymx9od6osvkZ testdir]$ ls -la test.txt 
         -rw-rw-r-- 1 tang root 5 Jun  5 17:40 test.txt
         [tang@iZwz9cn05w4ymx9od6osvkZ testdir]$ chmod 760 test.txt 
         [tang@iZwz9cn05w4ymx9od6osvkZ testdir]$ ls -la test.txt 
         -rwxrw---- 1 tang root 5 Jun  5 17:40 test.txt
         ```

         3. chmod 和 chown 命令是用于修改文件属性和权限醉常用的命令，如果修改目录就要加上大写参数 -R 来递归操作，即对目录内所有的文件进行整体操作。

3. SBIT

   1. 特殊权限位之粘贴位，SBIT可以保护用户只能删除自己的文件，而不能删除其它用户的文件。

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# su tang
      [tang@iZwz9cn05w4ymx9od6osvkZ root]$ ls -lad /tmp/
      drwxrwxrwt. 9 root root 4096 Jun  5 17:09 /tmp/
      [tang@iZwz9cn05w4ymx9od6osvkZ root]$ cd /tmp/
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ ls -lad
      drwxrwxrwt. 9 root root 4096 Jun  5 17:09 .
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ echo "tang toto" > test1.txt
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ ls -al test1.txt 
      -rw-rw-r-- 1 tang tang 10 Jun  5 18:30 test1.txt
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ chmod 777
      chmod: missing operand after ‘777’
      Try 'chmod --help' for more information.
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ chmod 777 test1.txt 
      [tang@iZwz9cn05w4ymx9od6osvkZ tmp]$ ls -al test1.txt 
      -rwxrwxrwx 1 tang tang 10 Jun  5 18:30 test1.txt
      ```

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ /]# cd /tmp/
      [tangxx@iZwz9cn05w4ymx9od6osvkZ tmp]$ ls -la test1.txt 
      -rwxrwxrwx 1 tang tang 10 Jun  5 18:30 test1.txt
      [root@iZwz9cn05w4ymx9od6osvkZ tmp]# su - tangxx
      Last login: Fri Jun  5 18:33:46 CST 2020 on pts/0
      [tangxx@iZwz9cn05w4ymx9od6osvkZ tmp]$ rm -fr test1.txt 
      rm: cannot remove ‘test1.txt’: Operation not permitted
      [tangxx@iZwz9cn05w4ymx9od6osvkZ tmp]$ 
      ```

   2. 要是也想对其他目录来设置SBIT特殊权限位，用chmod命令就可以了。对应的参数o+t代表设置SBIT粘滞位权限

      ```shell
      [root@iZwz9cn05w4ymx9od6osvkZ tmp]# cd ~
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# mkdir linux
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# chmod -R o+t linux/
      [root@iZwz9cn05w4ymx9od6osvkZ ~]# ls -ld linux/
      drwxr-xr-t 2 root root 4096 Jun  5 19:50 linux/
      ```

4. 

