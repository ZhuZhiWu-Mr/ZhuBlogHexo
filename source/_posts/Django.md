---
title: Django
date: 2021-05-05 14:39:18
tags: 
	- Django
	- Python
categories: Django
cover: /images/banners/VCG41N1158547769.jpg
---

## Django程序开机自动启动

:::details 通过shell脚本，设置开机自动启动Django的uwsgi服务（测试环境Cento8）

### 1、切换到/etc/init.d/，目录下创建一个自己的脚本

### 2、制作sh脚本 vim start_uwsgi
**注意，注释也要加进去**

```shell
#!/bin/sh
#add for chkconfig
#chkconfig: 2345 70 30
#description: the description of the shell   #关于脚本的简短描述
#processname: andyStart      #第一个进程名，后边设置自启动的时候会用到
#下面要启动服务的命令
source /418pro/env/djangoVenv/bin/activate && uwsgi --ini /418pro/uwsgi-configs/django.ini   #进入虚拟环境 && 启动uwsgi 配置文件的具体位置
```

**说明：**

（1）2345是指脚本的运行级别，即在2345这4种模式下都可以运行，234都是文本界面，5就是图形界面X

（1）70是指脚本将来的启动顺序号，如果别的程序的启动顺序号比70小（比如44、45），则脚本需要等这些程序都启动以后才启动。

（1）30是指系统关闭时，脚本的停止顺序号。

**linux 下shell脚本执行多个命令的方法**

(1)每个命令之间用;隔开

说明：各命令的执行给果，不会影响其它命令的执行。换句话说，各个命令都会执行，但不保证每个命令都执行成功。

(2)每个命令之间用&&隔开

说明：若前面的命令执行成功，才会去执行后面的命令。这样可以保证所有的命令执行完毕后，执行过程都是成功的。

**(3)每个命令之间用||隔开**

说明：||是或的意思，只有前面的命令执行失败后才去执行下一条命令，直到执行成功一条命令为止。

### 3、给脚本加上可执行权限：

```bash
chmod a+x start_uwsgi
```


### 4、利用chkconfig命令将脚本设置为自启动：

```bash
chkconfig --add start_uwsgi
```
:::

