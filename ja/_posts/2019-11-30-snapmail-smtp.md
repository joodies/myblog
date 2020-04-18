---
layout: post
title: どうやってSnapmail SMTPでメールを送りますか？
lang: ja
tags: 
stickie: 
---

#### どんな状況でSnapmail SMTPを使う必要がありますか？
+ メールアドレスの登録やパスワードのリセットなどの機能をテストしていますが、SMTPサーバがないのでメールを送ります。
              
#### SMTP Server
+ __Host__: mail.snapmail.cc
+ __ポート__: 25   
+ 彼は匿名です。ユーザ名とパスワードは必要ありません。
+ @snapmail.ccまでしかメールできません。
+ あなたのネットワーク環境が25ポートをブロックしているか確認してください。

#### C#を使ってメールのコードを送信します。    

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


<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体験Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> トップページに戻る </a>