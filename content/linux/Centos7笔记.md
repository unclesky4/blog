---
title: "Centos7笔记"
date: 2018-09-09T16:58:38+08:00
#lastmod: 2018-09-09T16:58:38+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

> centos最小好化安装没有ifconfig命令

yum install net-tools

ifconfig命令在net-tools软件包里

nslookup,dig在bind-utils中

centos使用了systemd来代替sysvinit

> systemd使用方法：

systemd的服务管理程序:systemctl

systemctl是最主要的工具。它融合service和chkconfig的功能于一体。你可以使用它永久性或只在当前会话中启用/禁用服务.

如何启动/关闭、启用/禁用服务？ 

运行一个服务： systemctl start foo.service

关闭一个服务： systemctl stop foo.service 重启一个服务： 

重启一个服务： systemctl restart foo.service 

显示一个服务（无论运行与否）的状态： systemctl status foo.service 

在开机时启用一个服务： systemctl enable foo.service 

在开机时禁用一个服务： systemctl disable foo.service

查看服务是否开机启动：systemctl is-enabled iptables.service;echo $?

| 任务                 | 旧指令                        | 新指令                                                                                                 |
| -------------------- | ----------------------------- | ------------------------------------------------------------------------------------------------------ |
| 使某服务自动启动     | chkconfig --level 3 httpd on  | systemctl enable httpd.service                                                                         |
| 使某服务不自动启动   | chkconfig --level 3 httpd off | systemctl disable httpd.service                                                                        |
| 检查服务状态         | service httpd status          | systemctl status httpd.service （服务详细信息） systemctl is-active httpd.service （仅显示是否 Active) |
| 显示所有已启动的服务 | chkconfig --list              | systemctl list-units --type=service                                                                    |
| 启动某服务           | service httpd start           | systemctl start httpd.service                                                                          |
| 停止某服务           | service httpd stop            | systemctl stop httpd.service                                                                           |
| 重启某服务           | service httpd restart         | systemctl restart httpd.service                                                                        |

> 修改运行级别

````

systemd使用比sysvinit的运行级更为自由的 target 概念作为替代。  
第 3 运行级用 multi-user.target替代。第 5 运行级用graphical.target替代。runlevel3.target 和 runlevel5.target 分别是指向 multi-user.target和graphical.target的符号链接。  
你可以使用下面的命令切换到“运行级 3 ”： 
systemctl isolate multi-user.target (or)
systemctl isolate runlevel3.target  
你也可以使用下面的命令切换到“运行级 5 ”： 
systemctl isolate graphical.target (or) 
systemctl isolate runlevel5.target   
如何改变默认运行级别？ 
systemd使用链接来指向默认的运行级别。在创建新的链接前，你可以通过下面命令删除存在的链接： rm /etc/systemd/system/default.target 
默认切换到运行级 3 ： 
ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target 
默认切换到运行级 5 ： 
ln -sf /lib/systemd/system/graphical.target/etc/systemd/system/default.target 
systemd不使用/etc/inittab文件。 如何查看当下运行级别？ 
runlevel命令在systemd下仍然可以工作。你可以继续使用它，尽管systemd使用 'target' 概念(多个的 'target' 可以同时激活)替换了之前系统的runlevel。
等价的systemd命令是 
systemctl list-units --type=target
````
