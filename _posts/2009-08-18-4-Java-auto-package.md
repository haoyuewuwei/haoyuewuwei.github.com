---
layout: post
title: "Java的自动装箱陷阱"
description: ""
category: JAVA
tags: [Java, 自动装箱]
---
{% include JB/setup %}

Reference:http://book.csdn.net/bookfiles/135/1001354614.shtml

 

范例4.6  AutoBoxDemo2.java：
{% highlight java %}
public class AutoBoxDemo2 {

    public static void main(String[] args) {

        Integer i1 = 100;
        Integer i2 = 100;

        if (i1 == i2) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");
    }

}
 {% endhighlight %} 

   从自动装箱与拆箱的机制来看，可能会觉得结果是显示i1 == i2，您是对的。


 范例4.7  AutoBoxDemo3.java
{% highlight java %}
public class AutoBoxDemo3 {

    public static void main(String[] args) {
        Integer i1 = 200;
        Integer i2 = 200;

        if (i1 == i2) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");

    }

}
 
 {% endhighlight %}

那么范例4.7的这个程序，您觉得结果是什么？

 

结果是显示i1 != i2，这有些令人惊讶，两个范例语法完全一样，只不过改个数值而已，结果却相反。

其实这与==运算符的比较有关，在第3章中介绍过==是用来比较两个基本数据类型的变量值是否相等，事实上==也用于判断两个对象变量名称是否参考至同一个对象。

在自动装箱时对于值从–128到127之间的值，它们被装箱为Integer对象后，会存在内存中被重用，所以范例4.6中使用==进行比较时，i1 与 i2实际上参考至同一个对象。如果超过了从–128到127之间的值，被装箱后的Integer对象并不会被重用，即相当于每次装箱时都新建一个Integer对象，所以范例4.7使用==进行比较时，i1与i2参考的是不同的对象。

所以不要过分依赖自动装箱与拆箱，您还是必须知道基本数据类型与对象的差异。范例4.7最好还是依正规的方式来写，而不是依赖编译器蜜糖(Compiler Sugar)。例如范例4.7必须改写为范例4.8才是正确的。

 

 



范例4.7  AutoBoxDemo4.java
{% highlight java %}
public class AutoBoxDemo4 {

    public static void main(String[] args) {
        Integer i1 = 200;
        Integer i2 = 200; 

        if (i1.equals(i2)) 
            System.out.println("i1 == i2");
        else 
            System.out.println("i1 != i2");
    }

}
  {% endhighlight %}

结果这次是显示i1 == i2。使用这样的写法，相信也会比较放心一些，对于这些方便但隐藏细节的功能到底要不要用呢？基本上只有一个原则：如果您不确定就不要用。
