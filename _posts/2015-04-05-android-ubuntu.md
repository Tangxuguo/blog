---
layout: post
title: "ubuntu下android开发环境配置"
tags: [android]
---

#1. 安装 Android Studio 

(1) 下载Android Studio开发环境

下载地址 : http://developer.android.com/sdk/installing/studio.html , Windows 和 Linux 不分 32 位 和 64 位;

安装启动Android Studio : 解压Android Studio 到目录中, 执行 bin 目录中的 studio.sh 文件, 启动Android Studio;

进入后会提示 bin 目录下的一些文件没有权限 : 修改执行文件 和 sh 文件的权限;

(2) 下载android SDK 
第一次运行studio.sh需要下载SDK,Android Studio:Fetching android sdk component information
由于谷歌被墙会报错,设置代理
在~/.AndroidStudio/config/options目录下修改other.xml,添加你需要的代理,重新启动


    <option name="PROXY_TYPE_IS_SOCKS" value="true" />
    <option name="USE_HTTP_PROXY" value="false" />
    <option name="USE_PROXY_PAC" value="false" />
    <option name="PROXY_HOST" value="192.168.101.22" />
    <option name="PROXY_PORT" value="1080" />
    <option name="PROXY_AUTHENTICATION" value="false" />
    <option name="PROXY_LOGIN" value="" />
    <option name="PROXY_PASSWORD_CRYPT" value="" />
    <option name="KEEP_PROXY_PASSWORD" value="false" />
    <option name="myGenericPasswords">

(3) 运行模拟器
当使用android的AVD时提示以下错误：
Starting emulator for AVD 'NexusOne' ERROR: 32-bit Linux Android emulator binaries are DEPRECATED, to use them you will have to do at least one of the following: - Use the '-force-32bit' option when invoking 'emulator'. - Set ANDROID_EMULATOR_FORCE_32BIT to 'true' in your environment. Either one will allow you to use the 32-bit binaries, but please be aware that these will disappear in a future Android SDK release. Consider moving to a 64-bit Linux system before that happens. 

+ 选中你的android stdio项目文件夹,标题栏选择Run,Edit Configurations...
+ 在Andorid application 或者Test模拟器下Emulator的Additional  Command Line Options,添加-force-32bit 保存后运行

android studio使用中碰到Failure [INSTALL_FAILED_OLDER_SDK], 这个问题出现在模拟器上的概率更大些，原因是某些应用使用了原生库（NDK，Native Lib），这些库的编译目标通常是arm架构的cpu，在x86上运行就会报这样的错误。 
更换成Genymotion模拟器,官方地址教程https://www.genymotion.com/#!/(先注册,下载一个bin文件安装后,再在android stdio安装Genymotion插件)

下载完成后新建虚拟机后运行报错java.lang.UnsatisfiedLinkError: Cannot load library: load_library[1098]: Library '/system/lib/libhoudini.so' not found
解决方案:下载一个ARM Translation Installer的压缩包，安装到Genymotion上，重启下，重新运行程序，ok[参考链接](http://forum.xda-developers.com/showthread.php?t=2528952)

