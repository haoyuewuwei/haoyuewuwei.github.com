---
layout: post
title: "JSP实现发送带有附件的邮件代码"
description: ""
category: Web开发
tags: [JAVA, Web, WebappClassLoader]
---
{% include JB/setup %}

SendAttanchment.jsp文件--------------------------------测试文件

由于在markdown下面写html代码直接会被解析，所以只能截图了：
<iframe src="https://skydrive.live.com/embed?cid=90ABA068241662DC&resid=90ABA068241662DC%21112&authkey=AEpHO3wkCZcyeNY" width="319" height="216" frameborder="0" scrolling="no"></iframe> 

AttachmentSender.java 

完成邮件的以下一些操作：

1) 设置邮箱名,密码

2) 设置邮件的发送者，接收者，主题，正文，以及附件

3) 实现发送功能

{% highlight java %}
package beans;

import java.util.Properties;

import javax.activation.DataHandler;
import javax.activation.FileDataSource;
import javax.mail.BodyPart;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.NoSuchProviderException;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.AddressException;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeBodyPart;
import javax.mail.internet.MimeMessage;
import javax.mail.internet.MimeMultipart;

public class AttachmentSender {
	private MimeMessage message;
	private Properties props;
	private Session session;
	private MimeMultipart mp;
	private String name = "";
	private String password = "";

	/**
	 * complete the initialization
	 * @param host
	 * @param name
	 * @param password
	 */
	public AttachmentSender(String host, String name, String password){
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
		mp = new MimeMultipart();
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
	 * set the mail content
	 * @param content
	 */
	public void setContent(String content){
		try{
			BodyPart bp = new MimeBodyPart();
			bp.setContent(content,"text/html");
			mp.addBodyPart(bp);
		}catch(Exception e){
			e.printStackTrace();
		}
	}
	
	/**
	 * attach the file
	 * @param filename
	 */
	public void addAttachMent(String filename){
		try{
			BodyPart bp = new MimeBodyPart();
			FileDataSource fileds = new FileDataSource(filename);
			bp.setDataHandler(new DataHandler(fileds));
			bp.setFileName(fileds.getName());
			mp.addBodyPart(bp);
		}catch(Exception e){
			e.printStackTrace();
		}
	}
	
	/**
	 * send mail
	 * @return
	 */
	public boolean send(){
		try{
			message.setContent(mp);
			message.saveChanges();
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
 

 

 

 

SendAttachment.java

主要完成以下操作：

1) 解析从JSP传来的参数请求（邮件接收者，发送者，正文，附件）

2) 此处实现添加附件的方法是：先将附件上传至server的工程目录下，然后attach到邮件，然后删除上传到server上的文件


{% highlight java %}
package servlets;

import java.io.IOException;
import java.io.PrintWriter;

import java.io.File;
import java.util.Iterator;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.commons.*;
import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;
import beans.AttachmentSender;

public class SendAttachment extends HttpServlet {

	/**
	 * Constructor of the object.
	 */
	public SendAttachment() {
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
		PrintWriter out = response.getWriter();
		
		//set the smtp address of sina
		String host = "smtp.sina.com";
		//set mailbox accountName
		String accountName = "test";
		//set mailbox password
		String password = "pass";
		//attachment
		File uploadedFile = null;
		//sender's address
		String from = "";
		//recipient's address
		String to = "";
		//mail subject
		String subject = "";
		//mail content
		String content = "";
		
		//upload the attachment to the server first
		boolean isMultipart = ServletFileUpload.isMultipartContent(request);
		if(isMultipart){
			FileItemFactory factory = new DiskFileItemFactory();
			ServletFileUpload upload = new ServletFileUpload(factory);
			Iterator items;
			try{
				items = upload.parseRequest(request).iterator();
				while(items.hasNext()){
					FileItem item = (FileItem) items.next();
					if(!item.isFormField()){
						//get the name of the upload file
						String name = item.getName();
						String fileName = name.substring(name.lastIndexOf('//')+1,name.length());
						String path = request.getRealPath(fileName);
						//upload the file
						uploadedFile = new File(path);
						item.write(uploadedFile);
					}
					else if(item.isFormField()){
						//get the parameter from the form
						if(item.getFieldName().equals("from")){
							from = item.getString();
						}
						else if(item.getFieldName().equals("to")){
							to = item.getString();
						}
						else if(item.getFieldName().equals("subject")){
							subject = item.getString();
						}
						else if(item.getFieldName().equals("content")){
							content = item.getString();
						}
					}
				}
			}catch(Exception e){
				e.printStackTrace();
			}
		}
		//set mail info
		AttachmentSender sender = new AttachmentSender(host,accountName,password);
		sender.setFrom(from);
		sender.setSubject(subject);
		sender.setTo(to);
		sender.setContent(content);
		if(uploadedFile != null){
			String attachment = request.getRealPath(uploadedFile.getName());
			sender.addAttachMent(attachment);
		}
		
		//print the result
		if(sender.send()){
			out.print("Send mail successfully!");
		}
		else{
			out.print("Send mail failed!");
		}
		//delete the file on the server after send the mail
		if(uploadedFile.exists())
			uploadedFile.delete();
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
 

 

 

 

 

MyAuthenticator.java

{% highlight java %}
package beans;

import javax.mail.Authenticator;
import javax.mail.PasswordAuthentication;

public class MyAuthenticator extends Authenticator{
	String name;
	String password;
	
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
