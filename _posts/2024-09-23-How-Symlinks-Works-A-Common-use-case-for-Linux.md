---
title: "How Symlink Works - A common use case for linux"
description: "Creating Shortcut in Linux"
date:  2024-09-23
categories: [Blog]
tags: [Symlink, Symbolic Link , soft link , hard link , shortcut]
---
I learned about how to create a symlink , but I din't understand a single use case for a symlink , till i encounter a hackthebox active machine permx .  Let's disuss little about `symlink`

#### <span style="color: Orange;"><b># What do we understand by _**Symlink**_ in first palce ?</b></span>

Symlink or symbolic link is nothing just a shortcut to another file or directory . 

- Here how we can create a symbolic link 


```bash
ln -s [OPTION] FILE_LINK
```

Here : <br>
    - `ln -s` : command creates a symlink <br>
    - `first argument` : it is the existing file or directory which we want to create symlink or shortcut for <br>
    - `second argument` : it is the link to the first argument or ( this is the shortcut file) <br>


```bash
$ echo "Nothing Here" > source.file
$ ln -s source.file nothing.file
$ cat nothing.file
Nothing Here
```

Command :  `ln -s source.file nothing.file`

![[Image]](https://i.imghippo.com/files/bzX5k1727067028.png)

For More : `man ln` 

- Creating a symlink for folder

```bash
ln -s /home/archtrmntor  archtrmntor
```

- Removing the Symlink file

```bash
rm <path-to-symlink>
```

- you can use the unlink to remove a `Symlink`

```bash
unlink <path_to_symlink>
```

#### <span style="color: Orange;"><b># What is _**Soft Link**_ 🖇️ and ***Hard Link***</b></span> 💭 ?

A hard link is a mirror copy of the original file, while a symbolic or soft link is an actual link to the original file. The soft link is useless if the original file is deleted since it leads to an invalid file.

However, the situation with a hard link is quite different. The hard link will retain the original file's data even if you delete the original file. Considering that a hard link functions as a mirror copy of the source file.


- Let's Create  A SOFT LINK 🖇️

```bash
$ echo "Nothing Here" > source.file
$ cat source.file
Nothing Here
$ ln -s source.file nothing.file
$ cat nothing.file
Nothing Here
$ echo "Something Here" >> nothing.file
$ cat source.file
Nothing Here
Something Here
```

makign any changes to the shortcut (nothing.file) can directly be seen in the source.file .

![[Image]](https://i.imghippo.com/files/kvSid1727067029.png)

when we create the softlink , the file permission and inode number is different for both files , example below

![[Image]](https://i.imghippo.com/files/bzX5k1727067028.png)

if we remove the original file in case of softlink , then symlink ( shortcut ) is of no use 

- Creating a Hard Link 🖇️

```bash
$ echo "Nothing Here" > source.file
$ cat source.file
Nothing Here
$ ln source.file nothing.file
$ cat nothing.file
Nothing Here
```

In the case of hard link file share inode number and filepermission are the exact same original file . if we remove the original file ( source.file ) , and cat out the content of the `nothing.file` then we can still view the content of the `nothing.file`


#### <span style="color: Orange;"><b># wonder then what is the difference between normal coping a file and creating   a hard link </b></span>🖇️ . 

A file that is copied will simply have its content duplicated. Therefore, changing the content of one file (either the original or the hard link) has no bearing on the other.

Nevertheless, if you make a hard link to a file and alter one of the files, the modifications will appear in both.







------------------------------------------------------------------



##### Final Thoughts

 I hope this space continues to be helpful in your learning journey!. If you find this blog helpful, I’d love to hear your thoughts—my inbox is always open for feedback. Please excuse any typos, and feel free to point them out so I can correct them. Thanks for understanding and happy learning!. You can contact me on
[Linkedin](https://www.linkedin.com/in/hitesh-sharma-413862245) and [Twitter](https://twitter.com/archtrmntor)
