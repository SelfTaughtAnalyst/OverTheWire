# Bandit
**URL:** https://overthewire.org/wargames/bandit/

Bandit is a fantastic war game for beginners who are looking to learn the absolute basics of how wargames/CTFs work as well as those looking to gain some familiarity with the Linux command line. You don't need to have Linux installed, you just need to be able to SSH into their machines which most modern systems should be able to do nowadays. If you don't know how SSH works, don't worry as that's what level 0 is about.

This game covers a ton of topics at an introductory level and it's normal to feel overwhelmed learning about it all. Each level has a ton of interconnected topics that you can deep dive into and you can easily get lost in the rabbit hole of information. As Bandit's main page suggests, **don't panic and don't give up**. Take things slow and spend time Googling things you don't understand. Also don't be afraid to take a break from a topic and come back to it later; you might learn more things down the line that help with something you didn't get before. If this is all new to you, I recommend circling back and going through all the levels again after completing them once. This allows you to see what knowledge you've retained and to approach the problems with a fresh set of eyes and different perspective.

This repo is organized into folders with each level having its own corresponding folder and README. On the site, levels are written as "Level X -> Level Y' which can raise some ambiguity when referencing a particular level. In this write up, levels will be referenced by the username used to log in. For example, logging in with bandit0 means that we are on level 0 and all the corresponding write up to get to the next level will be in the folder Level-0.

# Level 0
**URL:** https://overthewire.org/wargames/bandit/bandit0.html

## Level Goal
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **bandit.labs.overthewire.org**, on port 2220. The username is **bandit0** and the password is **bandit0**. Once logged in, go to the Level 1 page to find out how to beat Level 1.

## Level Info
**Hostname:** bandit.labs.overthewire.org\
**Port:** 2220\
**Username:** bandit0\
**Password:** bandit0

## Walkthrough
Simply put, SSH (Secure Shell) is a network protocol that allows you to connect to another computer (or service running on that computer) and send information securly over what may otherwise be an insecure network. In the case of Bandit, we'll be using SSH as a means of connecting the device we're working from to the machines at Over The Wire hosting the wargame. The secure part of SSH comes from the fact that the communication between the connected devices is encrypted, so theoretically if someone manages to intercept a message in transit, they wouldn't be able to read it.

To use SSH, you'll need at least three pieces of information: where you're connecting to (the hostname or IP address of the host), who you're connecting as (the username), and a means of authentication (a password or public/private keys). We'll learn about public and private keys later on, so if you're not familiar with what they are, don't worry about it. SSH connects to port 22 by default. If you're connecting to a different port, you'll need to specify the port number. For Bandit, we'll primarily connect through port 2220.

### Linux and Mac
To SSH on Linux and Mac, open a terminal and use the SSH command with the following syntax: `ssh <username>@<hostname> -p <port>` or `ssh ssh://<username>@<hostname>:<port>`. So to access level 0, we'll enter the following: `ssh bandit0@bandit.labs.overthewire.org -p 2220` and use the password `bandit0` when it prompts us for it. If you get a message about the authenticity of host and fingerprints and don't know what to do, refer to the fingerprints section below.

### Windows
Traditionally, there wasn't a way to use SSH natively on Windows. You would have to download an SSH client like [PuTTY](https://www.putty.org/) or [CygWin](https://www.cygwin.com/) or look for PowerShell modules that would provide that functionality. Nowadays however, it looks like newer versions of Windows 10 have SSH support built in. I was able to use SSH in both Command Prompt and PowerShell with the following syntax: `ssh <username>@<hostname> -p <port>` or `ssh ssh://<username>@<hostname>:<port>`. So to access level 0, we'll enter the following: `ssh bandit0@bandit.labs.overthewire.org -p 2220` and use the password bandit0 when it prompts us for it. If you get a message about the authenticity of host and fingerprints and don't know what to do, refer to the fingerprints section below.

If the SSH commands don't work for you right out of the box, you may have to do some setup. See https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/ for help.

If you Google search "PowerShell SSH" you may stumble upon Microsoft's documentation for remoting over SSH using `New-PSSession` and `Enter-PSSession` commands. Remoting with PowerShell is not the same thing as connecting via SSH. See https://poshsecurity.com/blog/2015/6/22/why-remoting-vs-ssh-vs-rdp-shouldnt-be-a-thing for some clarification on what the differences are.

If you have Git Bash installed, you can also use the command line to SSH from there as well.

The command to connect through Command Prompt/PowerShell/Git Bash is the same as Linux and Mac: `ssh bandit0@bandit.labs.overthewire.org -p 2220`. You should get a fingerprint prompt the first time you're connecting. After that, enter your password.

### Fingerprints
If it's your first time connecting to a host, you should get a prompt warning about the authenticity of the host and asking you to confirm its fingerprint. This is a securty feature that helps ensure you know who you're connecting to.

```
❯ ssh bandit0@bandit.labs.overthewire.org -p 2220
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([176.9.9.172]:2220)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

This prompt is here for your safety and to make sure you're connecting to the right source. Basically, your computer is saying "Hey, I don't recognize who I'm connecting to, are you sure this is the right place? Here's an identifier you can use to confirm this is the right place." If you type `yes`, you'll proceed with the connection, the host will be added to your known_hosts file, and you shouldn't see the message again on future connects.

If you didn't expect to see this message, double check that you typed the host and port correctly and if you can, verify that the fingerprint matches what's displayed. For instance, if you set up SSH with GitHub, you can confirm their fingerprint [here](https://docs.github.com/en/github/authenticating-to-github/githubs-ssh-key-fingerprints). Bandit doesn't have a source to verify their fingerprint so you'll just have to make sure you typed the host and port correctly.

With that out of the way, let's connect to bandit0 and see what challenge awaits us!
