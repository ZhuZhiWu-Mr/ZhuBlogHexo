---
title: nginx
date: 2021-05-5 12:39:04
tags: 
	- Nginx
categories: Nginx
cover: /images/banners/VCG41183545763.jpg
---

## 设置开机自动启动
:::details nginx开机自动启动(Centos8测试环境)
### 1、init.d下创建文件nginx
```bash
vim /etc/init.d/nginx
```
[nginx官方文档](https://www.nginx.com/resources/wiki/start/topics/examples/redhatnginxinit/)


### 2、将官方配置，添加到编辑的文件中：
```shell
nginx=”/usr/local/nginx/sbin/nginx” //修改成nginx执行程序的路径。
NGINX_CONF_FILE=”/usr/local/nginx/conf/nginx.conf” //修改成nginx.conf文
件的路径。
```
### 3、保存后设置文件的执行权限
```bash
chmod a+x /etc/init.d/nginx
```

### 4、通过下面指令控制启动停止
```bash
/etc/init.d/nginx start
/etc/init.d/nginx stop
```
**查看进程命令**

```bash
ps aux | grep nginx
也可以通过kill掉pid来关闭进程
```
### 5、上面测试通过后、将nginx服务加入chkconfig管理列表：
```bash
chkconfig --add /etc/init.d/nginx
```

### 6、加完5之后，可以使用service对nginx进行启动，重启等操作。
```bash
service nginx start
service nginx stop
service nginx restart
```
### 7、最后设置开机自动启动
```bash
chkconfig nginx on
```
:::