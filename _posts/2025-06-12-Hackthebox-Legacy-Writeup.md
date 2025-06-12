---
title: "Hackthebox Legacy Walkthrough"
description: "Hackthebox Machine Walkthrough"
date: 2025-06-12
categories: [Walkthrough]
tags: [hackthebox,Legacy, Walkthrough,windows]
image :
    path : https://api.pcloud.com/getpubthumb?code=XZKMUj5ZLLLRprH2PMmwtWb0EuUCpXy10A2V&linkpassword=&size=1116x717&crop=0&type=auto
---

Legacy is a fairly straightforward beginner-level machine which demonstrates the potential security risks of SMB on Windows. Only one publicly available exploit is required to obtain administrator access.


![Image1](https://api.pcloud.com/getpubthumb?code=XZLTUj5Z0KxOaglp0L7fEMpDMUF1fph1BD5X&linkpassword=&size=1562x183&crop=0&type=auto)

- Let's Spawn the Machine

![Image2](https://api.pcloud.com/getpubthumb?code=XZbTUj5Z3g7L99G5M70Aye4rkDKeGQqFrU9y&linkpassword=&size=1539x226&crop=0&type=auto)


## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
---------
- Let's Start with nmap scan 

```bash
┌──(kali㉿kali)-[~/Desktop/HTB/legacy]
└─$ nmap -sC -sV -p- 10.10.10.4 --min-rate=1500 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-08 06:01 EDT
Nmap scan report for 10.10.10.4
Host is up (0.27s latency).
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE      VERSION
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows XP microsoft-ds
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)
| smb-os-discovery: 
|   OS: Windows XP (Windows 2000 LAN Manager)
|   OS CPE: cpe:/o:microsoft:windows_xp::-
|   Computer name: legacy
|   NetBIOS computer name: LEGACY\x00
|   Workgroup: HTB\x00
|_  System time: 2025-06-13T15:00:26+03:00
|_nbstat: NetBIOS name: LEGACY, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b0:7e:61 (VMware)
|_clock-skew: mean: 5d00h27m39s, deviation: 2h07m16s, median: 4d22h57m39s
```

it shows SMB ports Microsoft Windows RPC (TCP 135), along with SMB/NetBIOS ports (TCP 139, 445), suggesting the system is running Windows and Windows XP.

- on quick google search about the identified system-os , i had found a related vulnerability ( MS08-067 )

![Image3](https://api.pcloud.com/getpubthumb?code=XZ9TUj5Z6bdnStfHA0FCnBrOPgIh4Qr1FmnX&linkpassword=&size=1772x474&crop=0&type=auto)

## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 
--------
- next we will sarch for the metasploit module , for this vulnerability and found one !!

![Image4](https://api.pcloud.com/getpubthumb?code=XZdTUj5Zr1INlfI3qq4CuSEOiE2GvLUK41wV&linkpassword=&size=1417x166&crop=0&type=auto)

- let's use and set the options ( like rhots , lhost , lport) etc ...
- on exploit we got shell as `NT AUTHORITY\SYSTEM` , which means we have highest level permission on the system . 

![Image](https://api.pcloud.com/getpubthumb?code=XZiTUj5Z8yr6xHNlNAu0wd0tTtQXv4Vg3gE7&linkpassword=&size=911x200&crop=0&type=auto)

- now we can grab drop a shell through meterpreter session and then we can get the user flag .

![Image5](https://api.pcloud.com/getpubthumb?code=XZmgUj5ZcEHnzJt5G6HTy9VPedYI60sRS59k&linkpassword=&size=536x167&crop=0&type=auto)

- similarly we can now get the root flag .... 

![Image6](https://api.pcloud.com/getpubthumb?code=XZTgUj5ZPH6BKl60S908OMGGILXCR7qfV4oV&linkpassword=&size=567x242&crop=0&type=auto)

> we can aslo go the manual way if we want ... 


<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor) <br>