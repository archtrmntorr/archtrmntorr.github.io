---
title: "Tryhackme Block Write-Up"
description: "Encryption? What Encryption?"
date:  2024-09-08
categories: [Writeup]
tags: [TryHackMe, Walkthrough, Writeup, Block Walkthrough, Tryhackme Block Walkthrough]
---


![Block Room Image](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FeCwn9P2gD42PZO5HkSRE%2Fuploads%2FhmoIRAtObq5VzxLHPKQy%2FPasted%20image%2020240909142449.png?alt=media&token=aee181b5-55ac-486f-a382-fd4eda07ff71)



> [TryHackMe Block Room ](https://tryhackme.com/r/room/blockroom) <br>
> Created by : @hadrian3689 <br>
> Created : 31 days old ( from the time , i am writing this writeup ) <br>
>Room Type : Free
{: .prompt-tip }



--------------------------------------------------------------
We are given a PCAP file and an LSASS dump by the challenge ( Local Security Authority Server Service (LSASS) is **a process in Microsoft Windows operating systems that is responsible for enforcing security policy on the system** ) . Finding encrypted SMB communication is the first thing we do after examining the PCAP file. It is clear from the room description that decryption is required. The task asks us to determine each user's login, password, hash, flag, and potentially even the substance of the files they sent. All of this ought to be achievable with the LSASS dump and the PCAP file.

## What I Found In The First Look 
------------------------------------------

I downloaded the zip file , attached in this challenge and then open it in the wireshark , by just looking at the i see SMB2  authentication request by the domain\user : WORKGROUP\mrealman , 

![Pasted image 20240909135359.png](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FeCwn9P2gD42PZO5HkSRE%2Fuploads%2FrjtuxLzjc2lTEFKIKQ3z%2FPasted%20image%2020240909135359.png?alt=media&token=6f586bab-a550-41f1-85e7-85f761e469a6)

- Then i found the file related to that in the `clients156.csv` format in the request .

![Pasted image 20240909135501.png](/assets/img/block/Pasted%20image%2020240909135501.png)

- Then scrolling down i found another WORKGROUP\USER : WORKGROUP\eshellstrop

![Pasted image 20240909135501.png](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FeCwn9P2gD42PZO5HkSRE%2Fuploads%2FGqIdpWNNqpxTAHVkClsy%2FPasted%20image%2020240909135444.png?alt=media&token=588b9e89-d7fb-4ab4-94cd-50f3096ce2be)

 - and then i found file `clients978.csv` related to that user he trying to access 

![Pasted image 20240909135537.png](/assets/img/block/Pasted%20image%2020240909135537.png)

## Investigating the First User
------------------------------------------

Investigating gives feel like doing forensics ;-) .
To show just packets containing NTLM (NT LAN Manager) Security Support Provider (SSP) protocol data, use Wireshark's ntlmssp filter. A security mechanism called NTLMSSP is employed in a number of Microsoft network authentication methods, especially in setups where Kerberos cannot be used. 

We got the answer of the first question of the this room , next we have to find the password for the user 1 , which is mrealman .

