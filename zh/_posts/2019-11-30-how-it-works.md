---
layout: post
title: Snapmail是怎么工作的?
lang: zh
tags: 
stickie: true
---

## Snapmail让这4种任务变的如此简单：

#### 1. 使用临时邮箱注册网站，保护个人真实邮箱。
![Register anonymously]({{site.hosturl}}/assets/post_resource/snapmail.gif)

#### 2. 当你需要很多个临时邮箱的时候，推荐使用前缀邮箱，一个前缀邮箱就够了，不需要添加多次。
添加一个前缀邮箱。 
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email1.jpg)

只要邮箱地址是这个前缀，你就能收到邮件了。
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email2.jpg)

你总是可以得到一个新的邮箱地址，当你点击[复制]的时候，所以你不用担心这个地址是不是已经使用过了。

![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email3.jpg)
#### 3. 使用Snapmail SMTP来发邮件。
<a target='_blank' href="https://www.snapmail.cc/blog/zh/2019/11/30/snapmail-smtp.html">查看更多 ></a>

使用C#来发送邮件的代码：    

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

#### 4. 在自动化测试中使用Snapmail API。
当涉及到网站的自动化测试时，您将需要解决使用户注册自动化的问题。
借助Snapmail API，您可以轻松完成它。 
<a target='_blank' href="https://www.snapmail.cc/blog/zh/2020/01/05/automation-test.html">查看更多> ></a>

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体验Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> 返回到首页 </a>