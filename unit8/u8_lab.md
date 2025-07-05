# Unit 8 Lab - Scripting

> If you are unable to finish the lab in the ProLUG lab environment we ask you `reboot`
> the machine from the command line so that other students will have the intended environment.

### Resources / Important Links

- [Killercoda Labs](https://killercoda.com/learn)
- [Wikipedia Page for Compilers](https://en.wikipedia.org/wiki/Compiler)
- [Wikipedia Page for the C Programming Lang](<https://en.wikipedia.org/wiki/C_(programming_language)>)
- [Wikipedia Page for Truth Table](https://en.wikipedia.org/wiki/Truth_table)
- [CProgramming.com Introductory](http://www.cprogramming.com/tutorial/c-tutorial.html)
- [Unit #1 Bonus (VIM) Page](https://professionallinuxusersgroup.github.io/lac/u1b.html)
- [Unit #8 Bonus (Bash Scripting) Page](https://professionallinuxusersgroup.github.io/lac/u8b.html)

### Required Materials

- Rocky 9.4+ - ProLUG Lab
  - Or comparable Linux box
- root or sudo command access

## Pre-Lab Warm-Up

---

```bash
vi /etc/passwd
# Put a # in front of all your local users you created in a lab a few weeks back
```

Review how to use vi, if you have a problem getting out or saving your file or use [Unit #1 Bonus (Vim) Page](https://professionallinuxusersgroup.github.io/lac/u1b.html) to brush up on your Vim Skills.

```bash
# Let us locate and inspect the GNU C Compiler Package
rpm -qa | grep -i gcc
dnf whatprovides gcc
dnf search gcc
Check out all the options of different compilers

[root@rocky13 ~]# rpm -qa | grep -i gcc
libgcc-11.5.0-2.el9.x86_64
[root@rocky13 ~]# dnf whatprovides gcc
Extra Packages for Enterprise Linux 9 - x86_64                      4.7 MB/s |  20 MB     00:04
Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_6 740  B/s | 2.5 kB     00:03
Rocky Linux 9 - BaseOS                                              3.0 MB/s | 2.5 MB     00:00
Rocky Linux 9 - AppStream                                            11 MB/s | 9.6 MB     00:00
Rocky Linux 9 - Extras                                               30 kB/s |  17 kB     00:00
gcc-11.5.0-5.el9_5.x86_64 : Various compilers (C, C++, Objective-C, ...)
Repo        : appstream
Matched from:
Provide    : gcc = 11.5.0-5.el9_5

[root@rocky13 ~]# dnf search gcc


# Now lets install it
dnf install gcc
# Look at what is going to be installed.

rpm -qi gcc
# Look at this package to see a little about what gcc does
# Repeat steps 2-6 for the software package strace

[root@rocky13 ~]# rpm -qi gcc
Name        : gcc
Version     : 11.5.0
Release     : 5.el9_5
Architecture: x86_64
Install Date: Fri Jul  4 19:26:24 2025
Group       : Unspecified
Size        : 89844785
License     : GPLv3+ and GPLv3+ with exceptions and GPLv2+ with exceptions and LGPLv2+ and BSD
Signature   : RSA/SHA256, Wed Feb 12 09:44:58 2025, Key ID 702d426d350d275d
Source RPM  : gcc-11.5.0-5.el9_5.src.rpm
Build Date  : Wed Feb 12 06:10:42 2025
Build Host  : pb-5e445082-056e-4562-99d1-1285e51d1fb1-b-x86-64
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : http://gcc.gnu.org
Summary     : Various compilers (C, C++, Objective-C, ...)
Description :
The gcc package contains the GNU Compiler Collection version 11.
You'll need this package in order to compile C code.

[root@rocky13 ~]# rpm -qi strace
Name        : strace
Version     : 6.12
Release     : 1.el9
Architecture: x86_64
Install Date: Fri Jul  4 19:26:58 2025
Group       : Unspecified
Size        : 3239155
License     : LGPL-2.1-or-later AND GPL-2.0-or-later AND GPL-3.0-or-later AND BSD-2-Clause AND BSD-3-Clause AND (GPL-1.0-or-later WITH Linux-syscall-note) AND (GPL-2.0-or-later WITH Linux-syscall-note) AND (GPL-2.0-only WITH Linux-syscall-note) AND (LGPL-2.0-or-later WITH Linux-syscall-note) AND (LGPL-2.1-or-later WITH Linux-syscall-note) AND ((GPL-2.0-only WITH Linux-syscall-note) OR Linux-OpenIB) AND ((GPL-2.0-only WITH Linux-syscall-note) OR MIT) AND ((GPL-2.0-or-later WITH Linux-syscall-note) OR BSD-3-Clause) AND ISC AND X11 AND FSFAP AND FSFUL AND FSFULLR AND FSFULLRWD
Signature   : RSA/SHA256, Fri May  2 14:43:16 2025, Key ID 702d426d350d275d
Source RPM  : strace-6.12-1.el9.src.rpm
Build Date  : Fri May  2 14:41:28 2025
Build Host  : pb-323b8609-e799-4308-a96a-3f80ec94cf5a-b-x86-64
Packager    : Rocky Linux Build System (Peridot) <releng@rockylinux.org>
Vendor      : Rocky Enterprise Software Foundation
URL         : https://strace.io
Summary     : Tracks and displays system calls associated with a running process
Description :
The strace program intercepts and records the system calls called and
received by a running process.  Strace can print a record of each
system call, its arguments and its return value.  Strace is useful for
diagnosing problems and debugging, as well as for instructional
purposes.

Install strace if you need a tool to track the system calls made and
received by a process.
```

### Compilers

A brief look at compilers and compiling code
So we did all this just to show you a quick rundown of how compiled code works on your system.
We are going to be doing scripting today, which is not compiled, but rather interpreted
code according to a type of interpreter so we’ll just breeze through this part.
You can come back and play with this at your leisure, I will provide links for more study.

```bash
Let’s write a C program
mkdir c_practice
cd c_practice

#Start a new file with Vi, in this case I am going with 'a'
vi a.c

#Add this to the file:
```

```c
#include <stdio.h>

main()
{

printf("My first compiled program \n");

return 0;
}
```

```bash
#Let’s use gcc to compile that program
gcc a.c
#This will create a file for you called a.out
#If there is an error, does it still work?

#Alternatively, and more correctly, use this:
gcc -o firstprogram a.c
#Which will create an executable file called first program
ls -salh
#Will show you all your files. Note how big those compiled programs are.
#Execute your programs
./a.out
./firstprogram

#Both of these should do the exact same thing, as they are the same code.
#Watch what your system is doing when you call it via strace
strace ./a.out
strace ./firstprogram


[root@rocky13 c_practice]# ./a.out
My first compiled program
[root@rocky13 c_practice]# ./firstprogram
My first compiled program
```

## Lab 🧪

---

Log into your Rocky server and become root.

## Module 2.1: Scripting

After all that pre-lab discussion, we won’t be using `gcc` today—or compiling any programs,
for that matter. Instead, we’ll focus on scripting, where we write code that the system 
interprets and executes step by step. Think of it like reading lines from a play, 
following sheet music, or executing a script—each command is processed in order.

There are plenty of resources available to learn scripting, but the key to improving is 
daily practice. If you’re serious about getting better, I recommend studying additional 
concepts over time. However, to get started, you only need to understand three fundamental ideas:

1. **Input and Output** - How to receive input and where to send the output.
2. **Conditionals** - How to test and evaluate conditions.
3. **Loops** - How to repeat actions efficiently.

### 2.2 Getting Input

Let’s use examples from our [Operate Running Systems lab](https://professionallinuxusersgroup.github.io/lac/u4lab.html) to see what it looks like to gather system information and store it in variables. Variables in scripting can be thought of as named boxes where we put things we want to look at or compare later. We can, by and large, stuff anything we want into these boxes.

```bash
# Try this:

echo $name  # No output
uname
name=`uname`
echo $name

echo $kernel  # No output
uname -r
kernel=`uname -r`
echo $kernel

echo $PATH  # This will have output because it is an environment variable

[root@rocky13 c_practice]# echo $PATH
/root/.local/bin:/root/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
```

There will be output because this is one of those special variables that make up
your environment variables. You can see them with:

```bash
printenv | more
```

These should not be changed, but if necessary, they can be. If you overwrite any, 
you can reset them by re-logging into your shell.

We can package things in variables and then reference them by their name preceded by a `$`.

```bash
# Try this to get numerical values from your system for later use in conditional tests.
cat /proc/cpuinfo  # Not very good as a count
cat /proc/cpuinfo | grep -i proc  # Shows processor count but not ideal for testing
cat /proc/cpuinfo | grep -i proc | wc -l  # Outputs a usable number
numProc=`cat /proc/cpuinfo | grep -i proc | wc -l`
echo $numProc

[root@rocky13 c_practice]# echo $numProc
2

free -m  # Displays memory info
free -m | grep -i mem  # Filters output
free -m | grep -i mem | awk '{print $2}'  # Extracts total memory value
memSize=`free -m | grep -i mem | awk '{print $2}'`
echo $memSize

[root@rocky13 c_practice]# echo $memSize
3892
```

### 2.3 Checking Exit Codes

One of the most important inputs in scripting is checking the exit code (`$?`) of a command. 
This allows us to determine whether a command succeeded or failed.

```bash
ps -ef | grep -i httpd | grep -v grep
echo $?
# Output: 1 (Nothing found, not good "0")

ps -ef | grep -i httpd
root      5514 17748  0 08:46 pts/0    00:00:00 grep --color=auto -i httpd
echo $?
# Output: 0 (Process found, exit code is 0)

[root@rocky13 c_practice]# ps -ef | grep -i httpd | grep -v grep
[root@rocky13 c_practice]# echo $?
1
[root@rocky13 c_practice]# ps -ef | grep -i httpd
root       10708    1045  0 19:39 pts/0    00:00:00 grep --color=auto -i httpd
[root@rocky13 c_practice]# echo $?
0
```

Checking for installed packages:

```bash
rpm -qa | grep -i superprogram
echo $?
# Output: 1 (Program not found)

[root@rocky13 c_practice]# rpm -qa | grep -i superprogram
[root@rocky13 c_practice]# echo $?
1

rpm -qa | grep -i gcc
libgcc-4.8.5-11.el7.x86_64
gcc-4.8.5-11.el7.x86_64
echo $?
# Output: 0 (GCC is found)

[root@rocky13 c_practice]# rpm -qa | grep -i gcc
libgcc-11.5.0-5.el9_5.x86_64
gcc-11.5.0-5.el9_5.x86_64
[root@rocky13 c_practice]# echo $?
0
```

`$?` only holds the exit status of the last executed command, so store it immediately:

```bash
rpm -qa | grep -i superprogram
superCheck=$?

rpm -qa | grep -i gcc
gccCheck=$?

echo $superCheck
echo $gccCheck

[root@rocky13 c_practice]# echo $superCheck
1
[root@rocky13 c_practice]# echo $gccCheck
0
```

### 2.4 Testing and Evaluating Conditions

#### 2.4.1 Basics of Logic and Truth Tests

I commonly say that **“All engineering is the test for truth.”** This is not meant
as a philosophical statement but a practical one. We take input, verify it, and compare 
it to our expectations. If it matches, the result is `true`; otherwise, it is `false`.

Testing for what something `is` is much easier than testing for what something `is not`, 
as logically, there are infinite possibilities for what something could `not` be.

Continue exploring these concepts by practicing input handling, storing values in 
variables, and testing conditions to build efficient scripts.

### 2.5 Exercise

The `Red bunny` is tall. We look at our examples and see that this is not true, 
so the statement evaluates to `false`.  
The `Blue bunny` is short. We look at our examples and see that this is not true, 
so the statement evaluates to `false`.

### The Idea of `AND` and `OR`

- `AND` is a restricting test.
- `OR` is an inclusive test.

I will prove that here shortly.

- `ANDing` checks both sides for truth and evaluates to _true_ only if `both` sides are true.
- `ORing` allows either side to be true, and the statement still evaluates to _true_. This makes OR a more inclusive test.

#### `AND` Examples

- The `right bunny` is `Red` and `Tall`.

  - This evaluates to `true` for the _Red_ test but `false` for the _Tall_ test.
  - The statement evaluates to `false`.

- The `left bunny` is `Blue` and `Tall`.
  - This evaluates to `true` for the _Blue_ test and `true` for the _Tall_ test.
  - The statement evaluates to `true`.

#### `OR` Examples

- The `right bunny` is `Red` or `Tall`.

  - This evaluates to `true` for the _Red_ test but `false` for the _Tall_ test.
  - The statement evaluates to `true`.

- The `left bunny` is `Red` or `Short`.
  - This evaluates to `false` for _Red_ and `false` for _Short_.
  - The statement evaluates to `false`.

### 2.6 - Truth Tables

Google **Truth Tables** to see engineering diagrams commonly used for testing truth in complex statements.  
We will not draw them out in this lab, as there are already well-documented examples. 
This is a **well-known, solved, and understood concept** in the engineering world. 
Instead of reinventing those diagrams, refer to the following link for more details:

[Truth Table - Wikipedia](https://en.wikipedia.org/wiki/Truth_table)

### 2.7 - Flow in a program

- All programs start at the top and run to the bottom. Data never flow back towards 
the start, unless on a separate path from a decision which always returns to the original path.

When we start thinking about how to lay something out and logically work through it, 
the idea of a formalized flow chart can help us get a handle on what we're seeing.

ome common symbols you'll see as we go through drawing out our logic. 
This example creates a loop in the program until some decision evaluates to yes (true).

### 2.8 - 3 Types of Decisions

There are 3 primary types of decisions you’ll run into with scripting, they are:

1. `Single` alternative
2. `Dual` alternative
3. `Multiple` alternative

#### 2.8.1 - Single Alternative `if/then`

Single alternatives either occur or they do not. They only branch from the primary 
path if the condition occurs. They either can or cannot occur, depending on the condition, 
but compared to alternative paths where a decision must occur if these do not evaluate to true, 
they are simply passed over.

Evaluate these from earlier and look at the difference.


```bash
if [ $superCheck -eq "0" ]; then echo "super exists"; fi
```

```bash
if [ $gccCheck -eq "0" ]; then echo "gcc exists"; fi

[root@rocky13 c_practice]# if [ $superCheck -eq "0" ]; then echo "super exists"; fi
[root@rocky13 c_practice]# if [ $gccCheck -eq "0" ]; then echo "gcc exists"; fi
gcc exists
```

You’ll note that only one of them caused any output to come to the screen, the other simply 
ran and the condition never had to execute.

#### 2.8.2 - Dual alternative (if/then/else)

Dual alternatives forces the code to split. A decision must be made. These are 
logically `if, then, else`. We test for a truth, and then, if that condition does 
not exist we execute the alternative. If you’re a parent or if you ever had a parent, 
this is the dreaded `or else`. One of two things is going to happen here, the path splits.

```bash
if [ $superCheck -eq "0" ]; then echo "super exists"; else echo "super does not exist"; fi #super does not exist
```

```bash
if [ $gccCheck -eq "0" ]; then echo "gcc exists"; else echo "gcc does not exist"; fi #gcc exists

[root@rocky13 c_practice]# if [ $superCheck -eq "0" ]; then echo "super exists"; else echo "super does not exist"; fi
super does not exist
[root@rocky13 c_practice]# if [ $gccCheck -eq "0" ]; then echo "gcc exists"; else echo "gcc does not exist"; fi
gcc exists
```

#### 2.8.3 - Multiple Alternative (if/then/elif/…/else or Case)

Multiple alternatives provide a branch for any numbers of ways that a program can go. 
They can be structured as if, then, `else if` `elif` in bash, `else`. They can also be 
framed in the case statement, which can select any number of cases (like doors) that 
can be selected from. There should always be a default `else` value for case statements, 
that is to say, if one of the many conditions don’t exist there is something that happens 
anyways (\*) in case statements.


```bash
superCheck=4
if [ $superCheck -eq "0" ]; then echo "super exists"; elif [ $superCheck -gt "1" ]; then echo "something is really wrong"; else echo "super does not exist"; fi
```

```bash
gccCheck=5
if [ $gccCheck -eq "0" ]; then echo "gcc exists"; elif [ $gccCheck -gt "1" ]; then echo "something is really wrong"; else echo "gcc does not exist"; fi

[root@rocky13 c_practice]# if [ $superCheck -eq "0" ]; then echo "super exists"; elif [ $superCheck -gt "1" ]; then echo "th
er is something really wrong"; else echo "super does not exist"; fi
super does not exist
[root@rocky13 c_practice]# if [ $gccCheck -eq "0" ]; then echo "gcc exists"; elif [ $gccCheck -gt "1" ]; then echo "ther is
something really wrong"; else echo "gcc does not exist"; fi
gcc exists
```

Set those variables to the conditions of 0, 1, and `anything else` to see what happens.

Think about why greater than 1 does not hit the condition of 1. Might it be easier to 
think of as greater than or equal to 2? Here’s a list of things you can test against.
http://tldp.org/LDP/abs/html/tests.html

Also a huge concept we don’t have a lot of time to cover is found here: 
File test operators http://tldp.org/LDP/abs/html/fto.html, do files exist and can you 
do operating system level things with them?

We didn’t get to go into case, but they are pretty straight forward with the following examples:
http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_03.html

We didn’t get to explore these much earlier, but to test `AND` and `OR` functionality use this.

`AND` condition
```bash
if [ $gccCheck -eq "0" -a $superCheck -eq "1" ]; then echo "We can install someprogram"; else echo "We can't install someprogram"; fi
```

We can't install someprogram

`OR` condition
```bash
if [ $gccCheck -eq "0" -o $superCheck -eq "1" ]; then echo "We can install someprogram"; else echo "We can't install someprogram"; fi
```
We can't install someprogram

```
[root@rocky13 c_practice]# if [ $gccCheck -eq "0" -a $superCheck -eq "1" ]; then echo "We can install someprogram"; else echo "We can't install someprogram"; fi
We can install someprogram
[root@rocky13 c_practice]# if [ $gccCheck -eq "0" -o $superCheck -eq "1" ]; then echo "We can install someprogram"; else echo "We can't install someprogram"; fi
We can install someprogram
```

#### 2.8.4 - Looping around

As with everything today, this is simply a primer and there are hundreds to thousands 
of examples online a simple google away. There are only two types of loops; counting loops 
and conditional loops. At the most basic level, we use counting loops when we (or the system) 
know how many times we want to go through something. Some examples of this are actual 
hard counts, lists, or lengths of files typically by line. While loops will continue 
until a condition exists or stops existing. The difference is until that condition 
occurs there’s no reasonable way of knowing how many times the loop may have to occur.

`For` loops

Counting is iteration.

We can count numbers
```bash
for i in 1 2 3 4 5; do echo "the value now is $i"; done

[root@rocky13 c_practice]# for i in 1 2 3 4 5; do echo "the value now is $i"; done
the value now is 1
the value now is 2
the value now is 3
the value now is 4
the value now is 5
```

We can count items
```bash
for dessert in pie cake icecream sugar soda; do echo "this is my favorite $dessert"; done

[root@rocky13 c_practice]# for dessert in pie cake icecream sugar soda; do echo "this is my favorite $dessert"; done
this is my favorite pie
this is my favorite cake
this is my favorite icecream
this is my favorite sugar
this is my favorite soda
```

But, it’s impractical to count for ourselves sometimes so we let the system do it for us.
```bash
seq 100 \
seq 4 100 \
seq 6 2 100 \
man seq

[root@rocky13 c_practice]# seq 4 10
4
5
6
7
8
9
10
[root@rocky13 c_practice]# seq 6 2 10
6
8
10
```

What did each of those do? Let’s put them in a loop we can use

- if only 1 number given, seq starts at 1 and counts up by one to the given number
- if two numbers given, seq starts at first number and counts up by one to the second number
- if three numbers given, start at first number, count up to third number, by increments of the second number


Maybe we want to count our 1000 servers and connect to them by name.
```bash
for i in `seq 1000`; do echo "Connecting to server p01awl$i"; done

[root@rocky13 c_practice]# for i in `seq 1000`; do echo "Connecting to server p01awl$i"; done
Connecting to server p01awl1
Connecting to server p01awl2
Connecting to server p01awl3
Connecting to server p01awl4
...
```


Maybe we need to create a list of all our servers and put it in a list
```bash
for i in `seq 1000`; do echo "p01awl$i" >> serverfile; done
```

Maybe someone else gave us a list of servers and we need to read from that list to connect and do work.
```bash
for server in `cat serverfile`; do echo "connecting to server $server"; done
[root@rocky13 c_practice]# for server in "cat serverfile"; do echo "connecting to server $server"; done
connecting to server cat serverfile
```

So, while those are even just a limited set of cases those are all times when, at the start, 
we know how many times we’re going to go through the loop. Counting or For loops always 
have a set number of times they’re going to run. That can change, but when it starts 
the end number of runs is known.

`While` loops

While loops are going to test conditions and loop while that condition evaluates to true. 
We can invert that, as we can with all logic, but I find that testing for truth is always easiest.

It is important to remember that `CRTL + C` will break you out of loops, as that will come handy here.

Administrators often find themselves looking at data and needing to refresh that data. 
One of the simplest loops is an infinite loop that always tests the condition of 
true (which always evaluates to true) and then goes around again. This is especially 
useful when watching systems for capacity issues during daemon or program startups.

```bash
while true; do date; free -m; uptime; sleep 2; done

[root@rocky13 c_practice]# while true; do date; free -m; uptime; sleep 2; done
Fri Jul  4 20:03:17 MST 2025
               total        used        free      shared  buff/cache   available
Mem:            3892        2606        1369        2159        2220        1285
Swap:              0           0           0
 20:03:17 up  1:02,  1 user,  load average: 0.00, 0.00, 0.00
```

This will run until you break it with `CTRL + C`. This will loop over the date, `free -m`, 
`uptime`, and `sleep 2` commands until the condition evaluates to false, which it will never do.

Let’s run something where we actually have a counter and see what that output is
```bash
counter=0
while [[ $counter -lt 100 ]]; do echo "counter is at $counter"; (( counter++ )); done

[root@rocky13 c_practice]# while [[ $counter -lt 100 ]]; do echo "counter is at $counter"; (( counter++ )); done
counter is at 0
counter is at 1
...
```
What numbers were counted through?

from 0 - 99

If you immediately run this again, what happens? Why didn’t anything happen?

the counter variable now has the value of 100. As long as it is not less than 100,
the loop condition is false.

```bash
#Reset counter to 0
counter=0
```

Re-run the above loop. Why did it work this time?

It works because the counter is now less than 100 meaning the condition is true.

Reset the counter and run it again. Try moving the counter to before the output. 
Can you make it count from 1 to 100? Can you make it count from 3 to 251? Are there 
challenges to getting that to work properly?

[root@rocky13 c_practice]# while [[ $counter -lt 252 ]]; do echo "counter is at $counter"; (( counter++ )); done

What if we wanted something to happen for every MB of free RAM on our system? Could we do that?
```bash
memFree=`free -m | grep -i mem | awk '{print $2}'`
counter=0
while [[ $counter -lt $memFree ]]; do echo "counter is at $counter"; (( counter++ )); done

[root@rocky13 c_practice]# while [[ $counter -lt $memFree ]]; do echo "counter is at $counter"; (( counter++ )); done
...
counter is at 3891
```

## 3.0 - Scripting System Checks

The main thing we haven’t covered is what to actually do with these things we’ve done. 
We can put them into a file and then execute them sequentially until the file is done executing. 
To do that we need to know the interpreter (bash is default) and then what we want to do.

```bash
touch scriptfile.sh
chmod 755 scriptfile.sh #let’s just make it executable now and save trouble later
vi scriptfile.sh
```
add the following lines:

```bash
#!/bin/bash

echo "checking system requirements"

rpm -qa | grep -i gcc > /dev/null
gccCheck=$?

rpm -qa | grep -i superprogram > /dev/null
superCheck=$?

if [ $gccCheck -eq "0" -a $superCheck -eq "1" ]
  then echo "We can install someprogram"
else
  echo "We can't install someprogram"
fi
```

Execute this with the following command and you’ll have your first completed script.
```bash
./scriptfile.sh

[root@rocky13 c_practice]# ./scriptfile.sh
checking system requirements
We can install someprogram
```

run the strace command to see what is happening with your system when it interprets this script.
```bash
strace ./scriptfile.sh
```

That's a lot of output! Now try adding -c for a summary. Here you can see all the 
syscalls made by the script and the time is took for each one.
```bash
strace -c ./scriptfile.sh
```

There are a lot of ways to use these tools. There are a lot of things you can do 
and include with scripts. This is just meant to teach you the basics and give you 
some confidence that you can go out there and figure out the rest. You can develop 
things that solve your own problems or automate your own tasks.

