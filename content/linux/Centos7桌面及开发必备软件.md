---
title: "Centos7桌面及开发必备软件"
date: 2018-09-09T15:49:23+08:00
lastmod: 2018-09-09T15:49:23+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

> 必备软件

**1. Ibus输入法软件**

**2. FireFox浏览器**

**3. 微信 (Electronic WeChat)**

**4. 压缩/解压工具**

yum install rar unrar zip unzip p7zip

需node.js环境, 安装npm

**4. VIM安装**

> 娱乐软件

**1. VLC 播放器**

**2. DeadBeef 音乐播放器**

**3. Transmission下载工具**

> 开发相关软件

**1. gcc系列**

```

yum install gcc gcc++ gcc-gfortran compat-gcc-44 compat-gcc-44-c++ compat-libf2c-34

yum install make cmake gdb clang
```

**2. 版本控制**

yum  install git

配置：

git config --global user.name "username"

git config --global user.email "email"

**3. java开发环境 && Eclipse IDE**

**4. Maven,Gradle项目管理工具**

**5. Atom编辑器**

对Markdown语法支持较好，可以跟Sublime Text一样安装插件

**6. XX-Net 软件**

用于翻墙，需安装miredo-client开启ipv6支持（linux mint安装miredo）

**7. VirtualBox虚拟机**

直接yum安装,为防止更新发生错误,需安装dkms

**8. 数据库**

mysql,miriadb

**9. DBeaver**

dbeaver是免费和开源（GPL）为开发人员和数据库管理员通用数据库工具

**10. Postman**

 Postman是一款功能强大的网页调试与发送网页HTTP请求的软件

> 其他工具/命令

**1. unetbootln**

制作linux U盘启动软件

**2. nload**

监控网络带宽

**3. httpry**

获HTTP数据包，并且将HTTP协议层的数据内容以可读形式列举出来

**4. tailf**

跟踪日志文件(代替tail -f),格式：tailf logfile

**5. xchm**

xchm文件阅读器

**6. tmux**

用于在一个终端窗口中运行多个终端会话

**7. vmstat**

查看整体CPU使用情况

**8. tree**

以树状图列出目录的内容

**9. du**

查看当前指定文件或目录(会递归显示子目录)占用磁盘空间大小

**10. nohup**

后台模式执行程序

**11. 远程桌面**

rdesktop,anydesk

**12. inxi**

显示出完整的硬件详情信息

**13. fail2ban**

fail2ban是一款实用软件，可以监视你的系统日志，然后匹配日志的错误信息（正则式匹配）执行相应的屏蔽动作。
