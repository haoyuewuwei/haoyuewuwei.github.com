---
layout: post
title: "Java各数据类型的范围"
description: ""
category: JAVA
tags: [Java, 数据范围]
---
{% include JB/setup %}

{% highlight java %}
public class DataRange { 

    public static void main(String[] args) { 

        System.out.printf("short /t数值范围：%d ~ %d/n", 

                             Short.MAX_VALUE, Short.MIN_VALUE); 

        System.out.printf("int /t数值范围：%d ~ %d/n", 

                             Integer.MAX_VALUE, Integer.MIN_VALUE); 

        System.out.printf("long /t数值范围：%d ~ %d/n",

                             Long.MAX_VALUE, Long.MIN_VALUE); 

        System.out.printf("byte /t数值范围：%d ~ %d/n", 

                             Byte.MAX_VALUE, Byte.MIN_VALUE); 

        System.out.printf("float /t数值范围：%e ~ %e/n", 

                             Float.MAX_VALUE, Float.MIN_VALUE); 

        System.out.printf("double /t数值范围：%e ~ %e/n", 

                             Double.MAX_VALUE, Double.MIN_VALUE); 

    } 

}
 
{% endhighlight %}
results：

<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21140&authkey=AH1cvvHfZW6fUqo" width="316" height="55" frameborder="0" scrolling="no"></iframe>
