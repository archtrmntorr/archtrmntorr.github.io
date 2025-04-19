---
title: "Hackthebox Active Directory Enumeration And Attacks Walkthrough"
description: "Questions walkthrough"
date:  2025-04-11
categories: [Walkthrough]
tags: [Hackthebox, CPTS, Active Directory, Enumeration, Questions, Answer] 
image :
    path : https://api.pcloud.com/getpubthumb?code=XZyIfY5ZUYn05P2n3KmB6DwoLhueluleMNOy&linkpassword=undefined&size=803x425&crop=0&type=auto
---



## <span style="color: orange;"># Initial Enumeration Section</span>
------
> External Recon and Enumeration Principles
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- While looking at inlanefreights public records; A flag can be seen. Find the flag and submit it. ( format == HTB{******} ).</span><br>
<span style="color: pink;">Answer :-</span> HTB{5Fz####################}

- as taught in module we will look for the public records on bgp.he.net

![image](https://api.pcloud.com/getpubthumb?code=XZe6fY5ZFQy70GGjxuz310VE6fRnEu1lrwGk&linkpassword=undefined&size=1439x602&crop=0&type=auto)

- and we can see flag in the output . 

> Intial Enumeration of the Domain
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- From your scans, what is the "commonName" of host 172.16.5.5 ?</span><br>
<span style="color: pink;">Answer :-</span> ACADEMY-##############################

- we will do the nmap scan and from the result we can filter out the `commonName` parameter for the flag .

```
nmap 172.16.5.5 -sC -sV --min-rate=1500
```
![Nmap Scan Result](https://api.pcloud.com/getpubthumb?code=XZkYBY5ZVx9EkllDx5fWdDkVypsNYBBh3a0y&linkpassword=undefined&size=783x399&crop=0&type=auto)


<span style="color: PaleGreen;">Question 2 :-  What host is running "Microsoft SQL Server 2019 15.00.2000.00"? (IP address, not Resolved name).</span><br>
<span style="color: pink;">Answer :-</span> 172.########

- again we will do the Nmap scan and save the result in host-enum file .


```
sudo nmap -v -A -oA -iL  host.txt -oN /home/htb-student/Documents/host-enum
```
- after that we can filter out string from the result using `grep`

![grep](https://api.pcloud.com/getpubthumb?code=XZmDBY5ZS3m9a2ggVaJvksjwxahvLS81OuGk&linkpassword=undefined&size=770x113&crop=0&type=auto)

- or we can use the pluma inbuilt text editor and search (CTRL+F) for keyword and then filter out the `ip` we get this for

![Pluma](https://api.pcloud.com/getpubthumb?code=XZ0eBY5Z6g89sxaVxJLJ6HHWOHSqkVeXXhG7&linkpassword=undefined&size=652x467&crop=0&type=auto)

## <span style="color: orange;"># Sniffing out a Foothold</span>

> LLMNR/NBT-NS Poisoning - from Linux
{: .prompt-info }

<span style="color: PaleGreen;">Question 1 :- Run Responder and obtain a hash for a user account that starts with the letter b. Submit the account name as your answer.</span><br>
<span style="color: pink;">Answer :-</span>

<span style="color: PaleGreen;">Question 2 :- Crack the hash for the previous account and submit the cleartext password as your answer.</span><br>
<span style="color: pink;">Answer :-</span> 

<span style="color: PaleGreen;">Question 3 :- Run Responder and obtain an NTLMv2 hash for the user wley. Crack the hash using Hashcat and submit the user's password as your answer.</span><br>
<span style="color: pink;">Answer :-</span>


.................................Coming-Soon...........................