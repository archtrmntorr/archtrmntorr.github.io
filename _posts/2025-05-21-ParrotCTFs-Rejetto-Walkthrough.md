---
title: "ParrotCTFs Rejetto Walkthrough"
description: "ParrotCTFs machine walkthrough"
date: 2025-05-21
categories: [Walkthrough]
tags: [ParrotCTFs,Rejetto, CVE-2024-23692, Rejetto-HFS]
image :
    path : https://s3.parrot-ctfs.com/68233b2b668972.86622474.png
---


Another Linux Machine Rejetto from ParrotCTFs , this room is release arena you can also try it if you want , Here : [Machine Link](https://parrot-ctfs.com/dashboard/release-arena?boxID=4554573733) , its very straight forward solution to rooting the machine . This machine is targeting the personal, starting in cybersecurity. 


| Name        | Rejetto                                                                                                                                                                                                                                                                                                        |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Difficulty  | Easy                                                                                                                                                                                                                                                                                                           |
| Category    | Server Side Template Injection                                                                                                                                                                                                                                                                                 |
| Description | A classic file-sharing service hums along, offering simple access to a few public resources. It looks stable, even nostalgic, perhaps a relic from another era. But age often brings oversight. Explore its behavior, peek into its features, and you might just find something that wasn’t meant to be shared |


So let's start: 

- First we can connect with the vpn 

![Image](https://api.pcloud.com/getpubthumb?code=XZXmNh5ZN6q4tAIjoEub7iYUyjVuuhESUXVX&linkpassword=&size=271x157&crop=0&type=auto)

- now we can start the machine ( if machine not seems to work , do reset and try again ) 

![Image](https://api.pcloud.com/getpubthumb?code=XZVYNh5Z5DsCwOIeyrhjlKx1FWJsgQ7bip6V&linkpassword=&size=568x391&crop=0&type=auto)

### <span style="color: DarkSalmon;"><b># Enumeration</b></span>

- let's first start with the basic nmap scan ...

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ nmap -sC -sV -p- 10.73.0.27 --min-rate=1500
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-21 06:28 EDT
Nmap scan report for 10.73.0.27
Host is up (0.20s latency).
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          HttpFileServer httpd 2.3m
|_http-server-header: HFS 2.3m
|_http-title: HFS /
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
5040/tcp  open  unknown
5357/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
7680/tcp  open  pando-pub?
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: REJETTO-HTTP-FI, NetBIOS user: <unknown>, NetBIOS MAC: bc:24:11:d5:52:0b (Proxmox Server Solutions GmbH)
| smb2-time: 
|   date: 2025-05-13T11:28:53
|_  start_date: N/A
|_clock-skew: -7d23h03m14s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 224.53 seconds
```

- i find out the at port 80 , `HttpFileServer(HFS) httpd 2.3` service is running and other ports are open too . 
- i search this service name on the google and find out that version is vulnerable and assigned a cve ( CVE-2024-23692 ) . 

![Image](https://api.pcloud.com/getpubthumb?code=XZ6SNh5Zd17F7iEdYgjVKGsXNGtudHqRCTw7&linkpassword=&size=1164x702&crop=0&type=auto)

- then i look in to the metasploit for module to this vulnerability . 
- and i find out one . 

![Image](https://api.pcloud.com/getpubthumb?code=XZTSNh5ZpXP2P30BgyfPzsQtSYuULH7U9GQk&linkpassword=&size=1328x180&crop=0&type=auto)

### <span style="color: DarkSalmon;"><b># Exploitation</b></span>

- let's select the module and see the options for this module , which have to be set before running this . 

![Image](https://api.pcloud.com/getpubthumb?code=XZ7jNh5ZRWL4LRza6VkEDSgcOEIxT8bIRycX&linkpassword=&size=1211x628&crop=0&type=auto)

- now i had setup all the options required for this module , let's exploit it ( we can also check before running this , that it is indeed vulnerable or not and it is bytheway)

![Image](https://api.pcloud.com/getpubthumb?code=XZQjNh5ZsvMRFiiiXz71Gh2f0MGnbSMUsDMk&linkpassword=&size=891x238&crop=0&type=auto)

- now i have shell of Administrator ( you can check `help` menu for command in metasploit )

![Image](https://api.pcloud.com/getpubthumb?code=XZPjNh5Z5CB6hyBs3tHXhBvBRxNJvYh6aof7&linkpassword=&size=417x58&crop=0&type=auto)

- as i get shell as administrator , now we can access both user flag and root flag without privilege Escalation .
- we can also get the native cmd shell using `shell` command in metasploit.
- i also note that `jady` is the user for this machine, from his desktop folder we have to extract the user flag . 

![Image](https://api.pcloud.com/getpubthumb?code=XZrjNh5ZxzzVCoAV8jbbrD2naW522urpUFf7&linkpassword=&size=655x438&crop=0&type=auto)

- root flag .

![Image](https://api.pcloud.com/getpubthumb?code=XZJuNh5ZyFDSnkBHB3RRxkVvUiVdRu6IJGuy&linkpassword=&size=657x299&crop=0&type=auto)

<br>

### <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>

I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor) <br>
[Machine Platform](https://parrot-ctfs.com/dashboard) <br>



