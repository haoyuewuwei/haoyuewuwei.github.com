---
layout: post
title: "WebappClassLoader类和rt.jar的attach源码【转】"
description: ""
category: Web开发
tags: [JAVA, Web, WebappClassLoader]
---
{% include JB/setup %}

理解Tomcat的WebappClassLoader

负责Web应用的类加载的是org.apache.catalina.loader.WebappClassLoader，它几个比较重要的方法：findClass(),loadClass(),findClassInternal(),findResourceInternal().类加载器被用来加载一个类的时候，loadClass()会被调用，loadClass()则调用findClass()。后两个方法是WebappClassLoader的私有方法，findClass()调用findClassInternal()来创建class对象，而findClassInternal()则需要findResourceInternal()来查找.class文件。

reference:http://www.diybl.com/course/3_program/java/javashl/20081119/151995.html

 

如何在Eclipse sdk中查看jar源代码如：*.jar


1.点 “window”-> "Preferences" -> "Java" -> "Installed JRES"

2.此时"Installed JRES"右边是列表窗格，列出了系统中的 JRE 环境，选择你的JRE，然后点边上的 "Edit..."， 会出现一个窗口(Edit JRE)

3.选中rt.jar文件的这一项：“c:/program files/java/jre_1.5.0_06/lib/*.jar”
点 左边的“+” 号展开它，

4.展开后，可以看到“Source Attachment:(none)”，点这一项，点右边的按钮“Source Attachment...”, 选择你的JDK目录下的 “src.zip”文件

5.一路点"ok",结束。

但是有的jar中只有方法说明，而没有具体的实现。
