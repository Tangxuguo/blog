---
layout: post
title: "基于Django的微信开发框架"
tags: [Django]
---

随着微信流行，越来越多商家和自媒体等开始在微信营销，这也催生了一个新型产业，微信开发。

如何快速开发一个公众号，这里介绍的是基于Django框架

在github搜索django weixin，你会发现二个特别火

 * [django\_weixin\_portal](https://github.com/wwj718/django_weixin_portal)
 * [weixin2py](https://github.com/winkidney/weixin2py)
 
###[django\_weixin\_portal](https://github.com/wwj718/django_weixin_portal)
django\_weixin\_portal功能非常强大，具体你可以去官网上看，他采用了亿米CH开源的第三方微信开发者账号管理平台，在上面做了一些应用，按照它的安装说明是能跑通的，我的是在digitalocean上跑的centos7x64。github有很教程
这里需要注意的是：

 * 它要求django版本是1.6，我在1.5和1.7版本上跑是有问题的
 * 这个开源框架要求你必须有app_id,也就是说你必须是服务号，或者早期的订阅号
 * 这个框架有两个后台，一个网站的，一个亿米的后台

贴几张图

网站后台
<img src="/blog/public/images/posts/weixin/weixin1.png" >

添加app_id
<img src="/blog/public/images/posts/weixin/weixin2.png" >

亿米后台
<img src="/blog/public/images/posts/weixin/weixin3.png" >

管理添加文章
<img src="/blog/public/images/posts/weixin/weixin4.png" >
由于我没有服务号，作罢


###[weixin2py](https://github.com/winkidney/weixin2py)
weixin2py是另一个非常强大的框架，最近也进行了一次更新，他的插件机制可以很方便进行二次开发，这是我打算使用的原因，另外框架写好了你在开发过程中所需要的库和函数，非常方便，如果你不需要太多功能的话，匹配回复事件处理直接可以拿来用，github有很详细的教程，就不罗嗦了

网站后台
<img src="/blog/public/images/posts/weixin/weixin5.png" >

管理添加消息
<img src="/blog/public/images/posts/weixin/weixin6.png" >

后台事件处理
<img src="/blog/public/images/posts/weixin/weixin7.png" >

后台文本处理，支持正则匹配，插件
<img src="/blog/public/images/posts/weixin/weixin8.png" >

说完这两个再介绍个有意思的

###littlesnail

基于有道翻译和django开发的微信应用

 * [littlesnail](https://github.com/liushuaikobe/littlesnail)

<img src="/blog/public/images/posts/weixin/demo.png" >

[作者博客详细介绍开发过程](http://blog.vars.me/blog/2013/07/24/wei-xin-ji-qi-ren-xiao-gua-niu-you-dao-fan-yi-xiao-zhu-shou-django-plus-sae-plus-wei-xin-gong-zhong-zhang-hao-zi-dong-hui-fu-kai-fang-jie-kou/)

这个可以作为微信开发的入门教材



