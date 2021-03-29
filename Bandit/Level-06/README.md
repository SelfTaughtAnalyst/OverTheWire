# Bandit Level 6 -> Level 7
**URL:** https://overthewire.org/wargames/bandit/bandit7.html

## Level Goal
The password for the next level is stored **somewhere on the server** and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit6\
**Password:** DXjZPULLxYr17uwoI01bNLQbtFemEgo7

## Walkthrough
The setup for this level is very similar to the [last one](../Level-05/README.md). We're told that the file we're looking for has some set of properties that we can use to search for it. However, we'll need to look across the entire file system this time, instead of having our search contained to just the home directory.

If we print our working directory, we'll notice that the path begins with a forward slash (`/`). In the Linux file system, this first slash represents the root directory, the very top directory in the hierarchy. Every single sub-directory and file will be contained under the root directory so using that as the starting point for `find` will allow us to easily search everything on this server.

```
bandit6@bandit:~$ pwd
/home/bandit6
```

The rest of our `find` command is easy to construct; we just have to dig through the man pages to find the options that are relevant to us. We're looking for a way to identify a file owned by a specific user, that belongs to a specific group, and is of a specific size, all of which can all be found under the `TESTS` section. I've listed the options we'll need below for easy reference.

```
-user uname
        File is owned by user uname (numeric user ID allowed).

-group gname
        File belongs to group gname (numeric group ID allowed).

-size n[cwbkMG]
        File uses n units of space, rounding up.  The following suffixes can be used:

        `b'    for 512-byte blocks (this is the default if no suffix is used)

        `c'    for bytes
```

Armed with that knowledge, you should end up with a command that looks similar to this: `find / -user bandit7 -group bandit6 -size 33c`. If you run that however, `find` will start throwing a bunch of `Permission denied` or `No such file or directory` errors at you as it traverses through the file system. Your user (`bandit6`) doesn't have the necessary permissions to access every folder and file, which is why you're getting these errors.

```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c
find: ‘/root’: Permission denied
find: ‘/home/bandit28-git’: Permission denied
find: ‘/home/bandit30-git’: Permission denied
find: ‘/home/bandit5/inhere’: Permission denied
...
```

If you sift though all the errors, you'll eventually see the file containing the password we need, but that feels like a lot of work. Any time you find yourself doing something tedious that's repetitive or has a pattern (in this case, ignoring all the lines with errors), there's probably a faster/better way to do it.

### Error Redirection
Back in [level 1](../Level-01/README.md#stdinstdout-redirection-with--), we learned a little bit about data streams (standard input and standard output) and a little bit about redirection. It turns out that there is a third stream called standard error, which you may have guessed, refers to the error messages that a program produces.

Errors having their own dedicated stream is helpful because we can easily distingush between what's an output and what's an error simply by looking at the streams alone. In this case we can take the errors which would normally print to our terminal and redirect them somewhere else so we don't end up with a wall of text, making it easier to see our output.

To do that, we'll append `2> /dev/null` to the end of our `find` command. To briefly breakdown what this does:
- `>` is a special character for redirection. This tells Linux to expect standard input from or send standard output and standard error to somewhere else instead of the program's default location.
- `2` is the file descriptor for standard error. Combined with `>`, we're saying that we want to send standard error somewhere else. Just for reference, the file descriptor for standard output is `1` and the descriptor for standard input is `0`.
- `/dev/null` is the location that we want to redirect to. `/dev/null` is a special file that will accept input and discard it. Since we're not looking to write our errors to a file, but we still have to send them _somewhere_ other than our screen, this is the perfect place for it. You might see this being referred to as a "null device" or "bit bucket" or "black hole".

Running the modified command should yield a much cleaner output now that the errors have been redirected away.

```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2> /dev/null
/var/lib/dpkg/info/bandit7.password
```

Access the file we've found to get the password for the next level.

```
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```
