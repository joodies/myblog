---
layout: post
title: Snapmail APIでのユーザー登録を自動化する
lang: ja
tags: 
stickie: 
---

## 自動化テストでSnapmail APIを使用する

ウェブサイトの自動化テストに関しては、ユーザー登録の自動化のケースをカバーする必要があります。
Snapmail APIを使用すると、すぐに操作できます。

#### Web UI自動化テストの手順例

+ 希望するSnapmail電子メールアドレスを使用してWebサイトに新しいユーザーを登録します。たとえば、**richard@snapmail.cc**を使用します。

+ 次に、メールサービスはアカウント検証メールをrichard@snapmail.ccに送信します
    
    SMTPサービスがない場合は、Snapmail SMTPを使ってメールを送ることができます。 <a target='_blank' href="https://www.snapmail.cc/blog/ja/2019/11/30/snapmail-smtp.html">詳細を見る ></a> 

+ Snapmail APIを使用して検証メールを取得するときです。話は安上がりです。コードを見せてください (Python, C#)。
    ```python
    # Code in Python
    import time
    import requests
    import json
    import re
    
    
    def get_verification_code(email):
        # We want to get account validation code in email
        validation_code = None
        # We will retry the request every 6 seconds to get the email
        for i in range(50):
            # Get emails from an email box
            req = requests.get('https://snapmail.cc/emaillist/' + email)
            if req.status_code == 200:
                # Get email text of the first email,
                # take "This is a test email." for example,
                # email_text = "This is a test email."
                email_text = json.loads(req.text)[0]['text']
                # Use regex to get the validation code, we'll get "test" here.
                # validation_code = "test"
                validation_code = re.search(r'This is a ([a-zA-Z0-9]{4}) email', email_text)
                break
    
            print("Waiting for next retry")
            time.sleep(6)
        if validation_code:
            print('validation_code:' + validation_code.group(1))
            return validation_code.group(1)
    
    
    get_verification_code('richard@snapmail.cc')
    ```

    ```c#  
    // Code in C#
    using Newtonsoft.Json.Linq;
    using System;
    using System.Net.Http;
    using System.Text.RegularExpressions;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace ConsoleApp1
    {
        class Program
        {
            static HttpClient client = new HttpClient();
            static void Main(string[] args)
            {
                Task t = new Task(GetValidationCode);
                t.Start();
                Console.ReadLine();
            }
    
            private static async void GetValidationCode()
            {
                // Get emails from an email box
                var url = "https://www.snapmail.cc/emaillist/richard@snapmail.cc";
                // We want to get account validation code in email
                var validation_code = "";
    
                // We will retry the request every 6 seconds to get the email
                for (int i = 0; i < 50; i++)
                {
                    HttpResponseMessage response = await client.GetAsync(url);
                    // If the response status is 200
                    if (response.StatusCode == System.Net.HttpStatusCode.OK)
                    {
                        var result = await response.Content.ReadAsStringAsync();
                        // Get the emails in array format
                        var emails = JArray.Parse(result);
                        // Get the email text of first email, 
                        // take "This is a test email." for example.
                        // email_text = "This is a test email."
                        var email_text = emails[0]["text"].ToString();
                        Regex regex = new Regex("This is a ([a-zA-Z0-9]{4}) email");
                        Match match = regex.Match(email_text);
                        // Use regex to get the validation code, we'll get "test" here.
                        // validation_code = "test"
                        validation_code = match.Groups[1].Value; 
                        Console.WriteLine(validation_code);
                        break;
                    }
                    Console.WriteLine("Waiting for next retry");
                    // Sleep 6 seconds
                    Thread.Sleep(6000);
                }
    
            }
        }
    }
    ```

+ 任意の電子メールアドレスを使用できます。このようなシナリオではプレフィックスの電子メールアドレスを使用することをお勧めします。Snapmail.ccに追加する必要はありません

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体験Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> トップページに戻る </a>