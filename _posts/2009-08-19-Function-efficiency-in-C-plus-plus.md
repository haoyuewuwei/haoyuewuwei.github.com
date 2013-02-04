---
layout: post
title: "C++中函数执行效率研究"
description: ""
category: C_CPP
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

void main()
{
	PerCounter myCounter;
	//calculate the first algorith's run time
	myCounter.startCount();
	algorithm1();
	myCounter.stopCount();   
	cout<<myCounter<<endl;
	
	//calculate the first algorith's run time again
	myCounter.startCount();
	algorithm1();
	myCounter.stopCount();
	cout<<myCounter<<endl;

	//calculate the second algorith's run time
	myCounter.startCount();
	algorithm2();
	myCounter.stopCount();
	cout<<myCounter<<endl;
}
 
  {% endhighlight %}
运行结果如下：
<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21142&authkey=ABhUDlUyEsiir_Y" width="317" height="36" frameborder="0" scrolling="no"></iframe>


由上面结果可以发现虽然第一次和第二次执行的是同一个函数，但是时钟周期还是不一样的，这是因为电脑里还有其他N多的程序在跑，时钟周期是相当小的一个单位，所以所受影响会较大，一般我们普通的用毫秒来计算一个算法的好坏就已经足够了，所以再附上一个以毫秒级表示的的程序代码：

#include <iostream>
using namespace std;
//此为本人电脑主频，可根据不同情况改变
#define MHZ 2990000000  
int toMilli = 1000;
class PerCounter{   
	unsigned int runTime, _dwLow, _dwHigh, mhz;   
public:
	PerCounter(unsigned int mhz){
		this->mhz = mhz;
	}
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
				mul   toMilli
				div   [ebx].mhz
				mov   [ebx].runTime,   eax   
		}   
	}   
    
	friend ostream& operator<< (ostream& out, PerCounter& pc){   
		return out<<pc.runTime;   
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

void main()
{
	PerCounter myCounter(MHZ);
	//calculate the first algorith's run time
	myCounter.startCount();
	algorithm1();
	myCounter.stopCount();   
	cout<<myCounter<<endl;
	
	//calculate the first algorith's run time again
	myCounter.startCount();
	algorithm1();
	myCounter.stopCount();
	cout<<myCounter<<endl;

	//calculate the second algorith's run time
	myCounter.startCount();
	algorithm2();
	myCounter.stopCount();
	cout<<myCounter<<endl;
} 

结果就不贴了，直接复制运行就好。

如果有人需要计算非常精确的程序运行时间，那还要考虑程序的缓存问题，当然有时候由于程序执行的无序性还会导致程序所得的周期数不一样，在这样的情况下可以用汇编的cupid指令来强制使前期的操作执行完成。

如果想更加深入可以参照以下文章：

Reference:  http://www.ccsl.carleton.ca/~jamuir/rdtscpm1.pdf
