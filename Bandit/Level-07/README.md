# Bandit Level 7 -> Level 8
**URL:** https://overthewire.org/wargames/bandit/bandit8.html

## Level Goal
The password for the next level is stored in the file **data.txt** next to the word **millionth**

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit7\
**Password:** HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

## Walkthrough
There is a very simple way to solve this level, which I go through in the [last section](#searching-with-grep). Since that write up is so short, I decided to introduce an alternative method first so that I can go over some Linux fundamentals that I haven't covered yet.

The level prompt hints that the password we're looking for is in a text file next to a keyword. Given the increasing complexity of the problems we've had to solve with each passing level, I'm willing to bet that it would take a long time to find the password if we opened up the file and scrolled through it line by line.

Before opening up the file, we can get an idea of the scale of the problem we're working with by taking a look at the file size.

### ls Long List Format
We can use the long option (`-l`) with `ls` to get more details on the file(s) it returns.

```
bandit7@bandit:~$ ls -l
total 4088
-rw-r----- 1 bandit8 bandit7 4184396 May  7  2020 data.txt
```

Unfortunately, `ls` doesn't provide column headers for the different fields, so I'll quickly break down what everything means:
- `-rw-r-----` The first character in this line (`-`) represents the file type and the next nine characters represent the file permissions.
  - There are a few different file types, but the most common characters you'll see are `-` (regular files), `d`(directories), and `l` (links). So the first `-` seen here tells us that `data.txt` is a regular file.
  - The file permissions are represented by nine characters, organized in three groups of three which show the owner, group, and world (also known as other) permissions respectively. So in this case, the owner permissions are `rw-`, the group permissions are `r--` and the world (other) permissions are `---`.
    - `r` = read permissions
    - `w` = write permissions
    - `x` = execute permissions
    - `-` = no permissions
    - There are other permission types, but again these are the most common ones you'll see.
  - Putting it all together in layman's terms, `data.txt` is a regular file that is readable and writeable by the file's owner and is readable by members of the file's group. Anyone outside of the file's owner or group can't read, write, or execute this file.
- `1` This is the number of hard links to the file. An oversimplified explanation of hard links is that they are how Linux points file names to data. If there are multiple hard links, then there are multiple files that point to the same data on the file system. If that doesn't make much sense to you, don't worry about it for now; we won't be encountering them here.
- `bandit8` This is the owner of the file. Notice that we are logged in as `bandit7` so we aren't the owner of this file.
- `bandit7` This is the group the file belongs to. Even though we don't own the file, we do belong to the group `bandit7`, so we have read permissions. You can confirm who you're logged in as and which group(s) you belong to by using the `id` command in the terminal.
  - ```
    bandit7@bandit:~$ id
    uid=11007(bandit7) gid=11007(bandit7) groups=11007(bandit7)
    ```
- `4184396` This is the size of the file in bytes. If you don't want to go through the mental effort of converting this number into something easier to understand, you can run `ls` with the `-h` flag to make the size human readable.
  - ```
    bandit7@bandit:~$ ls -lh
    total 4.0M
    -rw-r----- 1 bandit8 bandit7 4.0M May  7  2020 data.txt
    ```
- `May  7  2020` This is when the file was last motified.
- `data.txt` This is the file or directory's name.

So this file takes up over 4 million bytes of space, which inicates that here's going to be a lot of text. Searching through this file by hand is definitely out of the question.

### Searching With less
Since this file is likely going to contain a lot of text, printing it all out to the terminal with `cat` isn't a great idea. What we can do instead is use a pager program like `less` to view its contents. That way, we can view its contents once screen at a time (kind of like pages) and flip through them back and forth as we see fit. As a quick reminder, you can use `SPACE` and `b` to scroll forward/backward one page at a time or your mouse wheel to move up and down several lines at a time. Press `q` to exit.

Open up the file with `less data.txt` and take a gander at how many lines there are. Since we know the password is stored next to the word `millionth` We can use the search functionality built into `less` to find the line we're looking for. There are multiple ways to initiate a search, but we'll use the foward slash (`/`) here to search forward in the file. Type `/millionth` and hit `ENTER` on your keyboard to search for it. You can see what you're typing at the very bottom of the terminal.

The word `millionth` should be highlighted and the password should be right next to it.

`less` has a ton of features and functionalities built into it, far too many for me to cover here. I suggest going through the man pages (`man less`) to see what it's capable of. If nothing else, it'll at least make it easier for you to read through the manual pages since that's what `man` uses.

Excerpt of the man pages covering searching forward and the keys to repeat the search fowards and backwards:

```
/pattern
        Search forward in the file for the N-th line containing the pattern.  N defaults to 1.   The  pattern  is  a
        regular  expression,  as  recognized  by the regular expression library supplied by your system.  The search
        starts at the first line displayed (but see the -a and -j options, which change this).

n      Repeat  previous  search, for N-th line containing the last pattern.  If the previous search was modified by
        ^N, the search is made for the N-th line NOT containing the pattern.  If the previous search was modified by
        ^E, the search continues in the next (or previous) file if not satisfied in the current file.  If the previ‐
        ous search was modified by ^R, the search is done without using regular expressions.  There is no effect  if
        the previous search was modified by ^F or ^K.

N      Repeat previous search, but in the reverse direction.
```

### Searching With grep
We can also use the `grep` command to search for us. That way we don't have to bother with opening a file and we can also easily pass the results of the search to another command if we need to (something we'll explore later on). We can search with the syntax: `grep <options> <pattern> <file>`. Since this is simple search, all we'll need is `grep millionth data.txt`. Don't let the simplicity of this solution fool you into thinking that `grep` is only good for simple searches. It has some powerful pattern matching abilities and I again encourage you to at least glance through the manual pages (`man grep`) to get an idea of what it's capable of.

```
bandit7@bandit:~$ grep millionth data.txt
millionth	cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```
