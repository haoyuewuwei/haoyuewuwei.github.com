---
layout: post
title: "手工构建JAVA工程之java打包血泪史"
description: ""
category: JAVA
tags: [Java, 打包]
---
{% include JB/setup %}

     今天打算学习下工程构建工具ant，看的书是《零基础学Java Web开发》第十二章，刚开篇作者为了体现ant的好处，所以有个手动构建helloword工程的例子。本人一直觉得工具是好，但是用工具之前首先得会手工。不然就不知道其原理了，所以我就开始打起了这个例子，原以为很简单的事情，想不到我却居然搞了将近1个半小时，郁闷之极。

      好了，言归正传，我给大家介绍下我的所学吧：

      1) use notepad to code a simple program named MyDate.java

        import java.util.Date;
public class MyDate{
	public static void main(String [] args){
		Date now = new Date();
		System.out.println(now.toString());
	}
} 

      2) create a folder named HelloWorld.

      3) create two subfolders of the HelloWorld named build and src.

      4) remove the HelloWorld.java to the src folder.

      5) open the DOS cmd window and enter the MyDate folder then type the command as follows:

          javac -sourcepath src -d build/classes src/MyDate.java

          echo Main-Class: MyDate>MyManifest   //这里要注意冒号后面是有空格的，作用为指明jar执行时的主函数入口

          jar cvfm build/lib/MyDate.jar MyManifest -C build/classes .    //注意最后有一个句号（那本书上写的是逗号，郁闷死我），该命令作用为根据MyManifest来配置jar包的manifest，并放在build/lib/下面，包内的类来自build/classes目录下的所有class文件

      6) run the MyDate.jar

         enter the command as follows: java -jar build/lib/MyDate.jar

      The flow chart is as follows:

<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21139&authkey=AP-zel7SYcOBfXM" width="320" height="195" frameborder="0" scrolling="no"></iframe>     
