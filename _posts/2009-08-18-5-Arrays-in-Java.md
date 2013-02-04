---
layout: post
title: "Java中数组的理解"
description: ""
category: 读书笔记
tags: [Java数组]
---
{% include JB/setup %}

      thinking in java中写到everything is an object。所以java里面的数组跟C++里的也有所不同，java里面的数组是一个对象，所以可以用new来生成，所以可以动态分配，而C++不行。

      java中数组名可以理解为C里面的一个指针：

int [] arr = {1,2,3,4,5};
int [] temp = null;
temp = arr; 

上面的代码相当于把temp指向了arr所指向的那个数组地址。

所以当temp[i]的值改变的时候arr[i]也会随之改变。

如果要实现数组复制而不是只是把引用赋给temp则要用以下代码：

int [] arr = {1,2,3,4,5};
int [] temp = new int [arr.length];
for(int i = 0; i < temp.length; i++){
    temp[i] = arr[i];
} 

或者可以用 System.arraycopy()来进行数组复制：

函数原型：static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 

int [] arr = {1,2,3,4,5};
int [] temp = new [arr.length];
System.arraycopy(arr,0,temp,0,arr.length); 


