---
layout: post
title: "手把手叫你搭建代理(1) 使用Paypal购买VPS"
tags: [Paypal],[VPS],[BandwagonHost]
---


#1. 选择vps

从同学那里听来的BandwagonHost，性能非常优异，具体评测戳[这里](https://xuanwobbs.com.cn/archives/2013-03/eval-of-bandwagonhost.html)

优惠套餐
>* 64M内存 / 1.5G SSD硬盘/ G口 100G流量 / 年付3.99美元
[https://bandwagonhost.com/aff.php?aff=816&pid=19](https://bandwagonhost.com/aff.php?aff=816&pid=19)
* 96M内存 / 2.0G SSD硬盘/ G口 200G流量 / 年付4.99美元
[https://bandwagonhost.com/aff.php?aff=816&pid=20](https://bandwagonhost.com/aff.php?aff=816&pid=20)
* 128M内存 / 3G SSD硬盘/ G口 300G流量 / 年付5.99美元
[https://bandwagonhost.com/aff.php?aff=816&pid=21](https://bandwagonhost.com/aff.php?aff=816&pid=21)
* 512M内存 / 5G SSD硬盘/ G口 500G流量 / 年付9.99美元
[https://bandwagonhost.com/aff.php?aff=816&pid=22](https://bandwagonhost.com/aff.php?aff=816&pid=22)

其他套餐
>[https://bandwagonhost.com/aff.php?aff=816&pid=18](https://bandwagonhost.com/aff.php?aff=816&pid=18)

这里我选择了9.99美元的，地点选西海岸的

#2. 购买付款
BandwagonHost只支持paypal付款，这里参考[帖子](http://bbs.dgtle.com/thread-136249-1-1.html)

##注册Paypal
首先，支付需要paypal国际账户，因为涉及到跨境交易；测试的时候paypal中国贝宝的帐号不行，使用IE，前往www.paypal.com进行账户注册，记得一定是要注册国际账户哦。（我发现根据浏览器的不同，到达paypal.com之后的页面也不同,chrome注册的是贝宝，IE的是paypa））
<img src="/public/images/posts/how-to-build-a-proxy/Image1.png" >

然后，来到这个页面
<img src="/public/images/posts/how-to-build-a-proxy/Image2.png" >
选择“中国（全球范围）”，语言就简体中文，
账户类型就第一种（个人）就好咯。
用户姓名填自己真实姓名，这个后面还要绑定银行账户，
邮箱的确保可以收到邮件，后面要验证

完成注册之后记得先去邮箱点击确认链接，这样账户才能激活。如果跳转链接出现问题，可以使用确认码去验证页面手动验证。验证的地方是登陆账户---右边系统通知的地方点验证邮箱---点第二布的[使用验证码验证]----填入邮件中确认按钮下面的[电子邮件地址]下面的确认码，然后就完成验证了。

##绑定银联卡
然后就绑定银联卡，选择添加银联卡，这里需要注意的是paypal只支持62开头的银联卡
<img src="/public/images/posts/how-to-build-a-proxy/Image3.png" >

然后就是一系列信息填写，这些都完成之后，我们的账户就完成了。

##去BandwagonHost完成付款
然后，登录到BandwagonHost
选择付款，在付款的界面选择paypal，选择银联付款，然后就转到我们刚才添加到的网银付款界面，付完就购买成功了
<img src="/public/images/posts/how-to-build-a-proxy/Image4.png" >
<img src="/public/images/posts/how-to-build-a-proxy/Image5.png" >
<img src="/public/images/posts/how-to-build-a-proxy/Image6.png" >
<img src="/public/images/posts/how-to-build-a-proxy/Image7.png" >