### 一、进程相关

#### 1、ps

 Unix风格的参数

```txt
-A 显示所有进程
-N 显示与指定参数不符的所有进程
-a 显示除控制进程（session leader①）和无终端进程外的所有进程
-d 显示除控制进程外的所有进程
-e 显示所有进程
-C cmdlist 显示包含在cmdlist列表中的进程
-G grplist 显示组ID在grplist列表中的进程
-U userlist 显示属主的用户ID在userlist列表中的进程
-g grplist 显示会话或组ID在grplist列表中的进程②
-p pidlist 显示PID在pidlist列表中的进程
-s sesslist 显示会话ID在sesslist列表中的进程
-t ttylist 显示终端ID在ttylist列表中的进程
-u userlist 显示有效用户ID在userlist列表中的进程
-F 显示更多额外输出（相对-f参数而言）
-O format 显示默认的输出列以及format列表指定的特定列
-M 显示进程的安全信息
-c 显示进程的额外调度器信息
-f 显示完整格式的输出
-j 显示任务信息
-l 显示长列表
-o format 仅显示由format指定的列
-y 不要显示进程标记（process flag，表明进程状态的标记）
-Z 显示安全标签（security context）①信息
-H 用层级格式来显示进程（树状，用来显示父进程）
-n namelist 定义了WCHAN列显示的值
-w 采用宽输出模式，不限宽度显示
-L 显示进程中的线程
-V 显示ps命令的版本号
```

举例

```shell
#这个例子用了两个参数：-e参数指定显示所有运行在系统上的进程；-f参数则扩展了输出，这些扩展的列包含了有用的信息。
$ ps -ef
UID PID PPID C STIME TTY TIME CMD
root 1 0 0 11:29 ? 00:00:01 init [5]
root 2 0 0 11:29 ? 00:00:00 [kthreadd]
root 3 2 0 11:29 ? 00:00:00 [migration/0]
root 4 2 0 11:29 ? 00:00:00 [ksoftirqd/0]
root 5 2 0 11:29 ? 00:00:00 [watchdog/0]
root 6 2 0 11:29 ? 00:00:00 [events/0]
root 7 2 0 11:29 ? 00:00:00 [khelper]
root 47 2 0 11:29 ? 00:00:00 [kblockd/0]
root 48 2 0 11:29 ? 00:00:00 [kacpid]
68 2349 1 0 11:30 ? 00:00:00 hald
root 3078 1981 0 12:00 ? 00:00:00 sshd: rich [priv]
rich 3080 3078 0 12:00 ? 00:00:00 sshd: rich@pts/0
rich 3081 3080 0 12:00 pts/0 00:00:00 -bash
rich 4445 3081 3 13:48 pts/0 00:00:00 ps -ef

# UID：启动这些进程的用户。
# PID：进程的进程ID。
# PPID：父进程的进程号（如果该进程是由另一个进程启动的）。
# C：进程生命周期中的CPU利用率。
# STIME：进程启动时的系统时间。
# TTY：进程启动时的终端设备。
# TIME：运行进程需要的累计CPU时间。
# CMD：启动的程序名称。
```

```
# -l参数，它会产生一个长格式输出。
$ ps -l
F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
0 S 500 3081 3080 0 80 0 - 1173 wait pts/0 00:00:00 bash
0 R 500 4463 3081 1 80 0 - 1116 - pts/0 00:00:00 ps
# 注意使用了-l参数之后多出的那些列。
# F：内核分配给进程的系统标记。
# S：进程的状态（O代表正在运行；S代表在休眠；R代表可运行，正等待运行；Z代表僵化，进程已结束但父进程已不存在；T代表停止）。
# PRI：进程的优先级（越大的数字代表越低的优先级）。
# NI：谦让度值用来参与决定优先级。
# ADDR：进程的内存地址。
# SZ：假如进程被换出，所需交换空间的大致大小。
# WCHAN：进程休眠的内核函数的地址。
```

#### 2、top

```shell
$ top
```

结果如图

![image-20201210143351941](/Users/libin/Library/Application Support/typora-user-images/image-20201210143351941.png)

