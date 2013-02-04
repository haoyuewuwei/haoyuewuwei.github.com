---
layout: post
title: "java利用JavaMail实现邮件功能"
description: ""
category: JAVA
tags: [Java, Web]
---
{% include JB/setup %}

Mailbox.java: 表示邮箱的一些信息

{% highlight java %}
package common;

public class Mailbox {
	private String host;
	private String name;
	private String password;
	private String from;
	private String to;
	private String subject;
	private String content;
	
	/**
	 * 
	 * @param host
	 * @param name
	 * @param password
	 * @param mailInfo
	 */
	public Mailbox(String host, String name, String password,MailInfo mailInfo) {
		// TODO Auto-generated constructor stub
		this.host = host;
		this.name = name;
		this.password = password;
		this.from = mailInfo.getFrom();
		this.to = mailInfo.getTo();
		this.subject = mailInfo.getSubject();
		this.content = mailInfo.getContent();
	}
	
	/**
	 * 
	 * @return
	 */
	public boolean sendTextMail(){
		TextMail textMail = new TextMail(host,name,password);
		textMail.setFrom(from);
		textMail.setText(content);
		textMail.setTo(to);
		textMail.setSubject(subject);
		return textMail.send();
	}
}
 
{% endhighlight %}
 

MailInfo.java: 邮件的一些信息:from,to,suject,content

{% highlight java %}
package common;

public class MailInfo {
	private String from;
	private String to;
	private String subject;
	private String content;
	
	/**
	 * 
	 * @param from
	 * @param to
	 * @param subject
	 * @param content
	 */
	public MailInfo(String from, String to, String subject, String content){
		this.from = from;
		this.to = to;
		this.subject = subject;
		this.content = content;
	}
	
	public String getFrom() {
		return from;
	}
	public void setFrom(String from) {
		this.from = from;
	}
	public String getTo() {
		return to;
	}
	public void setTo(String to) {
		this.to = to;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	
}
 
{% endhighlight %}
 

TextMail.java:对要发送的邮件进行初始化

{% highlight java %}
package common;

import java.util.Properties;
import javax.mail.BodyPart;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.NoSuchProviderException;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

/**
 * function: Send a mail to the specific address
 * @author JAY
 *
 */
public class TextMail {
	private MimeMessage message;
	private Properties props;
	private Session session;
	private String name = "";
	private String password = "";

	/**
	 * complete the initialization
	 * @param host
	 * @param name
	 * @param password
	 */
	public TextMail(String host, String name, String password){
		this.name = name;
		this.password = password;
		props = System.getProperties();
		//set the SMTP host
		props.put("mail.smtp.host", host);
		//set SMTP authorization
		props.put("mail.smtp.auth", "true");
		//get the mail session
		MyAuthenticator auth = new MyAuthenticator(name,password);
		session = Session.getDefaultInstance(props,auth);
		//create MIME mail object
		message = new MimeMessage(session);
	}	
	
	/**
	 * set mail sender
	 * @param from
	 */
	public void setFrom(String from){
		try {
			message.setFrom(new InternetAddress(from));
		} catch (AddressException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (MessagingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	/**
	 * set mail recipient
	 * @param to
	 */
	public void setTo(String to){
		try {
			message.setRecipients(Message.RecipientType.TO, InternetAddress.parse(to));
		} catch (AddressException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (MessagingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	/**
	 * set mail subject
	 * @param subject
	 */
	public void setSubject(String subject){
		try {
			message.setSubject(subject);
		} catch (MessagingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	/**
	 * set mail text
	 * @param text
	 */
	public void setText(String text){
		try {
			message.setText(text);
		} catch (MessagingException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	/**
	 * send mail
	 * @return
	 */
	public boolean send(){
		try{
			//create SMTP sender object
			Transport transport = session.getTransport("smtp");
			//get the connection with server
			transport.connect((String) props.get("mail.smtp.host"),name,password);
			//send the mail via server
			transport.sendMessage(message, message.getRecipients(Message.RecipientType.TO));
			transport.close();
			return true;
		}catch(NoSuchProviderException e){
			e.printStackTrace();
			return false;
		}catch (MessagingException e){
			e.printStackTrace();
			return false;
		}
	}
}
 
{% endhighlight %}
 

MyAuthenticator.java

 
{% highlight java %}
package common;

import javax.mail.Authenticator;
import javax.mail.PasswordAuthentication;

public class MyAuthenticator extends Authenticator{
	String name;
	String password;
	
	/**
	 * 
	 * @param name
	 * @param password
	 */
	public MyAuthenticator(String name, String password){
		this.name = name;
		this.password = password;
		getPasswordAuthentication();
	}
	
	protected PasswordAuthentication getPasswordAuthentication(){
		return new PasswordAuthentication(name,password);
	}
}
 
{% endhighlight %}
 

Main.java:测试代码

{% highlight java %}
package common;

import java.util.*;

public class Main {

	/**
	 * @param args
	 */
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String from = "test@sina.com";
		String to = "test@163.com";
		String subject = "test";
		String content = "hello,world!";
		String host = "smtp.sina.com";
		String name = "test";
		String password = "pass";
/*		Scanner scan = new Scanner(System.in);
		System.out.println("Please select your emailbox:");
		System.out.println("mailbox host:");
		host = scan.next();
		System.out.println("mailbox name:");
		name = scan.next();
		System.out.println("mailbox password:");
		password = scan.next();
		System.out.println("send from:");		
		from = scan.next();
		System.out.println("send to:");
		to = scan.next();
		System.out.println("subject:");
		subject = scan.next();
		System.out.println("text:");
		text = scan.next();*/
		MailInfo mailInfo = new MailInfo(from,to,subject,content);
		Mailbox mailBox = new Mailbox(host,name,password,mailInfo);
		if(mailBox.sendTextMail()){
			System.out.println("Mail send successfully!");
		}
		else{
			System.out.println("Mail send failed!");
		}
	}

}
 
{% endhighlight %}
 
