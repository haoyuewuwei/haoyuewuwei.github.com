---
layout: post
title: "利用异或加密的简单例子"
description: ""
category: 加密解密
tags: [加密解密, Java]
---
{% include JB/setup %}

代码如下：

{% highlight java %}
public class XorCode { 

    public static void main(String[] args) { 

        char ch = 'A'; 
        System.out.println("编码前：" + ch); 

        ch = (char)(ch^7); 
        System.out.println("编码后：" + ch); 

        ch = (char)(ch^7); 
        System.out.println("解码：" + ch); 

    } 

} 

 {% endhighlight %}

解释：

0x7是Java中整数的十六进制写法，其实就是十进制的7，将位与1作XOR的作用其实就是位反转，0x7的最右边3个位为1，所以就是反转ch变量的最后两个位，如下所示：

ASCII中的'A'字符编码为65：01000001 
整数7：00000111
XOR运算后：01000110
01000110就是整数70，对应ASCII中的字符F的编码，所以用字符方式显示时会显示F字符。同样地，这个简单的XOR字符加密，要解密也只要再进行相同的位反转就可以了。执行结果如下：


