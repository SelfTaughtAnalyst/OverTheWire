# Bandit Level 1 -> Level 2
**URL:** https://overthewire.org/wargames/bandit/bandit2.html

## Level Goal
The password for the next level is stored in a file called - located in the home directory

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit1\
**Password:** boJ9jbbUNNfktd78OOpsqOltutMc3MY1

## Walkthrough
In this level, we're going to learn a little bit about keyboard interrupts, stdin/stdout, and absolute/relative file paths.

Log in using the `bandit1` as the username and the password we obtained from the last level. Depending on the terminal you're using, you may have to use `CTRL + SHIFT + V` to paste the password when prompted for it by `ssh` (if that's not it, do a bit of Googling or look at your terminal's preference/settings to see what the short cut for copying and pasting are). Once we log in, we can use `ls` to see what's in our home directory.

```
bandit1@bandit:~$ ls
-
```

There's a single file called `-`. Strange. Let's `cat` the file to see it's contents.
```
bandit1@bandit:~$ cat -
what is going on?
what is going on?
```

When we try to use `cat` on `-`, we're met with a blinking cursor. If you try to type anything in, the terminal just repeats back what you entered.

### Keyboard Interrupt
To get out of it, send a keyboard interrupt by pressing `CTRL + C`. This will tell your terminal to stop whatever it's doing and return you back to the prompt. Remember this shortcut because it's useful for exiting programs in the command line when you seemingly have no way out.

You might be wondering *If CTRL + C is an interrupt command in the terminal, how do I copy something then, because that's the shortcut I use to copy in other applications.* This depends on the terminal you're using, but `CTRL + SHIFT + C` is usually the shortcut to copy from the command line. If it isn't, look up your terminal's shortcuts in the preferences/settings or search it up on Google. It'll take a little bit of practice to break the habit of pressing `CTRL + C` to copy all the time in the command line, but you'll eventually get the hang of it.

Anyway, once we've sent the keyboard interrupt, we'll regain control of our prompt. Yay!

```
bandit1@bandit:~$ cat -
what is going on?
what is going on?
^C
bandit1@bandit:~$
```

You can also press `CTRL + D` to send an end of file (EOF) signal to the terminal to regain control of your prompt. How and why this works will be explained in the next section.

```
bandit1@bandit:~$ cat -
what is going on?
what is going on?
bandit1@bandit:~$
```

### STDIN/STDOUT Redirection with -
So what the heck just happened? Well it turns out that the dash `-` is a special operator that redirects `stdin` and `stdout`. When a dash is used in place of a file name, it causes a program to either accept input from `stdin` or redirects output to `stdout`, depending on the program used.

`stdin` (also known as standard input) is a data stream that programs use to receive input, which in our case is linked to our keyboard. Similarly, `stdout` (also known as standard output) is the data stream that programs output and that's linked to our display which is the terminal itself. The streaming part of this means that the data is sent continously until something signals that there is no more data to be had.

So when we run `cat -`, we are using the `cat` program to read `stdin` (input from our keyboard) instead of a file and `cat` by default outputs it to `stdout` (the terminal) which is why everything we type gets repeated back to us. Because `stdin` is a data stream, `cat` continues to wait for input until we either interrupt it (`CTRL + C`) to kill the program or send an end of file (`CTRL + D`) signal to say "Hey, this is the end of the file so there's no more data coming through."

As another example, you can try using the dash with the `file` command. Normally this would tell us what kind of file we're looking at, but because we used a `-`, `file` is going to read from `stdin` instead, which in this case is whatever we type, which will be plain text.

```
bandit1@bandit:~$ file -
testing
/dev/stdin: ASCII text
```

### Absolute and Relative File Paths
Now that we understand that `-` is meant for redirection, how do we read a file that's named `-`? One way to do it is to specify the file path so we're not just typing `-` by itself. If you forget, we can check what directory we're in with `pwd` and use that to help specify the entire (absolute) file path.

```
bandit1@bandit:~$ pwd
/home/bandit1
bandit1@bandit:~$ cat /home/bandit1/-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

Since this file is located in the home directory, we can use the `~` shortcut instead of typing out `/home/bandit1` to specify the absolute file path.

```
bandit1@bandit:~$ cat ~/-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

Instead of typing out the entire path which can sometimes be very long if you're several directories deep, you can use relative paths as well. The period `.` is a shortcut for representing your current directory.

```
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

This isn't used for this level, but for the sake of completeness, two periods in a row `..` is the shortcut to reference the directory one level above (in this case `/home`). We'll use these relative file path references more as we progress through the levels and I'll elaborate more when we come across it again.
