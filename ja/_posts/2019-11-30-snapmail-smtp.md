---
layout: post
title: どうやってSnapmail SMTPでメールを送りますか？
lang: ja
tags: 
stickie: 
---

#### どんな状況でSnapmail SMTPを使う必要がありますか？
+ あなたは、ユーザー登録やパスワードをリセットメールを開発したりテストしているし、メールを送信するSMTPサーバーを持っていない。
              
#### SMTP Server
+ __Host__: mail.snapmail.cc
+ __ポート__: 25   
+ 彼は匿名です。ユーザ名とパスワードは必要ありません。
+ @snapmail.ccまでしかメールできません。
+ あなたのネットワーク環境が25ポートをブロックしているか確認してください。

#### メールを送るコード (Python, C#)    

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

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体験Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> トップページに戻る </a>