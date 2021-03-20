# Bandit Level 4 -> Level 5
**URL:** https://overthewire.org/wargames/bandit/bandit5.html

## Level Goal
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit4\
**Password:** pIwrPrtPN36QITSp3EQaw936yaFoFgAB

## Walkthrough
We'll start by taking a look at the contents of the `inhere` directory. You can do that by changing directories and running `ls` inside of it or by passing the directory as an argument directly by running `ls inhere`. For this walkthrough, I'll opt to `cd inhere` instead of staying in the home directory.

```
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

We see a bunch of files beginning with a dash. If you try to `cat` one of these files by typing just the name, you'll get an error. The leading dash is causing `cat` to think that we are trying to pass in command options instead of a file.

```
bandit4@bandit:~/inhere$ cat -file00
cat: invalid option -- 'f'
Try 'cat --help' for more information.
```

To get around this, we can use the same trick we used in [level 1](../Level-01/README.md) and reference the file using a path instead of just the name itself. If you stayed in the home directory, you won't run into this problem as you're already using a path to reference the file since you have to preface the name with  `inhere/`. We'll try opening the first file, `-file00`.

**If you changed directories**

```
bandit4@bandit:~/inhere$ cat ./-file00
�/`2ғ�%��rL~5�g��� �����
```

**If you stayed in the home directory**

```
bandit4@bandit:~$ cat inhere/-file00
�/`2ғ�%��rL~5�g��� �����
```

We end up with a bunch of characters that our terminal can't display. Chances are, this is binary data and isn't something we can read directly. The level goal tells us that the password for the next level is in one of these files, but it should be human-readable so this one obviously isn't it. Since there are only ten files here, we _could_ open all of them one by one to see which file has the password we're looking for, but that would be terribly boring. Plus, what if instead of ten files, we had hundreds or thousands to search through instead?

Let's think of a better way to solve this. We'll start with the `file` command that we learned about in [level 0](../Level-00/README.md#file-types), which tells us what type of file we're looking at. If we run it on `-file00`, we can see the file type is `data`, which explains why we can't understand its contents.

```
bandit4@bandit:~/inhere$ file ./-file00
./-file00: data
```

If you stayed in the home directory you would run `file inhere/-file00` instead.

It would be great if there was a way for us to check the file types of all the files at once so we can see which ones are human-readable and which ones aren't. If we take a look at the man pages for `file` (`man file`), we see that this command can accept multiple files at once. (Look under `SYNOPSIS` and notice the ellipsis (`...`) after the word file. The ellipsis indicates that the command can accept multiples of that argument, in this case multiple files.)

```
SYNOPSIS
     file [-bcdEhiklLNnprsSvzZ0] [--apple] [--extension] [--mime-encoding] [--mime-type] [-e testname] [-F separator]
          [-f namefile] [-m magicfiles] [-P name=value] file ...
```

So how do we pass in multiple files at once?

### Expansion and Wildcards
The great thing about the Linux command line is that it supports expansion, meaning you can type in special characters to represent something and the terminal will automatically know how to interpret it based on its given context. You've already seen a few examples of these when we were learning about paths. `~` expands to the home directory, `.` expands to the current directory, and `..` expands to the parent directory.

In this case, we can use special characters called wildcards to select files based on a pattern we want to match (this is also known as _globbing_). There are several ways we can use wildcards to solve this problem, but to keep things simple I'll just use the asterisk (`*`). In wildcard terms, the asterisk represents any number of characters (including zero characters) so we can use `file ./-file0*` to match any file that starts with `-file0`. You could be even more general and run `file *` to run `file` on every file in the current directory or any combination in-between. The pattern matching aspect of wildcards is super flexible and you can manipulate it however you'd like to fit your needs.

```
bandit4@bandit:~/inhere$ file ./-file0*
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

If you stayed in the home directory, you would run `file inhere/-file0*` instead.

This provides a glimpse of how powerful the command line is. In a single command, we performed work on multiple files and managed to turn what would have otherwise been a tedious task into a quick and simple one. In the next few levels we'll see more examples of this in action. For now, `cat` the only ASCII text file in this directory to get the password for the next level.

```
bandit4@bandit:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```
