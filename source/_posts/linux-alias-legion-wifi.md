---
title: Linux别名alias的用法以及联想拯救者没有WiFi的解决方法
date: 2020-04-23 9:52:00
tags: linux
---

    rfkill list all

发现 ideapad_wlan: Wireless LAN中的hard block 为 yes

解决方法：

    sudo modprobe -r ideapad_laptop

由于这个问题出现了很多次，所以给这个命令设置一个别名

    alias wifi='sudo modprobe -r ideapad_laptop'

alias用法：
 
    alias //查看所有已经存在的别名
    alias 命令='完整命令'  //设置一个别名 
    unalias 命令 //移除一个别名

以后直接输入wifi 即可修复这个问题