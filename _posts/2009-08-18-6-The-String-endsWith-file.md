---
layout: post
title: "利用String的endsWith过滤文件"
description: ""
category: JAVA
tags: [Java, String类]
---
{% include JB/setup %}
{% highlight java %}
public class FileFilter{
		public static void main(String [] args){
				String [] fileName = {"one.jpg","two.txt","three.doc","four.jpg","five.bat","six.bmp","seven.jpg"};
				for(int i = 0; i < fileName.length; i++){
						if(fileName[i].endsWith("jpg")){
								System.out.print(fileName[i] + "  ");
							}
					}
					System.out.println();
			}
	}

  {% endhighlight %}