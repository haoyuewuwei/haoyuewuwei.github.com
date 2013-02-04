---
layout: post
title: "Java中StringBuilder.append和直接用String+String的效率比较"
description: ""
category: JAVA
tags: [String效率, Java]
---
{% include JB/setup %}
Reference: http://book.csdn.net/bookfiles/135/1001354628.shtml

public class AppendStringTest{
		public static void main(String [] args){
				String text = "";
				long beginTime = System.currentTimeMillis();
				for(int i = 0; i < 10000; i++){
						text += i;
					}
				long endTime = System.currentTimeMillis();
				System.out.println("Running time:" + (endTime - beginTime));
				StringBuilder builder = new StringBuilder("");
				beginTime = System.currentTimeMillis();
				for(int i = 0; i < 10000; i++){
						builder.append(String.valueOf(i));
					}
				endTime = System.currentTimeMillis();
				System.out.println("Runing time:" + (endTime - beginTime));
			}
	} 

Results:

<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21141&authkey=AOFZi-NgEveWHDM" width="314" height="39" frameborder="0" scrolling="no"></iframe>

注意到随着操作进行的次数增多，差距会更加明显。
