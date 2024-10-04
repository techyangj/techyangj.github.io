---
title: Linux命令学习(4)
date: 2021-07-27 10:36:36
urlname: Linux-Study4
tags:
---
## 用户管理

用户账号：1、可以登录操作系统 2、不同的用户具有不同的权限
唯一标识：UID GID

## 命令学习

- useradd 添加用户
useradd 选项 用户名
常用命令选项：
-u 用户id
-d 家目录路径
-s 登录解释器
-G 附加组    
```shell
useradd -u 3456 alex           设置用户id
useradd -s /sbin/nologin sarah 禁止登陆
useradd -G tarena nsd03        组 新用户

```
用户的基本信息存放在/etc/passwd 文件中
sarah:x/:3457:3457::/home/sarah:/sbin/nologin
用户名：密码占位符：UID：基础组的GID：用户描述信息：用户家目录：解释器
/sbin/nologin ：禁止用户登录操作系统
-  passwd 设置登录密码 
passwd 用户名
echo '密码' | passwd --stdin 用户名
用户密码信息存放在/etc/shadow文件中
lisi:\$6\$S43334h0$UB3TjLPkS4f5g0vpyh1LJKzEDLGBBgN7Ie13w5xftaErFjCXUzfJNsNndbVnAoz2LjD8OgUQbgV88sF/5BZQA0:18835:0:99999:7:::
用户名：密码加密字符串：上一次修改密码时间：等
上一次修改密码时间：自1970-1-1到达上一次修改密码时间经历的天数
- usermod 修改用户属性
usermod 选项 用户名
常用命令选项：
-u 用户id  -d 家目录  -s 登录解释器  -G附加组 #重置组
-  userdel 删除用户 
 常用命令选项： -r 连同家目录一并删除 

-  groupadd 添加组
groupadd [-g 组ID] 组名
组的基本信息存放在/etc/group文件中 
文件中：
root:\x:0:
组名：组密码占位符：组的GID：组的成员列表

- gpasswd 管理组成员
 gpasswd -a 用户名 组名             添加成员
 gpasswd -d 用户名 组名             删除成员

 - groupdel 删除组
  groupdel 组名
grep 组名 /etc/group   可确认结果

## 命令表格

函数 | 作用 
---------|----------
useradd | 添加用户 
id | 查看用户id 
groupadd | 添加组 
passwd | 设置登录密码 
usermod | 修改用户属性 
userdel | 删除用户 
gpasswd | 管理组成员 
groupdel | 删除组