---
title: "EasySharp - TryCrack.Me C# Code Review Challange Walkthrough"
description: "Are you ready for a Code Review Challenge ??"
date:  2024-10-03
categories: [Walkthrough]
tags: [trycrack.me, code , review , codereview , code-review , walkthrough , EasySharp , C# review , EasySharp, C Sharp] 
image :
    path : https://i.pinimg.com/originals/90/70/32/9070324cdfc07c68d60eed0c39e77573.gif
---

Let's dive into a C# code review from [TryCrack.ME](https://trycrack.me/) and elevate our skills stats.

- The purpose of this review is to go through the C#e provided for the **EasySharp** challenge from _TryCrack.Me_ and to solve the challenge effectively. We will break down the code, understand its functionality, and then proceed with solving the problem.

<span style="color: PaleGreen;"><b>Here is Our **Code**.....</b></span>

```c#

string your_input = "";
char[] array = your_input.ToCharArray();
string[] array2 = new string[array.Length];
string enc = "";
string correct = "144260662546426066";

for (int i = 0; i < your_input.Length; i++)
{
    array2[i] = Convert.ToString((int)array[i] - 55);
    enc += array2[i];
}

if (enc == correct)
    Console.WriteLine("Good job!");
else
    Console.WriteLine("Wrong Answer!");
```


### <span style="color: orange;"><b>What Does Code Says.........</b></span>

#### <span style="color: DarkSalmon;"><b># Initialization :</b></span>

```c#

string your_input = "";
char[] array = your_input.ToCharArray();
string[] array2 = new string[array.Length];
string enc = "";
string correct = "144260662546426066";
```

- **`your_input`**: This variable is initialized as an empty string, but it would typically hold the user's input.
- **`array`**: The `ToCharArray()` method converts the string `your_input` into an array of characters, where each character is stored in a separate index of the array.
- **`array2`**: This is an array of strings, where each element will later store a processed value of the corresponding character in `your_input`.
- **`enc`**: This is an empty string that will store the encoded version of the `your_input`.
- **`correct`**: This is a string representing the correct encoded value that will be compared against the encoded value of the input (`enc`).

#### <span style="color: DarkSalmon;"><b># The Loop :</b></span>

```c#

for (int i = 0; i < your_input.Length; i++)
{
    array2[i] = Convert.ToString((int)array[i] - 55);
    enc += array2[i];
}
```

- This loop iterates through each character in the `your_input` string.

<span style="color: skyblue;">**Key steps inside the loop:**</span>

- **`array2[i] = Convert.ToString((int)array[i] - 55);`**
    - **`(int)array[i]`**: This converts the character at position `i` in `your_input` to its corresponding ASCII (integer) value.
    - **`- 55`**: This operation subtracts 55 from the ASCII value of the character.
        - For example, for the character `'A'` (ASCII value 65), `(int)'A' - 55` gives `10`. Similarly, for `'B'` (ASCII value 66), the result is `11`.
    - **`Convert.ToString(...)`**: The resulting integer (after subtraction) is converted to a string and stored in `array2[i]`.
- **`enc += array2[i];`**
    - This appends the encoded value from `array2[i]` to the string `enc`. Over time, `enc` becomes the fully encoded version of `your_input`


#### <span style="color: DarkSalmon;"><b># Comparison :</b></span>

```c#
if (enc == correct)
    Console.WriteLine("Good job!");
else
    Console.WriteLine("Wrong Answer!");
```

- After the loop completes, the program compares the encoded string (`enc`) with the pre-defined correct encoded string (`"144260662546426066"`).
    - If the two strings match, it prints "Good job!"
    - Otherwise, it prints "Wrong Answer!"

### <span style="color: orange;"><b># Simple View :</b></span>

The encoding logic takes each character from `your_input`, converts it to its ASCII value, subtracts 55, and stores the result as a string in `array2`. All these values are concatenated into the `enc` string, which is then compared with the `correct` string.

#### <span style="color: DarkSalmon;"><b># Let's solve this challenge Backwards  .....</b></span>

- Let's take the value that was given to us in the code for comparison to our input `144260662546426066` , as the char are in the ASCII value format , let's split it with two digits  and add 55 to each digit ....

```c#

- 14 + 55 = 69
- 42 + 55 = 97
- 60 + 55 = 115
- 66 + 55 = 121
- 25 + 55 = 80
- 46 + 55 = 101
- 42 + 55 = 97
- 60 + 55 = 115
- 66 + 55 = 121
```

- Now converts These to Characters:

```c#
- 69 = E
- 97 = a
- 115 = s
- 121 = y
- 80 = P
- 101 = e
- 97 = a
- 115 = s
- 121 = y 
```

Concatenating these gives: **EasyPeasy** ..  

![[Pasted image 20241001104836.png]](https://i.imghippo.com/files/nAtIQ1727762919.png)

### <span style="color: orange;"><b># Build an Automated Python Script to Solve Our Challenge</b></span>

- Let's Create a python script that solves our challenge automatically ....Automating little things in your daily life can teach you many concepts ....

``` python
# The correct encoded string from the original challenge
correct = "144260662546426066"

# we will decrypt this encoded string back into the original characters.
decrypted = ""
i = 0

# Split the correct string into chunks corresponding to the original characters encoding

while i < len(correct):
	# The numbers in the encoded string are either 2 or 3 digita long , so we try both . 
	if correct[i:i+2].isdigit() and int(correct[i:i+2]) >= 10;
	# Convert the string back into the characters using the reverse formula ( add 55 )
		decrypt += chr(int(correct(i:i+2)) + 55)
		i += 2
	else 
		decrypted += chr(int(correct[i:i+3])) + 55)
		i += 3 

# Print the decrypted input 

print(f"Decrypted input: {decrypted}")
```

<span style="color: orange;">**Breakdown of Decrypt.py code**</span>

1 . <span style="color: DarkSalmon;">**Initialization**:</span>

``` python
correct = "144260662546426066"
decrypted = ""
i = 0
```

- **`correct`**: This variable stores the encoded string from the original challenge, which you need to decrypt.
- **`decrypted`**: This variable will hold the decoded string as we process the encoded values. It starts as an empty string.
- **`i`**: This is an index that will be used to traverse through the `correct` string, character by character.

2 . <span style="color: DarkSalmon;">**While Loop**:</span>

``` python
while i < len(correct):
```

This loop continues as long as `i` (the index) is less than the length of the `correct` string. It allows us to traverse the encoded string, processing it in chunks to decode the original characters.

3 . <span style="color: DarkSalmon;">**Conditional Check for Two-Digit or Three-Digit Numbers** :</span>

``` python
if correct[i:i+2].isdigit() and int(correct[i:i+2]) >= 10:
```

- **`correct[i:i+2]`**: This takes the next 2 characters (digits) from the string starting at index `i`.
- **`.isdigit()`**: This checks if these 2 characters are digits.
- **`int(correct[i:i+2]) >= 10`**: This checks if the number formed by these two digits is greater than or equal to 10. This is because valid encoded values in the original challenge are either 2 or 3 digits long. For example, `14` is valid but `06` is not.

If the condition is true (i.e., the next two digits are a valid number), we proceed to decode them.

4 . <span style="color: DarkSalmon;">**Decoding the Character**:</span>

``` python
decrypted += chr(int(correct[i:i+2]) + 55)
i += 2
```

- **`int(correct[i:i+2]) + 55`**: This converts the 2-digit number into an integer, adds 55 to it, and retrieves the original ASCII value. The `+55` operation reverses the `-55` that was done during encoding.
    - Example: For `14`, the original value was `chr(14 + 55)` which gives the character `'E'`.
- **`chr()`**: This function converts the integer back into a character.
- **`decrypted += ...`**: This appends the decoded character to the `decrypted` string.
- **`i += 2`**: This increments the index `i` by 2, so we move to the next chunk of the encoded string.

5 . <span style="color: DarkSalmon;">**Handling Three-Digit Numbers**:</span>

``` python
else:    
	decrypted += chr(int(correct[i:i+3]) + 55)    
	i += 3
```

- If the next 2 digits didn’t form a valid number, we assume the next chunk is a 3-digit number.
- **`correct[i:i+3]`**: This extracts the next 3 characters from the string.
- The decoding process is the same: we convert this number to an integer, add 55, convert it to a character using `chr()`, and append it to `decrypted`.
- The index `i` is incremented by 3 to move past the 3-digit number.

6 . <span style="color: DarkSalmon;">**Final Output :**</span>

``` python
print(f"Decrypted input: {decrypted}")
```

After processing all chunks in the `correct` string, the `decrypted` variable holds the original input that matches the encoded string. This line prints the decrypted input.

### <span style="color: PaleGreen;">Example Walkthrough:

Let’s say the `correct` string is `"144260662546426066"`. Here’s how the decryption works step by step:

1. Start with the first two digits, `14`:
    - `14 + 55 = 69`
    - `chr(69)` gives `'E'`
    - `decrypted` is now `'E'`.
2. Next two digits, `42`:
    - `42 + 55 = 97`
    - `chr(97)` gives `'a'`
    - `decrypted` becomes `'Ea'`.
3. Next two digits, `60`:
    - `60 + 55 = 115`
    - `chr(115)` gives `'s'`
    - `decrypted` becomes `'Eas'`.
4. The process continues until the entire string is decoded, eventually yielding the original input.

![[Pasted image 20241001110414.png]](https://i.imghippo.com/files/d4tSG1727762914.png)

- you can build this in the C# too , it is kind of similar to c programming language . 
<br>

-------------------------------

<br>

### <span style="color: DarkSalmon;"><b># Final Thoughts</b></span>
<br>
I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)



