---
title: Linux再学习1
date: 2021-08-12 21:21:48
urlname: Linux-Studay-more-1
tags:
collection: posts
---
## ISCSI
Internet SCSI 网络SCSI接口
1、一种基于C/S架构的虚拟磁盘技术
2、服务器提供磁盘空间，客户机连接并当成本地磁盘使用

ISCSI磁盘构成
backstore 后端存储
对应到服务端提供实际存储空间的设备，需要起一个管理名称
target  磁盘组
客户端的访问单元，作为一个框架，由多个lun组成
lun 逻辑单元
每个lun需要关联到某一个后端存储设备，在客户端会视为一块虚拟硬盘

ISCSI 名称规范
iqn.yyyy-mm.倒序域名：自定义标识
用来识别target磁盘组，也用来识别客户机身份

targetcli配置
/backstore/block  create 后端存储名 实际设备路径
/iscsi  create 磁盘组的IQN名称
/iscsi/磁盘组名/tpg1/luns create 后端存储路径
/iscsi/磁盘组名/tpg1/acls create 客户机IQN标识
/iscsi/磁盘组名/tpg1/portals create IP地址 端口号
其他辅助配置指令：
ls、saveconfig、exit


服务端：
1、防火墙修改成为默认模式trusted
2、规划分区，划分共享的分区
3、安装targetcli软件包
4、运行targetcli命令
   1）建立后端存储
   2）建立target磁盘组
   3）进行lun关联
   4）设置客户端声称的名字
   5）设置提供服务的IP地址及端口

客户端：
1、防火墙修改成为默认模式trusted
2、安装iscsi-initiator-utils软件包
3、修改配置文件/etc/iscsi/initiatorname.iscsi
4、重启iscsid刷新客户端标识的名字
5、执行iscsiadm命令发现共享存储
6、重启iscsi进行加载共享存储


## 数据库
常见的数据库系统
微软的SQL Server
IBM的DB2
甲骨文的Oracle、MySQL
开源社区版：MariaDB

### 部署数据库
```shell
yum -y install mariadb-server #安装mariadb-server软件包
systemctl restart mariadb   #重启mariadb服务
systemctl enable mariadb 

## 数据库基本操作
mysql   # 进入数据库
>show databases;  #显示所有库
>create database nsd; #建立nsd库
>drop database nsd; #删除nsd库
>create database nsd1905;
>use mysql; #进入mysql库
>show tables; #显示当前库中所有表格
>use nsd1905 #进入nsd1905库
>exit #退出数据库
```

### 数据库表格

增insert
删delete
改updata
查select
```shell
# mariadb 查询格式： select 表字段 from 库名.表名;
# 查询表结构： desc 表名 
select * from base;
select name,password from base;
select * from base where password='123'; #有条件的查询
desc base;
```

### 数据库授权
除了root用户，某些数据库只能被特定用户查询：
例如:nsd1905数据库，只能被lisi查询，lisi密码是123
GRANT 权限列表 on 数据库名.表名 to  用户名@  客户机地址 IDENTIFIED BY '密码'
```shell
grant select on nsd1905.* to lisi@localhost identified by '123'
```

### mariadb服务端配置
为数据库账号修改密码
mysqladmin [-u用户名] [-p[旧密码]] password '新密码'
```shell
mysqladmin -u root password '123456'
mysql -u root -p123456

mysqladmin -u root password '123456'
mysql -u root -p123456

```

## NFS共享
Network File System 网络文件系统
用途：为客户机提供共享使用的文件夹
协议：NFS(TCP/UDP 2049)、PRC(TCP/UDP 111)
所需软件包：nfs-utils
系统服务：nfs-server


```shell
#1、安装软件包nsf-utils
rpm -q nsf-utils #查询是否安装NFS服务
#2、创建共享目录及完成共享配置
mkdir /public
echo hahah > /public/1.txt
ls /public/
vim /etc/exports  #修改文件
## 内容为： /public    *(ro)
##         文件夹路径  客户机地址（权限） 客户机地址（权限）..

#3、重启nfs-server
systemctl restart nfs-server
systemctl enable nfs-server


#在desktop上
#1、查看服务端共享
showmount -e IP地址
#2、手动挂载
mkdir /mnt/nsd
ls /mnt/nsd
mount IP地址：/pulbic /mnt/nsd
ls /mnt/nsd
df -h
#3、开机自动挂载
#_netdev:声明网络设备  系统在网络服务完成后，在挂载本设备
vim /etc/fstab
#   写入   IP地址:/public /mnt/nsd nfs defaults,_netdev 0 0
umount /mnt/nsd/
df -h
mount -a 
df -h
```

