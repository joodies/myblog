---
layout: post
title: 怎么用Snapmail SMTP来发邮件?
lang: zh
tags: 
stickie: 
---

#### 你在什么情况需要使用Snapmail SMTP？
+ 你正在测试邮箱注册或者重置密码等功能，但是你没有SMTP服务器来发邮件。
              
#### SMTP Server
+ __Host__: mail.snapmail.cc
+ __端口__: 25   
+ 他是匿名的，不需要用户名和密码。
+ 你只能发邮件到@snapmail.cc
+ 请检查你的网络环境是否屏蔽了25端口。

#### 使用c#来发送邮件的代码：  

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


<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体验Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> 返回到首页 </a>