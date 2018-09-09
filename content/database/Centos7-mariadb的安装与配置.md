---
title: "Centos7 Mariadb的安装与配置"
date: 2018-09-09T15:09:18+08:00
lastmod: 2018-09-09T15:09:18+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---
> 安装MariaDB

安装命令
```
yum -y install mariadb mariadb-server
```

安装完成MariaDB，首先启动MariaDB
```
systemctl start mariadb
```
设置开机启动
```
systemctl enable mariadb
```
> 安全设置

执行命令:

+ mysql_secure_installation

首先是设置密码，会提示先输入密码

+ Enter current password for root (enter for none):<–初次运行直接回车

设置密码

+ Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车
+ New password: <– 设置root用户的密码
+ Re-enter new password: <– 再输入一次你设置的密码

其他配置

+ Remove anonymous users? [Y/n] <– 是否删除匿名用户，回车

+ Disallow root login remotely? [Y/n] <–是否禁止root远程登录,回车,

+ Remove test database and access to it? [Y/n] <– 是否删除test数据库，回车

+ Reload privilege tables now? [Y/n] <– 是否重新加载权限表，回车

初始化MariaDB完成，接下来测试登录

mysql -uroot -ppassword

完成。


> 配置MariaDB的字符集


vim /etc/my.cnf.d/client.cnf

添加：

[client]

default-character-set=utf8

<br/>

vim /etc/my.cnf.d/server.cnf

添加：

[server]

character_set_server=utf8

[mysqld]

character_set_server=utf8
