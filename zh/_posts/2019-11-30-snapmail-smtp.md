---
layout: post
title: 怎么用Snapmail SMTP来发邮件?
lang: zh
tags: 
stickie: 
---

#### 你在什么情况需要使用Snapmail SMTP？
+ 你正在开发或者测试邮箱注册或者重置密码等功能，但是你没有SMTP服务器来发邮件。
              
#### SMTP Server
+ __Host__: mail.snapmail.cc
+ __端口__: 25   
+ 他是匿名的，不需要用户名和密码。
+ 你只能发邮件到@snapmail.cc
+ 请检查你的网络环境是否屏蔽了25端口。

#### 发送邮件的代码(Python, C#)

```python
 # Code in Python
 #!/usr/bin/python
 # -*- coding: UTF-8 -*-
 
 import smtplib
 from email.mime.text import MIMEText
 from email.mime.multipart import MIMEMultipart
 
 sender = 'jerry@snapmail.cc'
 receiver = 'tom@snapmail.cc'
 
 mail_message = MIMEMultipart('alternative')
 mail_message['From'] = 'jerry@snapmail.cc'
 mail_message['To'] = 'tom@snapmail.cc'
 mail_message['Subject'] = 'subject'
 
 text_content = 'text body'
 html_content = '<p>html body</p>'
 
 mail_message.attach(MIMEText(text_content, 'plain'))
 mail_message.attach(MIMEText(html_content, 'html'))
 
 try:
     smtp_client = smtplib.SMTP('mail.snapmail.cc', 25)
     smtp_client.sendmail(sender, receiver, mail_message.as_string())
 except smtplib.SMTPException:
     print("Fails to send email.")

```
    
```c#
 // Code in C#
 using System;
 using System.Net.Mail;
 using System.Net.Mime;
 
 namespace ConsoleApp2
 {
     class Program
     {
         static void Main(string[] args)
         {
             try
             {
                 MailMessage mailMsg = new MailMessage();
 
                 // To
                 mailMsg.To.Add(new MailAddress("tom@snapmail.cc"));
 
                 // From
                 mailMsg.From = new MailAddress("jerry@snapmail.cc");
 
                 // Subject and multipart/alternative Body
                 mailMsg.Subject = "subject";
                 string text = "text body";
                 string html = @"<p>html body</p>";
                 mailMsg.AlternateViews.Add(AlternateView.CreateAlternateViewFromString(text, null, MediaTypeNames.Text.Plain));
                 mailMsg.AlternateViews.Add(AlternateView.CreateAlternateViewFromString(html, null, MediaTypeNames.Text.Html));
 
                 // Init SmtpClient and send
                 SmtpClient smtpClient = new SmtpClient("mail.snapmail.cc", 25);
                 // It's anonymous, no username and password required.
                 // System.Net.NetworkCredential credentials = new System.Net.NetworkCredential("username@domain.com", "password");
                 // smtpClient.Credentials = credentials;
 
                 smtpClient.Send(mailMsg);
             }
             catch (Exception ex)
             {
                 Console.WriteLine(ex.Message);
             }
         }
     }
 }    
```

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体验Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> 返回到博客首页 </a>