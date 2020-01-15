---
layout: post
title: Automate user registration with Snapmail API
lang: en
tags: 
stickie: 
---

## Use Snapmail API in automation test

When it comes to automation test for website, you will need to cover the case of automating user registration.
With Snapmail API, you can get it done at your fingertips. 

#### Example steps in web UI automation test

+ Register a new user in your website with any Snapmail email address you want, take __richard@snapmail.cc__ for example.

+ Then your email service would send account validation email to richard@snapmail.cc
    
    You can use the SMTP service of Snapmail's if you don't have one. <a target='_blank' href="https://www.snapmail.cc/blog/en/2019/11/30/snapmail-smtp.html">See more details ></a> 

+ It's time to use Snapmail API to get the validation email, talk is cheap, show me the code.
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

+ You can use any email address you want, it's recommended to use prefix email address for such scenario, no need to add it on Snapmail.cc

<a target="_blank" href="https://www.snapmail.cc"><i class="fa fa-envelope a"></i> Try Snapmail now</a>

<a href="https://www.snapmail.cc/blog/"><i class="fa fa-arrow-circle-left"></i> Back to Snapmail blog</a>