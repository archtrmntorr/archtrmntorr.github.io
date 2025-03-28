---
title: "AV Evasion & Obfuscation For Beginners"
description: "Learn AV Evasion with Shellter and Obfuscation using Invoke-Obfuscation"
date: 2024-09-16
categories: [Blog]
tags: [AV Evasion, Code Obfuscation]
image:
    path: assets\img\AV Evasion & Obfuscation For Beginners 28-03-2025 at 18-12-31.jpeg
---


> As this is a Very Beginner level Post and i keep things simple and publish more blogs about explaining techniques , about  how to do that .
{: .prompt-tip }

#### # What is AV Evasion in First Place ? 

+ Antivirus Evasion (AV) or you can say  Defense Evasion consists of techniques that adversaries use to avoid detection throughout their compromise . Techniques used for defense evasion include uninstalling/disabling security software or obfuscating/encrypting data and scripts . Adversaries also leverage and abuse trusted processes to hide and masquerade their malware . - MITRE 


## Antivirus Detection Methods


#### # How AV detect these payloads or malicious file in first place ?

AV software will utilize many techniques to indetify potential harmful file or a piece of code , some of them explained here :

***1. Signature Based Detection*** : An AV signature is a unique sequence of bytes that uniquely identifies malware . As a result , you will have to ensure that your obfuscated exploit or payload doesn't match any known signature in the AV database 

+ **Wondering what is signature is first place**, it refers to a special pattern or code that antivirus software employs to identify malware or viruses that are known to exist. Antivirus software providers compile a database of these "signatures" in order to recognize and prevent viruses and other malware. Each virus or piece of malware has unique code or behavior. For example , when we download a file and we calculate md5 hash for that file `md5sum <filename>` and we match that hash which was on the website it , by mathching those 2 md5 hashes means that a file or a piece of data has not been altered or curropted , during tranmission of the file  , if single piece of data is missing then , _md5hash_ does not match , this is somewhat how signatures are created . ( we discuss in detail in later post )


> We can bypass signature-based detectino by modifying the malware's byte sequence , therefore changing the signature .
{: .prompt-info }



***2. Heuristic-based detection*** : Relies on rules or decision to determine whether a binary is malicious or not . It also looks for specific patterns within the code or program calls . 

+ **What i mean by set of rules** , consider them as are like guidelines that you have learned over time about what bad files ( malware ) usually try to do . For example , hmmm Good programs usually don't try to delete important system files or Malware often tried to hide itself or spread by copying itself to many places on the computer . 

+ **And what is mean Looking at patterns** is , AV software examine the programs internal working during the execution or what it tries to do when it runs , they basically _Does this program try to do sneaky things?_ or _Does it behave in a way that make it look like a virus, even if it's new and hasn't been seen before?_ .



***3. Behavior Based detection*** : Relies on identifying malware monitoring it's behavior . ( used for newer strains of malware )

+ **For Example** , Let's imagine you download a file that your antivirus doesn't detect because it contains brand-new, unidentified malware. The antivirus examines the behavior of the file using _Behavior based detection_ as an alternative to depending on a known virus signature.

+ What i mean by this is , consider a file you downloaded attempts to copy itself into the starting folder of your computer on its own. This raises suspicions because a lot of trustworthy apps don't require it, but malicious apps frequently aim to launch automatically each time your computer boots up. Based on its unique behavior, the antivirus program labels the file as potentially hazardous even though it doesn't match any known virus signatures. This is an example of _behavior based detection_ in action; rather than searching for a precise match, it looks for traits or behaviors that are frequently connected to malware.

> as we known nothing is permanent solution for anything and as so called hackers/cyber-security-researchers whatever ever you call , we love to break things and make things works unintended way , and we are good at it 🗿 . 
{: .prompt-info}


## AV Evasion Techniques <img src="https://i.imghippo.com/files/hgSLq1726508638.gif" alt="AV Evasion Techniques GIF" style="vertical-align: middle; width: 20px; height: 20px;">

***1. On-Disk Evasion Techniques*** : That Sound bit odd but it basically means hiding our payload or malware from AV detection when it's stored on a computer's hard drive using techniques which are :

+ *Obfuscation :* Obsfuscation is basically when malware ( or any program indeed ) tries to hide its real purpose by making its code confusing or hard to understand . It scrambles or changes its code so that it looks like gibberish to an antivirus or anyone trying to analyze it . it basically like writing a message in a secret code , that make no scense but the message is there but it's hard to figure out what it says . 

+ *Encoding :* Encoding data is process that involve changing data into a newer formate using a scheme . Encoding is reversible proces; data can be encoded to a new format and decoded to its original format. 

+ *Packing :* Packing is when a program ( often malware ) is compressed or wrapped in a way that hides its original content , Think of it like zipping a file to make it smaller and harder to see what's inside . Malware conceals its actual code by packing, making it appear benign to antivirus software when it is inspected. However, the buried malicious code is exposed and begins to perform its nasty things as soon as it is "unpacked" or executed.

    + In simple terms, it’s like wrapping a gift with many layers of paper to make it harder to figure out what's inside until the layers are removed.

+ *Crypters :* Basically utilizing an erypter tool which is used to hide or encrypt the real code of malware so that it's harder for antivirus software to detect , and it decrypts the encrypted code in memory . It's like putting your malicous program in a locked program in a locked box with a secret code. The decryption key/function is usually stored in a stub. 


***2. In-Memory Evasion Techniques*** : As it name suggest , this techniques is used by malware to avoid detection by staying hidden in the computer;s memory , rather than leaving obvious sign on the hard drive . 

+ Here's little breakdown i can explain xD :
  
  1. *First The Malware Stay In RAM :* Malware does not save itself as a file on the hard drive; instead, it operates directly from the computer's RAM. This makes it more difficult to identify since it leaves behind a file that is difficult to scan.
  2. *Clears His Tracks :* After it has finished executing, it may remove its own traces from memory, leaving no evidence for antivirus software to find.
  3. *Injects Code :* In order to blend in with regular activity and be more difficult to detect, the malware may implant itself into the memory region of another genuine operation.

--------------------------------------------------------------------------------------

## Final Thoughts 

 We will see in later post , How we can utilize the tool like **shellter** to inject our malicious payloads in the legitimate windows executable file which also works as normal , and avoid windows defender and gets the reverse shell from the target. We will also learning about how to obfuscate windows payloads using the **Invoke_obfuscation** tool . Hope you have learned something then please ping me on [twitter](https://twitter.com/archtrmntor) or [Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) , love to hear your feedback , and improve in upcoming blogs .


