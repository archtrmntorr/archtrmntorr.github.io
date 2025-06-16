---
title: "Hackthebox Optimum Walkthrough"
description: "Hackthebox Optimum Walkthrough"
date: 2025-06-16
categories: [Walkthrough]
tags: [hackthebox,Optimum,Walkthrough,Windows]
image :
    path : https://lh3.googleusercontent.com/d/1jiXgiW5V1MXjO3dO2ZTTs6H07fs9unig
---

Optimum is a beginner-level machine which mainly focuses on enumeration of services with known exploits. Both exploits are easy to obtain and have associated Metasploit modules, making this machine fairly simple to complete.

<img src="https://lh3.googleusercontent.com/d/1-SAWujdWpLROhangh04VyYuo9jPFNEKR" alt="">

- Let's Spawn the machine.....

<img src="https://lh3.googleusercontent.com/d/1bwb5aDVwes3IpBtXk6Dywg7e7d08GcWh" alt="">


## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
---------

- Let's start with the Nmap scan 


```bash
┌──(kali㉿kali)-[~/Desktop/HTB/Optimum]
└─$ nmap -sC -sV -p- 10.10.10.8 --min-rate=1500 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 21:44 EDT
Nmap scan report for 10.10.10.8
Host is up (0.30s latency).
Not shown: 65534 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    HttpFileServer httpd 2.3
|_http-title: HFS /
|_http-server-header: HFS 2.3
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 146.36 seconds
```
- on `port 80` there a HFS server running on ....

<img src="https://lh3.googleusercontent.com/d/1Cti7UYKK1GyU6kX_lY-z-dDfW2z4Qvm6" alt="">

## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 
--------

- and bottom of the page i found the version to be 2.3 and so i googled it and found remote code execution vulnerability ... 

<img src="https://lh3.googleusercontent.com/d/1uGEaJ9hXKDUnsghQUz57Butwa6riGq-h" alt="">

- and there is a measploit module for this vulnerability in rejeeto-hfs `windows/http/rejetto_hfs-exec`
- we will use this module and set the required options and after running the module we will get the shell as `OPTIMUM\kostas`

<img src="https://lh3.googleusercontent.com/d/1iufRgvl-SCw1JylEqdjktfr2qAmdbMwA" alt="">

- let's grab the system information and its a `Windows server 2012` and we have a session as `x86` 

```bash
meterpreter > sysinfo
Computer        : OPTIMUM
OS              : Windows Server 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : el_GR
Domain          : HTB
Logged On Users : 2
Meterpreter     : x86/windows
meterpreter > 
```

- before privilege esclation we can now  grab the `user` flag ....

<img src="https://lh3.googleusercontent.com/d/1QcnSCQzpWErtz7prKtTpcKxwRE5A_9E2" alt="">

- as we see we don't have higher privileges and we got access denied , let's escalate our privielges...

<img src="https://lh3.googleusercontent.com/d/1Y3d2CZbm2Y51ifpmWEMMyKWHGSV-8h_C" alt="">

- let's check for the process running as x64 and we will migrate to that process .... 


<img src="https://lh3.googleusercontent.com/d/1rxaKihIa5bQtamcbgjxjCJR8-FbQSio-" alt="">

<img src="https://lh3.googleusercontent.com/d/1clw2B3KUIrGtYFtOdIgi5tNPK1Hlr9ti" alt="">

- now we will background the current meterpreter sesison (CTRL+Z) and use the `local_exploit_suggester` module to look for privilege escalation vectors ... 

<img src="https://lh3.googleusercontent.com/d/1SvYDPp4Xd22GzDi3br2VRSK9TKFNSfmF" alt="">


<img src="https://lh3.googleusercontent.com/d/1qHwlDQX2gZYdcH3fE6CbotIAQEAyw_cT" alt="">

- after looking at the results we will use the `windows/local/cve_2019_1458_wizardopium` module to escalate our privleges ... 
> we can also use the other identified methods or manul methods as i am going with this method

- let's configure the required options for this module ( which can be seen by using `show options` command) and run the module ...
- after exploitation , we will get the shell as `NT AUTHORITY\SYSTEM`
- as we got the shell as highest privilege possible in windows ( similar to root in linux ) now we can grab the user and root flag....

<img src="https://lh3.googleusercontent.com/d/1OJOmqx6RtKekPHOeW5OPddqk5hqvYdzd" alt="">


<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor)<br>















