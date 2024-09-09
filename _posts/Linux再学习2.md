---
title: Linux再学习2
date: 2021-08-14 09:26:14
urlname:  Linux-Studay-more-2
tags:
---
## zip压缩包
### 制作zip压缩包
归档+压缩操作：zip [-r] 备份文件.zip 被归档的文档

```shell
yum -y install zip
zip -r /opt/test.zip /home /etc/passwd
ls /opt/
zip -r /opt/root.zip /root
ls /opt
```
### 释放zip压缩包
释放归档+解压操作
unzip 备份文件.zip [-d 目标文件夹]

```shell
yum -y install unzip 
unzip /opt/root.zip /etc
```


## 源码编译
RPM包：rpm -ivh yum install
源码包————make gcc————可执行的程序————运行安装
主要优点：
1、获取软件的最新版，及时修复bug
2、软件功能可按需选择/定制，有更多的软件可供选择
3、源码适用于各种平台

基本实现过程：
1、下载源码包，tar解包，释放源代码到指定目录
2、./configure配置，指定安装目录/功能模块等选项
3、make编译，生成可执行的二进制程序文件
4、make install安装，将编译好的文件复制到安装目录



```shell
#1、安装所需服务
yum -y install make
yum -y install gcc
#2、tar解包，释放源码到指定目录
tar -xf /tools/inotify-tools-3.13.tar.gz -C /opt/
ls /opt/
ls /opt/inotify-tools-3.13
#3、./configure配置，指定安装目录/功能模块等选项
#作用1：检测本机师傅安装gcc 作用2：指定安装目录/功能模块等选项
cd /opt/inotify-tools-3.13/
./configure --prefix=/mnt/myrpm
#4、make编译，生成可执行的二进制程序文件
cd /opt/inotify-tools-3.13/
make
#5、make install安装，将编译好的文件复制到安装目录
cd /opt/inotify-tools-3.13/
make install
#6、查看
ls /mnt/
ls /mnt/myrpm/
ls /mnt/myrpm/bin/
```

## 虚拟化概述
virtualization 资源管理
x个物理资源 ———— y个逻辑资源
实现程度：完全、部分、硬件辅助（CPU）

虚拟化主要厂商及产品
VMware : vSphere  VMware Workstation
RedHat : KVM、RHEV

主要软件包：
qemu-kvm :为KVM提供底层仿真支持
libvirt-daemon:libvirtd 守护进程，管理虚拟机
libvirt-client:用户端软件，提供客户端管理命令
libvirt-daemon-driver-qemu:libvirtd连接qemu驱动
virt-manager：图形管理工具

virsh命令：
提供管理各虚拟机的命令接口
支持交互模式，查看/创建/停止/关闭。。。
格式： virsh 控制指令 [虚拟机名称] [参数]

查看虚拟化信息：
查看KVM节点信息 virsh nodeinfo
列出虚拟机     virsh list [--all]
列出虚拟网络   virsh net-list [--all]
查看指定虚拟机的信息 virsh dominfo 虚拟机名称

开关机操作：
运行|重启|关闭指定的虚拟机  virsh start|reboot|shutdown 虚拟机名称
强制关闭指定的虚拟机        virsh destroy 虚拟机名称
将指定的虚拟机设为开机自动运行  virsh autostart [--disable] 虚拟机名称


一台KVM虚拟机的组成： 
xml配置文件：定义虚拟机名称、UUID、CPU、内存、虚拟磁盘、网卡等各种参数设置
xml配置文件默认路径：/etc/libvirt/qemu
磁盘镜像文件：保存虚拟机的操作系统及文档数据，镜像路径取决于xml配置文件中的定义
磁盘镜像文件：/var/lib/libvirt/images/

常见的镜像盘类型：RAW  QCOW2

编辑虚拟机设置：
对虚拟机的配置进行调整
编辑：virsh edit 虚拟机名
若修改name、uuid、disk、mac，可自动保存为新的虚拟机配置


COW技术原理：
Copy On Write 写时复制
直接映射原始盘的数据内容
当原始盘的旧数据有修改时，在修改之前自动将旧数据存入前端盘
对前端盘的修改不回写到原始盘

快速创建qcow前端盘
qemu-img create -f qcow2 -b 后端盘（原始盘） 前端盘 前端盘大小G

离线访问虚拟机：
虚拟机无需开机的前提下，直接修改虚拟机磁盘镜像文件
利用guestmount工具：
支持离线挂载raw、qcow2格式虚拟机磁盘
可以在虚拟机关机的情况下，直接修改磁盘中的文档
方便对虚拟机定制、修复、脚本维护
基本用法：
guestmount -a 虚拟机磁盘路径 -i /挂载点
```
guestmount -o nonempty -a 虚拟机磁盘路径 -i /挂载点

```


```shell
#安装服务
yum -y install  qemu-kvm libvirt-daemon libvirt-client libvirt-daemon-driver-qemu virt-manager

#三合一操作命令 virsh edit 虚拟机名称
#1、创建磁盘镜像文件
cd /var/lib/libvirt/images/
cp .node_base.qcow2 /tmp/nsd03.qcow2
#2、生成虚拟机xml文件
virsh edit centos7.0
virsh list --all
virsh start nsd03

```



