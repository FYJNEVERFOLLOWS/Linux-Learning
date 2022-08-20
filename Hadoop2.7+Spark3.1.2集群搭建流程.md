#! https://zhuanlan.zhihu.com/p/481306645
# Hadoop2.7+Spark3.1.2集群搭建流程

使用Digital Ocean的云服务器，先创建一个节点作为master节点（在创建时即指定主机名称为master，选择Ubuntu 20.04(LTS) x64版本，1核2G）。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ac96zm5ej21h80u0gpp.jpg) 

通过第三方SSH客户端TermiusSSH登陆刚刚新建的云主机，先在master节点上完成实验环境的配置（主要包括Java, Hadoop, Spark等）。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ac9jjjllj215w0u0784.jpg) 

Java配置：

首先在自己的电脑上下载jdk-8u202-linux-x64.tar.gz，并通过命令行scp命令将该tar包上传至master云主机：
```
scp /Users/fuyanjie/Downloads/jdk-8u202-linux-x64.tar.gz root@128.199.217.19:/usr/local/
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acb8bv4ej23y80bsn0s.jpg) 

回到SSH客户端，可以看到master节点的/usr/local文件夹下已经有了刚刚上传的文件：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acbmhjf1j22620u0dmh.jpg) 

使用```tar -zxvf jdk-8u202-linux-x64.tar.gz```命令解压压缩包jdk-8u202-linux-x64.tar.gz


使用```vim ~/.bashrc```命令通过vim编辑器打开root用户的环境变量配置文件，在这个文件的开头位置，添加如下几行内容：

```
export JAVA_HOME=/usr/local/jdk1.8.0_202

export JRE_HOME=${JAVA_HOME}/jre

export CLASSPATH=.:\${JAVA_HOME}/lib:\${JRE_HOME}/lib

export PATH=\${JAVA_HOME}/bin:\$PATH
```

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acbudnazj217o0u0778.jpg) 

保存.bashrc文件并退出vim编辑器。然后，继续执行如下命令让.bashrc文件的配置立即生效：
```
source ~/.bashrc
```
通过下面这个命令检查安装是否成功：
```
java -version
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0accvfiglj23y80ha78w.jpg) 

 

Hadoop安装：

首先在自己的电脑上下载hadoop-2.7.7.tar.gz，并通过命令行scp命令将该tar包上传至master云主机：
```
scp /Users/fuyanjie/Downloads/hadoop-2.7.7.tar.gz root@128.199.217.19:/usr/local/
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acczzvf6j23y80f042v.jpg) 

回到SSH客户端，可以看到master节点的/usr/local文件夹下已经有了刚刚上传的文件：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acd4dv8ij21wp0u0grt.jpg) 

使用```tar -zxvf hadoop-2.7.7.tar.gz```命令解压压缩包hadoop-2.7.7.tar.gz

 

使用```vim ~/.bashrc```命令通过vim编辑器打开root用户的环境变量配置文件，在这个文件的开头位置，添加如下几行内容：
```
export HADOOP_HOME=/usr/local/hadoop-2.7.7

export PATH=\$PATH:\${HADOOP_HOME}/bin:\${HADOOP_HOME}/sbin:
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acdpk2stj213d0u0zn6.jpg) 

保存.bashrc文件并退出vim编辑器。然后，继续执行如下命令让.bashrc文件的配置立即生效：
```
source ~/.bashrc
```
通过下面这个命令检查安装是否成功：
```
hadoop version
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acdz8wnvj23y80m4dm2.jpg) 

hadoop 分布式集群配置：

输入命令```cd /usr/local/hadoop-2.7.7/etc/hadoop```

修改hadoop-env.sh

输入命令```vim hadoop-env.sh```

将原来export JAVA_HOME那一项修改为以下值（指明本机JAVA_HOME的路径），然后wq保存退出：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ace5lvypj23y80io41k.jpg) 

 

修改yarn-env.sh

输入命令```vim yarn-env.sh```

同样修改 JAVA_HOME ，然后wq保存退出

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acedsc0xj22e00u0432.jpg) 

cd 到 /usr/local 文件夹下
```
cd /usr/local
```
输入命令``` mkdir tmp-hadoop ```新建文件夹用作hadoop运行时的临时文件存放目录
```
cd /usr/local/hadoop-2.7.7/etc/hadoop
```
修改core-site.xml

输入命令```vim core-site.xml```

将以下内容复制粘贴到文件中，然后保存退出
```
<configuration>
​    <property>
​        <name>fs.default.name</name>
​        <value>hdfs://master:9000</value>
​        <description>配置NameNode的URL</description>
​    </property>
​    <!--用来指定hadoop运行时产生的存放目录 -->
​    <property>
​        <name>hadoop.tmp.dir</name>
​        <value>/usr/local/tmp-hadoop</value>
​    </property>
</configuration>
```
这里将hadoop运行时的临时文件存放目录设置为/usr/local/tmp-hadoop

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0aceqdzkjj214u0u0djf.jpg) 

修改 hdfs-site.xml

输入命令，修改 hdfs-site.xml
```
vim hdfs-site.xml
```
将以下内容添加到文件，然后保存退出：
```
<configuration>
​    <property>
​        <name>dfs.replication</name>
​        <value>2</value>
​    </property>
</configuration>
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acfedrt3j21870u0jus.jpg) 

