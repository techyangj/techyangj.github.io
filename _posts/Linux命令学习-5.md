---
title: Linux命令学习(5)
date: 2021-07-28 09:45:08
urlname: Linux-Study5
tags:
---

## tar备份与恢复
目的：1、整合分散数据 2、减小空间的占用

常用的压缩格式及其命令工具：
.gz     -->>  gzip压缩格式
.bz2    -->>  bzip2压缩格式
.xz     -->>  xz压缩格式


tar工具的常用选项：
-c 创建归档
-x 释放归档
-f 指定归档文件名称
-z、-j、-J 调用.gz .bz2 .xz格式的工具进行处理
-t 显示归档中的文件清单
-C 指定释放路径

```shell
tar -zcf /opt/file.tar.gz /etc/passwd /home   压缩
tar -Cf /opt/file.tar.gz  -C /home       解压
tar -tf /opt/file.tar.gz | less      查看tar包内容

```


## cron计划任务
用途：按照设置的时间间隔为用户反复执行某一项固定的系统任务
软件包：cronie、crontabs
系统服务：crond
日志文件：/var/log/crond

### crontab 管理计划任务策略
编辑： crontab -e [-u 用户名]
查看： crontab -l [-u 用户名]
清除： crontab -r [-u 用户名]


### 如何编写crontab任务记录
配置格式参考/etc/crontab文件

  \*  *  *  *  *  user-name  command to be executed
   分  时 日 月 周 

执行周期 | 配置说明 
---------|----------
 分钟 | 0-59 
 小时 | 0-23 
 日期 | 1-31 
 月份 | 1-12 
 星期 | 0-7，0和7都代表星期日

\* 匹配范围内任意时间
， 分割多个不连续的时间点
\- 指定连续时间范围
/n 指定时间频率




## 命令表格

函数 | 作用 
---------|----------
tar | 可压缩可解压 
crontab | 管理计划任务策略 











