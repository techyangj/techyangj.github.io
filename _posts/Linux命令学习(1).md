---
title: Linux命令学习(1)
date: 2021-07-21 17:52:53
urlname: Linux-Study1
tags:
---
### Linux热键
***
Tab 自动补全（可不全命令字，选项，参数，文件路径，软件名，服务名）
ctrl + c 中断程序
ctrl + d 相当于exit
ctrl + l 清屏
ctrl + u 清空至行首
ctrl + w 往回删除一个单词（以空格界定）
shift + pgup[pgdn] 翻页
ctrl + shift + +  放大字体
ctrl + - 缩小字体
esc + . 或者 Alt + . :粘贴上一个命令的参数
### linux命令学习
***
- exit 登出命令
- ls 列出文件夹
  参数 -al >> 列出主文件夹下的所有隐藏文件与相关文件属性
- date 显示时间和日期
  有关参数：
  date +%Y/%m/%d  >> 2021/07/21
  date +%H:%M    >> 18:05
  date +%F
  date +%Y
  date +%M
- locale 区域设置，主要用于设置地区和语系
- cal 显示日历
  有关参数：
  cal 年份  >>全年日历
  cal [month] [year] >> 某年某月的日历
- bc 简单好用的计算器
  有关参数：
  quit 退出计算器
  scale = number(填入数字) 表示小数点位数
***
命令 --help >>找文档，寻求帮助
man 命令  >>寻求帮助          退出按q   /字符串 >> 表示搜寻或高亮
date(1) 中1代表的数字总和
按/键向后查找关键词
man 5 passwd  5:表示帮助的类型为文件帮助信息

代号 | 内容 
---------|----------
1 | 可以操作的指令或可执行文件 
2  | 系统核心可调用的函数和工具等 
3  | 一些常见的函数和函数库 
4 | 设备文件的说明 
5  | 配置文件或者是某些文件的格式 
6 | 游戏 
7 | 惯例与协定 
8  | 系统管理员可用的管理命令 
9 | 跟kernel有关的文件 
***
- 关机指令 shutdown
- 重新开机，关机 reboot halt poweroff 
- 将数据同步写入硬盘中的指令 sync





函数 | 作用 
---------|----------
exit | 登出 
ls | 列出文件夹 
date | 显示时间和日期 
locale | 区域设置
cal | 显示日历 
bc | 简单好用的计算器 
man | 文档查找 
shutdown | 关机
halt | 关机 
poweroff | 关机 
reboot | 重启 
sync | 将数据同步写入硬盘中的指令 