修改 mapred-site.xml

输入命令，修改 mapred-site.xml
```
vim mapred-site.xml
```
将以下内容复制粘贴到文件中，保存退出
```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<!--

 Licensed under the Apache License, Version 2.0 (the "License");

 you may not use this file except in compliance with the License.

 You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software

 distributed under the License is distributed on an "AS IS" BASIS,

 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

 See the License for the specific language governing permissions and

 limitations under the License. See accompanying LICENSE file.

-->

<!-- Put site-specific property overrides in this file. -->

<configuration>
​    <!-- 指定mr框架为yarn方式 -->
​    <property>
​        <name>mapreduce.framework.name</name>
​        <value>yarn</value>
​    </property>
</configuration>
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acfpnbfdj214k0u0tbw.jpg) 

修改 yarn-site.xml

输入命令，修改 yarn-site.xml
```
vim yarn-site.xml
```
将以下内容复制粘贴到文件中：
```
<configuration>
<!-- Site specific YARN configuration properties -->
<property>
  <!--指定yarn的老大resoucemanager的地址-->
  <name>yarn.resourcemanager.hostname</name>
  <value>master</value>
</property>
<!--NodeManager 获取数据的方式-->
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
</configuration>
```
 
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acfxzmp9j211t0u041p.jpg) 


修改 slaves

输入命令，修改 slaves
```
vim slaves
```
将以下内容复制粘贴到文件中：
```
master
s1
s2
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acgdti3uj20u00v474d.jpg) 

至此，master节点配置基本上完成，之后需要将master关机并创建一个快照，并且分别使用快照创建 s1和s2 这两个 slave 节点。 

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acgixp7sj21h80u0n1d.jpg) 

创建成功显示如下：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acgnjja0j21h80u0tcv.jpg) 

保存snapshot，从snapshot在与master相同的SGP1站点新建slave节点

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acgrt4a3j21h80u0q7f.jpg) 

 

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acgwz4bcj21h80u0goy.jpg) 

新建s1, s2两个 slave 节点完成

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ach1rfmoj21h80u0adt.jpg) 

在 master, s1, s2 三台机器上依次执行sudo vim /etc/hosts命令增加如下三条IP和主机名映射关系：
```
128.199.217.19 master
159.223.49.13 s1
159.223.39.0 s2
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ach6yk30j21m20u0adk.jpg) 

修改完成以后，使用 reboot 命令重新启动所有节点的Linux系统。

这样就完成了master节点和所有slave节点的配置，然后，需要在各个节点上都执行如下命令，测试是否相互ping得通
```
ping master
ping s1
ping s2
```
如果ping通的话，会显示如下图所示的结果。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0achd44e5j20y20u045u.jpg) 

 

制作免密码登录

在 master, s1, s2 上依次执行一下命令：
```
ssh-keygen -t rsa
```
之后一直按回车直至命令运行结束

在master节点依次输入以下命令
```
ssh-copy-id master
ssh-copy-id s1
ssh-copy-id s2
```
然后根据提示输入master，s1，s2的密码。这里是为了实现master通过ssh登陆s1，s2和自己免密。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0achis4hvj21b70u0tep.jpg) 

在s1中依次输入以下命令：
```
ssh-copy-id master
ssh-copy-id s2
```

在s2中依次输入以下命令：
```
ssh-copy-id master
ssh-copy-id s1
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0achnd1xgj21p90u0n39.jpg) 


（亦可以通过ssh-keygen -t rsa 生成~/.ssh/id_rsa.pub 然后粘贴到各自的authorized_keys）

通过ssh命令测试互相登陆，如果相互登陆都不需要密码的话，表示免密码登陆设置成功。

测试命令：

ssh 主机名

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0achrzepjj216p0u0wii.jpg) 

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0achwp3twj21920u042q.jpg) 

 

 

启动Hadoop集群

格式化Hadoop文件系统
```
hdfs namenode -format
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0aci1o0fjj21x70u0wof.jpg) 

当看到如上图中所示 successfully 提示信息则表示格式化成功。

一行命令启动Hadoop集群
```
start-all.sh
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0aci6n6ouj22hb0u0gva.jpg) 


