---
layout: post
title: ubuntu基本服务配置
categories: [学习]
tags: [linux]
---


#使用su命令
先设置root命令
	sudo passwd root 
#修改用户名
切换到root打开如下两个配置文件

	sudo gedit /etc/passwd 
把我想改的"xxx"这个用户名改为"way"了,保存并退出 

	sudo gedit /etc/shadow 
同样把"xxx"改为"way" ,保存并退出 

重启后名字就变成了way@xxx-ubuntu:
#修改主机名

修改有两步：
①. 修改/etc/hosts：

	sudo gedit /etc/hosts
增加

	127.0.0.1       localhost
	192.168.76.21   way-ubuntu
	# The following lines are desirable for IPv6 capable hosts
	::1     localhost ip6-localhost ip6-loopback
	fe00::0 ip6-localnet
	ff00::0 ip6-mcastprefix
	ff02::1 ip6-allnodes
	ff02::2 ip6-allrouters
	ff02::3 ip6-allhosts
~                   
②. 修改/etc/hostname内容为
way-ubuntu
保存，退出，重启，就可以了。

#关闭防火墙

http://wiki.ubuntu.org.cn/UFW%E9%98%B2%E7%81%AB%E5%A2%99%E7%AE%80%E5%8D%95%E8%AE%BE%E7%BD%AE

	sudo ufw disable
	sudo ufw enable

#配置静态IP
http://www.linuxidc.com/Linux/2012-07/65266.htm

##配置静态ip地址

	$sudo vi /etc/network/interfaces
原有内容只有如下两行：

	auto lo
	iface lo inet loopback
向末尾追加以下内容：

	auto eth0
	iface eth0 inet static
	address 192.168.0.33
	gateway 192.168.0.1
	netmask 255.255.255.0
	network 192.168.0.0
	broadcast 192.168.0.255
然后保存退出；
##手动配置 dns
	$sudo vi /etc/resolv.conf
想末尾追加如下内容：

	nameserver 192.168.0.10
然后保存退出。
##重启 network 使修改生效
	$sudo /etc/init.d/networking restart
这里有点有问题，就是重启ubuntu后，发现又不能上网了，问题出在 /etc/resolv.conf。重启后，此文件配置的dns又被自动修改为默认值。所以需要永久性修改DNS。方法如下：

	cd /etc/resolvconf/resolv.conf.d/
	cat base
	nameserver 8.8.8.8
（或者关闭networkmanager）
##停用NetworkManager
Fedora

	service NetworkManager stop
	chkconfig NetworkManager off
	chkconfig network on
	service network start


Ubuntu

	sysv-rc-conf --level 2 \NetworkManager off
	sysv-rc-conf --level 2 \networking on
	
	/etc/init.d/NetworkManager stop
	/etc/init.d/networking start
##开启/关闭网卡
	
	ifconfig eth0 up
	ifconfig eth0 down
#增加交大源
http://wiki.ubuntu.org.cn/%E6%BA%90%E5%88%97%E8%A1%A8

备份

	sudo cp /etc/apt/sources.list /etc/apt/sources.list_backup
编辑源

	vim /etc/apt/sources.list

加入

	deb http://ftp6.sjtu.edu.cn/ubuntu/ raring main multiverse restricted universe
	deb http://ftp6.sjtu.edu.cn/ubuntu/ raring-backports main multiverse restricted universe
	deb http://ftp6.sjtu.edu.cn/ubuntu/ raring-proposed main multiverse restricted universe
	deb http://ftp6.sjtu.edu.cn/ubuntu/ raring-security main multiverse restricted universe
	deb http://ftp6.sjtu.edu.cn/ubuntu/ raring-updates main multiverse restricted universe
	deb-src http://ftp6.sjtu.edu.cn/ubuntu/ raring main multiverse restricted universe
	deb-src http://ftp6.sjtu.edu.cn/ubuntu/ raring-backports main multiverse restricted universe
	deb-src http://ftp6.sjtu.edu.cn/ubuntu/ raring-proposed main multiverse restricted universe
	deb-src http://ftp6.sjtu.edu.cn/ubuntu/ raring-security main multiverse restricted universe
	deb-src http://ftp6.sjtu.edu.cn/ubuntu/ raring-updates main multiverse restricted universe

更新源

	sudo apt-get update

#安装Http

	sudo apt-get install apache2
开放80端口 
默认网站路径在/var/www/
配置文件在/etc/apache2/apache2.conf 以及defaults

	service  apache2 start
	sudo /etc/init.d/apache2 restart

http://wiki.ubuntu.org.cn/Apache
http://www.blogjava.net/duanzhimin528/archive/2010/03/05/314564.html

#安装VLC

	sudo apt-get install vlc
	#(解码MP3缺个包)http://ubuntuforums.org/showthread.php?t=1999604&page=2&s=b2eacda393e1b179d3d2921d4f883d50
	sudo apt-get install  libavcodec-extra-53 

#安装ftp
	sudo apt-get  install vsftpd
默认禁止root用户登录，本地登录
禁止写权限（不能上传），安装后默认开启，修改配置文件

	vim /etc/vsftpd.conf
vsftpd的配置文件/etc/vsftpd.conf，两个关键选项。

	local_enable=YES
	write_enable=YES

/etc/vsftpd/ftpusers
记录不允许连接的用户列表，其中root也包含在里面。于是我立马注释root这一行，再重新连接，就可以了

	sudo service vsftpd restart
	sudo /etc/init.d/vsftpd start

http://wiki.ubuntu.org.cn/Vsftpd

#安装SSH

SSH分客户端openssh-client和openssh-server
如果你只是想登陆别的机器的SSH只需要安装openssh-client（ubuntu有默认安装，如果没有则安装）

	sudo apt-get install openssh-client
如果要使本机开放SSH服务就需要安装openssh-server

	sudo apt-get install openssh-server
然后确认sshserver是否启动了：

	ps -e |grep ssh
如果看到sshd那说明ssh-server已经启动了。
如果没有则可以这样启动：

	sudo /etc/init.d/ssh start
ssh-server配置文件位于/ etc/ssh/sshd_config，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义成其他端口号，如222。
然后重启SSH服务：

	sudo /etc/init.d/ssh stop
	sudo /etc/init.d/ssh start
然后使用以下方式登陆SSH：

	ssh username@192.168.1.112 
username为192.168.1.112 机器上的用户，需要输入密码。
断开连接：

	exitsh