​		输出的第一部分显示的是系统的概况：第一行显示了当前时间、系统的运行时间、登录的用户数以及系统的平均负载。平均负载有3个值：最近1分钟的、最近5分钟的和最近15分钟的平均负载。值越大说明系统的负载越高。由于进程短期的突发性活动，出现最近1分钟的高负载值也很常见，但如果近15分钟内的平均负载都很高，就说明系统可能有问题。
​		第二行显示了进程概要信息——top命令的输出中将进程叫作任务（task）：有多少进程处在运行、休眠、停止或是僵化状态（僵化状态是指进程完成了，但父进程没有响应）。
​		第三行显示了CPU的概要信息。top根据进程的属主（用户还是系统）和进程的状态（运行、空闲还是等待）将CPU利用率分成几类输出。
​		紧跟其后的两行说明了系统内存的状态。第一行说的是系统的物理内存：总共有多少内存，当前用了多少，还有多少空闲。后一行说的是同样的信息，不过是针对系统交换空间（如果分配了的话）的状态而言的。
​		最后一部分显示了当前运行中的进程的详细列表，有些列跟ps命令的输出类似。
 PID：进程的ID。
 USER：进程属主的名字。
 PR：进程的优先级。
 NI：进程的谦让度值。
 VIRT：进程占用的虚拟内存总量。
 RES：进程占用的物理内存总量。
 SHR：进程和其他进程共享的内存总量。
 S：进程的状态（D代表可中断的休眠状态，R代表在运行状态，S代表休眠状态，T代表跟踪状态或停止状态，Z代表僵化状态）。
 %CPU：进程使用的CPU时间比例。
 %MEM：进程使用的内存占可用内存的比例。
 TIME+：自进程启动到目前为止的CPU时间总量。
 COMMAND：进程所对应的命令行名称，也就是启动的程序名。

​		默认情况下，top命令在启动时会按照%CPU值对进程排序。可以在top运行时使用多种交互命令重新排序。每个交互式命令都是单字符，在top命令运行时键入可改变top的行为。键入f允许你选择对输出进行排序的字段，键入d允许你修改轮询间隔。键入q可以退出top。用户在top命令的输出上有很大的控制权。用这个工具就能经常找出占用系统大部分资源的罪魁祸首。当然了，一旦找到，下一步就是结束这些进程。这也正是接下来的话题。

#### 3、结束进程

​		在Linux中，进程之间通过信号来通信。进程的信号就是预定义好的一个消息，进程能识别它并决定忽略还是作出反应。进程如何处理信号是由开发人员通过编程来决定的。大多数编写完善的程序都能接收和处理标准Unix进程信号。

| 信 号 | 名 称 |   描 述|
|------|------|--------|
| 1    | HUP  |    挂起|
| 2    | INT  |    中断|
| 3    | QUIT |    结束运行|
| 9    | KILL |    无条件终止|
| 11   | SEGV |    段错误|
| 15   | TERM |    尽可能终止|
| 17   | STOP |    无条件停止运行，但不终止|
| 18   | TSTP |    停止或暂停，但继续在后台运行|
| 19   | CONT |    在STOP或TSTP之后恢复执行|

1）、kill

```shell
$ kill -9 pid
```

2)、killall

killall命令非常强大，它支持通过进程名而不是PID来结束进程。killall命令也支持通配符，这在系统因负载过大而变得很慢时很有用。

```shell
$ killall http*
```

上例中的命令结束了所有以http开头的进程，比如Apache Web服务器的httpd服务

### 二、磁盘相关

#### 1、df

​		有时你需要知道在某个设备上还有多少磁盘空间。df命令可以让你很方便地查看所有已挂载磁盘的使用情况

```shell
$ df
Filesystem 1K-blocks Used Available Use% Mounted on
/dev/sda2 18251068 7703964 9605024 45% /
/dev/sda1 101086 18680 77187 20% /boot
tmpfs 119536 0 119536 0% /dev/shm
/dev/sdb1 127462 113892 13570 90% /media/disk
```

​		df命令会显示每个有数据的已挂载文件系统。如你在前例中看到的，有些已挂载设备仅限系统内部使用。命令输出如下：
- 设备的设备文件位置；

- 能容纳多少个1024字节大小的块；

- 已用了多少个1024字节大小的块；

- 还有多少个1024字节大小的块可用；

- 已用空间所占的比例；

- 设备挂载到了哪个挂载点上。

  ​	df命令有一些命令行参数可用，但基本上不会用到。一个常用的参数是-h。它会把输出中的磁盘空间按照用户易读的形式显示，通常用M来替代兆字节，用G替代吉字节。

  ```shell
  $ df -h
  ```

#### 2、du

