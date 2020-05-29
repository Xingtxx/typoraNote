

### **Vim 编辑器与shell 命令脚本**

#### VIm 文本编辑器

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

#### 编写Shell 脚本

##### shell 简单介绍

1. ​	交互式（Interactive）：用户每输入一条命令就立即执行。
2. ​	批处理（Batch）：由用户事先编写好一个完整的Shell脚本，Shell会一次性执行脚本中诸多的命令。
3. 查看SHELL变量可以发现当前系统已经默认使用Bash作为命令行终端解释器了：

```shell
[root@linuxprobe ~]# echo $SHELL/bin/bash
```

##### 编写简单的脚本

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

##### 接收用户的参数

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

##### 判断用户参数

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

      

8. 

9. 

