#! https://zhuanlan.zhihu.com/p/516943332
# Linux awk 学习笔记
https://sites.ualberta.ca/~stothard/downloads/misc/awk_intro.txt

Awk has two faces: it is a utility for performing simple text-processing tasks, and it is a programming language for performing complex text-processing tasks.
Awk statements comprise a programming language. In fact, Awk is useful for simple, quick-and-dirty computational programming.

Awk actually stands for the names of its authors: "Aho, Weinberger, & Kernighan" and the picture of an auk often shows up on the cover of books on Awk.



List all the gold pieces as follows:

```shell
awk '/gold/' coins.txt
or
awk '/gold/ {print}' coins.txt
```

![image-20220424105154978](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kmf4ubarj211c0b4goc.jpg)

![image-20220424105442594](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kmi1aeypj213g0b40ud.jpg)

This example demonstrates the simplest general form of an Awk program:

```shell
awk '<search pattern> {<program actions>}'
```



正则表达式 (在两个/之间写 Regular Expression)

正则表达式教程：https://www.runoob.com/regexp/regexp-syntax.html



![image-20220423231402065](https://tva1.sinaimg.cn/large/e6c9d24ely1h1k2903m9kj21900fgjto.jpg)

NR: number of record (行数)

NF: number of field (列数) [record, field 是沿用数据库里的术语]

"\t": 制表符 tab (使列对齐)

$i: 第 i 列

$0: 打印整行

![image-20220423232303503](https://tva1.sinaimg.cn/large/e6c9d24ely1h1k2icm98sj20zo0u0n4u.jpg)

awk 中 的 space 只是把左右的字符串拼起来，comma 则代表加上默认分隔符（空格）

![image-20220423232731202](https://tva1.sinaimg.cn/large/e6c9d24ely1h1k2mz0d1jj21620fqjvh.jpg)

awk 会把 American Eagle 当作两列而非一列

![image-20220423233017974](https://tva1.sinaimg.cn/large/e6c9d24ely1h1k2pvks7yj20zs0fqgmr.jpg)

![image-20220423233215035](https://tva1.sinaimg.cn/large/e6c9d24ely1h1k2ryuk7uj210u0i0n20.jpg)

加上 if 条件

![image-20220424124127091](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kpl2a2cjj212g0m8jxr.jpg)

根据 NR, NF 过滤

![image-20220424130636521](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kqb8b8slj212i0fuae1.jpg)

修改列

![image-20220424130824203](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kqd3rr3qj20ul0u0dhn.jpg)

打印最后一列

![image-20220424130949210](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kqekks1vj20xo0fet9y.jpg)

打印倒数第二列

![image-20220424131253773](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kqhrtfp3j20w604iweu.jpg)

第一行要求输入，可以直接回车



# AWK Script

通过以下命令执行.awk脚本：

```shell
awk -f <awk program file name>
```

awk脚本示例：

```c
# This is an awk program that summarizes a coin collection.
#
/gold/    { num_gold++; wt_gold += $2 }      # Get weight of gold.
/silver/  { num_silver++; wt_silver += $2 }  # Get weight of silver.
END { val_gold = 485 * wt_gold;              # Compute value of gold.
      val_silver = 16 * wt_silver;           # Compute value of silver.
      total = val_gold + val_silver;
      print "Summary data for coin collection:";  # Print results.
      printf ("\n");
      printf ("   Gold pieces:                   %2d\n", num_gold);
      printf ("   Weight of gold pieces:         %5.2f\n", wt_gold);
      printf ("   Value of gold pieces:        %7.2f\n",val_gold);
      printf ("\n");
      printf ("   Silver pieces:                 %2d\n", num_silver);
      printf ("   Weight of silver pieces:       %5.2f\n", wt_silver);
      printf ("   Value of silver pieces:      %7.2f\n",val_silver);
      printf ("\n");
      printf ("   Total number of pieces:        %2d\n", NR);
      printf ("   Value of collection:         %7.2f\n", total); }
```

![image-20220424144126921](https://tva1.sinaimg.cn/large/e6c9d24ely1h1kt1x9zuzj20w80eimzi.jpg)



## Part 1 - How to Use Awk and Regular Expressions to Filter Text or String in Files

https://www.tecmint.com/use-linux-awk-command-to-filter-text-string-in-files/

The awk script is in the form ``'/pattern/ action'`` where **pattern** is a regular expression and the **action** is what awk will do when it finds the given pattern in a line.

## 如何在 Linux 中使用 awk 过滤工具

下面的例子打印文件 /etc/hosts 中的所有行，因为没有指定任何的模式。

```shell
awk '// {print}' /etc/hosts
```

下面的例子打印文件 /etc/hosts 中匹配 pattern [a-z] 的所有行

```shell
awk '/[a-z]/ {print}' /etc/hosts
```



## Part 2 - How to Use Awk to Print Fields and Columns in File

https://www.tecmint.com/awk-print-fields-columns-with-space-separator/

Field editing（字段编辑）是 awk 最重要的特性。

awk 中默认的 IFS（内部字段分隔符）是制表符和空格。awk 按照指定的 IFS 将读到的一行输入分割为不同字段，第一组字符（字段一）就是 $1。



## Part 3 - How to Use Awk to Filter Text or Strings Using Pattern Specific Actions

使用一个 `(*)` 符号去标记那些单价大于 $2 的食物：

```shell
awk '/ *\$[2-9]\.[0-9][0-9] */ { print $1, $2, $3, $4, "*" ; } / *\$[0-1]\.[0-9][0-9] */ { print ; }' food_prices.list
```

![image-20220514203151870](https://tva1.sinaimg.cn/large/e6c9d24ely1h287kpp97sj21lo0aaac8.jpg)

``printf`` 命令可以格式化输出

```shell
awk '/ *\$[2-9]\.[0-9][0-9] */ { printf "%-10s %-10s %-10s %-10s\n", $1, $2, $3, $4 "*" ; } / *\$[0-1]\.[0-9][0-9] */ { printf "%-10s %-10s %-10s %-10s\n", $1, $2, $3, $4; }' food_prices.list 
```

![image-20220514203341185](https://tva1.sinaimg.cn/large/e6c9d24ely1h287mktcacj21m20a6wh4.jpg)

`$0` 字段存储是整个行



## Part 4 - How to Use Comparison Operators with Awk in Linux

awk 中的比较运算符

* \>, <, >=, <=, ==, !=

* `some_value ~ / pattern/` – 如果 some_value 匹配模式 pattern，则返回 true

* `some_value !~ / pattern/` – 如果 some_value 不匹配模式 pattern，则返回 true

给食物数量小于或等于 30 的物品所在行的后面加上`(**)`：

![image-20220514204059297](https://tva1.sinaimg.cn/large/e6c9d24ely1h287u65c9rj21kq08y767.jpg)

通过在行的末尾增加 (TRUE) 来标记数量小于等于20的行：

![image-20220514204226682](https://tva1.sinaimg.cn/large/e6c9d24ely1h287vp15zgj21lu09ytao.jpg)



## Part 5 - How to Use Compound Expressions with Awk in Linux

复合表达式：&&, ||

打印出价格超过 $20 且其种类为 “Tech” 的物品，在其行末用 (*) 打上标记。

![image-20220515223310183](https://tva1.sinaimg.cn/large/e6c9d24ely1h29gp8pw08j21u60eoae6.jpg)



## Part 6 - How to Use ‘next’ Command with Awk in Linux

`next` 命令告诉 awk 跳过你所提供的所有剩下的模式和表达式，直接处理下一个输入行。

![image-20220516104828557](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a1yakt9lj21qo0hqtcp.jpg)

上例中，`next` 命令类似 C 语言中的 `break` 命令，避免不必要的条件判断。



## Part 7 - How to Read Awk Input from STDIN in Linux

![image-20220516105449370](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a24vwc1dj20uo0a6gnj.jpg)

我们使用 `dir -l` 命令的输出作为 awk 命令的输入，这样就可以打印出文件拥有者的用户名，所属组组名以及在当前路径下他／她拥有的文件。

![image-20220516110239091](https://tva1.sinaimg.cn/large/e6c9d24ely1h2a2d1ilrwj210204gt9d.jpg)

在 awk 命令里使用一个表达式筛选出字符串来打印出属于 root 用户的文件。

![image-20220516211233364](https://tva1.sinaimg.cn/large/e6c9d24ely1h2ajzo96ljj215i0ek429.jpg)



## Part 8 - Learn How to Use Awk Variables, Numeric Expressions and Assignment Operators

![image-20220516212216662](https://tva1.sinaimg.cn/large/e6c9d24ely1h2ak9r7g4pj21pu0betau.jpg)

![image-20220516212300091](https://tva1.sinaimg.cn/large/e6c9d24ely1h2akahvnopj21hi04q75l.jpg)

如果想要计算出域名 tecmint.com 在文件中出现的次数，我们就可以通过写一个简单的脚本实现这个功能：

```shell
#!/bin/bash
for file in $@; do
	if [ -f $file ] ; then
		#print out filename
		echo "File is: $file"
    #print a number incrementally for every line containing tecmint.com 
    awk  '/^tecmint.com/ { counter=counter+1 ; printf "%s\n", counter ; }'   $file
	else
    #print error info incase input is not a file
    echo "$file is not a file, please specify a file." >&2 && exit 1
	fi
done
#terminate script with exit code 0 in case of successful execution 
exit 0
```

该脚本中的 `counter=counter+1` 亦可替换为 `counter+=1`

![image-20220516215157023](https://tva1.sinaimg.cn/large/e6c9d24ely1h2al4m9svoj20qg0pmq5e.jpg)



## Part 9 - Learn How to Use Awk Special Patterns begin and end

```shell
awk '
BEGIN { actions } 
/pattern/ { actions }
/pattern/ { actions }
……….
END { actions } 
' filenames  
```

`BEGIN` 模式：是指 awk 将在读取任何输入行之前立即执行 `BEGIN` 中指定的动作。

`END` 模式：是指 awk 将在它正式退出前执行 `END` 中指定的动作。

将 Part 8 中脚本修改为：

```shell
#!/bin/bash
for file in $@; do
        if [ -f $file ] ; then
                #print out filename
                echo "File is: $file"
    #print a number incrementally for every line containing tecmint.com 
    awk  ' BEGIN {print "The number of times tecmint.com appears in the file is:" ; }
           /^tecmint.com/ { counter+=1 ; }
           END {printf "%s\n", counter ; }'   $file
        else
    #print error info incase input is not a file
    echo "$file is not a file, please specify a file." >&2 && exit 1
        fi
done
#terminate script with exit code 0 in case of successful execution 
exit 0
```

![image-20220517140912785](https://tva1.sinaimg.cn/large/e6c9d24ely1h2bddgddo7j20wc0l4wh9.jpg)



## Part 10 - Learn How to Use Awk Built-in Variables

awk 内置变量包括：

- `FILENAME` : 当前输入文件名称
- `NR` : 当前输入行编号（是指输入行 1，2，3……等）
- `NF` : 当前输入行的字段编号
- `OFS` : 输出字段分隔符
- `FS` : 输入字段分隔符
- `ORS` : 输出记录分隔符
- `RS` : 输入记录分隔符

![](https://camo.githubusercontent.com/0d295182c8921e86c9d31d40cf3e0597963dc214b96989032155953c79fb3114/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f41776b2d46494c454e414d452d5661726961626c652e706e67)

![](https://camo.githubusercontent.com/6f820d7dfbae984d3b1f2af3c42ab65e889b4bdb58bc13f28580d5dc5061792b/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f41776b2d436f756e742d4e756d6265722d6f662d4c696e65732e706e67)

![](https://camo.githubusercontent.com/c92a39b827f042a240a76ef00d6d5559cc7fe7d030437e1d27cf219787d30f7b/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f4c6973742d46696c652d436f6e74656e74732e706e67)

![](https://camo.githubusercontent.com/36d9c30180c77eb910e650b819adc92f56fdaeefaac2fecd34abc1a98ff530e2/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f41776b2d436f756e742d4e756d6265722d6f662d4669656c64732d696e2d46696c652e706e67)

通过 awk 的 -F 选项 / FS 内置变量指定输入文件分隔符，FS（文件分隔符）默认值为“空格”和“制表符”

将 `:` 指定为新的输入字段分隔符，示例如下：

![](https://camo.githubusercontent.com/761c55f6214ccfdb76429dbe32c72a428bf005c2980222853c32ac0ba15b4631/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f41776b2d46696c7465722d4669656c64732d696e2d50617373776f72642d46696c652e706e67)

![](https://camo.githubusercontent.com/7db36fb2b5c0a874484ba694818b55e41c0d9abda40a0441a0d5bc6714b89c66/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f46696c7465722d4669656c64732d696e2d46696c652d5573696e672d41776b2e706e67)

使用 OFS 内置变量来指定一个用于输出的字段分隔符，它会定义如何使用指定的字符分隔输出字段

![](https://camo.githubusercontent.com/f5bf82e69d8c10610192f3fe6213284df5f0b2aba76d05ddaaa76739ec6f30e2/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30372f4164642d536570617261746f722d746f2d4669656c642d696e2d46696c652e706e67)



## Part 11 - How to Allow Awk to Use Shell Variables

1. 使用 Shell 引用

```shell
#!/bin/bash

### 读取用户名
read -p "请输入用户名：" username

### 在 /etc/passwd 中搜索用户名，然后在屏幕上输出详细信息
cat /etc/passwd | awk "/$username/ "' { print $0 }'
```

![](https://camo.githubusercontent.com/8549cab5c258b60690f47b72ed9f7ffe0f0954e45e4f325217eca29dd80b0c16/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30382f5368656c6c2d5363726970742d746f2d46696e642d557365726e616d652d696e2d5061737377642d46696c652e706e67)

2. 使用 awk 进行变量赋值

   ![](https://camo.githubusercontent.com/58c3c59bb4980da42f074846ac10b042268e354983a9f0c1f78037f1390f5c22/687474703a2f2f7777772e7465636d696e742e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031362f30382f46696e642d557365726e616d652d696e2d50617373776f72642d46696c652d5573696e672d41776b2e706e67)

awk -v 选项用于声明一个变量，`username` 是 shell 变量，`name` 是 awk 变量。



## Part 12 - How to Use Flow Control Statements in Awk

```shell
if (condition1){
actions1
}
else if (conditions2){
actions2
}
else{
actions3
}
```

打印数字 0-10

```shell
awk 'BEGIN{ for(counter=0;counter<=10;counter++){ print counter} }'
```



```shell
#!/bin/bash
awk ' BEGIN{ counter=0 ;
while(counter<=10){
print counter;
counter+=1 ;
}
}
```



## Part 13 - How to Write Scripts Using Awk Programming Language

`#!`，Shebang, 指明使用哪个解释器来执行脚本中的命令

`/usr/bin/awk`，即解释器

`-f`，解释器选项，用来制定读取的程序文件

脚本示例：

```shell
#! /usr/bin/awk -f
# 使用 BEGIN 指定字符来设定 FS 内置变量
BEGIN { FS=":" }
# 搜索用户名 aaronkilik 并输出账号细节
/aaronkilik/ { print "Username :",$1,"User ID :",$3,"User GID :",$4 }
```

执行脚本：

```shell
./script.awk /etc/passwd
```