​		du命令可以显示某个特定目录（默认情况下是当前目录）的磁盘使用情况。这一方法可用来快速判断系统上某个目录下是不是有超大文件。
​		默认情况下，du命令会显示当前目录下所有的文件、目录和子目录的磁盘使用情况，它会以磁盘块为单位来表明每个文件或目录占用了多大存储空间。对标准大小的目录来说，这个输出会是一个比较长的列表。下面是du命令的部分输出：

```shell
$ du
484 ./.gstreamer-0.10
8 ./Templates
8 ./Download
8 ./.ccache/7/0
24 ./.ccache/7
368 ./.ccache/a/d
384 ./.ccache/a
424 ./.ccache
8 ./Public
8 ./.gphpedit/plugins
32 ./.gphpedit
72 ./.gconfd
128 ./.nautilus/metafiles
384 ./.nautilus
72 ./.bittorrent/data/metainfo
```

下面是能让du命令用起来更方便的几个命令行参数。
- -c：显示所有已列出文件总的大小。
- -h：按用户易读的格式输出大小，即用K替代千字节，用M替代兆字节，用G替代吉字节。
- -s：显示每个输出参数的总计。

### 三、处理数据文件

#### 1、sort

​		默认情况下，sort命令会把数字当做字符来执行标准的字符排序，产生的输出可能根本就不是你要的。解决这个问题可用-n参数，它会告诉sort命令把数字识别成数字而不是字符，并且按值排序。

| 单破折线 | 双破折线                  | 描述                                                         |
| -------- | ------------------------- | ------------------------------------------------------------ |
| -b       | --ignore-leading-blanks   | 排序时忽略起始的空白                                         |
| -C       | --check=quiet             | 不排序，如果数据无序也不要报告                               |
| -c       | --check                   | 不排序，但检查输入数据是不是已排序；未排序的话，报告         |
| -d       | --dictionary-order        | 仅考虑空白和字母，不考虑特殊字符                             |
| -f       | --ignore-case             | 默认情况下，会将大写字母排在前面；这个参数会忽略大小写       |
| -g       | --general-number-sort     | 按通用数值来排序（跟-n不同，把值当浮点数来排序，支持科学计数法表示的值） |
| -i       | --ignore-nonprinting      | 在排序时忽略不可打印字符                                     |
| -k       | --key=POS1[,POS2]         | 排序从POS1位置开始；如果指定了POS2的话，到POS2位置结束       |
| -M       | --month-sort              | 用三字符月份名按月份排序                                     |
| -m       | --merge                   | 将两个已排序数据文件合并                                     |
| -n       | --numeric-sort            | 按字符串数值来排序（并不转换为浮点数）                       |
| -o       | --output=file             | 将排序结果写出到指定的文件中                                 |
| -R       | --random-sort             | 按随机生成的散列表的键值排序                                 |
|          | --random-source=FILE      | 指定-R参数用到的随机字节的源文件                             |
| -r       | --reverse                 | 反序排序（升序变成降序）                                     |
| -S       | --buffer-size=SIZE        | 指定使用的内存大小                                           |
| -s       | --stable                  | 禁用最后重排序比较                                           |
| -T       | --temporary-directory=DIR | 指定一个位置来存储临时工作文件                               |
| -t       | --field-separator=SEP     | 指定一个用来区分键位置的字符                                 |
| -u       | --unique                  | 和-c参数一起使用时，检查严格排序；不和-c参数一起用时，仅输出第一例相似的两行 |
| -z       | --zero-terminated         | 用NULL字符作为行尾，而不是用换行符                           |

​		-k和-t参数在对按字段分隔的数据进行排序时非常有用，例如/etc/passwd文件。可以用-t参数来指定字段分隔符，然后用-k参数来指定排序的字段。举个例子，要对前面提到的密码文件/etc/passwd根据用户ID进行数值排序，可以这么做：

```shell
$ sort -t ':' -k 3 -n /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
news:x:9:13:news:/etc/news:
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
```

现在数据已经按第三个字段——用户ID的数值排序。
-n参数在排序数值时非常有用，比如du命令的输出。

```shell
$ du -sh * | sort -nr
1008k mrtg-2.9.29.tar.gz
972k bldg1
888k fbs2.pdf
760k Printtest
680k rsync-2.6.6.tar.gz
660k code
516k fig1001.tiff
496k test
496k php-common-4.0.4pl1-6mdk.i586.rpm
448k MesaGLUT-6.5.1.tar.gz
400k plp
```

注意，-r参数将结果按降序输出，这样就更容易看到目录下的哪些文件占用空间最多

#### 2、grep

