---
layout: post
title: Snapmail是怎么工作的?
lang: zh
tags: 
stickie: true
---

## Snapmail让这4种任务变的如此简单:

#### 1. Register anonymously on websites using Snapmail email address.
![Register anonymously]({{site.hosturl}}/assets/post_resource/snapmail.gif)

#### 2. A single inbox is enough for all with wildcard inbox.
Add an email address support prefix. 
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email1.jpg)

We can receive all the emails when the address start with the prefix.
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email2.jpg)

You can always get a new email address when you click [Copy], so you don't have to worry about using duplicated email address.

![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email3.jpg)
#### 3. Send email with Snapmail SMTP server. 
<a target='_blank' href="https://blog.snapmail.cc/2019/11/30/snapmail-smtp.html">See more details ></a>

Sending email using builtin C# libraries:    

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

#### 4. Automate user registration with Snapmail API.

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> Try Snapmail now</a>

<a href="https://blog.snapmail.cc"><i class="fa fa-arrow-circle-left"></i> Back to Snapmail blog</a>