## Web通信
基于B/S架构的网页服务
服务器提供网页
浏览器下载并显示网页
Hyper Text Markup Language 超文本标记语言
Hyper Text Transfer Protocol 超文本传输协议
http:超文本传输协议   默认端口80

软件包：httpd
系统服务：httpd
提供的默认配置
listen： 监听地址：端口（80）
ServerName:本站点注册的DNS名称
DocumentRoot：网页根目录（/var/www/html）
DirectoryIndex:起始页/首页文件名（index.html）

```shell
#1、安装
rpm -p httpd #查询是否安装httpd软件
yum -y install httpd #安装
#2、书写页面
echo '<h1>NSD1905' > /var/www/html/index.html
cat /var/www/html/index.html
#3、重启httpd服务
systemctl restart httpd
systemctl enable httpd
#4、验证
firefox IP地址 

#######################################
#设置网页文件根目录（/var/www/html）
mkdir /var/www/myweb
echo '<h1>wo shi myweb' > /var/www/myweb/index.html
vim /etc/httpd/conf/httpd.conf
# DocumentRoot "/var/www/myweb"
systemctl restart httpd
firefox IP地址
```
网络路径和实际路径
客户端：firefox IP地址/abc
服务端：firefox /var/www/myweb/abc

主配置文件：/etc/httpd/conf/httpd.conf
调用配置文件:/etc/httpd/conf.d/*.conf

## 配置虚拟站点
虚拟web主机:由同一台服务器提供多个不同的web站点
区分方式：
基于域名的虚拟主机
基于端口的虚拟主机
基于IP地址的虚拟主机

```
虚拟站点添加配置
<VirtualHost IP地址：端口>
   ServerName 此站点的DNS名称
   DocumentRoot 此站点的网页根目录
</VirtualHost>
客户机地址限制
使用<Directory>配置区段
每个文件夹自动继承其父目录的ACL访问权限
除非针对字目录由明确设置
<Directory 目录的绝对路径>
  Require all denied|granted
  Require ip IP或网段地址
</Directory>
```


## SElinux 三大策略
1、布尔值
2、安全上下文：为所有重点的数据打上标签
3、非默认端口开放





## parted分区
parted分区工具,适用于GPT分区模式
GPT分区模式：最多可以划分128个主分区，最大空间支持18EB

```shell
parted /dev/vbd
(parted) mktable gpt  # 指定分区表类型，指定分区模式
(parted) print        #输出分区表信息
(parted) mkpart       #划分新的分区
分区名称? []? haha    #分区名称随便写
文件系统类型？ [ext2]? xfs  #文件系统随便写，不起实际作用
起始点? 0
结束点? 2G
忽略/Ignore/放弃/Cancel? Ignore #选择忽略
(parted) unit GB      #使用GB作为显示单位
起始点? 2G
结束点? 4G
(parted) print
(parted) quit

```
## 基础邮件服务

电子邮件服务器的基础功能：
为用户提供电子邮箱存储空间
处理用户发出的邮件——传递给收件服务器
处理用户收到的邮件——投递到邮箱

```shell
#postfix软件包
yum -y install postfix
#修改配置文件
vim /etc/postfix/main.cf
#99  myorigin= 域名   #默认补全的域名后缀
#116 ine_interface =all   #本机所有网卡都可以提供邮件服务
#164 mydestination = 域名  #判定为本域邮件的依据
#vim末行模式下 ：set nu #开启行号

## 重启服务
systemctl restart postfix

## 测试
# mail 发信操作：mail -s '邮件标题' -r 发件人 收件人
mail -s 'test1' -r yg xln
# mail 收件操作：mail [-u 用户名]
mail -u xln


```