也可以使用start-dfs.sh进行启动，需要配置etc/hadoop里的workers文件：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0aciail7uj20u00v474d.jpg) 

 

用浏览器访问 http://128.199.217.19:50070 (hadoop 3.x的端口号改成了9870) 查看启动是否成功。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acifeeouj21h80u0go0.jpg) 

 

登陆yarn的WebUI http://128.199.217.19:8088

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acij1oe6j21h80u041y.jpg) 

 

安装spark

在本机登陆 Apache Spark 官网下载spark-3.1.2-bin-hadoop2.7.tgz

将下载的压缩包上传到master主机

在mac终端中输入：
```
scp /Users/fuyanjie/Downloads/spark-3.1.2-bin-hadoop2.7.tgz [root@128.199.217.19:/usr/local](mailto:root@128.199.217.19:/usr/local)
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acinq03pj23y804ygn1.jpg) 

回到SSH客户端，可以看到master节点的/usr/local文件夹下已经有了刚刚上传的文件：

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acir54t3j21jf0u0tem.jpg) 

使用```tar -zxvf spark-3.1.2-bin-hadoop2.7.tgz```命令解压压缩包 spark-3.1.2-bin-hadoop2.7.tgz

将spark-3.1.2-bin-hadoop2.7文件夹名字修改成spark

执行命令```mv spark-3.1.2-bin-hadoop2.7 spark```

修改环境变量，添加spark

使用```vim ~/.bashrc```命令通过vim编辑器打开root用户的环境变量配置文件，在这个文件的开头位置，添加如下几行内容：
```
export SPARK_HOME=/usr/local/spark

export PATH=\$PATH:\${SPARK_HOME}/bin:\${SPARK_HOME}/sbin
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0aciu8dsxj20zj0u0why.jpg) 

输入命令使修改的配置生效：
```
source ~/.bashrc
```
 

编辑spark-env.sh

先进入conf文件夹，输入命令```cd /usr/local/spark/conf```

复制一份spark-env.sh，并将其改名 ```cp spark-env.sh.template spark-env.sh```

编辑spark-env.sh ```vim spark-env.sh```

将以下内容添加到文件中：
```
export JAVA_HOME=/usr/local/jdk1.8.0_202
export SPARK_MASTER_IP=128.199.217.19
export SPARK_WORKER_MEMORY=1g
export HADOOP_CONF_DIR=/usr/local/hadoop-2.7.7/etc/hadoop
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acj6okoej212s0u00xp.jpg) 

添加s1，s2节点信息，输入命令```vim /usr/local/spark/conf/slaves```

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acjkztelj20u00xlq30.jpg) 

将spark文件夹复制到s1，s2节点

 

依次运行下面的命令：
```
cd /usr/local
scp -r spark root@s1:/usr/local
scp -r spark root@s2:/usr/local
```
 

进入spark文件夹，启动spark

依次运行以下命令：
```
cd /usr/local/spark
./sbin/start-all.sh
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acjo6pkyj23y80k2ahg.jpg) 

用浏览器访问 http://128.199.217.19:8080 查看启动是否成功。

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acjrj5vxj22330u0adl.jpg) 

运行以下命令
```
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://master:7077 /usr/local/spark/examples/jars/spark-examples_2.12-3.1.2.jar 100
```

然后打开spark的WebUI查看运行情况

![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0acjviaotj212k0u0gta.jpg) 

 

MySQL安装

在安装MySQL时发现由于节点内存较小无法成功安装，通过以下命令增加虚拟内存（即swap文件系统）来解决：
```
dd if=/dev/zero of=/swapfile bs=1M count=4096
mkswap /swapfile
swapon /swapfile
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ackimut1j22se0u0jzh.jpg) 

使用以下命令即可进行mysql安装，注意安装前先更新一下软件源以获得最新版本：
```
sudo apt-get update  #更新软件源

sudo apt-get install mysql  #安装mysql
```
 

启动和关闭mysql服务器：
```
service mysql start

service mysql stop
```
确认是否启动成功，mysql节点处于LISTEN状态表示启动成功：
```
sudo netstat -tap | grep mysql
```
![img](https://tva1.sinaimg.cn/large/e6c9d24ely1h0ackpg5d6j21sv0u00yq.jpg) 

 

JDBC配置

在https://dev.mysql.com/downloads/connector/j/找到相应版本的mysql-connector-java.deb的下载链接

 

依次执行以下命令：
```
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java_8.0.27-1ubuntu20.04_all.deb

dpkg -i mysql-connector-java_8.0.27-1ubuntu20.04_all.deb
```
 

 

 

==============