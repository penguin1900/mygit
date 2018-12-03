1.rm -rf * 和 rm -rf /命令使用前要三思

2.安装rpm包一定要留意其支持的操作系统（el6或el7），电脑位数（32或64），还要查看当前流行的版本及其评价，选择适用的版本

3.http是明文传输，不安全，https是采用SSL或者TLS加密协议非明文进行传输的，现在的大网站都是https的了，https的配置就是在安装nginx时配合编译OpenSSL模块，然后在nginx的配置文件中开启ssl

4.磁盘最多只能有4个分区，主分区加上拓展分区不能超过4个

磁盘：sda

主分区或者拓展分区：sda1,sda2,sda3,sda4（编号1-4）

逻辑分区：sda5（编号从5开始）

5.常用raid

Raid5：至少需要三块硬盘，校验码轮流存放在三块盘中，允许损坏一块盘，磁盘利用率为N-1

Raid10：raid0和raid1的组合，至少需要四块硬盘，每组两块硬盘互为镜像盘，每组允许损坏一块盘，磁盘利用率为1/2

6.常用查看系统状态的命令

ps -ef | grep 进程名  查看进程运行情况

lsof -i:端口号  查看端口占用情况

netstat -ntlp查看tcp协议网络

top查看cpu，内存和进程

vmstat查看cpu，I/O，内存使用情况

7.find 目录 -name “关键字”

8.stat 文件或者目录：查看文件或者目录的访问时间atime（access time） 创建时间ctime （creat time）修改时间mtime（modify time） 

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image002.gif)

 

9.怎么关闭selinux

用命令查看selinux运行状态：sestatus

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image004.jpg)

Enabled表示是在运行的

关闭selinux方法：

vim /etc/selinux/config

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

改成disabled，保存退出，然后reboot重启系统即可

再次查看selinux运行状态

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

状态变为disabled，说明selinux已经关闭

\10. 如何修改CentOS7系统运行级别

先查看当前系统运行级别 systemctl get-default 

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg)

目前为图形化界面

修改为多用户标准模式

systemctl set-default muti-user.target

然后再查看下当前系统运行级别

\11. 如何开放CentOS7防火墙端口

firewall-cmd --zone=public --add-port=8080/tcp --permanent

firewall-cmd –reload

 

\12. CentOS7添加脚本开机自启动

方法一

1、赋予脚本可执行权限（/opt/script/autostart.sh是你的脚本路径)

```
Chmod +x /opt/script/autostart.sh
```

2、在centos7中，/etc/rc.d/rc.local的权限被降低了，所以需要执行如下命令赋予其可执行权限

```
Chmod +x /etc/rc.d/rc.local
 
 
```

3、打开/etc/rc.d/rc/local文件，在末尾增加如下内容

```
/opt/script/autostart.sh
 
方法二
```

1、将脚本移动到/etc/rc.d/init.d目录下

```
mv /opt/script/autostart.sh /etc/rc.d/init.d
```

2、增加脚本的可执行权限

```
chmod+x /etc/rc.d/init.d/autostart.sh
```

3、添加脚本到开机自动启动项目中

cd /etc/rc.d/init.d

chkconfig --add autostart.sh

chkconfig autostart.sh on

.        

13.安全复制scp

将远端主机上的内容拷贝到本机

scp  –r  远端主机用户名@IP:要复制的目录  本机目录

将本机内容复制到远端主机

scp  –r 本机目录 远端主机用户名@IP:目标目录

 

14.Ubuntu14.04.4配置（不能直接用root用户登陆，要先登陆普通用户。再sudo su切换到root用户）

网络配置

Ifconfig eth0 ip后重启网卡/etc/init.d/networking restart

或者

vim /etc/network/interface

添加以下内容

auto eth0

iface eth0 inet static

address x.x.x.x

netmask x.x.x.x

gateway x.x.x.x

然后重启网卡/etc/init.d/networking restart

 

配置主机名

vim /etc/hostname

然后reboot重启即可

 

15.分区和格式化

fdisk –l看下当前磁盘情况

分区：fdisk /dev/sda，分出一个/sda1

格式化：mkfs –t ext4 /dev/sda1

在/mnt下新建一个目录

mkdir /mnt/data

挂载分区

mount /dev/sda1 /mnt/data

 

16.plsql软件的使用

在客户机上安装instantclient：只需复制instantclient解压文件即可，无需安装

修改D:\instantclient_11_2\instantclient_11_2\NETWORK\ADMIN\tnsnames.ora文件并保存！

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

只需修改HOST、PORT和SERVER_NAME即可

打开客户机上已安装好的plsql软件，配置以下两个路径

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image014.jpg)

 

然后重新打开plsql

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image016.jpg)

输入账号密码，选择数据库及类型，确定即可连接到数据库！

 

17.数据库基本操作

创建用户create user xxx identified by xxx;

授予权限grant connect, resource,dba to xxx;（DBA权限）

建库：create database xxx;

建表空间：create tablespace xxx datafile '创建表的路径' size 100M reuse autoextend on next 40M maxsize unlimited default storage

 

18.jdk环境变量配置

vim /etc/profile

添加以下内容

export JAVA_HOME=/usr/local/jdk1.8.0_60

export PATH=$MAVEN_HOME/bin:$PATH

然后source /etc/profile生效

 

19.查看某个目录下所有文件和目录大小并排序

du -h / --max-depth=1 | sort -rn，先调整max-depth=1，查看一级目录大小情况，比如说/home目录较大，然后再du -h /home --max-depth=1 | sort -rn进一步查看/home目录下哪些文件较大，以此类推，最终找出较大的文件并清理

 

20. 处理Linux系统下已删除文件继续占用空间问题

在Linux中，当我们使用rm在linux上删除了大文件，但是如果有进程打开了这个大文件，却没有关闭这个文件的句柄，那么linux内核还是不会释放这个文件的磁盘空间，最后造成磁盘空间占用100%，整个系统无法正常运行。这种情况下，通过df和du命令查找的磁盘空间，两者是无法匹配的，可能df显示磁盘100%，而du查找目录的磁盘容量占用却很小。

这时候可以使用lsof -n / | grep deleted | awk '{print $2}' | xargs kill命令（慎用，会把占用文件的进程kill掉，一般是在磁盘空间已经百分百的情况下的快速清理才会用到）把这些占用的进程全部kill掉，这样就可以把空间释放出来，之后还要把kill掉的程序起起来

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

![img](file:///C:/Users/ADMINI~1.USE/AppData/Local/Temp/msohtmlclip1/01/clip_image017.gif)

 

 

 

 

 

 