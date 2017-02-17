---
title: "If statement"
teaching: 10
exercises: 10
questions:
- "What is if statement?"
- "What are the test that we can used in if statement?"
- "How can we use boolean operation in if statement?"
objectives:
- "Learn to use if statement and complex if statement."
- "Learn to use different test and boolean operation."
- "Learn to use case statement."
keypoints:
- "If statement is fundatmental in programming bash."
- "Remember different test would help a lot when you are writing bash scripts."
---

## Basic if statement

A basic if statement effectively says, if a particular test is true, then perform a given set of actions. If it is not true then don't perform those actions. If follows the format below:

>      if [<some test>]
>      then
>          <commands>
>      fi

Anything between **then** and **fi** would be executed if the it pass the test.

A simple example would be :

```
#!/bin/bash
# if_example.sh

if [ $1 -gt 100 ]
then
  echo Hey that\'s a large number.
  pwd
fi

date
```

~~~
user@bash: ./if_example.sh 15
Wed Feb 15 16:22:40 SGT 2017
user@bash: ./if_example.sh 200
Hey that's a large number.
/home/yichao/test
Wed Feb 15 16:23:18 SGT 2017
~~~
{: .output}

## Test

We can use different test inside square brakets (**[]**).

| Operator	            | Description                                                          |
| --------------------- |:--------------------------------------------------------------------:|
| ! EXPRESSION	        | The EXPRESSION is false.                                             |
| -n STRING	            | The length of STRING is greater than zero.                           |
| -z STRING	            | The lengh of STRING is zero (ie it is empty).                        |
| STRING1 = STRING2	    | STRING1 is equal to STRING2                                          |
| STRING1 != STRING2	  | STRING1 is not equal to STRING2                                      |
| INTEGER1 -eq INTEGER2	| INTEGER1 is numerically equal to INTEGER2                            |
| INTEGER1 -gt INTEGER2	| INTEGER1 is numerically greater than INTEGER2                        |
| INTEGER1 -lt INTEGER2	| INTEGER1 is numerically less than INTEGER2                           |   
| -d FILE	              | FILE exists and is a directory.                                      |
| -e FILE	              | FILE exists.                                                         |   
| -r FILE	              | FILE exists and the read permission is granted.                      |  
| -s FILE	              | FILE exists and it's size is greater than zero (ie. it is not empty).|    
| -w FILE	              | FILE exists and the write permission is granted.                     |
| -x FILE	              | FILE exists and the execute permission is granted.                   |  

> ## A few points to be noted:
> * **=** is slightly different to **-eq**. [ 001 = 1 ] will return false as = does a string comparison (ie. character for character the same) whereas -eq does a numerical comparison meaning [ 001 -eq 1 ] will return true.
> * When we refer to **FILE** above we are actually meaning a path. Remember that a path may be absolute or relative and may refer to a file or a directory.
> * Because **[ ]** is just a reference to the command test we may experiment and trouble shoot with test on the command line to make sure our understanding of its behaviour is correct.
{: .callout}

## Nested if statement

Like other programming language. We can nest if statements inside an if statement:

```
#!/bin/bash
# Nested if statements
if [ $1 -gt 100 ]
then
  echo Hey that\'s a large number.
  if (( $1 % 2 == 0 ))
  then
    echo And is also an even number.
  fi
fi
```

## If elif else

Sometimes we may encounter complex logic flow when we are coding. If statement can be like this:

>      if [ <some test> ]
>      then
>          <commands>
>      elif [ <some test> ]
>      then
>          <different commands>
>      else
>          <other commands>
>      fi

For example:


```
#!/bin/bash
# Nested if statements

if [ $1 -gt 100 ]
then
  echo Hey it is greater than 100.
elif [ $1 -lt 10 ]
then
  echo Hey it is less than 10.
else
  echo Hey it is between 10 and 100.
fi
```

## Boolean operations

We can use boolean operation if we want the test to meet multiple conditions.

* **AND** - &&

* **OR** - &#124;&#124;

Following are the occasion where we want the file we are considering to be readable as well as greater than zero in size.

```
#!/bin/bash
# AND example

if [ -r $1 ] && [ -s $1 ]
then
  echo This file is useful.
fi
```

## Case statement

Sometimes we may wish to consider variables that meet a series of pattern. It is hardly readable if we use **if** and **elif**. Here is where case statement can help us:

>      case <variable> in
>      <pattern 1>)
>          <commands>
>          ;;
>      <pattern 2>)
>          <other commands>
>          ;;
>      esac

A case is always included by **case** and **esac**, with **;;** to separate different cases.

```
#!/bin/bash
# case example

case $1 in
  start)
  echo starting
  ;;
  stop)
  echo stoping
  ;;
  restart)
  echo restarting
  ;;
  *)
  echo don\'t know
  ;;
esac
```

~~~
user@bash: ./case.sh start
starting
user@bash: ./case.sh restart
restarting
user@bash: ./case.sh blah
don't know
~~~
{: .output}

> ## Exercise
>
> 1. Write a script that take 2 integers as command line argument and print out the larger of the them on the screen.
>
> 2. Write a greeting script that print different greeting greeting message base on which day on the week.
>
> > ## Solution
> >
> > Hint:
> >
> > 1. Make sure that you consider all the possible(When they are equal).
> >
> > 2. You might want to get the information of day using `date` command.
> >
> {: .solution}
{: .challenge}
