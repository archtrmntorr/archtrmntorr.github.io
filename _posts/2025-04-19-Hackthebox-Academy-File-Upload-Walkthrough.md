---
title: "Hackthebox Academy File Upload Walkthrough"
description: "Questions walkthrough"
date:  2025-04-19
categories: [Walkthrough]
tags: [Hackthebox, CPTS, Active Directory, Enumeration, Questions, Answer, File-Upload, Attacks, CBBH] 
image :
    path : https://api.pcloud.com/getpubthumb?code=XZ3voY5Z4sRPGTKueMz79VUzUP1zBF1FduPk&linkpassword=undefined&size=1024x1024&crop=0&type=auto
---

## <span style="color: orange;"># Basic Exploitation</span>
------
> Absent Validation
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- Try to upload a PHP script that executes the (hostname) command on the back-end server, and submit the first word of it as the answer.</span><br>
<span style="color: pink;">Answer :-</span> ng-9##################################

- when we visit our exercise `http://<machine-ip>:port/`. 
- as the index page is usually hidden by default. But, if we try visiting http://SERVER_IP:PORT/index.php, we would get the same page, which means that this is indeed a PHP web application .
- Several other techniques may help identify the technologies running the web application, like using the Wappalyzer extension.

![Image](https://api.pcloud.com/getpubthumb?code=XZUioY5ZjpvGQq7uvRBnP49RUF3vl8aKxsCk&linkpassword=undefined&size=699x386&crop=0&type=auto)

- Now that we have identified the web framework running the web application and its programming language, we can test whether we can upload a file with the same extension. As an initial test to identify whether we can upload arbitrary PHP files and  it got uploaded . 
- which means that the web application has no file validation whatsoever on the back-end . 

![Image](https://api.pcloud.com/getpubthumb?code=XZkroY5ZAMlO6Oix9LJrDJRHSQBzPpt7L91X&linkpassword=undefined&size=692x383&crop=0&type=auto)

- after that when we click on the download button the web application will take us to our uploaded file , where we can execute commands , as we uploaded web-shell php script , and now we can grab `hostname` that is required to solve this challenge. 

![image](https://api.pcloud.com/getpubthumb?code=XZ7coY5ZVy27I6q5LGky4156GPECIBQvzVQk&linkpassword=undefined&size=1128x429&crop=0&type=auto)



> Upload Exploitation
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- Try to exploit the upload feature to upload a web shell and get the content of /flag.txt.</span><br>
<span style="color: pink;">Answer :-</span> HTB{g0##################}

- as previously dicussed we can upload php shell , it can be [reverse shell](https://github.com/pentestmonkey/php-reverse-shell) , [web shell](https://github.com/JohnTroony/php-webshells) etc...
- now we can search for the flag.txt file from the webshell. 

![Image](https://api.pcloud.com/getpubthumb?code=XZMcoY5ZaIKJr3sEmh5U4bV4AgPUw0uX49L7&linkpassword=undefined&size=1083x362&crop=0&type=auto)

- and then we can read the flag . 

![Image](https://api.pcloud.com/getpubthumb?code=XZxcoY5ZJJVmAEdINm7dB8TEYrqQop8dW8L7&linkpassword=undefined&size=1013x356&crop=0&type=auto)

## <span style="color: orange;"># Bypassing Filters</span>
------
> Client-Side Validation
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- Try to bypass the client-side file type validations in the above exercise, then upload a web shell to read /flag.txt (try both bypass methods for better practice).</span><br>
<span style="color: pink;">Answer :-</span> HTB{cl###########################}

- The exercise at the end of this section shows a basic Profile Image functionality, frequently seen in web applications that utilize user profile features, like social media web applications:


![Image](https://api.pcloud.com/getpubthumb?code=XZikym5ZTqmbwdANakhPHaXLgkUBmVj7NsKX&linkpassword=undefined&size=583x336&crop=0&type=auto)

- but  we can click [CTRL+SHIFT+C] to toggle the browser's Page Inspector, and then click on the profile image, which is where we trigger the file selector for the upload form . 

`<input type="file" name="uploadFile" id="uploadFile" onchange="checkFile(this)" accept=".jpg,.jpeg,.png">`

-  we see that the file input specifies (.jpg,.jpeg,.png) as the allowed file types within the file selection dialog. 

![Image](https://api.pcloud.com/getpubthumb?code=XZ5Xym5ZkqSRnU2aCx03fcL0sWuYkunEOYOk&linkpassword=undefined&size=1441x336&crop=0&type=auto)

- If we capture the upload request with Burp, we see the following request being sent by the web application:
- The web application appears to be sending a standard HTTP upload request to /upload.php. This way, we can now modify this request to meet our needs without having the front-end type validation restrictions. If the back-end server does not validate the uploaded file type, then we should theoretically be able to send any file type/content, and it would be uploaded to the serve
- The two important parts in the request are filename="HTB.png" and the file content at the end of the request. If we modify the filename to new.php and modify the content to the web shell we used in the previous section; we would be uploading a PHP web shell instead of an image.

![Image](https://api.pcloud.com/getpubthumb?code=XZDXym5ZVDVrgTUqLzJ3qTWBsFjwxHf25Qiy&linkpassword=undefined&size=1089x423&crop=0&type=auto)

> Note: We may also modify the Content-Type of the uploaded file, though this should not play an important role at this stage, so we'll keep it unmodified.

- As we can see, our upload request went through, and we got File successfully uploaded in the response. So, we may now visit our uploaded file and interact with it and gain remote code execution.
- now as previously done , we will search for the `flag.txt` file .

![Image](https://api.pcloud.com/getpubthumb?code=XZgXym5ZfX1Tnfbj4Qj2vOGKwhwS05BdqvjX&linkpassword=undefined&size=1060x402&crop=0&type=auto)

- and then we will get our required flag .

![Image](https://api.pcloud.com/getpubthumb?code=XZKXym5ZfwrtmK1rYzuS27qmIx8VUhb64qMy&linkpassword=undefined&size=1080x399&crop=0&type=auto)


> Blacklist Filters
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- Try to find an extension that is not blacklisted and can execute PHP code on the web server, and use it to read "/flag.txt".</span><br>
<span style="color: pink;">Answer :-</span> HTB{1_#######################}

- one of the client-side bypasses we learned in the previous section to upload a PHP script to the back-end server. We'll intercept an image upload request with Burp, replace the file content and filename with our PHP script's, and forward the request.

![Image](https://api.pcloud.com/getpubthumb?code=XZKXym5ZfwrtmK1rYzuS27qmIx8VUhb64qMy&linkpassword=undefined&size=1080x399&crop=0&type=auto)

- As we can see, our attack did not succeed this time, as we got Extension not allowed. This indicates that the web application may have some form of file type validation on the back-end, in addition to the front-end validations.

There are generally two common forms of validating a file extension on the back-end:<br>
    1. Testing against a blacklist of types<br>
    2. Testing against a whitelist of types<br>

- Furthermore, the validation may also check the file type or the file content for type matching. The weakest form of validation amongst these is testing the file extension against a blacklist of extension to determine whether the upload request should be blocked. 
- As the web application seems to be testing the file extension, our first step is to fuzz the upload functionality with a list of potential extensions and see which of them return the previous error message. Any upload requests that do not return an error message, return a different message, or succeed in uploading the file, may indicate an allowed file extension.
- There are many lists of extensions we can utilize in our fuzzing scan. PayloadsAllTheThings provides lists of extensions for [PHP](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) and `.NET` web applications. We may also use SecLists list of common [Web Extensions](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt).
- We may use any of the above lists for our fuzzing scan. As we are testing a PHP application, we will download and use the above [PHP](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) list. Then, from Burp History, we can locate our last request to /upload.php, right-click on it, and select Send to Intruder. From the Positions tab, we can Clear any automatically set positions, and then select the .php extension in filename="HTB.php" and click the Add button to add it as a fuzzing position:

![Image](https://api.pcloud.com/getpubthumb?code=XZJjym5ZNg1J775YMfJ9vHGGnfypxJ8Tqbnk&linkpassword=undefined&size=1390x440&crop=0&type=auto)

- as i see, i got so many 200 and file successfully uploaded 

![Image](https://api.pcloud.com/getpubthumb?code=XZSjym5ZLDQQGsW6zzHzo3xiVHDwuQ4JHe9k&linkpassword=undefined&size=1385x452&crop=0&type=auto)

- but when i am testing those file , none of them are working which even showinf 200 status code . 

![Image](https://api.pcloud.com/getpubthumb?code=XZwjym5ZUYSf2UVQLu0rThqsaO7ou4A2TWV7&linkpassword=undefined&size=791x461&crop=0&type=auto)

- so i decided to test them one by one , and i got hit . 

![Image](https://api.pcloud.com/getpubthumb?code=XZIjym5ZVapFvaeGXykQPYGGYEjD1LzqYre7&linkpassword=undefined&size=1400x461&crop=0&type=auto)

- and i got hit , and then i grab the flag from the webshell i uploaded . 

![Image](https://api.pcloud.com/getpubthumb?code=XZijym5Zaf261Y0NLd4sEXazlTIopucNiTX7&linkpassword=undefined&size=1100x411&crop=0&type=auto)

> Whitelist Filters
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- The above exercise employs a blacklist and a whitelist test to block unwanted extensions and only allow image extensions. Try to bypass both to upload a PHP script and execute code to read "/flag.txt".</span><br>
<span style="color: pink;">Answer :-</span> HTB{1_##############}

- Conjuction with Reverse Double Extension there another method of bypassing a whitelist validation test through Character Injection. We can inject several characters before or after the final extension to cause the web application to misinterpret the filename and execute the uploaded file as a PHP script.
- Each character has a specific use case that may trick the web application to misinterpret the file extension. For example, (shell.php%00.jpg) works with PHP servers with version 5.X or earlier, as it causes the PHP web server to end the file name after the (%00), and store it as (shell.php), while still passing the whitelist. The same may be used with web applications hosted on a Windows server by injecting a colon (:) before the allowed file extension (e.g. shell.aspx:.jpg), which should also write the file as (shell.aspx). Similarly, each of the other characters has a use case that may allow us to upload a PHP script while bypassing the type validation test.
- We can write a small bash script that generates all permutations of the file name, where the above characters would be injected before and after both the PHP and JPG extensions, as follows:


![Image](https://api.pcloud.com/getpubthumb?code=XZlfym5ZbhUrCP7ecdRRAnXoY1j7YyjXeywX&linkpassword=undefined&size=1318x438&crop=0&type=auto)

- now as previously discussed , we sent the post request to intruder and set all the like before and start the atack . 

![Image](https://api.pcloud.com/getpubthumb?code=XZ6fym5ZYfiFAdynTwf81ysAt63gffshv0JX&linkpassword=undefined&size=1393x493&crop=0&type=auto)

- and then we got hit ( we can filter out the result based on string , by going to setting(1) and then setting the string )

![Image](https://api.pcloud.com/getpubthumb?code=XZ3fym5ZjufgorJoDKB4RofGyOponRU21AtV&linkpassword=undefined&size=1420x520&crop=0&type=auto)

- now we can go the path where we have uploaded the file and get our flag .

![Image](https://api.pcloud.com/getpubthumb?code=XZCBym5ZiRF9JvSwzmhNfyJgaHjDoFV1vAcV&linkpassword=undefined&size=1003x158&crop=0&type=auto)


> Whitelist Filters
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- The above exercise employs a blacklist and a whitelist test to block unwanted extensions and only allow image extensions. Try to bypass both to upload a PHP script and execute code to read "/flag.txt".</span><br>
<span style="color: pink;">Answer :-</span> HTB{1_##############}





















.................Coming-Soon............................















































