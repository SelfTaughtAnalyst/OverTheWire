# Bandit Level 0 -> Level 1
**URL:** https://overthewire.org/wargames/bandit/bandit1.html

## Level Goal
The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit0\
**Password:** bandit0

## Walkthrough
The commands we'll need to get through this level will teach us the very basics of interacting with files in Linux.

**Home Directory**\
The prompt tells us that the file we're looking for is located in the home directory. If you're familiar with Windows, the home directory there is usually located at `C:\Users\<USER>`. In Linux, the home directory for a user is usually located at `/home/$USER`, so in this case we're looking for `/home/bandit0`. To figure out where we currently are within the file system, we can use `pwd` to print our working directory.

```
bandit0@bandit:~$ pwd
/home/bandit0
```

So we've started out right where we need to be, sweet!

You might have noticed the tilde (`~`) in the prompt. This is Linux's shorthand for the home directory of whatever user you're logged in as. In other words, `~` is the same as `/home/bandit0` when we're talking about directories. It's sort of the Windows equivalent of `%USERPROFILE%`.

**Listing Directory Contents**\
Now that we know we're in the right place, we can see what files are in our current directory by using the `ls` command.

```
bandit0@bandit:~$ ls
readme
```

There's the readme file mentioned in the prompt.

**File Types**\
Before we go about trying to read the contents in the readme, let's talk quickly talk about file types. Unlike Windows, file extensions are usually irrelevant in Linux. They can be excluded or just out right wrong. People usually include them to make it easy to identify which files are what at a glance, but they aren't necessary. To figure out what we're looking at, we can use the `file` command on the file we want to identify. The syntax would be `file <filename>`.

```
bandit0@bandit:~$ file readme
readme: ASCII text
```

So the readme is an ASCII text file which means it's human readable and we should be able to view and understand the contents.

If you're not familiar with ASCII, the short version of it is that it's a type of character encoding that takes information that the computer understands and translates them into standard English characters, numbers, and symbols that we can more easily read.

**Reading the File**\
*Enough with this nonsense, just show me how to read the file!* you scream internally.

There are many ways to view what's in a file. In this case, we'll use `cat` to output its contents directly into our terminal. The syntax is `cat <filename>`.

```
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

Save that password somewhere as we'll need it to log into level 1.

And there it is, the password to to level 1. Save it somewhere and let's see what's in the next level.

**Exiting**\
To close your current session, just type `exit`.
```
bandit0@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.
```
