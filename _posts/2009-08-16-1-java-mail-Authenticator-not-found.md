---
layout: post
title: "javax.mail.Authenticator not Founder的问题"
description: ""
category: JAVA
tags: [java]
---
{% include JB/setup %}


What's the problem?

用JSP实现邮件功能，报错：ClassNotFound: javax.mail.Authenticator not Founder

How to reslove it?

分别到以下网站下载：JavaMail API和JavaBeans Activation Framework

http://java.sun.com/products/javamail/downloads/index.html JavaMail API

http://java.sun.com/javase/technologies/desktop/javabeans/glasgow/jaf.html JavaBeans Activation Framework

把下载下来的两个jar包放到项目的WEB-INF/lib目录下

What's the root cause?

使用IDE：MyEclipse7.1，虽然其自带J2EE的lib，而且lib里有一个mail的包，包含了程序所要使用的class，所以在编写程序的时候，你引入包不会有错，可是WebappClassLoader却没有到这个路径下面去读取这个类，导致出现ClassNotFound的异常。
