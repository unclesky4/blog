---
title: "Centos7_Setting"
date: 2018-09-09T15:32:10+08:00
lastmod: 2018-09-09T15:32:10+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

> 1.安装 - 自定义分区

/ ：根目录，合理使用并及时清理的话15G就够了，不过建议30G以上；

swap ：与物理内存大小一致即可；

/boot : 300Mb足够;

/home ：余下的全部空间

**注：除以上分区外，可以再自定义分区,如/opt,/data**

> 2.删除多余的kernel

```

yum update 执行之后，可能会将kernel也一起更新，则在启动CentOS时启动项中会有很多项。
确认当前使用的kernel版本号:
$ uname -r
3.10.0-123.9.3.el7.x86_64

查找当前系统安装的所有kernel:
$ rpm -qa | grep kernel | sort
kernel-3.10.0-123.8.1.el7.x86_64
kernel-3.10.0-123.9.2.el7.x86_64
kernel-3.10.0-123.9.3.el7.x86_64
kernel-devel-3.10.0-123.8.1.el7.x86_64
kernel-devel-3.10.0-123.9.2.el7.x86_64
kernel-devel-3.10.0-123.9.3.el7.x86_64
kernel-headers-3.10.0-123.9.3.el7.x86_64
kernel-tools-3.10.0-123.9.3.el7.x86_64
kernel-tools-libs-3.10.0-123.9.3.el7.x86_64

可以看出有三个版本的kernel，123.8.1、123.9.2和123.9.3。除了最新的kernel外，建议多保留一个旧kernel，以免新kernel出现问题时可以通过旧kernel进入系统。因而此处删除123.8.1版本的kernel:
sudo yum remove kernel-3.10.0-123.8.1.el7.x86_64
sudo yum remove kernel-devel-3.10.0-123.8.1.el7.x86_64
```

> 3.NTFS驱动

CentOS下默认无法挂载NTFS格式的硬盘。需安装nfts-3g即可实现即插即用:

sudo yum install ntfs-3g

> 4.安装第三方源

+ epel源

sudo yum install epel-release

+ Nux Dextop源

安装参考：https://li.nux.ro/repos.html