I found a tool on google named as PCredz . [This tool extracts Credit card numbers, NTLM(DCE-RPC, HTTP, SQL, LDAP, etc), Kerberos (AS-REQ Pre-Auth etype 23), HTTP Basic, SNMP, POP, SMTP, FTP, IMAP, etc from a pcap file or from a live interface.](https://github.com/lgandx/PCredz)

`-f` : flag is used to specify the file and 
`-d` : flag is used to specify directory 

This too need the PCAP file and be able to retrieve the NTLMv2 hashes of `mrealman` and `eshellstrop` , which i was not hoping for , at least for this while .

**These are the Components that are necessary to create a NTLMv2 structure ;**

1. Username : The username used for the NTLMv2 Authentication .
2. Domain : The domain used for the NTLMv2 Authentication .
3. Server Challenge :  An 8-byte challenge sent by the server to the client
4. NT Proof String ( NTLMv2 Response ) : The first 16 bytes of the NTLMv2 response, which is the HMAC-MD5 hash.
5. Challenge blob : The remaining part of the NTLMv2 response, including the timestamp, client challenge, target information, etc.

Next, combine the required components into an NTLMv2 structure:

```
<username>::<domain>:<server_challenge>:<nt_proof_string>:<blob>
```

![[Pasted image 20240909135739.png]](/assets/img/block/Pasted%20image%2020240909135739.png)

- i use the `hashcat` tool to crack the password of these , hashes and i only got the `mrealman` user hash not the other one , and here i am hoping to get both ;-(

```
┌──(archtrmntor㉿kali)-[~/Desktop/thm/PCredz]
└─$ hashcat -m 5600 -a 0 "mrealman::WORKGROUP:2a9c5234abca01e7:16E816DEAD<REDECTED>" /usr/share/wordlists/rockyou.txt
```

![[Pasted image 20240909135944.png]](/assets/img/block/Pasted%20image%2020240909135944.png)

**flag for the mrealman user**  is next question , while researching i got this blog during researching [Decrypting SMB3 Traffic with just a PCAP? Absolutely ( maybe.)](https://medium.com/maverislabs/decrypting-smb3-traffic-with-just-a-pcap-absolutely-maybe-712ed23ff6a2) , it tell us how to decrypt the encrypted SMB traffic to get the transferred item and extract the information and see them . 

All we need key exchange key , to decrypt , which can be derived from the content of the  PCAP file we have  . In summary , the Random Session Key can be calculated by :

```
-Unicode (utf-16le) of password-MD4 hash of the above _(This is also the NTLM Hash of the password)_-Unicode(utf-16le) and Uppercase of Username and Domain/Workgroup together -Calculating the ResponseKeyNT via HMAC_MD5(NTLM Hash, Unicode of User/Domain above)-NTProofStr _(can be calculated but not needed as it is present in the PCAP)_-Calculating the KeyExchangeKey via HMAC_MD5(ResponseKeyNT,NTProofStr)-Decrypt the Encrypted Session Key via RC4 and the Key Exchange Key to finally get the Random Session Key
```

- `NTProofString` can be found in the `ntlmssp` packet

![[Pasted image 20240909140053.png]](/assets/img/block/Pasted%20image%2020240909140053.png)

- .........so as encrypted session key.

![[Pasted image 20240909140148.png]](/assets/img/block/Pasted%20image%2020240909140148.png)

- we can use the python script given by `Maveris lab` , it not only take the passwords but also the hashes can be used directly , might be use in the case of second user  bcoz we are not able to crack the hash for that user .
- Here is the Python

```
import hashlib
import hmac
import argparse
import binascii

# stolen from impacket. Thank you all for your wonderful contributions to the community
try:
    from Cryptodome.Cipher import ARC4
    from Cryptodome.Cipher import DES
    from Cryptodome.Hash import MD4
except Exception as e:
    print("Warning: You don't have any crypto installed. You need pycryptodomex")
    print("See https://pypi.org/project/pycryptodomex/")
    raise e

def generate_encrypted_session_key(key_exchange_key, exported_session_key):
    cipher = ARC4.new(key_exchange_key)
    cipher_encrypt = cipher.encrypt
    session_key = cipher_encrypt(exported_session_key)
    return session_key

parser = argparse.ArgumentParser(description="Calculate the Random Session Key based on data from a PCAP (maybe).")
parser.add_argument("-u", "--user", required=True, help="User name")
parser.add_argument("-d", "--domain", required=True, help="Domain name")
parser.add_argument("-p", "--password", help="Password of User")
parser.add_argument("-ph", "--passwordhash", help="NTLM Hash of the Password (in Hex)")
parser.add_argument("-n", "--ntproofstr", required=True, help="NTProofStr. This can be found in PCAP (provide Hex Stream)")
parser.add_argument("-k", "--key", required=True, help="Encrypted Session Key. This can be found in PCAP (provide Hex Stream)")
parser.add_argument("-v", "--verbose", action="store_true", help="Increase output verbosity")

args = parser.parse_args()

# Validate that either password or password hash is provided
if not args.password and not args.passwordhash:
    parser.error("You must provide either --password or --passwordhash")

# Upper Case User and Domain
user = str(args.user).upper().encode('utf-16le')
domain = str(args.domain).upper().encode('utf-16le')

# If password is provided, calculate the NTLM hash
if args.password:
    passw = args.password.encode('utf-16le')
    hash1 = hashlib.new('md4', passw)
    password_hash = hash1.digest()
else:
    # Use provided password hash (in hex) instead of calculating it
    password_hash = binascii.unhexlify(args.passwordhash)

# Calculate the ResponseNTKey
h = hmac.new(password_hash, digestmod=hashlib.md5)
h.update(user + domain)
resp_nt_key = h.digest()

# Use NTProofSTR and ResponseNTKey to calculate Key Exchange Key
nt_proof_str = binascii.unhexlify(args.ntproofstr)
h = hmac.new(resp_nt_key, digestmod=hashlib.md5)
h.update(nt_proof_str)
key_exch_key = h.digest()

# Calculate the Random Session Key by decrypting Encrypted Session Key with Key Exchange Key via RC4
r_sess_key = generate_encrypted_session_key(key_exch_key, binascii.unhexlify(args.key))

if args.verbose:
    print("USER WORK: " + user.decode('utf-16le') + domain.decode('utf-16le'))
    if args.password:
        print("PASS HASH: " + binascii.hexlify(password_hash).decode())
    print("RESP NT:   " + binascii.hexlify(resp_nt_key).decode())
    print("NT PROOF:  " + binascii.hexlify(nt_proof_str).decode())
    print("KeyExKey:  " + binascii.hexlify(key_exch_key).decode())
print("Random SK: " + binascii.hexlify(r_sess_key).decode())
```

- we need the python3 package to run this file , and we can use the various flag to able to retrieve the session key. 

```
python3 decrypt.py -u mrealman -d WORKGROUP -p <Password> -n 16e816dead16d4ca7d5d6dee4a015c14  -k fde53b54cb676b9bbf0fb1fbef384698
```

![[Pasted image 20240909140308.png]](/assets/img/block/Pasted%20image%2020240909140308.png)

- as mention in the above blog we need the session id to decrypt the SMB traffic .

![[Pasted image 20240909140357.png]](/assets/img/block/Pasted%20image%2020240909140357.png)

- All that's left to do is add the session ID and the computed session key to the `Edit->Preferences->Protocols->SMB2` protocol settings. However, as the example below illustrates, the option expects a different representation than the one in the packet.
- i am writing the writeup later , so i attached the session for the another user too in below screenshot . 
- we have to format the `Session ID` as `4100000000100000` . we need to reverse the bytes because of the endianness ( endianness is the order in which bytes within a word of digital data are transmitted over a data communication medium or addressed in computer memory, counting only byte significance compared to earliness. )

![[Pasted image 20240909140512.png]](/assets/img/block/Pasted%20image%2020240909140512.png)

- and that's how we successfully decrypt the SMB traffic . 
- Export the SMB objects to a file .

`file -> Export Objects -> SMB`

![[Pasted image 20240909140615.png]](/assets/img/block/Pasted%20image%2020240909140615.png)

sdsdf

- save and open the file `clients156.csv` and you got the flag for the first user .

![[Pasted image 20240909140711.png]](/assets/img/block/Pasted%20image%2020240909140711.png)

## Investigating The Second User
----------------------------------------

- as we already now the second username `eshellstrop`  , which answer another question of the room , for next question we have to find the hash of the second user . 
![[Pasted image 20240909135444.png]]

- We can utilize another tool name [pypykatz ](https://github.com/skelsec/pypykatz) , which can extract the NTLM hash from the LSASS dump .

```
$ pypykatz lsa minidump lsass.DMP --json | grep NThash -A10 
```

![[Pasted image 20240909140810.png]](/assets/img/block/Pasted%20image%2020240909140810.png)

- we can copy the hash of the `eshellstrop`  , which we will use with the Pcredz tool , to decrypt the SMB traffic .

![[Pasted image 20240909140834.png]](/assets/img/block/Pasted%20image%2020240909140834.png)

- as previous the get the `NTProofstr: ` for user `eshellstrop`

![[Pasted image 20240909141047.png]](/assets/img/block/Pasted%20image%2020240909141047.png)

- and .....same grep the `Session Key:`

![[Pasted image 20240909140935.png]](/assets/img/block/Pasted%20image%2020240909140935.png)

- and......same grep the `Session ID`

![[Pasted image 20240909141215.png]](/assets/img/block/Pasted%20image%2020240909141215.png)

- now we can use the `maverislabs` python script with the different flag
	- `-u` : for user
	- `-d` : for domain
	- `-ph` : for password hash
	- `-n` : for NTProofStr
	- `-k` : for session key

```
python3 decrypt.py -u eshellstrop -d WORKGROUP -ph 3f29138a04aadc19214e9c04028bf381 -n 0ca6227a4f00b9654a48908c4801a0ac -k c24f5102a22d286336aac2dfa4dc2e04
```

![[Pasted image 20240909153217.png]](/assets/img/block/Pasted%20image%2020240909153217.png)

- ...And repeat the steps as already known of first user. 

`Edit->Preferences->Protocols->SMB2`

![[Pasted image 20240909140615.png]](/assets/img/block/Pasted%20image%2020240909140512.png)

 
`file -> Export Objects -> SMB`

![[Pasted image 20240909140615.png]](/assets/img/block/Pasted%20image%2020240909140615.png)

-  Open the saved file `clients978.csv` , and you got the final user flag .

![[Pasted image 20240909141400.png]](/assets/img/block/Pasted%20image%2020240909141400.png)