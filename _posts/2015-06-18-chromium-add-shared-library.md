---
layout: post
title: "chromium中添加动态库的方法"
tags: [网络编程]
---

增加动态库的方法：

1. 隐式调用

 
	linux平台添加方式如下
	 
	(1) 在src/build/linux/system.gyp中添加新的target(参考里面的libresolv):
	(猜测，这里的作用是把系统库转换到chromium里面)
	
		+   {
		+      'target_name': 'libopenfec',
		+      'type': 'none',
		+      'link_settings': {
		+        'libraries': [
		+          '-lopenfec',
		+        ],
		+      },
		+    },
	
	
	(2) 在使用这个库所在的模块gyp中添加依赖，例如在src/net/mmt/mmtp.gyp中添加	
	

		'target_name': 'libmmtp',
	      # 'type': '<(component)',
	      # 'type': 'loadable_module',
	      'type': 'static_library',
	      'product_name': 'mmtp',
	    +  'dependencies': ['../../../build/linux/system.gyp:libopenfec',],
	      'sources': [
	        'mmtp.cc',
	        'DList.cc',
	        'Mlist.cc',
	
	
	(3) 重新gyp prepare然后编译link即可

2. 显式调用动态加载
	
	参考如下网上教程，
	需要注意的是，dlsym返回的函数需要强制类型转化，由于dlsym返回值是一个函数指针，在使用的时候，需要取内容，加*符号，另外库文件的位置也需要指定，最好写成绝对路径，防止找不到文件
	
	一个使用demo
	
			EXAMPLE
		Load the math library, and print the cosine of 2.0: 
		
		#include <stdio.h>
		#include <dlfcn.h>
		
		int main(int argc, char **argv) {
		    void *handle;
		    double (*cosine)(double);
		    char *error;
		
		    handle = dlopen ("libm.so", RTLD_LAZY);
		    if (!handle) {
		        fprintf (stderr, "%s\n", dlerror());
		        exit(1);
		    }
		
		    cosine = dlsym(handle, "cos");
		    if ((error = dlerror()) != NULL)  {
		        fprintf (stderr, "%s\n", error);
		        exit(1);
		    }
		
		    printf ("%f\n", (*cosine)(2.0));
		    dlclose(handle);
		    return 0;
		}
		
		
		 If this program were in a file named "foo.c", you would build the program with the following command: 
		
		 gcc -rdynamic -o foo foo.c -ldl




	[linux下动态链接库(.so)的显式调用和隐式调用]http://blog.csdn.net/lc_910927/article/details/42393121

	[LINUX下动态链接库的使用-dlopen dlsym dlclose dlerror]）http://blog.csdn.net/jernymy/article/details/6903683
 	[返回类型强制转换]http://bbs.chinaunix.net/thread-610868-1-1.html
