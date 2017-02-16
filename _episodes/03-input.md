---
title: "Input in bash"
teaching: 20
exercises: 0
questions:
- "How we can give input in bash"
objectives:
- "To introduce different ways of input in bash"
keypoints:
- "We can have a interactive input from the terminal."
- "Read from STDIN is one of the useful feature in Linux."
---

There are various way to input in bash.

## Ask user to input

We can ask the user to input when we use `read`.

> read myvar

```
#!/bin/bash
# Ask the user for name

echo Hello, who am I talking to?

read varname

echo It\'s nice to meet you $varname

```

~~~
Hello, who am I talking to?
yichao
It's nice to meet you yichao
~~~
{: .output}

We can also read in multiple variables at the same time. For example,

```
#!/bin/bash

echo Name me the top three fruit that you like?

read fruit1 fruit2 fruit3

echo Your first car was: $fruit1
echo Your second car was: $fruit2
echo Your third car was: $fruit3
```

## Reading from STDIN

It's common in Linux to pipe a series of simple, single purpose commands together to create a larger solution tailored to our exact needs. The ability to do this is one of the real strengths of Linux. It turns out that we can easily accommodate this mechanism with our scripts also. By doing so we can create scripts that act as filters to modify data in specific ways for us.

Bash accommodates piping and redirection by way of special files. Each process gets it's own set of files (one for STDIN, STDOUT and STDERR respectively) and they are linked when piping or redirection is invoked. Each process gets the following files:

* STDIN - /proc/<processID>/fd/0
* STDOUT - /proc/<processID>/fd/1
* STDERR - /proc/<processID>/fd/2

To make life more convenient the system creates some shortcuts for us:

* STDIN - /dev/stdin or /proc/self/fd/0
* STDOUT - /dev/stdout or /proc/self/fd/1
* STDERR - /dev/stderr or /proc/self/fd/2

fd in the paths above stands for file descriptor.

So if we would like to make our script able to process data that is piped to it all we need to do is read the relevant file. All of the files mentioned above behave like normal files.

```
#!/bin/bash
# A basic summary of my sales report

echo Here is a summary of the sales data:
echo ====================================
echo

cat /dev/stdin | cut -d' ' -f 2,3 | sort
```

~~~
user@bash: cat salesdata.txt
Fred apples 20 February 4
Susy oranges 5 February 7
Mark watermelons 12 February 10
Terry peaches 7 February 15
user@bash: cat salesdata.txt | ./summary
Here is a summary of the sales data:
====================================
apples 20
oranges 5
peaches 7
watermelons 12
~~~
{: .output}
