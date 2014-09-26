---
layout: post
title: "linux 运维笔记（一）安全配置"
tags: [linux]
---


###禁用root登录
添加用户

	[root@root ~]# adduser admin 
	[root@root ~]# id admin uid=10018(admin) gid=10018(admin) groups=10018(admin)
	[root@root ~]# ls -lad /home/admin/ 
	drwx------ 2 admin admin 4096 Jun 25 16:01 /home/admin/

设置密码

	[root@root ~]# passwd admin
	Changing password for user admin. 
	New UNIX password: 
	Retype new UNIX password: 
	passwd: all authentication tokens updated successfully. 
	[root@root ~]#

测试是否能使用admin登录ssh

	[root@root ~]#ssh admin@my.ip.or.hostname
	admin@my.ip.or.hostname's password: 
	[admin@admin ~]$
测试是否能切换到root

	[admin@admin ~]$ su - 
	Password: 
	[root@root ~]$ whoami root
禁用root使用22端口登录

	root@root ~]# vi /etc/ssh/sshd_config
修改
	
	#PermitRootLogin yes
为
	PermitRootLogin no

修改 
	
	#Port 22
为
	
	Port 9999
重启

	[root@root ~]# /etc/init.d/sshd restart 
	Stopping sshd: [ OK ] 
	Starting sshd: [ OK ] 
	[root@root ~]#
###开启防火墙
#####UFW
1.安装

	sudo apt-get install ufw
2.启用

	sudo ufw enable
	sudo ufw default deny
运行以上两条命令后，开启了防火墙，并在系统启动时自动开启。关闭所有外部对本机的访问，但本机访问外部正常。

3.开启/禁用

	sudo ufw allow|deny [service]
打开或关闭某个端口，例如：

	sudo ufw allow smtp　允许所有的外部IP访问本机的25/tcp (smtp)端口
	sudo ufw allow 22/tcp 允许所有的外部IP访问本机的22/tcp (ssh)端口
这个很重要，ssh远程登录用于SecureCRT等软件建议开启。或者不要开防火墙。

	sudo ufw allow 53 允许外部访问53端口(tcp/udp)
	sudo ufw allow from 192.168.1.100 允许此IP访问所有的本机端口
	sudo ufw allow proto udp 192.168.0.1 port 53 to 192.168.0.2 port 53
	sudo ufw deny smtp 禁止外部访问smtp服务
	sudo ufw delete allow smtp 删除上面建立的某条规则
4.查看防火墙状态

	sudo ufw status
一般用户，只需如下设置：
	
	sudo apt-get install ufw
	sudo ufw enable
	sudo ufw default deny
以上三条命令已经足够安全了，如果你需要开放某些服务，再使用

	sudo ufw allow开启。
开启/关闭防火墙 (默认设置是’disable’)

	sudo ufw enable|disable
转换日志状态

	sudo ufw logging on|off

#####iptables

1、查看本机关于IPTABLES的设置情况

	[root@tp ~]# iptables -L -n

2、清除原有规则.

不管你在安装linux时是否启动了防火墙,如果你想配置属于自己的防火墙,那就清除现在filter的所有规则.

	[root@tp ~]# iptables -F 清除预设表filter中的所有规则链的规则

	[root@tp ~]# iptables -X 清除预设表filter中使用者自定链中的规则

我们在来看一下

	[root@tp ~]# iptables -L -n
提前说一句,这些配置就像用命令配置IP一样,重起就会失去作用),怎么保存.

	[root@tp ~]# /etc/rc.d/init.d/iptables save

这样就可以写到/etc/sysconfig/iptables文件里了.写入后记得把防火墙重起一下,才能起作用.

	[root@tp ~]# service iptables restart
3、设定预设规则

	[root@tp ~]# iptables -p INPUT DROP

	[root@tp ~]# iptables -p OUTPUT ACCEPT

	[root@tp ~]# iptables -p FORWARD DROP
上面的意思是,当超出了IPTABLES里filter表里的两个链规则(INPUT,FORWARD)时,不在这两个规则里的数据包怎么处理呢,那就是DROP(放弃).应该说这样配置是很安全的.我们要控制流入数据包

而对于OUTPUT链,也就是流出的包我们不用做太多限制,而是采取ACCEPT,也就是说,不在着个规则里的包怎么办呢,那就是通过.

可以看出INPUT,FORWARD两个链采用的是允许什么包通过,而OUTPUT链采用的是不允许什么包通过.

这样设置还是挺合理的,当然你也可以三个链都DROP,但这样做我认为是没有必要的,而且要写的规则就会增加.但如果你只想要有限的几个规则是,如只做WEB服务器.还是推荐三个链都是DROP.

注:如果你是远程SSH登陆的话,当你输入第一个命令回车的时候就应该掉了.因为你没有设置任何规则.

怎么办,去本机操作呗!

4、添加规则.

首先添加INPUT链,INPUT链的默认规则是DROP,所以我们就写需要ACCETP(通过)的链

为了能采用远程SSH登陆,我们要开启22端口.

	[root@tp ~]# iptables -A INPUT -p tcp --dport 22 -j ACCEPT

	[root@tp ~]# iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT (注:这个规则,如果你把OUTPUT 设置成DROP的就要写上这一部,好多人都是望了写这一部规则导致,始终无法SSH.在远程一下,是不是好了.

