---
title: "OverTheWire Bandit Walkthrough"
date:  2024-09-08
categories: [Writeup]
tags: [bandit walkthrugh, OverTheWrite, Waregames]
---


## <span style="color: purple;"><b><u>Introduction</u></b></span>

OverTheWire is a free online platform designed to teach and practice security concepts through engaging games. It offers a variety of 'Wargames,' each focused on a different aspect of security.

I suggest starting with the Bandit game, an excellent introduction to Linux and Git commands, covering essential basics for tackling other Wargames, particularly those focused on these fundamental skills.

I made the decision to record my experience in a walkthrough for my blog as I advanced through the levels. Even though there are a ton of existing walkthroughs online that provide various viewpoints and fixes, I wanted to make my own. This not only makes writing easier for me, but it also gives me access to another resource that can be useful to someone who thinks my explanations and reasoning are more understandable. It also provides me with a reference that I can use later.

I will provide a concise overview of the main ideas, with further exploration encouraged. The onus is on you, and the game, to independently investigate these concepts further.

Having familiarized yourself with the article's focus and my intentions, we can now proceed to Level 0's step-by-step guide.

-------Steps for reading:------------

1. Read it once and take notes of words you may be unfamiliar with.
2. Research the words you don't understand.
3. Reread and see if you understand it better


## <span style="color: orange;"><b>--> Level 0</b></span>

```ssh : ssh bandit.labs.overthewire.org -l bandit0 -p 2220   ```      
```Password :- bandit0```

#### <span style="color: green;"><b>Task :</b></span>
The password for the next level is stored in a file called readme located in the home directory.

#### <span style="color: green;"><b>About :</b></span>
In this level , you will start learning basic Linux Commands to interact with the filesystem.

- pwd :- print name of current/working directory
- ls :- list directory contents
- cat :- concatenate files and print on the standard output


![Level-0](/assets/img/Pasted image 20240824180207.png)

#### <span style="color: green;"><b>Solution :</b></span> 

1. First we log in through SSH with the information above . we will have shell for user bandit0 ( as we are on this level ).
2. then , with ls command we list of the content of the directory and found that that there is a file names readme .
3. now , we can print the content of a file with the `cat` command .

`cat <file_name>`

```bash
ssh bandit.labs.overthewire.org -l bandit0 -p 2220
-----------------------------------------------------

bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

- The resulting string is the password for the ‘bandit1’ user.


## <span style="color: orange;"><b>--> Level 1</b></span>

#### Login Info :

```SSH: ssh bandit.labs.overthewire.org -l bandit1 -p 2220 ```   <br>
```Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If ```

#### <span style="color: green;"><b>Task :</b></span> 

Get the password from the file called '-'.

#### <span style="color: green;"><b>About :</b></span>

'-' :- It is a unique symbol in linux . It has already been demonstrated for adding so-called flags to commands that indicate particular options (such as the -l flag that selects a login_name for the ssh command). standard choice. Files containing this symbol as the initial character cannot therefore just be referenced as regular files.

#### <span style="color: green;"><b>Solution :</b></span>

1. By printing every file, we first confirm that the file is in the folder , using `ls` command


```
 ssh bandit.labs.overthewire.org -l bandit1 -p 2220
--------------------------------------------------------

$ ls -lah

bandit1@bandit:~$ ls -lah
total 24K
-rw-r-----  1 bandit2 bandit1   33 Jul 17 15:57 -
drwxr-xr-x  2 root    root    4.0K Jul 17 15:57 .
drwxr-xr-x 70 root    root    4.0K Jul 17 15:58 ..
-rw-r--r--  1 root    root     220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root    root    3.7K Mar 31 08:41 .bashrc
-rw-r--r--  1 root    root     807 Mar 31 08:41 .profile
bandit1@bandit:~$ 

----------------------------------------------------------
```

2. The command cat - returns nothing when used. Thus, we add the path and write./-instead of just -, and the command functions as it should ,
      `cat ./-`


```
bandit1@bandit:~$ ls -lah
total 24K
-rw-r-----  1 bandit2 bandit1   33 Jul 17 15:57 -
drwxr-xr-x  2 root    root    4.0K Jul 17 15:57 .
drwxr-xr-x 70 root    root    4.0K Jul 17 15:58 ..
-rw-r--r--  1 root    root     220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root    root    3.7K Mar 31 08:41 .bashrc
-rw-r--r--  1 root    root     807 Mar 31 08:41 .profile
bandit1@bandit:~$ cat < -
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

- Here we got the password for next level .

## <span style="color: orange;">--> Level 2</span>

#### Login Info :

```SSH: sh bandit.labs.overthewire.org -l bandit2 -p 2220``` <br>
```Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx```

#### <span style="color: green;"><b>Task :</b></span>

The spaces file in this filename, which is situated in the home directory, contains the password for the next level.

#### <span style="color: green;"><b>About :</b></span>

like in previous level , it is not recommanded to use the spaces in the filename or directory name . Instead, you can use an underscore (_) or a dash(-).

Spaces in a command can indicate new additinos . for instance, 'cat'command takes multiple filenames , separated by the spaces . simple trying to use the filename does not work .

for ex :- `cat spaces in this filename`

if a file name has space, use quotes to show words belong together as one name . Like in Solution

Another way to handle spaces in the filenames in a command is by using backslash . The backslash is used to escape the special meaning of the space character, treating it as a regular character.This method is useful when you want to avoid using quotes or when dealing with multiple spaces and special characters.

#### <span style="color: green;"><b>Solution :</b></span>

1. Use the quotes to show words belong together , like

`cat "spaces in this filename`

2. or Another way to handle spaces in the filenames in a command is by using backslash `\`. 

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit2 -p 2220
-------------------------------------------------------------

bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ ls -lah
total 24K
drwxr-xr-x  2 root    root    4.0K Jul 17 15:57 .
drwxr-xr-x 70 root    root    4.0K Jul 17 15:58 ..
-rw-r--r--  1 root    root     220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root    root    3.7K Mar 31 08:41 .bashrc
-rw-r--r--  1 root    root     807 Mar 31 08:41 .profile
-rw-r-----  1 bandit3 bandit2   33 Jul 17 15:57 spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename 
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

- Here we got the password for next level


## <span style="color: orange;"><b>--> Level 3</b></span>

#### Login Info : 

```SSH: ssh bandit.labs.overthewire.org -l bandit3 -p 2220``` <br> 
```Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx ```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in a hidden file in the inhere directory.

#### <span style="color: green;"><b>About :</b></span>

- To <b> change the directory </b> we use the command : `cd <Directory_Path>`
- Some `cd` command uses :
  - `cd ..` :- goes to the parent directory
  - `cd /` :- goes to the root directory
  - `cd ~` :- goes to the home directory ( of current user)

- to look for hidden file we use `-a` flag with the `ls` command .
   -  `-l` :- flag is used for long listing 
   -  `-h` :- flag is used to print the size

- the `.` represent the current directory and `..` represent the parent directory 

#### <span style="color: green;"><b>Solution :</b></span>

1. first change the directory to 'inhere' using `cd inhere`
2. Then list the content of the directory using `ls -a`.
3. you find a hidden file names <b>`...Hiding-From-You`</b> .
4. Now read the content of the file using `cat ...Hidding-From-You`.

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit3 -p 2220
--------------------------------------------------------

bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ ls -R
.:
inhere

./inhere:
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -R
.:
bandit3@bandit:~/inhere$ ls -lah
total 12K
drwxr-xr-x 2 root    root    4.0K Jul 17 15:57 .
drwxr-xr-x 3 root    root    4.0K Jul 17 15:57 ..
-rw-r----- 1 bandit4 bandit3   33 Jul 17 15:57 ...Hiding-From-You
bandit3@bandit:~/inhere$ cat ...Hiding-From-You 
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
bandit3@bandit:~/inhere$ 
```

