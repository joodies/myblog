---
layout: post
title: Snapmailはどうやって働いていますか？
lang: ja
tags: 
stickie: true
---

## Snapmailはこの4つのタスクをこのように簡単にします。

#### 1. 臨時メールでウェブサイトを登録して、個人の真実なメールボックスを保護します。
![Register anonymously]({{site.hosturl}}/assets/post_resource/snapmail.gif)

#### 2. 多くの臨時メールが必要な場合は、プレフィックスメールを使うことをおすすめします。プレフィックスメールで十分です。何回か追加する必要はありません。
プレフィックスメールボックスを追加します。
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email1.jpg)

メールアドレスがこのプレフィックスであれば、メールが届きます。
![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email2.jpg)

あなたはいつも新しいメールアドレスを得ることができます。コピーをクリックすると、この住所はすでに使われているかどうか心配しなくてもいいです。

![Register anonymously]({{site.hosturl}}/assets/post_resource/how_it_works/prefix_email3.jpg)
#### 3. Snapmail SMTPを使ってメールします。
<a target='_blank' href="https://www.snapmail.cc/blog/ja/2019/11/30/snapmail-smtp.html">もっと見る> ></a>

C#を使ってメールのコードを送信します。    

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

#### 4. 自動化テストではSnapmail APIを使用します。
ウェブサイトの自動化テストに関しては、ユーザー登録の自動化のケースをカバーする必要があります。
Snapmail APIを使用すると、すぐに操作できます。
<a target='_blank' href="https://www.snapmail.cc/blog/ja/2020/01/05/automation-test.html">もっと見る> ></a>


<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体験Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> トップページに戻る </a>