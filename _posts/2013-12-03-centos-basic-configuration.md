---
layout: post
title: centos基本服务配置
categories: [学习]
tags: [linux]
---


#服务器安装教程

http://www.jb51.net/os/85895.html

#centos关闭防火墙

新安装完CentOS Linux，发现配置完apache后没法访问，估计是防火墙问题。

	/etc/init.d/iptables status

会得到一系列信息，说明防火墙开着。

	/etc/init.d/iptables stop

永久关闭:

	chkconfig --level 35 iptables off

另外，bz2的格式的解压也记录一下

	bunzip2 filename.bz2

这样得到一个 filename的文件，原先文件消失了。 
http://www.linuxidc.com/Linux/2008-04/12461.htm

#centos配置静态IP

通常，如果我们想更改主机地址为静态地址或者更改主机名，需要修改的几个文件包括：

	/etc/sysconfig/network #Centos设置主机名和网络配置
	/etc/sysconfig/network-scripts/ifcfg-eth0#针对特定的网卡进行设置
	/etc/resolv.conf#设置DNS
	/etc/hosts#设置指定的域名解析地址

一般我们只需要修改网卡的Centos配置文件就可以了，例如我的Centos配置文件如下：

	DEVICE=eth0
	BOOTPROTO=static
	TYPE=Ethernet
	NAME="System etho0"
	BROADCAST=192.168.56.255
	HWADDR=08:00:27:24:F8:9B
	IPADDR=192.168.56.101
	IPV6INIT=yes
	IPV6_AUTOCONF=yes
	NETMASK=255.255.255.0
	NETWORK=192.168.56.1
	ONBOOT=yes

Centos设置IP完成后，重启一下网卡就可以了：

	service network restart
http://os.51cto.com/art/201001/177909.htm

#更新源，加入交大源
新安装CentOS 6.2 * 1 添加上海交大源*1
yum 的.repo文件如下:

	[CentOS-ftp.sjtu.edu.cn]
	name=CentOS 6
	baseurl=http://ftp.sjtu.edu.cn/centos/6/os/$basearch/
	enabled=1
	gpgcheck=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/6/os/$basearch/RPM-GPG-KEY-CentOS-6
	 
	[updates-ftp.sjtu.edu.cn]
	name=CentOS updates
	#baseurl=http://ftp.sjtu.edu.cn/centos/6/updates/$basearch/
	baseurl=http://ftp.sjtu.edu.cn/centos/6/updates/$basearch/
	enabled=1
	gpgcheck=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/RPM-GPG-KEY-CentOS-6
	 
	[extras-ftp.sjtu.edu.cn]
	name=CentOS-6-Extras
	baseurl=http://ftp.sjtu.edu.cn/centos/6/extras/$basearch/
	gpgcheck=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/RPM-GPG-KEY-CentOS-6
	 
	[centosplus-ftp.sjtu.edu.cn]
	name=CentOS-6-Plus
	baseurl=http://ftp.sjtu.edu.cn/centos/6/centosplus/$basearch/
	gpgcheck=0
	enabled=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/RPM-GPG-KEY-CentOS-6
	 
	[contrib-ftp.sjtu.edu.cn]
	name=CentOS-6-Contrib
	baseurl=http://ftp.sjtu.edu.cn/centos/6/contrib/$basearch/
	gpgcheck=0
	enabled=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/RPM-GPG-KEY-CentOS-6
	 
	[fasttrack-ftp.sjtu.edu.cn]
	name=CentOS-6-fasttrack
	baseurl=http://ftp.sjtu.edu.cn/centos/6/fasttrack/$basearch/
	gpgcheck=0
	enabled=0
	gpgkey=http://ftp.sjtu.edu.cn/centos/RPM-GPG-KEY-CentOS-6
用法:
保存为 sjtu.repo (或者 xxxx.repo)
terminal进入repo文件所在文件夹


	sudo cp *.repo /etc/yum.repos.d/
有了上交的东西,可以删掉它自带的源了

	su
	cd /etc/yum.repos.d/
	ls -al
按自己要求删吧

完了之后记得重新生成cache

yum makecache
有时候需要清空重来

	yum clean all
	yum makecache
这东西挺简单的,毕竟工具嘛
http://argcv.com/articles/586.c

#安装FTP

yum install vsftpd
暂时还没调通，无法登录，后台显示server已经运行（root用户和本地用户登录，21号端口）

安装Http

	yum install httpd

#安装SSH
ssh 默认安装可以登录


sftp可以登录，利用ssh，使用22号端口

#安装vlc
centos官方没有源，需要寻找第三方
这里推荐linuxtech的(但是好像还是有点问题)

	su - root
	cd /etc/yum.repos.d/
	wget http://pkgrepo.linuxtech.net/el6/release/linuxtech.repo
	yum install vlc

http://scientificlinuxforum.org/index.php?showtopic=1425


#安装VNC

