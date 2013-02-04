---
layout: post
title: "JSP实现接收邮件"
description: ""
category: JAVA
tags: [Java]
---
{% include JB/setup %}

servlet:

ReceiveTextMail.java

{% highlight java %}
package servlets;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import javax.mail.*;
import beans.TextMailReceiver;

public class ReceiveTextMail extends HttpServlet {

	/**
	 * Constructor of the object.
	 */
	public ReceiveTextMail() {
		super();
	}

	/**
	 * Destruction of the servlet. <br>
	 */
	public void destroy() {
		super.destroy(); // Just puts "destroy" string in log
		// Put your code here
	}

	/**
	 * The doGet method of the servlet. <br>
	 *
	 * This method is called when a form has its tag value method equals to get.
	 * 
	 * @param request the request send by the client to the server
	 * @param response the response send by the server to the client
	 * @throws ServletException if an error occurred
	 * @throws IOException if an error occurred
	 */
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out
				.println("<!DOCTYPE HTML PUBLIC /"-//W3C//DTD HTML 4.01 Transitional//EN/">");
		out.println("<HTML>");
		out.println("  <HEAD><TITLE>A Servlet</TITLE></HEAD>");
		out.println("  <BODY>");
		out.print("    This is ");
		out.print(this.getClass());
		out.println(", using the GET method");
		out.println("  </BODY>");
		out.println("</HTML>");
		out.flush();
		out.close();
	}

	/**
	 * The doPost method of the servlet. <br>
	 *
	 * This method is called when a form has its tag value method equals to post.
	 * 
	 * @param request the request send by the client to the server
	 * @param response the response send by the server to the client
	 * @throws ServletException if an error occurred
	 * @throws IOException if an error occurred
	 */
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		response.setContentType("text/html");
		response.setCharacterEncoding("gb2312");
		PrintWriter out = response.getWriter();
		
		
		String name = request.getParameter("name");
		String password = request.getParameter("password");
		String host = request.getParameter("host");
		TextMailReceiver receiver = new TextMailReceiver(name,password,host);
		try{
			Message [] messages = receiver.getMai();
			out.print("<table border='1'><tr><td>Sequence</td><td>Sender</td><td>Recipient</td><td>Subject</td><td>Content</td>");
			for(int i = 0; i < messages.length; i++){
				Message message = messages[i];
				out.print("<tr>");
				out.print("<td>" + i + 1 + "</td>");
				out.print("<td>" + message.getFrom()[0].toString() + "</td>");
				out.print("<td>" + message.getAllRecipients()[0].toString() + "</td>");
				out.print("<td>" + message.getSubject() + "</td>");
				out.print("<td>" + message.getContent() + "</td>");
				out.print("</tr>");
			}
			out.print("</table>");
		}catch(MessagingException e){
			e.printStackTrace();
		}
	}

	/**
	 * Initialization of the servlet. <br>
	 *
	 * @throws ServletException if an error occurs
	 */
	public void init() throws ServletException {
		// Put your code here
	}

}

{% endhighlight %}

JavaBean:

TextMailReceiver.java

{% highlight java %}
package beans;

import java.util.*;
import javax.mail.*;
public class TextMailReceiver {
	Folder inbox;
	Store store;
	String name = "";
	String password = "";
	String host = "";
	
	/**
	 * Initialize the Receiver
	 * @param name
	 * @param password
	 * @param host
	 */
	public TextMailReceiver(String name, String password, String host){
		this.name = name;
		this.password = password;
		this.host = host;
	}
	
	/**
	 * Get all the mails in the mailbox
	 * @return
	 * @throws MessagingException
	 */
	public Message [] getMai() throws MessagingException{
		Properties prop = new Properties();
		prop.put("mail.pop3.host", host);
		MyAuthenticator auth = new MyAuthenticator(name,password);
		Session mailSession = Session.getDefaultInstance(prop,auth);
		store = mailSession.getStore("pop3");
		store.connect(host,name,password);
		inbox = store.getDefaultFolder().getFolder("inbox");
		inbox.open(Folder.READ_WRITE);
		Message [] msg = inbox.getMessages();
		return msg;
	}
}

{% endhighlight %} 

测试的JSP代码：

{% highlight java %}
<%@ page language="java" import="java.util.*" pageEncoding="gb2312"%>
<html>
  <head>
    <title>My JSP 'ReceiveTextMail.jsp' starting page</title>   

  </head>
  
  <body>
    <form method="post" action="servlet/ReceiveTextMail" >
 Mailbox:<input type="text" size="40" name="account" "/><br><br>
 userName:<input type="text" size="40" name="name" /><br><br>
 password: <input type="password" size="40" name="password" /><br><br>
 POP3:<input type="text" size="40" name="host" /><br><br>
 <input type="submit" value="submit"/>
 <input type="reset" value="cancle"/>
 </form>
  </body>
</html>

 {% endhighlight %}

 

What's the problem?：

Encouter the exception:  javax.mail.NoSuchProviderException: Invalid protocol: null 

What does it cause?

getStore method doesn't has a parameter.

How to reslove it?

add "pop3" to the method "store = mailSession.getStore("pop3");"
