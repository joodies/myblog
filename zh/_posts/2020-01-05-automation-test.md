---
layout: post
title: 使用Snapmail API自动注册用户
lang: zh
tags: 
stickie: 
---

## 在自动化测试中使用Snapmail API

当涉及到网站的自动化测试时，您将需要解决使用户注册自动化的问题。
借助Snapmail API，您可以轻松完成它。 

#### Web UI自动化测试中的示例步骤

+ 使用您想要的任何Snapmail电子邮件地址在您的网站中注册一个新用户，例如**richard@snapmail.cc**。

+ 然后，您的电子邮件服务会将帐户验证电子邮件发送到richard@snapmail.cc
    
    你可以使用Snapmail SMTP来发邮件，如果你没有SMTP服务。 <a target='_blank' href="https://www.snapmail.cc/blog/zh/2019/11/30/snapmail-smtp.html">查看详情 ></a> 

+ 现在是时候使用Snapmail API来获取验证邮件了，空谈无用，给我看看代码。
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

+ 您可以使用任何想要的电子邮件地址，建议在这种情况下使用前缀电子邮件地址，无需将其添加到Snapmail.cc

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> 体验Snapmail </a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> 返回到首页 </a>