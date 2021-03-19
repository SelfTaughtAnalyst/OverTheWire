# Bandit Level 3 -> Level 4
**URL:** https://overthewire.org/wargames/bandit/bandit4.html

## Level Goal
The password for the next level is stored in a hidden file in the inhere directory.

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit3\
**Password:** UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

## Walkthrough
Once we log in, we can see the inhere directory with `ls`.

```
bandit3@bandit:~$ ls
inhere
```

### Navigating The File System
To move ourselves out of our home directory and into `inhere`, we'll need to help of the `cd` command. Using the syntax `cd <path>` we can change directories (or folders, as some people like to call them) using either absolute or relative paths (something we talked a little bit about in [level 1](../Level-01/README.md#absolute-and-relative-file-paths)). So in this case, we can use `cd /home/bandit3/inhere` (absolulte) or `cd ./inhere` (relative). If the location we're referring to is in our current directory, we can omit the preceeding `./` and just use `cd inhere`. (Reminder: `.` represents your current directory when talking about file paths which is why we can drop it because `cd` is going to look at your current directory by default.)

```
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ pwd
/home/bandit3/inhere
```

A quick aside on `cd`: using relative paths, we can navigate back up a folder by using `..` to reference the parent directory of where we currently are. We can chain the `..` to move up several folders at once. For example, if we're in `inhere` and we run `cd ../..`, this will take us up two directories into `/home`.

```
bandit3@bandit:~/inhere$ cd ../..
bandit3@bandit:/home$ pwd
/home
```

To get back to `inhere` we can use the full path `cd /home/bandit3/inhere`, the relative path `bandit3/inhere` or we can use a dash (`-`) which `cd` will treat as a shortcut to access the last directory we were in. This is handy if you need to jump back and forth between directories and don't want to do a lot of typing.

```
bandit3@bandit:/home$ cd -
/home/bandit3/inhere
```

Entering `cd ~` or just `cd` by itself will jump you to your home directory wherever you are.

### Manual Pages
Once we're in `inhere` folder, running `ls` again yields us with... nothing. This place looks empty.

```
bandit3@bandit:~/inhere$ ls
banit3@bandit:~/inhere$ 
```

The prompt clues us in that the file is hidden. Well how do we view hidden files? Enter the manual pages, which is documenation that you can access easily via the command line. Not every command will have a manual page, but most will and they're quite handy for learning how things work. Whenever you're using a new command and you're unsure about something, `man <command>` is the first place you should look for more information. (After completeing this section, I encourage you to go back and look up the man pages for commands that we've learned about so far. Except for `cd`. `cd` doesn't have a dedicated man page; you'll have to use `man builtins` instead.)

To access the documenation for `ls`, type `man ls` and you'll get something like this:

```
LS(1)                                                  User Commands                                                 LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftu‐
       vSUX nor --sort is specified.
...
```

The man pages are opened via `less`, which is a program that's useful for viewing text documents without printing it out to the command line like `cat` does. We won't get into the details of how `less` works right now; just know that you can use your scroll wheel to move and down the document or use `space` on your keyboard to move up a page or `b` to move back a page. To quit `less`, hit `q`.

If we read through the documentation, we can see under `SYNOPSIS` that `ls` accepts options and under `DESCRIPTION` we can see what those options are. The one we need to view hidden files is `-a` or `--all` which tells `ls` to display files that start with a period (`.`). This is relevant because files that start with a period (also known as dotfiles) are hidden by default in Linux and this is the reason why the `inhere` directory looks empty. If we run `ls -a` we'll see the file we're looking for.

```
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
```

Bingo. `cat` the hidden file (or use `less .hidden` now that you know about it) to view the password for the next level.

```
bandit3@bandit:~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```
