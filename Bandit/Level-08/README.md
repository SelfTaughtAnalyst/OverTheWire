# Bandit Level 8 -> Level 9
**URL:** https://overthewire.org/wargames/bandit/bandit9.html

## Level Goal
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit8\
**Password:** cvX2JJa4CFALtqS87jk27qwqGhBM9plV

## Walkthrough
As usual, let's start off with an `ls` to see what we're working with.

```
bandit8@bandit:~$ ls -l
total 36
-rw-r----- 1 bandit9 bandit8 33033 May  7  2020 data.txt
```

If we open up the file with `less`, we can see that there's a bunch of text in there, which would be a pain to sift through by hand. Luckily for us, we have access to tools that will turn this into a trivial task.

### uniq
We can use the `uniq` command with the `-u` flag to print out only the unique lines in a text document. This does exactly what our prompt is looking for, how convenient!

```
UNIQ(1)                                                User Commands                                               UNIQ(1)

NAME
       uniq - report or omit repeated lines

SYNOPSIS
       uniq [OPTION]... [INPUT [OUTPUT]]

DESCRIPTION
       Filter adjacent matching lines from INPUT (or standard input), writing to OUTPUT (or standard output).

...

       -u, --unique
              only print unique lines
```

There's only one caveat though: if you read the description in the man pages, you'll see that `uniq` looks for matches in adjacent lines. This means that if our document has repeated lines that aren't next to each other, `uniq` won't consider them to be duplicates and will print them out. You can test this out by running `uniq -u data.txt` to see that it'll print the entire document to the terminal, instead of the single line we're expecting.

```
bandit8@bandit:~$ uniq -u data.txt
VkBAEWyIibVkeURZV5mowiGg6i3m7Be0
zdd2ctVveROGeiS2WE3TeLZMeL5jL7iM
sYSokIATVvFUKU4sAHTtMarfjlZWWj5i
...
```

### sort
To get around this, we can use the `sort` command, which can sort lines in a text file for us.

```
SORT(1)                                                User Commands                                               SORT(1)

NAME
       sort - sort lines of text files

SYNOPSIS
       sort [OPTION]... [FILE]...
       sort [OPTION]... --files0-from=F

DESCRIPTION
       Write sorted concatenation of all FILE(s) to standard output.

       With no FILE, or when FILE is -, read standard input.
```

If you run `sort data.txt`, you'll see that all the duplicate lines are now grouped up together.

```
bandit8@bandit:~$ sort data.txt
07KC3ukwX7kswl8Le9ebb3H3sOoNTsR2
07KC3ukwX7kswl8Le9ebb3H3sOoNTsR2
07KC3ukwX7kswl8Le9ebb3H3sOoNTsR2
...
```

We still have a bit of a problem though. `sort` doesn't actually modify the file it's sorting; it just prints the result out to the terminal. How are we supposed to take this and feed it to `uniq` then?

You _could_ save the output of `sort` into another file and then run `uniq` on that new file, which is a perfectly okay approach. If you pay close attention to the descriptions of `uniq` and `sort` however, you'll notice that both of these commands read from standard input and write to standard output. I've hinted in [prior levels](../Level-06/README.md#error-redirection) that standard input and output can be redirected so that's exactly what we're going to do here.


### Redirecting With Pipes |
Time to introduce the pipe (`|`). The pipe is a special character that takes the output of the program to the left of it and feeds it as input to the program on its right. If you don't know where the pipe key is, it's usually between the backspace and enter keys on most keyboards, where the backslash (`\`) key is located.

So if we write `sort data.txt | uniq -u`, instead of printing the output of `sort` to the screen, the pipe will take that output and use it directly as input for `uniq`. This saves us the hassle of having to create an intermediary file to operate on. You can use multiple pipes to chain mulitple commands together, which is something we'll experiment with a few levels later.

```
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```