你会经常需要在大文件中找一行数据，而这行数据又埋藏在文件的中间。这时并不需要手动
翻看整个文件，用grep命令来帮助查找就行了。grep命令的命令行格式如下。

```
grep [options] pattern [file]
```

grep命令会在输入或指定的文件中查找包含匹配指定模式的字符的行。grep的输出就是包含了匹配模式的行。

```shell
# 在文件file1中搜索能匹配模式three的文本。grep命令输出了匹配了该模式的行
$ grep three file1
three
# 如果要进行反向搜索（输出不匹配该模式的行），可加-v参数。
$ grep -v t file1
one
four
five
$
# 如果要显示匹配模式的行所在的行号，可加-n参数。
$ grep -n t file1
2:two
3:three
$
# 如果只要知道有多少行含有匹配的模式，可用-c参数。
$ grep -c t file1
2
$
# 如果要指定多个匹配模式，可用-e参数来指定每个模式。
# 这个例子输出了含有字符t或字符f的所有行。
$ grep -e t -e f file1
two
three
four
five
$
```

#### 3、gzip

gzip是Linux上最流行的压缩工具。
- gzip：用来压缩文件。
- gzcat：用来查看压缩过的文本文件的内容。
- gunzip：用来解压文件。

```shell
$ gzip myprog
$ ls -l my*
-rwxrwxr-x 1 rich rich 2197 2007-09-13 11:29 myprog.gz
$
# gzip命令会压缩你在命令行指定的文件。也可以在命令行指定多个文件名甚至用通配符来一次性批量压缩文件。
$ gzip my*
$ ls -l my*
-rwxr--r-- 1 rich rich 103 Sep 6 13:43 myprog.c.gz
-rwxr-xr-x 1 rich rich 5178 Sep 6 13:43 myprog.gz
-rwxr--r-- 1 rich rich 59 Sep 6 13:46 myscript.gz
-rwxr--r-- 1 rich rich 60 Sep 6 13:44 myscript2.gz
$
# gzip命令会压缩该目录中匹配通配符的每个文件。
```

#### 4、tar

虽然zip命令能够很好地将数据压缩和归档进单个文件，但它不是Unix和Linux中的标准归档工具。目前，Unix和Linux上最广泛使用的归档工具是tar命令。tar命令最开始是用来将文件写到磁带设备上归档的，然而它也能把输出写到文件里，这种用法在Linux上已经普遍用来归档数据了。下面是tar命令的格式：

```tex
tar function [options] object1 object2 ...
```

function参数定义了tar命令应该做什么，如表所示

| 功能 | 长名称        | 描述                                                         |
| ---- | ------------- | ------------------------------------------------------------ |
| -A   | --concatenate | 将一个已有tar归档文件追加到另一个已有tar归档文件             |
| -c   | --create      | 创建一个新的tar归档文件                                      |
| -d   | --diff        | 检查归档文件和文件系统的不同之处                             |
|      | --delete      | 从已有tar归档文件中删除                                      |
| -r   | --append      | 追加文件到已有tar归档文件末尾                                |
| -t   | --list        | 列出已有tar归档文件的内容                                    |
| -u   | --update      | 将比tar归档文件中已有的同名文件新的文件追加到该tar归档文件中 |
| -x   | --extract     | 从已有tar归档文件中提取文件                                  |

每个功能可用选项来针对tar归档文件定义一个特定行为。下表列出了这些选项中能和tar命令一起使用的常见选项。

| 选项    | 描述                              |
| ------- | --------------------------------- |
| -C dir  | 切换到指定目录                    |
| -f file | 输出结果到文件或设备file          |
| -j      | 将输出重定向给bzip2命令来压缩内容 |
| -p      | 保留所有文件权限                  |
| -v      | 在处理文件时显示文件              |
| -z      | 将输出重定向给gzip命令来压缩内容  |

这些选项经常合并到一起使用。首先，你可以用下列命令来创建一个归档文件：
```shell
$ tar -cvf test.tar test/ test2/
```
上面的命令创建了名为test.tar的归档文件，含有test和test2目录内容。接着，用下列命令：
```shell
$ tar -tf test.tar
```
列出tar文件test.tar的内容（但并不提取文件）。最后，用命令：
```shell
$ tar -xvf test.tar
```
通过这一命令从tar文件test.tar中提取内容。如果tar文件是从一个目录结构创建的，那整个目录结构都会在当前目录下重新创建。
如你所见，tar命令是给整个目录结构创建归档文件的简便方法。这是Linux中分发开源程序源码文件所采用的普遍方法。

