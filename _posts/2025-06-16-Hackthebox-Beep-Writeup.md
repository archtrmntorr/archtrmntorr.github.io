---
title: "Hackthebox Beep Walkthrough"
description: "Hackthebox Beep Walkthrough"
date: 2025-06-15
categories: [Walkthrough]
tags: [hackthebox,Beep,Walkthrough,Linux]
image :
    path : https://lh3.googleusercontent.com/d/1fM4HJUuPoxlk9_1-Y2ULJu0V76BNN1LP
---

Beep has a very large list of running services, which can make it a bit challenging to find the correct entry method. This machine can be overwhelming for some as there are many potential attack vectors. Luckily, there are several methods available for gaining access.

<img src="https://lh3.googleusercontent.com/d/1lswbGzcUfVpaPp1q_o3pthGHFm_Cnux2" alt="">

- Let's spawn the machine

<img src="https://lh3.googleusercontent.com/d/1IJlvZtyeHGrN2EkMbDC0i5maLbfB0zDo" alt="">

## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
---------

- Let's start with the Nmap scan 

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/Beep]
└─$ nmap -sC -sV -p-  10.10.10.7  --min-rate=1500    
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 20:35 EDT
Stats: 0:01:52 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 93.75% done; ETC: 20:37 (0:00:04 remaining)
Nmap scan report for 10.10.10.7
Host is up (0.27s latency).
Not shown: 65519 closed tcp ports (reset)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 4.3 (protocol 2.0)
| ssh-hostkey: 
|   1024 ad:ee:5a:bb:69:37:fb:27:af:b8:30:72:a0:f9:6f:53 (DSA)
|_  2048 bc:c6:73:59:13:a1:8a:4b:55:07:50:f6:65:1d:6d:0d (RSA)
25/tcp    open  smtp       Postfix smtpd
|_smtp-commands: beep.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN
80/tcp    open  http       Apache httpd 2.2.3
|_http-title: Did not follow redirect to https://10.10.10.7/
|_http-server-header: Apache/2.2.3 (CentOS)
110/tcp   open  pop3       Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_pop3-capabilities: IMPLEMENTATION(Cyrus POP3 server v2) EXPIRE(NEVER) PIPELINING TOP APOP RESP-CODES USER AUTH-RESP-CODE STLS UIDL LOGIN-DELAY(0)
111/tcp   open  rpcbind    2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1            790/udp   status
|_  100024  1            793/tcp   status
143/tcp   open  imap       Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_imap-capabilities: MULTIAPPEND ID OK X-NETSCAPE MAILBOX-REFERRALS LISTEXT URLAUTHA0001 ATOMIC ANNOTATEMORE CHILDREN NO ACL BINARY STARTTLS QUOTA CONDSTORE THREAD=REFERENCES LIST-SUBSCRIBED SORT UNSELECT LITERAL+ IMAP4rev1 SORT=MODSEQ IDLE Completed THREAD=ORDEREDSUBJECT CATENATE NAMESPACE RENAME UIDPLUS IMAP4 RIGHTS=kxte
443/tcp   open  ssl/http   Apache httpd 2.2.3 ((CentOS))
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2017-04-07T08:22:08
|_Not valid after:  2018-04-07T08:22:08
|_http-title: Elastix - Login page
|_ssl-date: 2025-06-09T00:39:54+00:00; -1s from scanner time.
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Apache/2.2.3 (CentOS)
793/tcp   open  status     1 (RPC #100024)
993/tcp   open  ssl/imap   Cyrus imapd
|_imap-capabilities: CAPABILITY
995/tcp   open  pop3       Cyrus pop3d
3306/tcp  open  mysql      MySQL (unauthorized)
4190/tcp  open  sieve      Cyrus timsieved 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4 (included w/cyrus imap)
4445/tcp  open  upnotifyp?
4559/tcp  open  hylafax    HylaFAX 4.3.10
5038/tcp  open  asterisk   Asterisk Call Manager 1.1
10000/tcp open  http       MiniServ 1.570 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Service Info: Hosts:  beep.localdomain, 127.0.0.1, example.com, localhost; OS: Unix
```

- there we so many things to check and every port have something different to check , so we go with standard way with first port 443
- and we first visit this page we got the tls version error 

<img src="https://lh3.googleusercontent.com/d/1iaydSVFZzS6nr31UFafbL_ayjNQRCqRu" alt="">

- which we can fix by chagin the tls support version in the web-browseer's config .. here how we can do it ....

<img src="https://lh3.googleusercontent.com/d/1I5E4Qq0CC65aDwyJDdxZ-sB5OmtuqY2F" alt="">

- here how it looks 

<img src="https://lh3.googleusercontent.com/d/15xxNpo7V4VcmNN7mBRYjGeIiNASuYhDY" alt="">

- and after fixing this issue we got `elastix` page 

<img src="https://lh3.googleusercontent.com/d/1ay7b1QbJF42Rk1eho6wT0vl62W-ZurOu" alt="">

## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 
--------

- and after googling the version of the software and related vulnerability and we found this version is affected LFI vulnerabilty.... 

<img src="https://lh3.googleusercontent.com/d/1yzOalsECd0g1M3S7ht2jnJoFzeJdx_SJ" alt="">

- here is the vulnerable endpoint 

<img src="https://lh3.googleusercontent.com/d/1sZxKkSH0u364_jJ-LCNmuF1lpprkej-B" alt="">

- now we can grab the file important information from like configuration file 

<img src="https://lh3.googleusercontent.com/d/1eyD0YaAYy3kbyduTX8xZqBA10mC1jCL8" alt="">

- and  in `/etc/amportal.conf` ( found this file from the exploit-db code base ) file we got some username and passwords .... 

<img src="https://lh3.googleusercontent.com/d/1XUSTU5WhD6iNWsBrEJNbFVjIoiRnZPoN" alt="">

- now we tried to get the content of the `/etc/passwd` file , to find the username on the system and other information .... 

<img src="https://lh3.googleusercontent.com/d/1o-WIG_-f-1wqTfsoK8rBP7PzWxd1Apek" alt="">

- after little tingling around , found that we can use the credential from the configuration file to login through ssh ...
- and we got shell as root we can grab both user and root flag collectively without privilege escalation .... 

<img src="https://lh3.googleusercontent.com/d/17YI1KorX7dyMZVVU_7qWTBhV50SSNQ8B" alt="">


<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor)<br>