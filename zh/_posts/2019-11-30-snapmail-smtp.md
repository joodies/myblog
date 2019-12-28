---
layout: post
title: How to send an email with Snapmail SMTP?
lang: zh
tags: 
stickie: 
---

#### When do you need this service?
+ You are testing user-registration or password-reset email, and you don't have an SMTP
              server to send email.
              
#### SMTP Server
+ __Host__: mail.snapmail.cc
+ __Port__: 25   
+ It's anonymous, no username and password required.
+ You can only send email to snapmail email address with snapmail SMTP service.
+ Please check port 25 is not blocked in your network.

#### Sending email using builtin C# libraries    

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


<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> Try Snapmail now</a>

<a href="https://blog.snapmail.cc"><i class="fa fa-arrow-circle-left"></i> Back to Snapmail blog</a>