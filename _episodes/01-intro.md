---
title: "Introduction to bash scripts"
teaching: 10
exercises: 0
questions:
- "What is the difference between sh and bash?"
- "What is a bash script?"
objectives:
- "Introduce the histrory of bash briefly."
- "Explain the concept of bash."
- "Explain how bash script work and how we run them."
keypoints:
- "Bash is a implementation of sh."
- "A bash script is a plain text file contains a series of commands."
- "Bash script run in a process."
- "Every bash script starts with a shebang (#!) at the first line."
---

## What is sh

**sh** (or the Shell Command Language) is a programming language described by the [POSIX standard](http://pubs.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html).
It has many implementations (ksh88, dash, ...). bash can also be considered an implementation of sh (see below).

Because sh is a specification, not an implementation, /bin/sh is a symlink (or a hard link) to an actual implementation on most POSIX systems.

## A brief history of sh

Steve Bourne wrote the Bourne shell which appeared in the Seventh Edition Bell Labs Research version of Unix.
Many other shells have been written; this particular tutorial concentrates on the Bourne and the Bourne Again shells.
Other shells include the Korn Shell (ksh), the C Shell (csh), and variations such as tcsh.
This tutorial does not cover those shells.

## What is bash
**bash** started as an sh-compatible implementation (although it predates the POSIX standard by a few years), but as time passed it has acquired many extensions. Many of these extensions may change the behavior of valid POSIX shell scripts, so by itself bash is not a valid POSIX shell. Rather, it is a dialect of the POSIX shell language.

bash supports a --posix switch, which makes it more POSIX-compliant. It also tries to mimic POSIX if invoked as sh.

## sh = bash?

For a long time, /bin/sh used to point to /bin/bash on most GNU/Linux systems. As a result, it had almost become safe to ignore the difference between the two. But that started to change recently.

Some popular examples of systems where /bin/sh does not point to /bin/bash (and on some of which /bin/bash may not even exist) are:

1. Modern Debian and Ubuntu systems, which symlink sh to dash by default;
2. Busybox, which is usually run during the Linux system boot time as part of initramfs. It uses the ash shell implementation.
3. BSDs, and in general any non-Linux systems. OpenBSD uses pdksh, a descendant of the Korn shell. FreeBSD's sh is a descendant of the original UNIX Bourne shell. Solaris has its own sh which for a long time was not POSIX-compliant; a free implementation is available from the Heirloom project.

## What is a bash scripts

A Bash script is a plain text file which contains a series of commands. It may contains a mixture of commands we would normally type on the command line (such as `ls` or `cp` for example) and commands we could type on the command line but generally wouldn't. An important point to remember is:

> Anything you can run normally on the command line can be put into a script
> and it will do exactly the same thing. Similarly, anything you can put into a
> script can also be run normally on the command line and it will do exactly
> the same thing.

You don't need to change anything. Just type the commands as you would normally and they will behave as they would normally. It's just that instead of typing them at the command line we are now entering them into a plain text file. In this sense, if you know how to do stuff at the command line then you already know a fair bit in terms of Bash scripting.

It is convention to give files that are Bash scripts an extension of .sh (myscript.sh for example). As you would be aware (and if you're not maybe you should consider reviewing our Linux Tutorial), Linux is an extensionless system so a script doesn't necessarily have to have this characteristic in order to work.

## How do they work?

In the realm of Linux (and computers in general) we have the concept of programs and processes. A program is a blob of binary data consisting of a series of instructions for the CPU and possibly other resources (images, sound files and such) organised into a package and typically stored on your hard disk. When we say we are running a program we are not really running the program but a copy of it which is called a process. What we do is copy those instructions and resources from the hard disk into working memory (or RAM). We also allocate a bit of space in RAM for the process to store variables (to hold temporary working data) and a few flags to allow the operating system (OS) to manage and track the process during it's execution.

> Essentially a process is a running instance of a program.

There could be several processes representing the same program running in memory at the same time. For example I could have two terminals open and be running the command `cp` in both of them. In this case there would be two `cp` processes currently existing on the system. Once they are finished running the system then destroys them and there are no longer any processes representing the program `cp`.

When we are at the terminal we have a Bash process running in order to give us the Bash shell. If we start a script running it doesn't actually run in that process but instead starts a new process to run inside.

## How to run a bash script?

Running a bash script is fairly easy. Just to make sure that you have the executing permission, which normally not set by default.

~~~
user@bash: ./myscript.sh
~~~
{: .bash}

~~~
bash: ./myscript.sh: Permission denied
~~~
{: .output}

You can use `chmod` to add execute permission to your script.

~~~
user@bash: chmod +x myscript.sh
user@bash: ./myscript.sh
~~~
{: .bash}

~~~
Hello World!
~~~
{: .output}

Here is the content of myscript.sh:

```
#!/bin/bash

echo Hello World!
```

### What is the shebang(#!)

The first line of the script is something like:

> #!/bin/bash

The hash exclamation mark ( **#!** ) character sequence is referred to as the Shebang. Following it is the path to the interpreter (or program) that should be used to run (or interpret) the rest of the lines in the text file.

> ## Format is important here!
> The shebang **MUST** be on the very first line of the bash script.
>
> There **MUST** be no space between before **#** or between the **!** and the path to the interpreter.
{: .callout}

It is possible to leave out the line with the shebang and still run the script but it is unwise. If you are at a terminal and running the Bash shell and you execute a script without a shebang then Bash will assume it is a Bash script. So this will only work assuming the user running the script is running it in a Bash shell and there are a variety of reasons why this may not be the case, which is dangerous.

You can also run the script with `bash` :

~~~
user@bash: bash myscript.sh
~~~
{: .bash}

~~~
Hello World!
~~~
{: .output}

### Why the ./

You've possibly noticed that when we run a normal command (such as `ls`) we just type its name but when running the script above I put a ./ in front of it. When you just type a name on the command line Bash tries to find it in a series of directories stored in a variable called $PATH. We can see the current value of this variable using the command echo (you'll learn more about variables in the next section).

~~~
user@bash: echo $PATH
~~~
{: .bash}

~~~
/home/user/bin:/usr/local/bin:/usr/bin:/bin
~~~
{: .output}

The directories are separated by " : "

Bash only looks in those specific directories and doesn't consider sub directories or your current directory. It will look through those directories in order and execute the first instance of the program or script that it finds.

The $PATH variable is an individual user variable so each user on a system may set it to suit themselves(.bashrc file).

> ## Why this?
> * It allows us to have several different versions of a program installed. We can control which one gets executed based on where it sits in our $PATH.
> * It allows for convenience. As you saw above, the first directory for myself is a bin directory in my home directory. This allows me to put my own scripts and programs there and then I can use them no matter where I am in the system by just typing their name. I could even create a script with the same name as a program (to act as a wrapper) if I wanted slightly different behaviour.
> * It increases safety - For example a malicious user could create a script called ls which actually deletes everything in your home directory. You wouldn't want to inadvertantly run that script. But as long as it's not in your $PATH that won't happen.
{: .callout}

You can also use absolute path to execute your bash script. Here we used relative path to save time in typing.