其他的端口也一样,如果开启了web服务器,OUTPUT设置成DROP的话,同样也要添加一条链:

	[root@tp ~]# iptables -A OUTPUT -p tcp --sport 80 -j ACCEPT ,其他同理.)

如果做了WEB服务器,开启80端口.

	[root@tp ~]# iptables -A INPUT -p tcp --dport 80 -j ACCEPT

如果做了邮件服务器,开启25,110端口.

	[root@tp ~]# iptables -A INPUT -p tcp --dport 110 -j ACCEPT

	[root@tp ~]# iptables -A INPUT -p tcp --dport 25 -j ACCEPT

如果做了FTP服务器,开启21端口

	[root@tp ~]# iptables -A INPUT -p tcp --dport 21 -j ACCEPT

	[root@tp ~]# iptables -A INPUT -p tcp --dport 20 -j ACCEPT

如果做了DNS服务器,开启53端口

	[root@tp ~]# iptables -A INPUT -p tcp --dport 53 -j ACCEPT

如果你还做了其他的服务器,需要开启哪个端口,照写就行了.

上面主要写的都是INPUT链,凡是不在上面的规则里的,都DROP

允许icmp包通过,也就是允许ping,

	[root@tp ~]# iptables -A OUTPUT -p icmp -j ACCEPT (OUTPUT设置成DROP的话)

	[root@tp ~]# iptables -A INPUT -p icmp -j ACCEPT (INPUT设置成DROP的话)

允许loopback!(不然会导致DNS无法正常关闭等问题)

	IPTABLES -A INPUT -i lo -p all -j ACCEPT (如果是INPUT DROP)

	IPTABLES -A OUTPUT -o lo -p all -j ACCEPT(如果是OUTPUT DROP)

下面写OUTPUT链,OUTPUT链默认规则是ACCEPT,所以我们就写需要DROP(放弃)的链.

减少不安全的端口连接

	[root@tp ~]# iptables -A OUTPUT -p tcp --sport 31337 -j DROP

	[root@tp ~]# iptables -A OUTPUT -p tcp --dport 31337 -j DROP

有些些特洛伊木马会扫描端口31337到31340(即黑客语言中的 elite 端口)上的服务。既然合法服务都不使用这些非标准端口来通信,阻塞这些端口能够有效地减少你的网络上可能被感染的机器和它们的远程主服务器进行独立通信的机会

还有其他端口也一样,像:31335、27444、27665、20034 NetBus、9704、137-139（smb）,2049(NFS)端口也应被禁止,我在这写的也不全,有兴趣的朋友应该去查一下相关资料.

当然出入更安全的考虑你也可以包OUTPUT链设置成DROP,那你添加的规则就多一些,就像上边添加

允许SSH登陆一样.照着写就行了.

下面写一下更加细致的规则,就是限制到某台机器

如:我们只允许192.168.0.3的机器进行SSH连接

	[root@tp ~]# iptables -A INPUT -s 192.168.0.3 -p tcp --dport 22 -j ACCEPT

如果要允许,或限制一段IP地址可用 192.168.0.0/24 表示192.168.0.1-255端的所有IP.

24表示子网掩码数.但要记得把 /etc/sysconfig/iptables 里的这一行删了.

-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT 因为它表示所有地址都可以登陆.

或采用命令方式:

	[root@tp ~]# iptables -D INPUT -p tcp --dport 22 -j ACCEPT

然后保存,我再说一边,反是采用命令的方式,只在当时生效,如果想要重起后也起作用,那就要保存.写入到/etc/sysconfig/iptables文件里.

	[root@tp ~]# /etc/rc.d/init.d/iptables save

这样写 !192.168.0.3 表示除了192.168.0.3的ip地址

其他的规则连接也一样这么设置.

在下面就是FORWARD链,FORWARD链的默认规则是DROP,所以我们就写需要ACCETP(通过)的链,对正在转发链的监控.

开启转发功能,(在做NAT时,FORWARD默认规则是DROP时,必须做)

	[root@tp ~]# iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT

	[root@tp ~]# iptables -A FORWARD -i eth1 -o eh0 -j ACCEPT

丢弃坏的TCP包

	[root@tp ~]#iptables -A FORWARD -p TCP ! --syn -m state --state NEW -j DROP

处理IP碎片数量,防止攻击,允许每秒100个

	[root@tp ~]#iptables -A FORWARD -f -m limit --limit 100/s --limit-burst 100 -j ACCEPT

设置ICMP包过滤,允许每秒1个包,限制触发条件是10个包.

	[root@tp ~]#iptables -A FORWARD -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT

我在前面只所以允许ICMP包通过,就是因为我在这里有限制。



#参考资料
+ [How do I disable SSH login for the root user?](http://kb.mediatemple.net/questions/713/How+do+I+disable+SSH+login+for+the+root+user%3F#dv)
+ [Ubuntu默认防火墙安装、启用、配置、端口、查看状态相关信息](http://www.jb51.net/os/ubuntu/72109.html)
+ [Linux操作系统下IPTables配置方法详解](http://tech.ccidnet.com/art/9513/20070601/1098119_2.html)