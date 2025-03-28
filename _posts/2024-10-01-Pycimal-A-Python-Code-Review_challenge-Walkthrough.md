---
title: "Pycimal - TryCrack.Me Python Code Review Challange Walkthrough"
description: "Are you ready for a Code Review Challenge ??"
date:  2024-10-01
categories: [Walkthrough]
tags: [trycrack.me, code , review , codereview , code-review , walkthrough , pycimal , python code review , Pycimal] 
image :
    path : https://i.pinimg.com/736x/78/fc/68/78fc687dc57910ef4348c7c0003f9785.jpg
---


Let's dive into a Python code review challenge from [TryCrack.ME](https://trycrack.me/) and elevate our skills stats.

- The purpose of this review is to go through the Python code provided for the **Pycimal** challenge from _TryCrack.Me_ and to solve the challenge effectively. We will break down the code, understand its functionality, and then proceed with solving the problem.

<span style="color: Skyblue;"><b>-------**Difficulty : EASY**-------</b></span>

<span style="color: Orange;"><b>Here is our code.....</b></span>

```python
your_input = ""
password = ""
intPass = "83,117,112,101,114,83,101,99,117,114,\
101,80,97,115,115,119,111,114,100,87,104,105,99,\
104,67,97,110,116,66,101,67,114,97,99,107,101,100,\
87,105,116,104,111,117,116,66,108,97,99,107,109,97,\
103,105,99"
 
intPass = intPass.split(",")
 
for c in intPass:
    password = password + chr(int(c))
    
if your_input == password:
    print("Good job!")
    
else:
    print("Wrong password")
```

### <span style="color: orange;"><b># What does this code says ???...</b></span>

If you know a bit about programming, you'll notice that the password is just numbers. The `chr()` function turns those numbers into their matching ASCII characters.

The `intPass` string contains comma-separated numbers, which are converted to their corresponding ASCII characters using the `chr()` function. Decoding these numbers give us the `password` value.

Here's how the password is derived:
####  <span style="color: orange;"># Step-by-Step Decoding :</span>

1. Split `intPass` by commas.
2. Convert each number to its corresponding ASCII character using `chr()`.
3. Concatenate the characters to form the password.

#### <span style="color: orange;"># The Numbers in`intPass` variable ,</span>

```
83, 117, 112, 101, 114, 83, 101, 99, 117, 114, 101, 80, 97, 115, 115, 119, 111, 114, 100, 87, 104, 105, 99, 104, 67, 97, 110, 116, 66, 101, 67, 114, 97, 99, 107, 101, 100, 87, 105, 116, 104, 111, 117, 116, 66, 108, 97, 99, 107, 109, 97, 103, 105, 99
```

#### <span style="color: orange;"># Decoding  Characters</span> 

```
SuperSecurePasswordWhichCantBeCrackedWithout<redacted>
```
![image](https://i.imghippo.com/files/nAtIQ1727762919.png)

If `your_input` matches this string, the code will print "Good job!"


### <span style="color: orange;"># Build an Automated Python Script to Solve Our Challenge</span>

- Let's Create a python script that solves our challenge automatically ....Automating little things in your daily life can teach you many concepts ....


```python
intPass = "83,117,112,101,114,83,101,99,117,114,\ 101,80,97,115,115,119,111,114,100,87,104,105,99,\ 104,67,97,110,116,66,101,67,114,97,99,107,101,100,\ 87,105,116,104,111,117,116,66,108,97,99,107,109,97,\ 103,105,99"

# Split intPass into individual ASCII Codes 

intPass = intPass.split(",")

# Generate the password by converting ASCII codes to chracters 
password = ""
for c in intPass:
	password += chr(int(c))

# Display the Password 
print(f"Generated password : {password}")

# Example input for comparision
your_input = input("Enter you input to compare:")

if you_input == password:
	print("Good job!")
else:
	print("Wrong Password")
```


![[Pasted image 20241001103039.png]](https://i.imghippo.com/files/lidQy1727762920.png)

<span style="color: WHITE;">**CODE REVIEW STATS = 1**</span>
<br>

-------------------------------

<br>

##### <span style="color: Green;"><b># Final Thoughts</b></span>
<br>
I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)