- Here we got the password for next level. 


## <span style="color: orange;"><b>--> Level 4</b></span> 

#### Login Info :

```SSH: ssh bandit.labs.overthewire.org -l bandit4 -p 2220``` <br>
```Password:- 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ```

#### <span style="color: green;"><b>Task :</b></span> 

The password for the next level is stored in the only human-readable file in the inhere directory.

#### <span style="color: green;"><b>About :</b></span>

The `file` command give us the type of data of the file is . Some example would be like : 'txt', 'ruby',exe',ASCII text, etc .

Here in this task we are looking for the human-redable file and ELF file is not human-readable. The most common data encoding that are humar redable are ASCII and Unicode . 

we can use the `*` wildcard with the `file` command , this wildcard stand for any number of literal or character  ,it can also be used to select 'everything' in one . Here's how we do it 

#### <span style="color: green;"><b>Solution :</b></span>

-  First change directory to the 'inhere' directory .
-  Second , we can read each and every file one by one so we use the wildcard to get the type of all the files , using command : `file ./*`


```
|──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit4 -p 2220
--------------------------------------------------------

bandit4@bandit:~/inhere$ ls
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
bandit4@bandit:~/inhere$ cat < -file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
bandit4@bandit:~/inhere$ cat < -file08
ʧh?+>����R0\��q�c��(|�^��
                   ы�Ϣ��bandit4@bandit:~/inhere$ 
```

-  we can see that only <b>'-file07'</b> is of type 'ASCII test'.
-  Now we can read the content of the file using: `cat ./-file07`

```
bandit4@bandit:~/inhere$ file ./*

./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

- Here we got the password for next level.

## <span style="color: orange;"><b>--> Level 5</b></span>

#### Login Info :

```SSH:  ssh bandit.labs.overthewire.org -l bandit5 -p 2220``` <br>
```Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw```

#### <span style="color: green;"><b>Task :</b></span>


The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

#### <span style="color: green;"><b>About :</b></span>

`find` command is used for searching and locating file or directories within a file system based on a specific crietria such as name,type,date,execution,size,ownership etc. with designated flag .

`grep` command searches its input for lines containing a specific pattern defined by th user . It can also be used to do the opposite, meaning when using the `-v` flag , a line witha defined pattern will be printed . 

Some flag usages of `find` command:
    - `-size` is used to looking at file size in bytes.
    - `-type f` is used to look at files.
    - `-readable` flag , mean you have the permission to read the files .
    - Instead , we could use `-exec <command>` flag with `{}` as a path, meaning the chosen command will be executed on all the files. This could be used to execute another command like `file`.

#### <span style="color: green;"><b>Solution :</b></span>

1. Change the directory to <b>'inhere'</b> .
2. we have to find the file who have
    - `-size 1033`
    - `-type f`
    - `-exec file '{}' \;` , to execute the file command and get the file data type .and then we need to grep the <b>ASCII'</b> as file type . 
3. Then we read the file content to get the password for next level . 


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit5 -p 2220
----------------------------------------------------------

bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII

./maybehere07/.file2: ASCII text, with very long lines
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

- Here we got the password for next level.

## <span style="color: orange;"><b>--> Level 6</b></span>


#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit6 -p 2220``` <br>
```Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG```


#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size


#### <span style="color: green;"><b>About :</b></span>

Here the main topic is File Permission , Specially to the area of the ownership > each and every file is ownmed by a user and a group . we can see this information using the `ls` command with `-l` flag.

we can also combine it with the find command to search through , specific detail  of the file using 
    - `-user` :- flag to specify the user 
    - `-group` :- to specify the group 
    - `-size` :- to specify the size 


#### <span style="color: green;"><b>Solution :</b></span>

1. use the command `find` with specific flag , we can lcoate the exact file we are searching for 

Command :- `find / -user bandit7 -group bandit6 -size 33c 2>/dev/null`

- `Here 2>/dev/null` :- syntax in Linux is used to redirect error output to the null device, effectively discarding it. This is useful when you want to run a command without seeing error messages. The number 2 refers to the standard error stream, and /dev/null is a special file that discards all data written to it.


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit6 -p 2220
-----------------------------------------------------------

bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
bandit6@bandit:~$ 
```

- Here we got the password for the next level.

## <span style="color: Orange;"><b>--> Level 7</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit7 -p 2220 ``` <br>
```Passwd: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in the file **data.txt** next to the word **millionth**

#### <span style="color: green;"><b>About :</b></span>

`grep` command is used to serch lines for specific patter using : `grep <Pattern>`.
with the '(|)' , we can pipe the output of cat to grep as input to look through a text file . 

#### <span style="color: green;"><b>Solution :</b></span>

1. first list out the content in the directory using `ls`.
2. Second  , we can pipe the output of `cat` to `grep` to look for string **millionth** , next to which is our passsword  

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit7 -p 2220
-----------------------------------------------------------

bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ cat data.txt | grep millionth
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
bandit7@bandit:~$ 
```

- Here we got the password for the next level . 

## <span style="color: orange;"><b>--> Level 8</b><span>


#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit8 -p 2220``` <br> 
```Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

#### <span style="color: green;"><b>About :</b></span>

`sort` : Sorting is necessary because the `uniq` command works on consecutive duplicate lines.

The `sort` command arranges the lines in order, so identical lines appear consecutively or in the ascending order . By Default , it sorts lexicographically ( i.e, alphabetically )
- The `uniq -u` command is used to filter out unique lines from the sorted input > it only print the lines that are not repeated . 


#### <span style="color: green;"><b>Solution :</b></span>

1. First list out the content of the directory using the `ls` command , then 
2. sort the data using the sorted command , 
3. then with the help of piping `|` character that passed the output of the previous command as input to the next command , then 
4. use the `uniq -u` to filter out unique lines from the sorted input . 

Command : `sort data.txt | uniq -u `


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit8 -p 2220
-----------------------------------------------------------

bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
bandit8@bandit:~$ 
```

- Here we got the password for the next level .

## <span style="color: orange;"><b>--> Level 9</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit9 -p 2220``` <br>
```Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM```
\
#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

#### <span style="color: green;"><b>About :</b></span>

`strings` command finds the human-redable strings in files. It prints printable character sequences specifically. It is mostly used for non-printable files, such as executables or hex dumps.

#### <span style="color: green;"><b>Solution :</b></span>

1. first list out the content of the directory ,
2. then use the `strings` command with the `grep` command to extrac th several `===` strings form the `data.txt` file we have given .

Command : `strings data.txt | grep ===`

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit9 -p 2220
-----------------------------------------------------------

bandit9@bandit:~$ strings data.txt | grep ===
\a!;========== the
========== passwordf
========== isc
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
bandit9@bandit:~$ 

```

- Here we got the password for the next level.


## <span style="color: orange;"><b>--> Level 10</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit10 -p 2220``` <br>
```Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey```


#### <span style="color: green;"><b>Task :</b></span>


The password for the next level is stored in the file **data.txt**, which contains base64 encoded data


#### <span style="color: green;"><b>About :</b></span>

1. base64 is a binary-to-text encoded scheme. it is usually by the equal sign in the end of the data , but not always . In Linux has a command named `base64` that allows encoding and decoding in `base64` . For decoding , we need to use the flag `-d`

