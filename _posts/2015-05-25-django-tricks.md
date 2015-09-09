---
layout: post
title: "Django Tricks"
tags: [Django]
---


1. django项目安排
	http://www.cnblogs.com/holbrook/archive/2012/02/25/2368231.html
	http://www.loonapp.com/blog/11/
2. 静态资源存放及部署
	http://blog.yangyubo.com/2012/07/26/django-staticfiles/
3. 模板页面安排
	http://www.weiguda.com/blog/14/
4. 实现表格单双不同颜色
5. 多表查询及反向查询，反向查询方式
   http://my.oschina.net/u/993130/blog/209460
6. 邮箱通知，模板制作及渲染-mail
	http://blog.csdn.net/yima1006/article/details/8991145
7. 检查访问条件，避免大量重复代码-修饰符
	http://blog.csdn.net/shangzhihaohao/article/details/6928808
8. 不同页面传递参数-cookie和session
	http://blog.csdn.net/bravezhe/article/details/7711186
	http://dokelung-blog.logdown.com/posts/222552-django-notes-9-cookies-and-sessions
9. request和post方式捕获
10. 外键使用默认值
	http://stackoverflow.com/questions/937954/how-do-you-specify-a-default-for-a-django-foreignkey-model-or-adminmodel-field
11. 避免crsf攻击
12. 构建漂亮的界面
13. 重写后台model save方法
	http://www.lihuai.net/program/python/1465.html
14. manytomany filed前台呈现
	http://www.4byte.cn/question/159570/showing-django-many-to-many-in-template.html
15. manytomany filed后台呈现
16. 模型迁移工具-south
	http://www.cnblogs.com/BeginMan/p/3324774.html
17. manytomany及外键查询 
18. tango教程http://hackerxu.com/Twd/#3-tango
19. 时区设置及库内数据转换
	http://blog.feshine.net/technology/1220.html
20. 虚拟环境管理
	http://liuzhijun.iteye.com/blog/1872241
21. json dumps序列化datetime类型变量,以及初始化model
	http://stackoverflow.com/questions/20502075/django-serialize-valuequeryset
	from django.core.serializers.json import DjangoJSONEncoder
HttpResponse(json.dumps(mydict,cls=DjangoJSONEncoder),content_type="application/json")
22. POST 403错误，ajax
	http://stackoverflow.com/questions/19333098/403-forbidden-error-when-making-an-ajax-post-request-in-django-framework
	https://docs.djangoproject.com/en/1.7/ref/contrib/csrf/#ajax
23. 在Django中实现一个高性能未读消息计数器
	http://www.zlovezl.cn/articles/implement-an-efficient-counter-in-django/
24. Django中使用缓存，如何处理set，list以及更新操作
25. 乱码、
在所有涉及到中文显示的页面头加上 

	    #coding:utf-8
	修改settings.py文件，添加
	    FILE_CHARSET = 'utf-8'
	    DEFAULT_CHARSET = 'utf-8'

    然后修改LANGUAGE_CODE = 'zh-cn'
26. python(django) 使用PIL报错IOError at decoder jpeg not available
 http://blog.csdn.net/leiyonglin/article/details/7190142
27. Mysql字符乱码，修改默认字符编码
mysqldump --user=username --password=password --default-character-set=latin1 --skip-set-charset dbname > dump.sql
sed -r 's/latin1/utf8/g' dump.sql > dump_utf.sql
mysql --user=username --password=password --execute="DROP DATABASE dbname; CREATE DATABASE dbname CHARACTER SET utf8 COLLATE utf8_general_ci;"
mysql --user=username --password=password --default-character-set=utf8 dbname < dump_utf.sql