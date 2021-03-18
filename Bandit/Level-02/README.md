# Bandit Level 2 -> Level 3
**URL:** https://overthewire.org/wargames/bandit/bandit2.html

## Level Goal
The password for the next level is stored in a file called spaces in this filename located in the home directory

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit2\
**Password:** CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

## Walkthrough
Once we log in, let's `ls` to see what files are in our home directory.

```
bandit2@bandit:~$ ls
spaces in this filename
```

There's the file mentioned in the prompt. When we try to use `cat spaces in this filename` however, we run into a problem. It looks like `cat` is treating this as four different files instead of one single one.

```
bandit2@bandit:~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

In the command line, spaces usually act as a delimiter, meaning that they're a character that programs use to separate one argument/parameter from the next which is why `cat` thinks we're trying to open four different files.

### Escape Characters
One way we can get around this problem is through the use of escape characters. Escape characters tell programs to interpret characters literally and not as a special character. In this case, our terminal is running the bash shell and the escape character in bash is the backslash (`\`). The backslash isn't a universal escape character, but it is a common one. If you're running a different shell and you don't know what the escape character is, you can always look it up on Google.

If we put a `\` before every space in the filename, we'll be able to use `cat` on it.

```
bandit2@bandit:~$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

This is a giant pain to type out. Some shells have auto complete features you can take advantage of for complicated names. If you start typing `cat s` and hit tab, the rest of the filename should automatically populate for you with the backslashes in all the right places.

### Quotes
We can also use quotes to work around the spaces. Enclose the name in either single quotes (`'`) or double quotes (`"`) and `cat` should work as expected.

```
bandit2@bandit:~$ cat 'spaces in this filename'
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

bandit2@bandit:~$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

I won't get into it here, but if you're interested in knowing what the difference between single and double quotes are in bash, you can check out this Stack Overflow answer here: https://stackoverflow.com/a/6697781. Basically, it has to do with how bash will interpret other special characters wrapped up in the quotes.

The lesson here is to avoid using spaces in filenames, especially if you plan on doing a lot of work in the command line. The extra spaces are a hassle to deal with when typing the names in commands and thus create more opportunity for error. Instead, a common way to visually distinguish words in a filename is to use dashes (`-`) or underscores (`_`) in lieu of spaces.

### Extra Credit - How to Tell What Shell You're Running
Earlier I mentioned that our terminal was running bash. If you're curious how I knew that, then this section is for you.

There are a couple different ways to figure out what shell you're currently running, but I like to use `ps -p $$` at the terminal. `ps` is a command that displays information about the current processes. The `-p` option says we want to look up a specific process by its process ID. The `$$` is a shortcut to reference ID for the current shell process.

```
bandit2@bandit:~$ ps -p $$
  PID TTY          TIME CMD
30570 pts/21   00:00:00 bash
```

Like the escape character, this isn't a universal command, but it will work in a lot of shells. If it doesn't, then at the very least you now have a clue as to what you might *not* be running.