#### <span style="color: green;"><b>Solution :</b></span>

1. First list out the content of the directory , 
2. then use the `base64` command to decode the data from the text file .

Command : `base64 -d data.txt`

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit10 -p 2220
-----------------------------------------------------------

bandit10@bandit:~$ base64 -d data.txt 
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
bandit10@bandit:~$ cat data.txt 
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
bandit10@bandit:~$ 
```

- Here we got the password for the next level.


## <span style="color: orange;"><b>--> Level 11</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit11 -p 2220``` <br>
```Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

#### <span style="color: green;"><b>About :</b></span>

rotation by 13 is basically the **ROT13** substitution cipher . Substition means replacing one character with another . 
In Linux `tr` command means `translate` that allows replacing characters with others . Here its syntax looks like `tr <old_chars> <new_chars>`

tr 'A-Za-z' 'N-ZA-Mn-za-m'`: This command tells `tr` to replace each uppercase letter (A-Z) with the letter 13 positions ahead (N-Z followed by A-M) and each lowercase letter (a-z) with the corresponding letter 13 positions ahead (n-z followed by a-m)

#### <span style="color: green;"><b>Solution :</b></span>

-  First , this can solved with the online tool names [cyberchef](https://gchq.github.io/CyberChef/) , and selecting **ROT13** as cooking . 

[Cyber chef solution](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13)&input=R3VyIGNuZmZqYmVxIHZmIDdrMTZKQXJVVnY1THhWdUpmc1NWZGJidGFIR2x3OUQ0Cg)

or 

-  for **ROT13** we have to do `A->N` and `Z->M` , with `tr` commads looks like :

Command :- `tr 'A-Za-a' 'N-ZA-Mn-za-m'`

-  we have to use this with piping character with `cat` , that inputs the data of 'data.txt' to `tr`.

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit11 -p 2220
-----------------------------------------------------------

