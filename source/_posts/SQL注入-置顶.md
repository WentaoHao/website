---
title: SQL注入
date: 2021-04-20 19:00:01
tags: SQL
---

通过修改创建时间实现置顶,文章完结后修改,短期内作为记录,实时更新

记录:

* 来自CSDN  [通过sqli-labs学习sql注入——基础挑战之less1-10](https://blog.csdn.net/u012763794/article/details/51207833)

* 来自知乎  [SQL注入：Error Based Injection](https://zhuanlan.zhihu.com/p/74907340)

## 配置环境:mysql+Apache

### Apache的注意事项

<https://blog.csdn.net/maq56310/article/details/80580286>

如果443端口被占用,可以用

    netstat -ano //查看端口号对应的pid
    taskkill /f /pid 1234 //杀死1234进程,也可在任务管理器中杀死.

conf/httpd.conf的监听端口默认为80,改为8088解决

Sqli-labs文件夹应该放在C:\Apache24\htdocs下,而不是网上所有的其他博客说的web或者www

启动只能用 <http://localhost:8088/Sqli-labs/> ,不能用IP地址,不然打不开