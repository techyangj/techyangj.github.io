---
title: Linux再学习3
date: 2021-08-16 20:12:16
urlname: Linux-Studay-more-3
tags:
---
## 进程管理：
程序：静态的代码,占用磁盘空间
进程：动态的代码，占用CPU，内存
唯一标识：PID 编号
相关进程：
父进程/子进程      
僵尸进程    
孤儿进程

### 查看进程树
• pstree — Processes Tree
– 格式:pstree [选项] [PID或用户名]

• 常用命令选项
– -a:显示完整的命令行
– -p:列出对应PID编号

systemd:所有进程的父进程,上帝进程

### 查看进程快照
ps [选项]
常用选项：
a:显示当前终端所有进程
x:当前用户在所有终端下的进程
u:以用户格式输出
e:显示系统内所有进程
l:以长格式输出信息
f:包括最完整的进程信息
• ps aux 操作
– 列出正在运行的所有进程,信息非常的详细
用户 进程ID %CPU %内存 虚拟内存 固定内存 终端 状态 起始时间 CPU时间 程序指令

• ps -elf 操作
– 列出正在运行的所有进程,查询进程的父进程  PPID:父进程的ID

### 进程动态排名
top 交互式工具
格式:top [-d 刷新秒数] [-U 用户名]
例如：top -d 1 
     大写P  进行CPU排序
     大写M  进行内存排序 

## 检索进程
• pgrep — Process Grep
– 用途:pgrep [选项]... 查询条件
• 常用命令选项                                
– -l:输出进程名,而不仅仅是 PID
– -U:检索指定用户的进程
– -x:精确匹配完整的进程名
## 进程的前后台调度

• 后台启动
– 在命令行末尾添加“&”符号,不占用当前终端
• Ctrl + z 组合键
– 挂起当前进程(暂停并转入后台)
• jobs 命令
– 查看后台任务列表
• fg 命令
– 将后台任务恢复到前台运行
• bg 命令
– 激活后台被挂起的任务

## 杀死进程
• 干掉进程的不同方法
– Ctrl+c 组合键,中断当前命令程序

– kill [-9] PID... 、kill [-9] %后台任务编号
– killall [-9] 进程名...
– pkill 查找条件  
```shell
#强制踢出一个用户(杀死该用户开启的所有进程)
killall -9 -u lisi
```

```shell
yum -y install psmisc #安装pstree命令
pstree #查看进程树
 id lisi
 pstree  lisi   #查询lisi用户运行的进程

 pstree -p lisi

pstree -ap lisi
bash,1139
  └─vim,1164 abc.txt
pstree -ap      #显示当前运行的所有进程


```


## 日志管理


### 日志的功能
• 系统和程序的“日记本”
– 记录系统、程序运行中发生的各种事件
– 通过查看日志,了解及排除故障
– 信息安全控制的依据

• 由系统服务rsyslog统一记录/管理
    时间、地点、人物、发生何事
   
• 常见的日志文件        
	/var/log/messages 记录内核消息、各种服务的公共消息
	/var/log/dmesg    记录系统启动过程的各种消息
	/var/log/cron     记录与cron计划任务相关的消息
	/var/log/maillog  记录邮件收发相关的消息
	/var/log/secure   记录与访问限制相关的安全消息

## 用户日志
由登录程序负责记录/管理
日志消息采用二进制格式
记录登录用户的时间、来源、执行的命令等信息

日志文件 | 主要用途 
---------|----------
 /var/log/lastlog | 记录最近的用户登录事件 
 /var/log/wtmp | 记录成功的用户登录/注销事件
 /var/log/btmp | 记录失败的用户登录事件
 /var/run/utmp | 记录当前登录的每个用户的相关信息
 
## 日志分析
### 分析工具
• 通用分析工具
– tail、tailf、less、grep等文本浏览/检索命令
– awk、sed等格式化过滤工具
• 专用分析工具
Webmin系统管理套件
Webalizer、AWStats等日志统计套件
• 专用分析工具
users、who、w 命令
– 查看已登录的用户信息,详细度不同
pts:图形命令行界面
[root@svr7 ~]# users
[root@svr7 ~]# who
[root@svr7 ~]# w

### 日志消息的优先级
• Linux内核定义的事件紧急程度
– 分为 0~7 共8种优先级别
– 其数值越小,表示对应事件越紧急/重要

  0  EMERG（紧急）          会导致主机系统不可用的情况
  1  ALERT（警告）          必须马上采取措施解决的问题
  2  CRIT（严重）	        比较严重的情况
  3  ERR（错误）	       运行出现错误
  4  WARNING（提醒）      可能会影响系统功能的事件
  5  NOTICE（注意）        不会影响系统但值得注意
  6  INFO（信息）	       一般信息
  7  DEBUG（调试）          程序或系统调试信息等

### 日志命令

使用journalctl工具
• 提取由 systemd-journal 服务搜集的日志
– 主要包括内核/系统日志、服务日志

• 常见用法
– journalctl | grep 关键词
– **journalctl -u 服务名   [-p 优先级]**
– journalctl -n 消息条数
– journalctl --since="yyyy-mm-dd HH:MM:SS" --
until="yyyy-mm-dd HH:MM:SS"

## systemctl控制

• Linux系统和服务管理器
– 是内核引导之后加载的第一个初始化进程(PID=1)
– 负责掌控整个Linux的运行/服务资源组合

systemd
• 一个更高效的系统&服务管理器
– 开机服务并行启动,各系统服务间的精确依赖

– 配置目录:/etc/systemd/system/
– 服务目录:/lib/systemd/system/
– 主要管理工具:systemctl


对于服务的管理
 systemctl restart  服务名    #重起服务
 systemctl start    服务名    #开启服务
 systemctl stop     服务名    #停止服务
 systemctl status   服务名    #查看服务当前的状态
 
 systemctl enable   服务名      #设置服务开机自启动
 systemctl disable  服务名      #设置服务不开机自启动



RHEL6 运行级别     200           
     0：关机      
     1：单用户模式（基本功能的实现，破解Linux密码） 
	 2：多用户字符界面（不支持网络）     
	 3：多用户字符界面（支持网络）服务器默认的运行级别  
	 4：未定义    
	 5：图形界面    
	 6：重起   


RHEL7 运行模式 

   字符模式：multi-user.target
   图形模式：graphical.target

```shell
#当前直接切换到字符模式
systemctl isolate multi-user.target
#当前直接切换到图形模式
 systemctl isolate graphical.target

#查看每次开机默认进入模式
 systemctl get-default
graphical.target

#设置永久策略，每次开机自动进入multi-user.target
 systemctl set-default multi-user.target
 reboot 
```