bandit11@bandit:~$ cat data.txt 
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
bandit11@bandit:~$ tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
bandit11@bandit:~$ 
```
- here we got the password for next level .


## <span style="color: orange;"><b>--> Level 12</b></span>


#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit12 -p 2220``` <br>
```Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

#### <span style="color: green;"><b>About :</b></span>

`file` command is used to determine the type of a file . The command examines the files and make a guess based on its content or magic numbers , and then return the file type . For example , `file text.txt` might return `text.txt: ASCII text`

1. `tar`: This is a tape archiving utility in Unix and Unix-like operating systems. It's used to collect multiple files into a single tarball (.tar file) for archiving and compression.

2. `gzip`: This is a compression utility that reduces the size of files using Lempel-Ziv coding (LZ77). It's often used in conjunction with `tar` to create compressed archives (.tar.gz or .tgz files).

3. `b2z`: BZ2 (bzip2) is an open source compressor for high-quality compression. It mainly works in UNIX based OS. 


#### <span style="color: green;"><b>Solution :</b></span>

1. We have to first want to know the type of the file like wise if file is in data.bin but doing `file data.bin` shows that , that a gzip file , then we have to move it to the .gzip formate like `mv data.bin data.gzip` then unzip this using `gzip -d data.gzip` then there is a file in that , 
2. do the same until you got `data: ASCII text` , that is the file which contain our password for next level .



```
bandit12@bandit:~$ cat data.txt 
00000000: 1f8b 0808 d2e9 9766 0203 6461 7461 322e  .......f..data2.
00000010: 6269 6e00 0141 02be fd42 5a68 3931 4159  bin..A...BZh91AY
00000020: 2653 59ea 2468 ae00 0017 7fff dadb b7fb  &SY.$h..........
00000030: dbff 5ffb f3fb d776 3d6f fffb dbea fdbd  .._....v=o......
00000040: 85db edfc ffa9 7def faaf efdf b001 386c  ......}.......8l
00000050: 1001 a0d0 6d40 01a0 1a00 0006 8006 8000  ....m@..........
00000060: 0000 d034 01a1 a34d 0034 3d43 40d0 0d34  ...4...M.4=C@..4
00000070: d034 34da 9ea1 b49e a7a8 f29e 5106 4326  .44.........Q.C&
00000080: 9a19 1934 d1a0 341a 6234 d018 d468 6834  ...4..4.b4...hh4
00000090: 00c9 a308 6434 0000 0308 d068 0680 1900  ....d4.....h....
000000a0: 0034 d068 1a34 d068 c3a7 a41a 0c9a 0d34  .4.h.4.h.......4
000000b0: 641a 0646 8346 4003 4d34 1a68 6806 9a06  d..F.F@.M4.hh...
000000c0: 9a64 d064 001a 0681 a343 10d0 d00d 1840  .d.d.....C.....@
000000d0: 01a3 21a0 68c9 a050 008a 0009 619a 9541  ..!.h..P....a..A
000000e0: 25d5 8bc0 0ff3 e679 7fd0 31b2 c784 e7f7  %......y..1.....
000000f0: 8fcb 33b8 28a5 bf86 4ac4 274f ce21 eeea  ..3.(...J.'O.!..
00000100: 2c19 2633 60e9 ddd1 8d60 18e9 b189 4a94  ,.&3`....`....J.
00000110: 3a14 ee61 ac8d d369 f545 a964 2617 f1fd  :..a...i.E.d&...
00000120: 72dc 51d1 e601 1071 745d 846c 4677 4ba2  r.Q....qt].lFwK.
00000130: 0562 5d79 894a 9150 dfe1 8083 e4c0 896f  .b]y.J.P.......o
00000140: b75c d58b 4264 021c 625c c4f2 816a 8907  .\..Bd..b\...j..
00000150: 8b80 2b3e 4d2a f1b3 4fb4 6cee a869 1316  ..+>M*..O.l..i..
00000160: c318 cdb5 b1cd 21c4 a23a 0297 65ae 8a2a  ......!..:..e..*
00000170: 0cd2 0864 8a47 ed68 48f3 a65f 5803 dc9f  ...d.G.hH.._X...
00000180: b2e5 bbe0 daac 3d56 8c8b 4181 510f 017f  ......=V..A.Q...
00000190: 1328 9a47 6027 62c1 e4b4 db74 bb3a 9455  .(.G`'b....t.:.U
000001a0: 07dd fd5b 19b5 e522 32e0 9b3e a3cf 0189  ...[..."2..>....
000001b0: 4d9a 5edb 27be 1855 880f 7517 0ec0 a878  M.^.'..U..u....x
000001c0: 2ee0 92a3 e339 4138 5cb7 517a a8b7 4dab  .....9A8\.Qz..M.
000001d0: 8645 a681 214b 7f27 0cee 8ee5 3f4b 3a60  .E..!K.'....?K:`
000001e0: 530a 74b2 8acf 9044 e73c ca09 0d28 e5b4  S.t....D.<...(..
000001f0: 1471 0963 4a9c 3b75 73c0 4057 0c9c d0f2  .q.cJ.;us.@W....
00000200: 132a bb2c cc84 29cf 3568 9101 0a77 f033  .*.,..).5h...w.3
00000210: 41a4 8cfa f520 3ed5 8a4a 9528 1314 7b32  A.... >..J.(..{2
00000220: 87c6 4825 698a 921e e1da 8f2d 4237 2da1  ..H%i......-B7-.
00000230: 3f68 051d fe05 08cb 096d 4a17 ed35 2130  ?h.......mJ..5!0
00000240: 9d75 6c2f a414 8003 e650 ea14 4eb1 5fe2  .ul/.....P..N._.
00000250: ee48 a70a 121d 448d 15c0 8914 1b20 4102  .H....D...... A.
00000260: 0000                                     ..
bandit12@bandit:~$ mkdir /tmp/archtrmntor
bandit12@bandit:~$ cd /tmp/ar
-bash: cd: /tmp/ar: No such file or directory
bandit12@bandit:~$ cd /tmp/archtrmntor
bandit12@bandit:/tmp/archtrmntor$ xxd -r ~/data.txt > data
bandit12@bandit:/tmp/archtrmntor$ file data
data: gzip compressed data, was "data2.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 577
bandit12@bandit:/tmp/archtrmntor$ mv data file.gz
bandit12@bandit:/tmp/archtrmntor$ gzip -d file.gz 
bandit12@bandit:/tmp/archtrmntor$ ls
file
bandit12@bandit:/tmp/archtrmntor$ file file
file: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/archtrmntor$ mv file file.bz2
bandit12@bandit:/tmp/archtrmntor$ bzip2 -d file.bz2 
bandit12@bandit:/tmp/archtrmntor$ ls
file
bandit12@bandit:/tmp/archtrmntor$ file file
file: gzip compressed data, was "data4.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/archtrmntor$ mv file file.gz
bandit12@bandit:/tmp/archtrmntor$ ls
file.gz
bandit12@bandit:/tmp/archtrmntor$ gzip -d file.gz 
bandit12@bandit:/tmp/archtrmntor$ ls
file
bandit12@bandit:/tmp/archtrmntor$ file file 
file: POSIX tar archive (GNU)
bandit12@bandit:/tmp/archtrmntor$ mv file file.tar
bandit12@bandit:/tmp/archtrmntor$ ls
file.tar
bandit12@bandit:/tmp/archtrmntor$ tar xf file.tar 
bandit12@bandit:/tmp/archtrmntor$ ls
data5.bin  file.tar
bandit12@bandit:/tmp/archtrmntor$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/archtrmntor$ ls
data5.bin  file.tar
bandit12@bandit:/tmp/archtrmntor$ rm file.tar 
bandit12@bandit:/tmp/archtrmntor$ mv data5.bin data.tar
bandit12@bandit:/tmp/archtrmntor$ tar xf data.tar 
bandit12@bandit:/tmp/archtrmntor$ ls
data6.bin  data.tar
bandit12@bandit:/tmp/archtrmntor$ file data
data: cannot open `data' (No such file or directory)
bandit12@bandit:/tmp/archtrmntor$ file data
data6.bin  data.tar   
bandit12@bandit:/tmp/archtrmntor$ file data6.bin 
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/archtrmntor$ rm data.tar 
bandit12@bandit:/tmp/archtrmntor$ ls
data6.bin
bandit12@bandit:/tmp/archtrmntor$ mv data6.bin data.b2z
bandit12@bandit:/tmp/archtrmntor$ mv data.b2z data.bz2
bandit12@bandit:/tmp/archtrmntor$ ls
data.bz2
bandit12@bandit:/tmp/archtrmntor$ bzip2 -d data.bz2 
bandit12@bandit:/tmp/archtrmntor$ ls
data
bandit12@bandit:/tmp/archtrmntor$ file data 
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/archtrmntor$ ls
data
bandit12@bandit:/tmp/archtrmntor$ mv data data.tar
bandit12@bandit:/tmp/archtrmntor$ ls
data.tar
bandit12@bandit:/tmp/archtrmntor$ tar xf data.tar 
bandit12@bandit:/tmp/archtrmntor$ ls
data8.bin  data.tar
bandit12@bandit:/tmp/archtrmntor$ file data8.bin 
data8.bin: gzip compressed data, was "data9.bin", last modified: Wed Jul 17 15:57:06 2024, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/archtrmntor$ mv data
data8.bin  data.tar   
bandit12@bandit:/tmp/archtrmntor$ rm data.tar
bandit12@bandit:/tmp/archtrmntor$ mv data8.bin data.gz
bandit12@bandit:/tmp/archtrmntor$ ls
data.gz
bandit12@bandit:/tmp/archtrmntor$ gzip -d data.gz 
bandit12@bandit:/tmp/archtrmntor$ ls
data
bandit12@bandit:/tmp/archtrmntor$ file data 
data: ASCII text
bandit12@bandit:/tmp/archtrmntor$ cat data
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

- Here we got the password for next level .

## <span style="color: orange;"><b>--> Level 13</b></span>


#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit13 -p 2220``` <br>
```Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn```


#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on


#### <span style="color: green;"><b>About :</b></span> 

An alternate to a password for logging into SSH is public-key cryptography . In essence, the public key is stored on the computers that the key's owner (the user) wishes to grant access to. The private key, similar to a password, should be known only to the user. The -i flag is used to facilitate login using the private key.

SCP, which operates via SSH, is a tool for transferring data across a network. If you want to retrieve a file from a remote host, the command would look like this: scp -P <port> <user>@<IP>:<remotefilepath> <localfilepath>. To transmit a file to a remote host, the local file path should be specified first.

We first have to change the permission of the file using `chmod 600 sshkey.private ` . The `600 is octal representation of the permission , where 6 stands for read and write permissions for the owner, and no permissions for the group and others. 

Another approach when SSH access isn't available is to start a basic web server using Python. In the same directory as the file you wish to transfer, start the server with the command python3 -m http.server. On the other device, you can receive the file by issuing an HTTP request with the command wget http://<ip>:8000/<pathtofile>. This method is particularly useful when traditional file transfer methods are not an option.


#### <span style="color: green;"><b>Solution :</b></span>

1. First change the permission of the downloaded file `sshkey.private` using `chmod 600 sshkry.private` . 
2. Now , use the file to connect to another user using the command `ssh bandit.labs.overthewire.org -l bandit14 -p 2220 -i sshkey.private` 


```
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ cat sshkey.private 
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----
```

- save this in the localhost on you machine and change the permission 

```
┌──(archtrmntor㉿kali)-[~]
└─$ chmod 600 sshkey.private 

┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit14 -p 2220 -i sshkey.private
```

- and we got the ssh shell to bandit14 .


## <span style="color: orange;"><b>--> Level 14</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit14 -p 2220 -i sshkey.private``` <br>
```Password:- sshkey.private```



#### <span style="color: green;"><b>Task :</b></span>

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

#### <span style="color: green;"><b>About :</b></span>

localhost is hostname and its IP address is `127.0.0.1`. When you connect to localhost, you're establishing a connection to your own machine, which is useful for testing applications, accessing local services, or running a server and client on the same device.

The command nc, often known as netcat, enables reading and writing of data across a network connection. Both TCP and UDP connections can use it. The command syntax to establish a client connection to a network service is nc <host> <port>. The command to start a server that receives incoming packets is nc -l <port>.

#### <span style="color: green;"><b>Solution :</b></span> 

1. First , we need to find the password for bandit14 but we know that its in `/etc/bandit_pass/bandit14`.
2. then we need to submit the password to port number 30000 on localhost . we can use the nc to submit the password on port 30000

```
bandit14@bandit:~$ ls -lah
total 24K
drwxr-xr-x  3 root root 4.0K Jul 17 15:57 .
drwxr-xr-x 70 root root 4.0K Jul 17 15:58 ..
-rw-r--r--  1 root root  220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root root 3.7K Mar 31 08:41 .bashrc
-rw-r--r--  1 root root  807 Mar 31 08:41 .profile
drwxr-xr-x  2 root root 4.0K Jul 17 15:57 .ssh
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
bandit14@bandit:~$ 
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

- Here we got the password for the next level .

## <span style="color: orange;"><b>--> Level 15</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit15 -p 2220```
```Password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

#### <span style="color: green;"><b>About :</b></span> 

A library for safe network communication is called OpenSSL. It puts into practice the Secure Sockets Layer (SSL) and Transport Layer Security (TLS) cryptographic protocols, which are used, for instance, in HTTPS to encrypt online traffic.

The implementation of a basic SSL/TLS client that connects to a server is called `openssl s_client`.


#### <span style="color: green;"><b>Solution :</b></span>

- Since password can be retrieved using SSL encryption  , we can connect to localhost with Openssl client and send the password from this level , server then sends back the password for the next level. 


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit15 -p 2220  
--------------------------------------------------------

bandit15@bandit:~$ ncat localhost 30001 --ssl
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

- Here we got the password for the next level . 


## <span style="color: orange;"><b>--> Level 16</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit16 -p 2220``` <br>
```Password: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx```

#### <span style="color: green;"><b>Task :</b></span>

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

#### <span style="color: green;"><b>About :</b></span>

Nmap is a network scanner tool . It can do Host discovery , portscanning , version discovery , service detectino and operating system detection and much nmore . For this `-p` flag is used to define the port randge or the port number we want to scan 

[Nmap Reference Guide](https://nmap.org/book/man.html)

#### <span style="color: green;"><b>Solution :</b></span>

-  as we need to find the port beetween the range `31000 to 32000` , so we use the nmap with `-p` flag , such as `nmap localhost -p 31000-32000` to scan the port range with `-sV` flag which is used to do service/version detection scan , to know which service or service verion running on the port .  

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit16 -p 2220  
--------------------------------------------------------

bandit16@bandit:~$ nmap localhost -sV -p 31000-32000
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-24 14:32 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00060s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```

- as we find two port running the ssl service , in which is in only running echo service , and other is unkown . 
- so we try to coonec to the port on localhost with unknown mark and send the password 

```
Nmap done: 1 IP address (1 host up) scanned in 0.09 seconds
bandit16@bandit:~$ ncat localhost 31790 --ssl
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

- ans we got he private SSH key . now we can create a file and save this to connect with ssh using `-i` flag , and changing the permission, like in level 14 .



## <span style="color: orange;"><b>--> Level 17</b></span>

#### <span style="color: green;"><b>login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit17 -p 2220 -i sshkey.private``` <br>
```Password: sshkey.private```



#### <span style="color: green;"><b>Task :</b></span>

There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**


#### <span style="color: green;"><b>About :</b></span>

you can use the `diff` command in Linux. This command compares two files line by line and shows the differences between them.

#### <span style="color: green;"><b>Solution :</b></span>


- If you run:

`diff passwords.old passwords.new`

- And get output like this:

`< oldpassword123 `
`> newpassword456`

- The line `> newpassword456` is the changed line in `passwords.new` and is the password for the next level.

```
┌──(archtrmntor㉿kali)-[~]
└─$ nano sshkey.private       

┌──(archtrmntor㉿kali)-[~]
└─$ chmod 600 sshkey.private

┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit17 -p 2220 -i sshkey.private
---------------------------------------------------------------------------

bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
bandit17@bandit:~$ diff passwords.old passwords.new | grep '>' | cut -d ' ' -f2
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
bandit17@bandit:~$ 
```

- Here we got the password for another level . 


## <span style="color: orange;"><b>--> Level 18</b></span>


#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit18 -p 2220``` <br>
```Password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO```

#### <span style="color: green;"><b>Task :</b></span>

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

#### <span style="color: green;"><b>About :</b></span>

Every time a terminal is loaded, a file called ".bashrc" is executed. This indicates that since logging in via SSH loads a terminal as well, it is also executed during that process.

SSH does not just allows us to log into a machine remotely, but it also allows remote execution of commands by adding the commands after the common SSH expression.

#### <span style="color: green;"><b>Solution :</b></span>

1. We use SSH to run a command through the system rather than logging in. Before using cat to examine the readme file, we use ls to confirm that it is present in the folder.
2. then we can read the file content using the command `ssh bandit.labs.overthewire.org -l bandit18 -p 2220 cat readme` or 
3. we can run `/bin/bash` or `-t /bin/bash` this will help in spawn a bash shell .


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit18 -p 2220 ls
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
readme
                                                                               
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit18 -p 2220 cat readme
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

- Here is the password for the another level. 


## <span style="color: orange;"><b>--> Level 19</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit19 -p 2220```
```Password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8```

#### <span style="color: green;"><b>Task :</b></span>

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

#### <span style="color: green;"><b>About :</b></span>

Each file in Linux has an owner and a group that owns the file. The permissions for the user, group, and other users can be set separately, determining whether the file is readable, writable, or executable. The ls -l command displays the file's owner, group, and permissions. The permissions are represented as a string of nine characters (e.g., rwxrwxrwx), with the first three denoting the user's permissions, the next three for the group, and the last for others. If a permission is not granted, the corresponding character is replaced with a '-'.

A special permission called Suid (Set User ID) can be set for specific binaries. When a binary has Suid permissions, it runs with the permissions of the file's owner rather than the user who executes it. To give a binary Suid permissions, use the command chmod u+s <filename>. This can be useful in certain scenarios, such as allowing users to run a program with elevated privileges, but it should be used with caution due to potential security risks.

#### <span style="color: green;"><b>Solution :</b></span>

- First check the owner of the setuid binary file using `ls -la`

```
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ls -lah
total 36K
drwxr-xr-x  2 root     root     4.0K Jul 17 15:57 .
drwxr-xr-x 70 root     root     4.0K Jul 17 15:58 ..
-rwsr-x---  1 bandit20 bandit19  15K Jul 17 15:57 bandit20-do
-rw-r--r--  1 root     root      220 Mar 31 08:41 .bash_logout
-rw-r--r--  1 root     root     3.7K Mar 31 08:41 .bashrc
-rw-r--r--  1 root     root      807 Mar 31 08:41 .profile
```
- the owner is badit20 and the group is bandit19, this with ‘-rwsr-x—’ means the user bandit19 can execute the binary, but the binary is executed as user bandit20.

- it simply means executing commands as another user , means we can read the password file to bandit20 , easily .

```
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass
cat: /etc/bandit_pass: Is a directory
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
bandit19@bandit:~$ 
```

- Here i got the password for next level.


## <span style="color: orange;"><b>--> Level 20</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit20 -p 2220``` <br>
```Password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO```

#### <span style="color: green;"><b>Task :</b></span>


There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

#### <span style="color: green;"><b>About :</b></span>


 To create a server on localhost, the `-l` flag (which means listening) is needed. To specify a port, the `-p` flag is needed. To create a “onetime server”, a server that sends one message and then disconnects, we can use the pipe (`|`) and `echo` to input the message.

If a command needs to be run, but you don’t need to interact with it for a while and want to keep using the same terminal with other commands while the command is executing, you can use `&`. The ampersand will send the command in the background. This is a part of the Linux process management. Specifically, if you want to learn more about this, also look into the `jobs` command, it shows processes/commands/jobs running in the background and foreground.

#### <span style="color: green;"><b>Solution :</b></span>

- first we need to create a server mode using netcat - to listen for inblound connect. we can use echo to send the password with netcat with `-n` flag is to prevent newline characters in the input , let run the process ,meanwhile


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit20 -p 2220 
-------------------------------------------------------------------------


bandit20@bandit:~$ echo -n '0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO' | nc -nvlp 1234
Listening on 0.0.0.0 1234
Connection received on 127.0.0.1 36176
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```
- run te setuid binary with port 1234 means it will connect to our server , recives the password inputed through the `echo` and sends back the next password.

```
bandit20@bandit:~$ ./suconnect 1234
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password

```

- yup , we got the password for next level.

## <span style="color: orange;"><b>--> Level 21</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit21 -p 2220``` <br>
```Password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt```

#### <span style="color: green;"><b>Task :</b></span>

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

#### <span style="color: green;"><b>About :</b></span>

Programs that run automatically on a regular basis are known as cronjobs. These cronjobs can be found in several Linux files, including cron.d, cron.daily, cron.hourly, cron.monthly, crontab, and cron.weekly. The directories hold files containing runtime instructions for the programs. The first five columns set forth the schedule for when and how often the program should be run. The command or program that has to be run comes next.

#### <span style="color: green;"><b>Solution :</b></span>

- we can want to list the content of `/etc/cron.d` , for this level , we have work with `cronjon_bandit22`.


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit21 -p 2220   
----------------------------------------------------------------

bandit21@bandit:~$ ls -lah /etc/cron.d
total 44K
drwxr-xr-x   2 root root 4.0K Jul 17 15:59 .
drwxr-xr-x 121 root root  12K Aug  1 14:49 ..
-rw-r--r--   1 root root  120 Jul 17 15:57 cronjob_bandit22
-rw-r--r--   1 root root  122 Jul 17 15:57 cronjob_bandit23
-rw-r--r--   1 root root  120 Jul 17 15:57 cronjob_bandit24
-rw-r--r--   1 root root  201 Apr  8 14:38 e2scrub_all
-rwx------   1 root root   52 Jul 17 15:59 otw-tmp-dir
-rw-r--r--   1 root root  102 Mar 31 00:06 .placeholder
-rw-r--r--   1 root root  396 Jan  9  2024 sysstat
bandit21@bandit:~$ ls -lah /etc/cron.d/cronjob_bandit22
-rw-r--r-- 1 root root 120 Jul 17 15:57 /etc/cron.d/cronjob_bandit22
bandit21@bandit:~$ cat  /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
- we can see a file running as bandit22 `/usr/bin/cronjob_bandit22.sh` , five star indicate that it runs every five minutes . 
```
bandit21@bandit:~$ cat  /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
- we can cat out the contain of the file , what exactly happening in the bash file.
- in this it reading the password for the `bandit22` user and saving it some `tmp` file `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`
- we can read the content of the file to get the password for bandit22.

```
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
bandit21@bandit:~$ 
```

- Here we got the password for the next level . 


## <span style="color: orange;"><b>--> Level 22</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit22 -p 2220``` <br>
```Password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q```

#### <span style="color: green;"><b>Task :</b></span>

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

#### <span style="color: green;"><b>About :</b></span>

Variables in bash scripting are new in this work. A variable can be thought of as a value's container. In Bash scripting, a variable can be declared using the following syntax: Var_name and var_value are equal. The output of a command can be saved in a variable using the following syntax: var_name = $(instruction). Use this method to retrieve the value of an existing variable: $var_name

#### <span style="color: green;"><b>Solution :</b></span>

- first list out the content of `/etc/cron.d` , we are working for bandit23 user.

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit22 -p 2220 
-----------------------------------------------------------

bandit22@bandit:~$ ls -lah /etc/cron.d/
total 44K
drwxr-xr-x   2 root root 4.0K Jul 17 15:59 .
drwxr-xr-x 121 root root  12K Aug  1 14:49 ..
-rw-r--r--   1 root root  120 Jul 17 15:57 cronjob_bandit22
-rw-r--r--   1 root root  122 Jul 17 15:57 cronjob_bandit23
-rw-r--r--   1 root root  120 Jul 17 15:57 cronjob_bandit24
-rw-r--r--   1 root root  201 Apr  8 14:38 e2scrub_all
-rwx------   1 root root   52 Jul 17 15:59 otw-tmp-dir
-rw-r--r--   1 root root  102 Mar 31 00:06 .placeholder
-rw-r--r--   1 root root  396 Jan  9  2024 sysstat
```
- cat out the content of the file `/etc/cron.d/cronjob_bandit23`
- looking at the scipt `/usr/bin/cronjob_bandit23.sh` ,"Myname" is the first variable, and it stores the results of the whoami command. The whoami command will print "bandit23" since this script will be executed as bandit23. The final l'ine so informs us that a file in the '/tmp' folder will contain the password from bandit23. The line echo creates the filename. User <b>'$myname `|` md5sum `|` cut -d'' -f 1'</b> is who I am. All we have to do is replace $myname with bandit23, run it, and the filename will appear.


```
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$ whoami
bandit22
bandit22@bandit:~$ cat /etc/bandit_pass/8ca319486bfbbc3663ea0fbe81326349
cat: /etc/bandit_pass/8ca319486bfbbc3663ea0fbe81326349: No such file or directory
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
bandit22@bandit:~$ echo I am user bandit23  | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

The string "I am user bandit23" is entered into md5sum in the filename creation line, and the program outputs the md5 hash derived from the string. Everything after the space is removed by the final instruction, and that's how we got the password for the next level. 


## <span style="color: orange;"><b>--> Level 23</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit23 -p 2220 ```
```Password: forgot to save```

#### <span style="color: green;"><b>Task :</b></span>

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

#### <span style="color: green;"><b>About :</b></span>

Like in previous level , it also deals with cronjobs and bash scripting .

#### <span style="color: green;"><b>Solution :</b></span>

- first we have to look into cronjobs `cat /etc/cron.d/cronjob_bandit24` then scripts they are running 

```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit23 -p 2220 
------------------------------------------------------------------

bandit23@bandit:/tmp/tmp.0ZBsjCbm2W$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
- After the script runs, every file in the "/var/spool/bandit24" folder is deleted. This is the situation since the variable "myname" has the value "bandit24" because it is being run as the bandit24 user. The files are accessed via the for loop. The first if statement ensures that the current and preceding folders, represented by the directories "." and "..," are ignored. The code to run a script is enclosed in an if statement, but it will only run if bandit23 is the owner. The file will thereafter be removed.
- We can write a script that will provide us with the bandit24 password as we are now logged in as the bandit23 user. Make a file in the "tmp" folder first. This keeps the file from being deleted too soon and gives you a backup in case something goes wrong. The file will then be run after being moved to the "/var/spool/bandit24" folder.
- make a temp directory using `mktemp -d`
- then change to that directory 
- then create this script.

```bash
my script.sh

"
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.0ZBsjCbm2W/passwd
"
```
- give the necessary permission , and move the file into the current tmp folder . 
- then after a minute wait we can read the password file , and we get the password for user bandit24 

```
bandit23@bandit:/tmp/tmp.0ZBsjCbm2W$ cp new.sh /var/spool/bandit24/foo/nothing.sh
bandit23@bandit:/tmp/tmp.0ZBsjCbm2W$ cat passwd
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

- Here we got the password for next level .

## <span style="color: orange;"><b>--> Level 24</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit24 -p 2220```
```Password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8```

#### <span style="color: green;"><b>Task :</b></span>

A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time

#### <span style="color: green;"><b>About :</b></span>

we have to use the previous level knowledge and bashing scripting ( specially loops for this level)

A **for-loop** in bash has the following syntax :

```bash
for var in 1 2 .... N
do 
    #code here for something
done
```

#### <span style="color: green;"><b>Solution :</b></span>

- we can't check password for everypincode one by one so we have to automate this thing 

```bash
#!/bin/bash
for i in {0000..9999}
do
        echo UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ $i >> possibilities.txt
done
cat possibilities.txt | nc localhost 30002 > result.txt
```
- and upon reading the file we got the our password s success 

```
bandit24@bandit:/tmp/tmp.RI7ykKeRrt$ ls
new.sh
bandit24@bandit:/tmp/tmp.RI7ykKeRrt$ chmod +x new.sh 
bandit24@bandit:/tmp/tmp.RI7ykKeRrt$ ./new.sh 
bandit24@bandit:/tmp/tmp.RI7ykKeRrt$ ls
new.sh  possibilities.txt  result.txt
bandit24@bandit:/tmp/tmp.RI7ykKeRrt$ cat result.txt 
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

- Here we got the password for next level.


## <span style="color: orange;"><b>--> Level 25</b></span> 

#### <span style="color: green;"><b>login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit25 -p 2220 ```
```Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4```

#### <span style="color: green;"><b>Task :</b></span>

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use Powershell to `ssh` into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

#### <span style="color: green;"><b>About :</b></span>

what shell is the default for a user, can be found at the end of the line for the user in the ‘/etc/passwd’ file.

One shell command that enables interactive file display is called `more`. To be more precise, this interactive mode is only functional in situations where the file's content is too big to fit entirely on the terminal window. The command "v" is one that can be used in the interactive mode. Using this command, the file will open in the "vim" editor.

![Level 25-26](/assets/img/Pasted image 20240824220112.png)

`Vim` is an editor for text. You can also execute shell commands with it. Vim can be used to generate a shell and escape a restricted environment. Use of the command :shell spawns the user's default shell. The command :set shell=/bin/sh can be used to convert the shell to "/bin/bash."


#### <span style="color: green;"><b>Solution :</b></span>

1. we whave to set the shell variable to `/bin/bash` then we enter `:shell` we got the bash shell .
2. Then we will find a private key in the file , copy-paste the private key in to a file on our machine , give the necessary permission to it .


```
┌──(archtrmntor㉿kali)-[~]
└─$ ssh bandit.labs.overthewire.org -l bandit25 -p 2220 
---------------------------------------------------------------------
v
:set shell=/bin/bash
:shell
```

- Now we can move to the next level .

## <span style="color: orange;"><b>--> Level 26</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>
```SSH: ssh bandit.labs.overthewire.org -l bandit26 -p 2220 -i sshkey.private```<br>
```Password : sshkey.private```


#### <span style="color: green;"><b>Task :</b></span>


Good job getting a shell! Now hurry and grab the password for bandit27!

#### <span style="color: green;"><b>About :</b></span>

explained in the previous level . 

#### <span style="color: green;"><b>Solution :</b></span> 

1. we can see a file names `bandit27-do` in the `bandit26's` home directory .
2. Its similar to old challenge where we can run the command as another user .
3. and by this privilege we can grab the password `/etc/bandit_pass/bandit27` for **bandit27**

```
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do id
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
bandit26@bandit:~$ 
```

- Here we got the password for another level .
  

## <span style="color: orange;"><b>--> Level 27</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit27 -p 2220 ``` <br>
```Password: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB```

#### <span style="color: green;"><b>Task :</b></span>

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

#### <span style="color: green;"><b>About :</b></span>

According to https://git-scm.com, "Git is a free and open source distributed version control system." It enables you to store not only your code but also its history and modifications. Additionally, it simplifies teamwork and communication when working on the same code.

Some of the git command for our use case , there are lot of them :
    - `git init` :- to create a new git reposiroty/project 
    - `git clone` :- to copy an existig git repository
    - `git push` :- updates remote repository
    - `git pull` :- to get updates form remote repository , we can also use the Git client that has [GUI interface](https://git-scm.com/downloads/guis) . 

Every piece of data needed for version control is contained in the '.git' directory. It has details about commits, the address of the remote repository, a log, and more.

Frequently located in a git repository is the README file. It serves as a summary of every file in a directory or within a git repository. A README file helps developers and users recall the purpose of the project and other information they may require when starting a git project.

#### <span style="color: green;"><b>Solution :</b></span> 

1. First make a temp directory to clone the project using `mktemp -d`
2. afterwards clone the repository usign the the command `it clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo` 
3. then change to that clone github directory
4. then list out the content of the directory usnig the `ls -la`,
5. There will be a file name README that containt he password of the next level.

```
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
-----------------------------------------------------------------

bandit27-git@localhost's password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3$ ls
repo
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3$ cd repo
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3/repo$ ls
README
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3/repo$ cat README 
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
bandit27@bandit:/tmp/tmp.r6HhAVUWZ3/repo$ 
```

- Here we go the password for the next level .

## <span style="color: orange;"><b>--> Level 28</b></span>

#### <span style="color: green;"><b>Login Info :</b></span> 

```SSH: ssh bandit.labs.overthewire.org -l bandit28 -p 2220``` <br>
```Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN```

#### <span style="color: green;"><b>Task :</b></span>

There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

#### <span style="color: green;"><b>About :</b></span>

git has so many features and function . git track the commit logs ( what changes are made ) with a commit number . Here are some command we need to know related to that :
    - `git log` :- shows us the commit log.
    - `git show <commit>` :- reveals the contents of a commit (It's crucial to be mindful of the data you upload to a public repository because both updates and earlier iterations are preserved). Passwords and other sensitive information may therefore still be recovered.

#### <span style="color: green;"><b>Solution :</b></span>

- like in the prevous level make tmp directory and clone the git repository , and chnage to the /tmp directory we have created .


```
bandit28@bandit:/tmp/tmp.DSIhAfbhgZ$ git clone  ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit28/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit28/.ssh/known_hosts).
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit28-git@localhost's password: 
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit28@bandit:/tmp/tmp.DSIhAfbhgZ$ 
```

- when we read the README file , it says `password: xxxxxxx` , means we have to see the git history to see what changes are made to the README file .

```
bandit28@bandit:/tmp/tmp.DSIhAfbhgZ/repo$ strings README.md 
# Bandit Notes
Some notes for level29 of bandit.
## credentials
- username: bandit29
- password: xxxxxxxxxx
```

- `git log` shows the log history of the commits , 
- one commit has the description **fix info leak** , lets see what changes were made .

```
bandit28@bandit:/tmp/tmp.DSIhAfbhgZ/repo/.git$ git log
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md
```
- we can use the command `git show <commit>` to see the commit 

```
bandit28@bandit:/tmp/tmp.DSIhAfbhgZ/repo/.git$ git show 8cbd1e08d1879415541ba19ddee3579e80e3f61a
commit 8cbd1e08d1879415541ba19ddee3579e80e3f61a (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
+- password: xxxxxxxxxx
```

- Here we got the password for the next level , in the git logs .

## <span style="color: orange;"><b>--> Level 29</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit29 -p 2220``` <br>
```Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7```

#### <span style="color: green;"><b>Task :</b></span>

There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

Clone the repository and find the password for the next level.

#### <span style="color: green;"><b>About :</b></span> 

Another element of the version control system is Git branching. It enables you to divide the development process into several branches. To be more precise, the program can be pulled from the master branch and worked on independently. While keeping a functional master branch, you can add and modify features. It can be reintegrated into the master branch after the work is finished. This makes more version control possible. It is possible to provide a usable software in a production branch and add functionality or fix issues in a separate development branch.

Here some basic command to interact with branches :
    - `git branch` :- list (`-a`), create , or delte branches
    - `git checkout <branch_name>/git switch <branch_name>` :- switches branches
    - `git merge` :- Join two or more branches 

#### <span style="color: green;"><b>Solution :</b></span> 

- same as the previous level , create a tmp folder , clone the repository , then move that git directory .
- on reading the README file it says `password: <no passwords in production!>` , there might be some more branches to work on . 
- we can check using the `git branch -a`
- here some mulitple branch `dev`, `master`, `sploits-dev` . as description , we might be intrested in the dev branch . Since if the password is not in the production branch, it is most likely in the development branch.
- 
```
bandit29@bandit:/tmp/tmp.4MBIAiYbey/repo/.git$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```
- Lets checkout its content , we can switch to the `dev` branch using the command `git checkout dev` . 

```
bandit29@bandit:/tmp/tmp.4MBIAiYbey/repo$ git checkout dev
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
bandit29@bandit:/tmp/tmp.4MBIAiYbey/repo$ ls
code  README.md
bandit29@bandit:/tmp/tmp.4MBIAiYbey/repo$ cat README.md 
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

- Here in this branch we got the password in the README file for the next level. 



## <span style="color: orange;"><b>--> Level 30</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit30 -p 2220```
```Password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL```

#### <span style="color: green;"><b>Task :</b></span>

There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

#### <span style="color: green;"><b>About :</b></span>

Here is another git tagging , is a means of designating particular moments in the repository's history. Marking the software's release points is one example. Git tag is the command to view the tags. The command git tag -a <tag_name> -m <"tag description/message"> creates a new tag. Use the command git show <tag_name> to view more information, including the commit and tag message.

#### <span style="color: green;"><b>Solution :</b></span>  

- same as the previous level , create a tmp folder , clone the repository , then move that git directory .

```
bandit30@bandit:/tmp/tmp.mCPRJDkKA8$ cd repo
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo$ cd .git
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo/.git$ ls
branches  description  hooks  info  objects      refs
config    HEAD         index  logs  packed-refs
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo/.git$ cd ..
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo$ cat README.md 
just an epmty file... muahaha
```

- The README does not give us any information. Checking the git tag, we find a point in the history called ‘secret’, which looks promising.

```
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo$ git tag
secret
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
bandit30@bandit:/tmp/tmp.mCPRJDkKA8/repo$ 
```
- Here we got the password for another level 



## <span style="color: orange;"><b>--> Level 31</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit31 -p 2220```
```Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy```

#### <span style="color: green;"><b>Task :</b></span>

There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo` via the port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.

Clone the repository and find the password for the next level.

#### <span style="color: green;"><b>About :</b></span>

`Git Commit` stores the modifications that are presently being made together with a note outlining them. All altered or deleted files are ensured to be staged by the flag -a.

Local modifications are updated in distant repositories via `Git Push`. You ought to define the branch with `-u` when pushing for the first time.

A file with the extension ".gitignore" is called `Git Ignore`. All file names and extensions that the commit should ignore are written in this file. This implies that a file that is included in the ignore file will not be included in the commit or repository if it is created or modified. Wildcards are also supported by `git ignore`. (For instance, '*.pyc' indicates that all files ending in '.pyc' shall be disregarded.) Pre-written files (such as this Python file) are available for use with particular scenarios and languages.

Git Add modifies the files that will be included in the upcoming commit. Files that are often ignored must be able to be committed in order for the -f flag to be used.

#### <span style="color: green;"><b>Solution :</b></span>  

- start same as always 
- We must push a file to the repository, according to the `'README'`. Thus, we start by creating the file:
- We may now attempt to push the updated file by committing it first and then pushing it. But because of the file `".gitignore"` in the repository, this won't function. The files/-types that git will not push to the repository are listed in this file. with this instance, every file that ends with **".txt."**
- Git add is required in order to add the file in any case. After that, make a commitment and push. The commit will launch the "nano" editor and anticipate a message, which you are free to write.

Commands involve :
    - `echo 'May I come in?' > key.txt`
    - `ls -la`
    - `cat key.txt`
    - `git add -f key.txt`
    - `git commit -a`
    - `git push -u origin master`

Note :- if things got complex for , please do visit `hackersploit` bandit youtube solution ( great resource to learn more in depth )

```
 Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K 
```
- Here we got the password for another level .



## <span style="color: orange;"><b>--> Level 32</b></span>

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit32 -p 2220``` <br>
```Password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K```

#### <span style="color: green;"><b>Task :</b></span>

After all this `git` stuff, it’s time for another escape. Good luck!

#### <span style="color: green;"><b>About :</b></span> 

Variables in Linux are categorized as environment variables (valid across the system), shell variables (made up by the shell), and local variables (valid in the current shell). The names of these variables are only written in uppercase. By using VAR_NAME=var_value in the command line, they are defined. You can use echo $VAR_NAME to view the contents of a variable and we can use the printenv to print all the enviroment variable .


Some common that are good to know are:
    - `TERM` :-  current terminal emulation <br>
    - `HOME` :- the path to home directory of currently logged in user <br>
    - `LANG` :- current locales settings <br>
    - `PATH` :- directory list to be searched when executing commands <br>
    - `PWD` :- pathname of the current working directory <br>
    - `SHELL/0` :- the path of the current user’s shell <br>
    - `USER` :- currently logged-in user <br>

#### <span style="color: green;"><b>Solution :</b></span> 

- here we are granted a little different sheel we we got hint `WELCOME TO UPPERCASE SHELL` 
- It appears that everything we type is capitalized. Nevertheless, the instructions we have tried thus far are all lowercase and are ineffective. Variables are the only capital word in Linux. In particular, a shell is referenced by the variable $0. If you use echo $0 on your computer, you can see this.
- we can break out of the uppercase shell ans we can use the commands again ,
- 

```bash
>> $0
$ ls -la
total 28
drwxr-xr-x  2 root     root     4096 May  7  2020 .
drwxr-xr-x 41 root     root     4096 May  7  2020 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
-rwsr-x---  1 bandit33 bandit32 7556 May  7  2020 uppershell
```
- It is evident that the file "uppershell" (owned by user "bandit33" and SUID) is executed as bandit33. By verifying this, we are able to access the password file since we are, in fact, "bandit33":

```
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

- here we got the password for another level .

## <span style="color: orange;"><b>--> Level 33</b></span> 

#### <span style="color: green;"><b>Login Info :</b></span>

```SSH: ssh bandit.labs.overthewire.org -l bandit33 -p 2220```
```Password: tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0```

#### <span style="color: green;"><b>ENDING :</b></span> 

- Here we got the simple message when we read the file . 

```
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```

-------------------------------------END---------------------------------------------

This is just the beginning! In the coming days, I'll be sharing more blog posts, write-ups on HackTheBox, TryHackMe, CrackMe challenges, and other exciting topics. This is my first blog, and I'd love to hear your thoughts on it. Please connect with me on Twitter and LinkedIn, and feel free to drop me a message if you enjoyed reading it—I'd be happy to hear from you!

Currently, I'm diving into Active Directory, and I'll definitely be posting about that soon. I'll be sharing my journey and insights here on my blog, so stay tuned for more interesting articles. Keep in touch with me on Twitter and LinkedIn.

I spend a lot of time learning and working on my laptop, and I encourage you to stay curious, work hard, and dive into cool, amazing things too!  


#HAPPYHACKING 