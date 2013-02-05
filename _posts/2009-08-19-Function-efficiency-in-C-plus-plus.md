---
layout: post
title: "C++中函数执行效率研究"
description: ""
category: C/C++
tags: [C++函数]
---
{% include JB/setup %}
先fuck一下！刚才写到一半的文章居然没有掉了……心碎啊！！！

好了，言归正传。

先附上C++中验证两个函数执行效率的方法：（指令周期级）

{% highlight C %}
#include <iostream>
using namespace std;
class PerCounter{   
	unsigned int _dwLow, _dwHigh;   
public:
	void startCount(){   
		_asm{   
			rdtsc   
				mov   ebx,   this   
				mov   [ebx]._dwLow,   eax   
				mov   [ebx]._dwHigh,   edx   
		}   
	}  
	void stopCount(){   
		_asm{   
			rdtsc   
				mov   ebx,   this   
				sub   eax,   [ebx]._dwLow   
				sub   edx,   [ebx]._dwHigh
				mov   [ebx]._dwLow,   eax
                                     mov   [ebx]._dwHigh,  edx   
		}   
	}   
    
	friend ostream& operator<< (ostream& out, PerCounter& pc){   
		return out<<pc._dwHigh<<":"<<pc._dwLow;   
	}   
  }; 

void algorithm1()
{
	//type your code
	for(int i = 0; i < 10000000; i++);
}

void algorithm2()
{
	//type your code
	for(int i = 0; i < 10000; i++);
}
