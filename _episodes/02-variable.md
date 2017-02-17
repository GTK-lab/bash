---
title: Variables
teaching: 10
exercises: 10
questions:
- How can we pass values to variables through command line arguments?
- How to declare and set your own variables.
- How to assign complex value to variables using quotes?
- How can we export variables?
objectives:
- Explain assign variable with command line arguments.
- Explain the ways to set variables.
- Explain different scope of variable through exporting.
keypoints:
- Bash has its own built-in variables to be used.
- Bash uses a space to separate items.
- You can store the output of programs to variable using command substitution.
- Variables has different scope. Normally variables used within one process.
---

## Command line arguments

Command line arguments are commonly used and easy to work with so they are a good place to start.

When we run a program on the command line you would be familiar with supplying arguments
after it to control its behaviour. For instance we could run the command `ls -l /etc`. In
this example, `-l` and `/etc` are both command line arguments to the command `ls`.
Arguments are generally separated by spaces.

We can do the same thing with our bash scripts. To do this we use the variables $1 to
represent the first command line argument, $2 to represent the second command line
argument and so on. These are automatically set by the system when we run our script so
all we need to do is refer to them.

For example, I need to check the current working directory

```
#!/bin/bash
# A simple bash script

echo $1
# Show the first command line argument.
```

> ## Try it yourself
> 
> Write a bash script that can tell the number of files and folders under the directory
> that is passed to it as a command line argument
>
> Use the script you just wrote to see how many files and folders are under /etc and your home directory.
{: .challenge}

There are many special variables that are already defined by the system.

> ### Special variable
> * $0 - The name of the Bash script.
> * $1 - $9 - The first 9 arguments to the Bash script. (As mentioned above.)
> * $# - How many arguments were passed to the Bash script.
> * $@ - All the arguments supplied to the Bash script.
> * $? - The exit status of the most recently run process.
> * $$ - The process ID of the current script.
> * $USER - The username of the user running the script.
> * $HOSTNAME - The hostname of the machine the script is running on.
> * $SECONDS - The number of seconds since the script was started.
> * $RANDOM - Returns a different random number each time is it referred to.
> * $LINENO - Returns the current line number in the Bash script.
{: .callout}


## Set your own variables

The basic form of assigning a variable would be like this:

> variable=value

Notes:

No space before or after the "=";

Leave of the **$** when we first setting it. But remember to add **$** when you are using them.

Here is a simple example:

```
#!/bin/bash
# A simple script of assigning variables

myvariable=Hello
yourvariable=World

echo $myvariable $yourvariable
echo

mydirectory=/etc

ls $mydirectory
```

~~~
Hello World

acpi adduser.conf alternatives apache2 ...
~~~
{: .output}

## Quotes

In the content above, we keep our variable simple, that the variables only need to store
one word. In reality, we might need to assign variables with more complex value.

```
#!/bin/bash
# A simple script that would produce an error

myvar=Hello World

echo $myvar
```

~~~
myscript.sh: line 3: World: command not found

~~~
{: .output}

This is because bash uses space to determine separate item.

Therefore, we can quotes to enclose out content and bash would take the contents as a single item. We normally use single quote (**'**) and double quote (**"**).

The difference between single quote and double quote is that: single quote will treat every character literally; double quote will alow you to do substitution.

~~~
user@bash: myvar='Hello world'
user@bash: echo $myvar
Hello world
user@bash: secondvar="More $myvar"
user@bash: echo $secondvar
More Hello world
user@bash: secondvar='More $myvar'
user@bash: echo $secondvar
More $myvar
~~~
{: .bash}

> ## Remember
>
> Because commands work exactly the same on the command line as in a script, it can sometimes be easier to experiment on the command line.
>
{: .callout}

We can also do command substitution. For example, store the output of a program to a variable. In this case, we should include the command in brackets preceded by a **$**.

~~~
user@bash: myvar=$( ls /etc | wc -l )
user@bash: echo There are $myvar entries in the /etc
~~~
{: .bash}

## Export variables

Variables are limited to the process they were created in. Sometimes, our script may need to run another script as one of its commands. In this case, we need to export variables.

```
#!/bin/bash
# scrip1.sh

var1=Wee
var2=Haa

echo $0 --- var1 : $var1, var2 : $var2
# Let's verify the value

export var1
./script2.sh

echo $0 --- var1 : $var1, var2 : $var2
# Let's see what happened

```

```
#!/bin/bash
# scrip2.sh

echo $0 --- var1 : $var1, var2 : $var2
# Let's verify their current value

var1=Hee
var2=Waa
# Let's change their values

```

We may see the output like this:

~~~
./script1.sh --- var1 : Wee, var2 : Haa
./script2.sh --- var1 : Wee, var2 :
./script1.sh --- var1 : Wee, var2 : Haa
~~~
{: .output}


The output above may seem unexpected. What actually happens when we export a variable is that we are telling Bash that every time a new process is created (to run another script or such) then make a copy of the variable and hand it over to the new process. So although the variables will have the same name they exist in separate processes and so are unrelated to each other.

Exporting variables is a one way process. The original process may pass variables over to the new process but anything that process does with the copy of the variables has no impact on the original variables.

Exporting variables is something you probably won't need to worry about for most Bash scripts you'll create. Sometimes you may wish to break a particular task down into several separate scripts however to make it easier to manage or to allow for reusability (which is always good). For instance you could create a script which will make a dated (ie todays date prepended to the filename) copy of all filenames exported on a certain variable. Then you could easily call that script from within other scripts you create whenever you would like to take a snapshot of a set of files.

## Environment variables VS. ordinary variables

**Global variables and local variables**

Global variables or environment variables are available in all shells. The `env` or `printenv` commands can be used to display environment variables. These programs come with the sh-utils package.

Local variables are only available in the current shell. Using the set built-in command without any options will display a list of all variables (including environment variables) and functions. The output will be sorted according to the current locale and displayed in a reusable format.

**Environment variables**

A variable created by command line is only available to the current shell. It is a local variable: child processes of the current shell will not be aware of this variable. In order to pass variables to a subshell, we need to export them using the export built-in command. Variables that are exported are referred to as environment variables. Setting and exporting is usually done in one step:

export VARNAME="value"

Each shell script running is, in effect, a subprocess (child process) of the parent shell.
A subshell can change variables it inherited from the parent, but the changes made by the child don't affect the parent.

~~~
user@bash: mine=foo
user@bash: echo $mine
foo
user@bash: ./test.sh
Start of test.sh
mine is foo
blah
End of test.sh
user@bash: echo $mine
foo
~~~
{: .bash}

Here is what inside of test.sh:

```
#!/bin/bash

echo "Start of test.sh"

echo mine is $mine
# To see whether subshell can inherited the exported variable
mine=blah
# Change the variable
echo $mine
export mine="weehaa"

echo "End of test.sh"
```

> ## Exercise
>
> Try to write a bash script that can back files and does the following things:
>
> 1. Takes on two arguments passed by command line. First one is file name and the second one is directory name
>
> 2. New a directory with directory name and copy the file into the new directory.
>
> 3. Show the all the file in long listing format in the new directory. Also show the line number details of all the file. Use your own defined variables to echo this information.
>  
> > ## Solution
> >
> > Hint:
> >
> > Command substitution.
> >
> {: .solution}
{: .challenge}
