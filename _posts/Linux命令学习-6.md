---
title: Linux命令学习(6)
date: 2021-07-29 17:44:34
urlname: Linux-Study6
tags:
---

## 虚拟机之间的通信
1、确定虚拟机的IP地址：
可使用ifconfig查看地址
2、看是否能通信：
可使用ping IP地址 进行
3、操作对方：
ssh 用户@对方的IP地址
ctrl + shift + t：开启新的终端
ssh -X 用户@对方的IP地址
-X（大写）:可以在远程管理时，开启对方图形程序

## 永久别名的设置
设置文件进行别名设置

```shell
对于root用户来说:
vim ~/.bashrc
对于普通用户lisi来说：
vim /home/lisi/.bashrc
打开后：
alias gos = 'ssh -X root@IP地址'
```
注意设置完成后必须新开一个终端，才能生效。


## 权限
### 访问方式
读取：允许查看内容 read
写入：允许修改内容 write
可执行：允许运行和切换 execute
分别对应 
r ：能够ls浏览目录内容
w ：能够执行rm/mv/cp/mkdir/touch/...等更改目录
x : 能够cd切换到此目录

对于文本文件来说：
r: cat head less tail
w: vim > >>
x：Shell脚本


### 归属
所有者：拥有文件/目录的用户 user
所属组：拥有文件/目录的组  group
其他用户：除所有者、所属组以外的用户 other
![1](/Linux命令学习-6/1.png)


Linux判断用户具备的权限
1、判断用户角色
2、查看相应角色权限位

匹配及停止
顺序：所有者>所属组>其他人

以-开头：文本文件
以d开头：目录
以l开头：快捷方式

特殊权限：/tmp/ 最后权限为t

### 设置基本权限
命令：chmod
chmod [-R] 归属关系+-=权限类别 文档

### 设置文档归谁
chmod [-R] 属主 文档
chmod [-R] :属组 文档
chown [-R] 属主：属组 文档

### Set GIF
附加在属组的X位上
属组的权限标识会变成s
适用于目录，Set GID可以使目录下新增的文档自动设置与父目录相同的属组
让子文档自动继承父目录的所属组身份

### Set UID
附加在属主的x位上
属主的权限标识会变成s
适用于可执行文件，Set UID可以让使用者具有文件属主的身份及部分权限

### Sticky Bit
附加在其他人的x位上
其他人的权限标识会变成t
适用于开放w权限的目录，可以阻止用户滥用w写入权限（禁止操作别人的文档）

### 权限的数值表示
权限的数值化：
基础权限：r=4 w=2 x=1
附加权限：SUID=4 SGID=2 Sticky Bit=1




### acl策略
文档归属的局限性：
1、任何人只属于三种角色：属主、属组、其他人
2、无法实现更精细的控制
acl访问策略：
1、能够对个别用户、个别组设置独立的权限
2、大多数挂载的EXT3/4、XFS文件系统默认已支持

getfacl setfacl命令
getfacl 文档
setfacl [-R] -m u：用户名：权限类别 文档
setfacl [-R] -m g：组名：权限类别 文档
setfacl [-R] -b 文档

实现bc用户，可以查看/etc/shadow文件内容：
1、利用其他人进行设置
chmod o+r /etc/shadow
2、利用所属组进行设置
chown :bc /etc/shadow
chmod g+r /etc/shadow
3、利用所有者进行设置
chown bc /etc/shadow
chmod u+r /etc/shadow
4、利用acl进行设置
setfacl -m u:bc:r /etc/shadow


### 零散软件管理
1、虚拟机server具备软件包
2、虚拟机classroom构建web服务，让光盘的内容以网页形式提供
web服务：提供网页内容

http协议：超文本传输协议
https协议：安全超文本传输协议

### 使用rpm命令管理软件
RPM Package Manager RPM包管理器
rpm -q 软件名
rpm -ivh 软件名-版本信息.rpm
rpm -e 软件名


### 升级Linux内核
Linux内核文件：
默认位置：/boot/vmlinuz-*
支持多个内核文件，开机时选择其中一个版本进系统
GRUB2多系统启动配置：
引导信息：/boot/grub2/grub.cfg



### yum软件包仓库
yum软件包仓库：自动解决依赖关系



```shell
yum repolist  #列出仓库信息
yum -y install httpd #安装软件包
rpm -q httpd 
rpm -ql httpd # 查看软件包的安装清单
```

## 历史命令
管理/调用曾经执行过的命令
history :查看历史命令列表
history -c:清空历史命令
！n:执行命令历史中的第n条命令
！str:执行最近一次以str开头的历史命令
例如： 
！cat : 执行最近一条以cat开头的历史命令

## 使用小命令工具
### du命令
du 统计文件的占用空间
du [选项] [文件或目录]
-s:只统计每个参数所占用的总空间大小
-h:提供易读容量单位（K M G）


## 快捷方式
ln -s /路径/源数据   /路径/快捷方式名字 （软连接）
ln  /路径/源数据   /路径/快捷方式名字 （硬连接）

软连接：若原始文件或目录被删除，连接文件将失效，软连接可存放在不同分区/文件系统
硬链接：若原始文件被删除，连接文件仍可用，硬链接与原始文件必须在同一分区/文件系统








