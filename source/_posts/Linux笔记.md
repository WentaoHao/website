---
title: Linux笔记
date: 2020-04-20 19:00:00
tags: linux
---

[点击这里查看鸟哥的Linux私房菜](http://cn.linux.vbird.org/)


**点击上面的链接跳转查看时,建议使用鼠标中键以免难以返回**

---

ls -al   a：所有；l：详细信息

man： 显示用法

### 权限

* chgrp: change group; 
* chown:change owner;
* chmod:改变权限 三个数字分别为 owner group others r:4 w:2 x:1

-R ：递归，修改子目录所有的文件

* x对于文件来说是可执行，对目录来说是是否可切换到这个目录（cd命令）r可以读取列表（ls）
* 目录属于用户，拥有744权限，目录下的文件对于用户来说权限都是7

建立空档案： touch 也可以用于修改文件的时间

## Linux档案类型

* 正规：-  纯文本，二进制，数据格式文件（data）  
* 目录：d  
* 联结 l  link，类似于快捷方式  
* 区块设备 block，b，硬盘软盘都是 
* 字符设备文件 键盘鼠标 character，c
* 资料接口 socket ；
* 数据输送文件 FIFO，pipe， p ；脚本的扩展名是sh

## 路径：

* 绝对路径: 开头是/
* 相对路径:开头不是/
* ./表示本目录
* ../表示上层目录
* -表示上个工作目录
* ~home目录
* 指令 cd(Change Directory)
* mkdir  -P忽视错误 -m777指定权限
* pwd Print Working Directory
* rmdir删除空目录

ls:

* **-a 全部文件**
* t 时间排序
* S 文件大小排序
* r 倒序
* R 递归
* **-l 详细信息**

cat  Concatenate （连续查看一个文件）

* -n,行号;-b,空行不占行号
* tac倒着输出行
* -v,显示看不见的字符

chattr (配置文件隐藏属性)

* 选项与参数：
*      +   ：添加某一个特殊参数，其他原本存在参数则不动.
*      -   ：移除某一个特殊参数，其他原本存在参数则不动。
*      =   ：配置一定，且仅有后面接的参数
* i 不能被删除,增删,改名
* a 只能添加数据

lsattr (显示文件隐藏属性)

file 观察文件类型

---

## 搜索文件

whereis  定位某个**命令** 的二进制文件、源码和帮助页文件。

locate 定位的是**文件**.用的是数据库,默认每天更新一次,可以手动更新,命令是**updatedb** (我自己测试需要sudo)

find 定位文件,搜索指定目录

* +4代表大於等於5天前的档名：ex> find /var -mtime +4
* -4代表小於等於4天内的文件档名：ex> find /var -mtime -4
* 4则是代表4-5那一天的文件档名：ex> find /var -mtime 4

---

## vim

vim的操作特别多，我一般用IDE，只记录几个

* nG 去第n行
* gg 去第一行
* / ? word 向下或向上查找word字符串
* n N 向下或向上接着查找刚才的字符串
* :n1,n2s/word1/word2/g n1 与 n2 为数字。在第 n1 与 n2 行之间寻找 word1 这个字符串，并将该字符串取代为 word2！举例来说，在 100 到 200 行之间搜寻 vbird 并取代为 VBIRD 则：
『:100,200s/vbird/VBIRD/g』。(常用)
* 上面的g改为gc,每个替换之前会要求用户确认
* yy 复制一行
* p 粘贴
* u undo,撤销
* Ctrl+r 重做

---

* :w [filename]	将编辑的数据储存成另一个档案（类似另存新档）
* :r [filename]在编辑的数据中，读入另一个档案的数据。亦即将 『filename』 这个档案内容加到游标所在行后面

---
* ctrl+z 暂存并离开

---

## bash

---
