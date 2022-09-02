[#]: subject: "The Basic Concepts of Shell Scripting"
[#]: via: "https://www.opensourceforu.com/2022/05/the-basic-concepts-of-shell-scripting/"
[#]: author: "Sathyanarayanan Thangavelu https://www.opensourceforu.com/author/sathyanarayanan-thangavelu/"
[#]: collector: "lkxed"
[#]: translator: "FYJNEVERFOLLOWS"
[#]: reviewer: " "
[#]: publisher: " "
[#]: url: " "

有关 shell 脚本的一些基本概念
======
如果你希望将常规任务自动化并使你的生活更轻松，那么使用 shell 脚本是一个很好的选择。本文将向你介绍一些基本概念，这些概念将帮助你编写高效的 shell 脚本。

![Shell-scripting][1]
shell 脚本是一种被设计用来运行 UNIX shell（命令行解释器）的计算机程序。shell 脚本的各类变种被认为是脚本语言。shell 脚本执行的典型操作包括文件操作、程序执行和文本打印。设置环境、运行程序并执行任何必要的清理或日志记录的脚本称为封装。

### 识别 shell 命令提示符 
你可以通过查看终端窗口中的提示符符号来识别 Linux 系统的计算机上的shell 命令提示符的用户是普通用户还是超级用户。`#` 符号用于超级用户，`$` 符号用于具有标准权限的用户。

![Figure 1: Manual of date command][2]

### 基本命令
该脚本附带了很多可以在终端窗口上执行的命令，以管理您的计算机。每个命令的详细信息可以在该命令附带的使用手册中找到。你可以使用如下命令来查看手册：

```
$man <command>
```

一些常用的命令有：

```
$date # 显示当前日期和时间
$cal # 显示当前月份日历
$df # 显示磁盘使用情况
$free # 显示内存使用情况
$ls # 列出文件和目录
$mkdir # 创建目录
```

每个命令都附带了几个可以一起使用的选项。你可以参考使用手册以了解更多的细节。`$man date` 的输出如图 1 所示。


### 重定向操作符
当你希望捕获文件中的命令输出或重定向到文件时，可以使用重定向操作符。


| 命令 | 描述 |
| :- | :- |
| $ls -l /usr/bin >file | 默认标准输出到文件 | 
| $ls -l /usr/bin 2>file | 重定向标准错误到文件 | 
| $ls -l /usr/bin > ls-output 2>&1 | 重定向标准错误和标准输出到文件 | 
| $ls -l /usr/bin &> ls-output | 重定向标准错误和标准输出到文件 | 
| $ls -l /usr/bin 2> /dev/null | 写入 /dev/null 丢弃输出 |

### 大括号扩展
大括号扩展是 UNIX 提供的强大选项之一。它有助于在一行指令中使用最少的命令完成大量操作。例如：

```
$echo Front-{A,B,C}-Back
Front-A-Back, Front-B-Back, Front-C-Back

$echo {Z..A}
Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```
```
$mkdir {2009..2011}-0{1..9} {2009..2011}-{10..12}
```
这条命令会为 2009 到 2011 年里的每个月建立一个目录。

### 环境变量
环境变量是一个动态命名的值，它可以影响计算机上运行的进程的行为方式。此变量是进程运行环境的一部分。

| 命令 | 描述 |
| :- | :- |
| printenv | 打印出所有环境变量的值。 | 
| set | 设置 shell 选项 | 
| export | 导出环境到随后执行的程序 | 
| alias | 为命令创建别名 |

### 网络命令
网络命令对于排查网络问题和检查连接到客户机的特定端口非常有用。

| 命令 | 描述 |
| :- | :- |
| ping | 发送 ICMP（网际网路控制讯息协定）数据包 | 
| traceroute | 打印数据包在网络中的路径 | 
| netstat | 打印网络连接信息、路由表、接口数据 | 
| ftp/lftp | 互联网文件传输程序 | 
| wget | 非交互式网络下载器 | 
| ssh | OpenSSH SSH 客户端 （远程登录程序） | 
| scp | 安全拷贝 | 
| sftp | 安全文件传输程序 |


https://www.runoob.com/linux/linux-comm-netstat.html
### Grep 命令
Grep 命令用于查找系统和日志中的错误。它是 shell 拥有的强大工具之一。
Grep commands are useful to find the errors and debug the logs in the system. It is one of the powerful tools that shell has.

| 命令 | 描述 |
| :- | :- |
| grep -h ‘.zip’ file.list | `.` 表示任意字符 | 
| grep -h ‘^zip’ file.list | 以 `zip` 开头 | 
| grep -h ‘zip$’ file.list | 以 `zip` 结尾 | 
| grep -h ‘^zip$’ file.list | 只含有 `zip` | 
| grep -h ‘[^bz]zip’ file.list | 不含 `b` 和 `z` | 
| grep -h ‘^[A-Za-z0-9]’ file.list | 所有文件名有效的文件 |

### 量词
下面是一些量词的例子：

| 命令 | 描述 |
| :- | :- |
| ? | 匹配出现 0 次或 1 次的元素 | 
| * | 匹配出现 0 次或多次的元素 | 
| + | 匹配出现 1 次或多次的元素 | 
| {} | 匹配出现特定次数的元素 |

### Text processing
文本处理是当今 IT 世界中的另一项重要任务。程序员和管理员可以使用这些命令来切片、剪切和处理文本。

| 命令 | 描述 |
| :- | :- |
| cat -A $FILE | 显示 `$FILE` 文件的所有内容 | 
| sort file1.txt file2.txt file3.txt > final_sorted_list.txt | 一次性将所有文件排序 | 
| ls - l \| sort -nr -k 5 | 按指定的第 5 列进行排序 | 
| sort --key=1,1 --key=2n distor.txt | 对第 1 列进行排序（默认按字母表顺序），对第 2 列进行数值排序 | 
| sort foo.txt \| uniq -c | 查找重复的行并显示该行重复的次数 | 
| cut -f 3 distro.txt | cut column 3 | 
| cut -c 7-10 | cut character 7 - 10 | 
| cut -d ‘:’ -f 1 /etc/password | delimiter : | 
| sort -k 3.7nbr -k 3.1nbr -k 3.4nbr
 distro.txt | 3 rd field 7 the character, 
3rd field 1 character | 
| paste file1.txt file2.txt > newfile.txt | merge two files | 
| join file1.txt file2.txt | join on common two fields |

### Hacks and tips

In Linux, we can go back to our history of commands by either using simple commands or control options.

| Command | Description |
| :- | :- |
| clear | clears the screen | 
| history | stores the history | 
| script filename | capture all command execution in a file |


Tips:

> History  : CTRL + {R, P}
> !!number : command history number
> !!       : last command
> !?string : history containing last string
> !string  : history containing last string

```
export HISTCONTROL=ignoredups
export HISTSIZE=10000
```

As you get familiar with the Linux commands, you will be able to write wrapper scripts. All manual tasks like taking regular backups, cleaning up files, monitoring the system usage, etc, can be automated using scripts. This article will help you to start scripting, before you move to learning advanced concepts.

--------------------------------------------------------------------------------

via: https://www.opensourceforu.com/2022/05/the-basic-concepts-of-shell-scripting/

作者：[Sathyanarayanan Thangavelu][a]
选题：[lkxed][b]
译者：[FYJNEVERFOLLOWS](https://github.com/FYJNEVERFOLLOWS)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]: https://www.opensourceforu.com/author/sathyanarayanan-thangavelu/
[b]: https://github.com/lkxed
[1]: https://www.opensourceforu.com/wp-content/uploads/2022/04/Shell-scripting.jpg
[2]: https://www.opensourceforu.com/wp-content/uploads/2022/04/Figure-1-Manual-of-date-command.jpg
