# Bandit Level 5 -> Level 6
**URL:** https://overthewire.org/wargames/bandit/bandit6.html

## Level Goal
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit5\
**Password:** koReBOKuIDDepwhWk7jZC0RTdopnAYKh

## Walkthrough
Similar to the last couple of levels, the password we're looking for is located somewhere under the `inhere` directory. Just to show case a slightly different workflow, I won't `cd` into the directory. Instead, I'll execute all the commands from the home directory so you have an example of how it's done.

First, we'll verify the existence of `inhere` before running `ls` on it to see what's inside. Just in case there may be hidden files, we'll use the `-a` option as well.

```
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ ls -a inhere
.            maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
..           maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
```

Looks like there are 20 different sub-folders that we'd have to search through for our password. If we peek into one of the folders, we'll see that there are multiple files, some of which are hidden or are named oddly (starting with a dash or having a space in the filename).

```
bandit5@bandit:~$ ls -a inhere/maybehere00
.  ..  -file1  .file1  -file2  .file2  -file3  .file3  spaces file1  spaces file2  spaces file3
```

Going though all of these files manually would be a pain. Luckily, the level prompt gives us a hint that that helps with our search. The file we're looking for has three properties: it's human-readable, 1033 bytes in size, and not executable.

### The Find Command
We can use these properties in conjunction with the `find` command to quickly isolate the file we're looking for. Let's pull up the manual pages (`man find`) to see how to construct something to solve our problem.

```
FIND(1)                                           General Commands Manual                                          FIND(1)

NAME
       find - search for files in a directory hierarchy

SYNOPSIS
       find [-H] [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]
```

To use `find`, we have to first tell it where we want it to start its search followed by an expression that explains what we want to search for. The expression can consist of a number of things, but for this problem we'll focus on two tests and an action.

Looking under the `TESTS` section of the man pages, we see that we can test if a file is an exectuable with an `-executable` flag. Since we're looking for a file that is **not** executable, we'll need a way to get the opposite result. If you look further down the man pages in the `OPERATORS` section, you'll see we can return the oppsite of an expression by preceeding it with an excalamation mark (`!`) or `-not`. If you use an exclamation mark, you may have to escape it with a backslash (`\`) so the command line doesn't interepret it as a special character.

```
-executable
        Matches files which are executable and directories which are searchable (in a file name resolution sense) by
        the current user.  This takes into account access control lists and other permissions  artefacts  which  the
        -perm  test  ignores.  This test makes use of the access(2) system call, and so can be fooled by NFS servers
        which do UID mapping (or root-squashing), since many systems implement access(2) in the client's kernel  and
        so  cannot  make  use of the UID mapping information held on the server.  Because this test is based only on
        the result of the access(2) system call, there is no guarantee that a file for which this test succeeds  can
        actually be executed.
```

```
OPERATORS
...
    ! expr True if expr is false.  This character will also usually need protection from interpretation by the shell.

    -not expr
            Same as ! expr, but not POSIX compliant.
```

Also under the `TESTS` section is a way to limit our results by the size of a file. We can specify that the file we're looking for is 1033 bytes in size with `-size 1033c`. 

```
-size n[cwbkMG]
        File uses n units of space, rounding up.  The following suffixes can be used:

        `b'    for 512-byte blocks (this is the default if no suffix is used)

        `c'    for bytes

        `w'    for two-byte words

        `k'    for kibibytes (KiB, units of 1024 bytes)

        `M'    for mebibytes (MiB, units of 1024 * 1024 = 1048576 bytes)

        `G'    for gibibytes (GiB, units of 1024 * 1024 * 1024 = 1073741824 bytes)
```

The last property we want our file to have is being "human readable". Unfortunately, that doesn't translate into an actual test for `find`. There's the `-type` test, but it doesn't look for what the level prompt is specifying (that the conents of the file are interpretable by a human). What we can do instead is take the results of our `find` command and send it to the `file` command. Normally this would be done with a pipe (`|`) in Linux, but that won't work in this case so we'll have to use `-exec`, which you can read about in the `ACTIONS` section. We'll learn more about pipes and how they work in Linux in a later level.

We can use `-exec file '{}' \;` to tell `find` to pass the file being processed to `file`. The curly braces (`'{}'`) represent the current file `find` is processing. When using `-exec` we need to signify the end of the command with a semi-colon (`;`) which we also need to escape with a backslash (`\`) so the command line doesn't interpret it as a special character.

```
-exec command ;
        Execute command; true if 0 status is returned.  All following arguments to find are taken to be arguments to
        the  command until an argument consisting of `;' is encountered.  The string `{}' is replaced by the current
        file name being processed everywhere it occurs in the arguments to the command, not just in arguments  where
        it is alone, as in some versions of find.  Both of these constructions might need to be escaped (with a `\')
        or quoted to protect them from expansion by the shell.  See the EXAMPLES section for examples of the use  of
        the  -exec option.  The specified command is run once for each matched file.  The command is executed in the
        starting directory.  There are unavoidable security problems surrounding use of the -exec action; you should
        use the -execdir option instead.
```

Put all of those pieces together and we'll end up with something that looks like this: `find . \! executable -size 1033c -exec file '{}' \;`. If you're confused as to why there's a period (`.`) after `find`, remember that it's a short cut to represent the current directory, which is where we're starting our search.

```
bandit5@bandit:~$ find . \! -executable -size 1033c -exec file '{}' \;
./inhere/maybehere07/.file2: ASCII text, with very long lines
```

Only one file matches the critera of being non-executable with a size of 1033 bytes and `file` tells us that it consists of ASCII text, which is human readable, so this must be our password file. Access its contents to move on to the next level.

```
bandit5@bandit:~$ cat inhere/maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```
