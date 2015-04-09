---
layout: post
title: "OJ入门-基本输入和输出处理"
tags: [算法]
---
>注：内容来自杭电ACM入门课件，详细情况可以查阅相关资料

##输入
###输入第一类
输入不说明有多少个Input Block,以EOF为结束标志。


C语法：

	while(scanf("%d %d",&a, &b) != EOF) 
	{      ....  } 
C++语法：

	while( cin >> a >> b ) 
	{      ....  } 
	
###输入_第二类：
输入一开始就会说有N个Input Block,下面接着是N个Input Block。

C语法：

	scanf("%d",&n) ; 
	for( i=0 ; i<n ; i++ ) 
	{      ....  } 
C++语法：

	cin >> n; 
	for( i=0 ; i<n ; i++ ) 
	{      ....  } 
	
###输入_第三类：
输入不说明有多少个Input Block,但以某个特殊输入为结束标志。

C语法：

	while(scanf(“%d”,&n) ！=EOF && n!=0 ) 
	{      ....  } 

C++语法：

	while( cin >> n && n != 0 ) 
	{      ....  } 
	
###输入_第四类：
输入是一整行的字符串的

C语法：

	char buf[20]; 
	gets(buf); 
C++语法：

如果用string buf;来保存：
	
	 	getline( cin , buf ); 
如果用char buf[ 255 ]; 来保存：
	 
	 cin.getline( buf, 255 );
	
	
1. scanf(“ %s%s”,str1,str2)，在多个字符串之间用一个或多个空格分隔；
2. 若使用gets函数，应为gets(str1); gets(str2); 字符串之间用回车符作分隔。
通常情况下，接受短字符用scanf函数，接受长字符用gets函数。
而getchar函数每次只接受一个字符，经常c=getchar()这样来使用。

3. getline 是一个函数，它可以接受用户的输入的字符，直到已达指定个数，或者用户输入了特定的字符。它的函数声明形式（函数原型）如下：

	istream& getline(char line[], int size, char endchar = '\n');
不用管它的返回类型，来关心它的三个参数：
char line[]： 就是一个字符数组，用户输入的内容将存入在该数组内。
int size : 最多接受几个字符？用户超过size的输入都将不被接受。
char endchar :当用户输入endchar指定的字符时，自动结束。默认是回车符。

	结合后两个参数，getline可以方便地实现： 用户最多输入指定个数的字符，如果超过，则仅指定个数的前面字符有效，如果没有超过，则用户可以通过回车来结束输入。

	char name[4];
	cin.getline(name,4,'\n');
由于 endchar 默认已经是 '\n'，所以后面那行也可以写成：
cin.getline(name,4);

##输出
###输出_第一类：
一个Input Block对应一个Output Block，Output Block之间没有空行。

	C语法：
	{      ....     printf("%d\n",ans); 
	} 
	C++语法：
	{      ...      cout << ans << endl;  	} 

###输出_第二类：
一个Input Block对应一个Output Block，每个Output Block之后都有空行。

	C语法：
	{      ....     printf("%d\n\n",ans); 
	} 

	C++语法：
	{      ...      cout << ans << endl << endl;  	}
	
###输出_第三类：
一个Input Block对应一个Output Block，Output Block之间有空行。

	#include <stdio.h>
 	int main()
 	{ 
    int icase,n,i,j,a,sum;
    scanf("%d",&icase);
    for(i=0;i<icase;i++)
  	{
		sum=0;
         scanf("%d",&n);
		for(j=0;j<n;j++)
		{
	     scanf("%d",&a);
               sum+=a;
		}
		if(i<icase-1)
   	   		printf("%d\n\n",sum);
         else
 	   		printf("%d\n",sum);
 	}
 	} 
 
 
 C语法：
 
	for (k=0;k<count;k++) 
	{ 	
	while (…)
	{
		printf(" %d\n",result);
	}
	if (k!=count-1) printf("\n"); 
	}  
C++语法：
	类似，输出语句换一下即可。