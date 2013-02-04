---
layout: post
title: "Java转换十进制为其他进制的两种方法"
description: ""
category: JAVA
tags: [进制转换, Java]
---
{% include JB/setup %}

利用printf():

{% highlight java %}
public class TigerNumberDemo {

    public static void main(String[] args) {

        // 输出 19 的十进制表示
        System.out.printf("%d%n", 19);

        // 输出 19 的八进制表示
        System.out.printf("%o%n", 19);

        // 输出 19 的十六进制表示
        System.out.printf("%x%n", 19); 
    }

}
{% endhighlight %}

利用Integer类方法:

{% highlight java %}
public class NumberDemo {

    public static void main(String[] args) {

        // 十进制 19 转成二进制 10011
        System.out.println(Integer.toBinaryString(19));

        // 十进制 19 转成十六进制 13
        System.out.println(Integer.toHexString(19));

        // 十进制 19 转成八进制 23
        System.out.println(Integer.toOctalString(19));

    }

}
 
{% endhighlight %}