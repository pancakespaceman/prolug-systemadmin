# Unit 1 Lab

## Resources / Important Links

[Killercode Labs](https://killercoda.com/learn)
[Top 50+ Linux CLI commands](https://www.digitalocean.com/community/tutorials/linux-commands)

Top 50 Commands

```
ls - list files
pwd- present working directory
cd
mkdir
mv
cp
rm
touch
ln
clear
cat
echo
less
man
uname
whoami
tar
grep
head
tail
diff
cmp
comm
sort
export
zip
unzip
ssh
service
ps
kill and killall
df
mount
chmod
chown
ifconfig
traceroute
wget
ufw
iptables
apt,pacman,yum,rpm
sudo
cal
alias
dd
whereis
whatis
top
useradd and usermod
passwd

```

## Required Materials

- Rocky 9.4+ – ProLUG Lab
  - Or comparable Linux box
- root or sudo command access


## Pre-lab Warm-up

```
mkdir lab_essentials
cd lab_essentials
ls
touch testfile1
ls
touch testfile{2..10} -this is pretty great actually. i think its parameter expansion?
ls

# What does this do differently?
the first touch creates a single file named testfile1
the second touch creates 9 files, named testfile2 through testfile10

# Can you figure out what the size of those files are in bytes? What command did you use?
ls -lh can tell the size of a file
du -b filename can also tell the file size in bytes
wc -c filename
stat -c %s filename



touch file.`hostname`
touch file.`hostname`.`date +%F`
touch file.`hostname`.`date +%F`.`date +%s`
ls

# What do each of these values mean? `man date` to figure those values out.

these commands create files that have names starting with file. then the hostname of the
machine, followed by dates.
%F gives the full date
$s gives the seconds since the epoch

# Try to set the following values in the file

... do you mean write these in the file?
or this perhaps?
[root@rocky19 lab_essentials]# date +%y >> file.rocky19
[root@rocky19 lab_essentials]# date +%e >> file.rocky19
[root@rocky19 lab_essentials]# date +%c >> file.rocky19
[root@rocky19 lab_essentials]# cat file.rocky19
25
27
Thu Mar 27 14:03:03 2025

# year, just two digits
# today's day of the month
# Just the century

date +%y
date +%e
date +%C
```

## Lab

**Working with Files:**

```
# Creating empty files with touch
touch fruits.txt

ls –l fruits.txt
# You will see that fruits.txt exists and is a 0 length (bytes) file

-rw-r--r--. 1 root root 0 Jun 22 07:59 fruits.txt
# Take a look at those values and see if you can figure out what they mean.
mentioned later in lab lol. permissions, # links, uid, group, size, date, name

# man touch and see if it has any other useful features you might use. If
# you’ve ever used tiered storage think about access times and how to keep data
# hot/warm/cold. If you haven’t just look around for a bit.

tiered storage is a method of placing data in different types of storage based
on frequency of access.
hot/warm/cold is a rating of how frequent, and determines what media is used for the data.

rm –rf fruits.txt

ls –l fruits.txt
# You will see that fruits.txt is gone.
```

**Creating files just by stuffing data in them:**

```
echo “grapes 5” > fruits.txt
cat fruits.txt
echo “apples 3” > fruits.txt
cat fruits.txt

echo “ “ > fruits.txt

echo “grapes 5” >> fruits.txt
cat fruits.txt
echo “apples 3” >> fruits.txt
cat fruits.txt

---- results

[root@rocky19 rawr]# cat fruits.txt

grapes 5
apples 3
```

What is the difference between these two?
>> appends to a file
> overwrites a file


**Creating file with vi or vim:**

```
# It is highly recommended the user read vimtutor. To get vimtutor follow
# these steps:
sudo –i
yum –y install vim
vimtutor

# There are about 36 short labs to show a user how to get around inside of vi.
# There are also cheat sheets around to help.

vi somefile.txt
# type “i” to enter insert mode

# Enter the following lines
grapes 5
apples 7
oranges 3
bananas 2
pears 6
**pineapples 9**

# hit the “esc” key at the top left of your keyboard
# Type “:wq”
# Hit enter

cat somefile.txt

```
i use neovim btw

**Copying and moving files:**

```
cp somefile.txt backupfile.txt
ls
cat backupfile.txt
mv somefile.txt fruits.txt
ls
cat fruits.txt
```

Can you explain the difference between cp and mv?
  - cp copies files
  - mv moves a file. can also be used for renaming a file (cause it removes the source file)
What does the -r flag do?
  - -r means recursive, which means it recursively goes through all files in a directory

**Searching/filtering through files:**

```
# So maybe we only want to see certain values from a file, we can filter
# with a tool called grep

cat fruits.txt
cat fruits.txt | grep apple
cat fruits.txt | grep APPLE

# read the manual for grep and see if you can cause it to ignore case.
grep searches for a string, and it also finds that string as a substring.
grep -i ignores case

# See if you can figure out how to both ignore case and only find the
# word apple at the beginning of the line.

# If you can’t, here’s the the answer. Try it:
cat fruits.txt | grep –i "^apple"
```
Can you figure out why that worked?
What does the ^ do?
^ is a regular expression that matches the beginning of a line.
$ is the end of a line
\b is a word boundary
\B is not a word boundary

Anchoring is a commong term for this. What anchors to the end of a string?

**Sorting Files with sort:**

```
# Let’s sort our file fruits.txt and look at what happens to the output
# and the original file

sort fruits.txt
cat fruits.txt

# Did the sort output come out different than the cat output? Did sorting
# your file do anything to your original data? 
the sort output has all the lines sorted alphabetically, but the
file itself didn't change

# So let’s sort our data again and figure out what this command does differently

sort –k 2 fruits.txt

**# You can of course man sort to figure it out, but –k refers to the “key” and**
# can be useful for sorting by a specific column

# But, if we cat fruits.txt we see we didn’t save anything we did. What if we
# wanted to save these outputs into a file. Could you do it? If you couldn’t,
# here’s an answer:

sort fruits.txt > sort_by_alphabetical.txt
sort –k 2 fruits.txt > sort_by_price.txt

# Cat both of those files out and verify their output


---

[root@rocky19 rawr]# cat sort_by_alphabetical.txt
7 apples
apples 7
bananas 2
grapes 5
oranges 3
pears 6
pineapples 9
[root@rocky19 rawr]# cat sort_by_price.txt
bananas 2
oranges 3
grapes 5
pears 6
apples 7
pineapples 9
7 apples
```

**Advanced sort practice:**

```
# Consider the command
ps –aux

-a select all processes except both session leaders and processes not associated with a terminal
-u select by effective user ID or name. selects whose EUID is in userlist
x lifts "must have a tty" restriction which is imposed upon a set of all processes
when some BSD-style (without "-") options are used or when the ps personality setting is BSD-like

ps -aux prints all processes owned by a user name x, as well as printing all processes that would be
selected by the -a option. if a user named x does not exxist, this ps may interpret the command as
ps aux instead and print a warning.

# But that’s too long to probably see everything, so let’s use a command
# to filter just the top few lines
**ps –aux | head**

# So now you can see the actual fields (keys) across the top that we could sort by

USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND

# So let’s say we wanted to sort by %MEM
ps –aux | sort –k 4 –n –r | head -10

oh so the keydef is refering to words/columns?
-n is numeric sort
-r is reverse
```

Read man to see why that works.
ps displays the proccesses.
piped into sort which sorts by the 4th key which is %MEM, and it sorts numberically.
piped into head displaying the first 10 lines.

Why do you suppose that it needs to be reversed to have the highest numbers at the top?
because sort -n sorts lowest to highest

What is the difference between using the -n or not using it?
sort will interpret as strings by default and sort alphabetically.

Read man ps to figure out what other things you can see or sort by from the ps command.

**Working with redirection:**

```
cat fruits.txt | grep apple
# This cats out the file, all of it, but then only shows the things that
# pass through the filter of grep. We could continually add to these and make
# them longer and longer

cat fruits.txt | grep apple | sort | nl | awk ‘{print $2}’ | sort –r
pineapples
apples
cat fruits.txt | grep apple | sort | nl | awk '{print $3}' | sort -r
9
7
cat fruits.txt | grep apple | sort | nl | awk '{print $1}' | sort -r
2
1

# Take these apart by pulling the end pipe and command off to see what is
# actually happening:

cat fruits.txt | grep apple | sort | nl | awk '{print $1}' | sort -r
2
1
cat fruits.txt | grep apple | sort | nl | awk '{print $1}'
1
2
cat fruits.txt | grep apple | sort | nl
1 apples 7
2 pineapples 9
cat fruits.txt | grep apple | sort
apples 7
pineapples 9
cat fruits.txt | grep apple
apples 7
pineapples 9

```

What do each of these commands do?

cat prints contents of the file to stdOut
piped to grep
grep searches and returns lines that match the given search string
piped to sort
sort sorts the input of lines
piped to nl
nl prints out the line numbers of each line
piped to awk
awk runs the print command on column 2
piped to sort -r
sort sorts the input but in reversed order


**Throwing the output into a file:**

When we redirect, we are catching it before it comes on the screen.
Tee is also useful for catching data

```
date
# comes to the screen

date > datefile
# redirects and creates a file datefile with the value

date | tee –a datefile
# will come to screen, redirect to the file.
```
man tee
tee will take input from stdIn and output to stdOut as well as any number of files
what does -a do?
tee -a will append to a file instead of overwriting

**Ignoring pesky errors or tossing out unqanted output:**

```
ls fruits.txt
# You should see normal output

ls fruity.txt
# You should see an error unless you made this file

ls fruity.txt 2> /dev/null
# You should no longer see the error.

# But, sometimes you do care how well your script runs against 100 servers,
# or you’re testing and want to see those errors. You can redirect that to a file, just as easy

ls fruity.txt 2> error.log
cat error.log
# You’ll see the error. If you want it see it a few times do the error line to see it happen.
```

Useful for later when we stress out systems.
Here is a command that burn cpu cycles by creating random numbers, zipping up the output
and throwing it away.

`time dd if=/dev/urandom bs=1024k count=20 | bzip2 -9 >> /dev/null`

Use ctrl+c to break
The only numbers you can play with there are the 1024k and the count.
other numbers should be only changed if you use man to read about them first.

"Poor man's" answer file. Something we used to do when we needed to answer some
values into a script or installer. Still works and is accurate, but might be
a bit advanced with a lot of advanced topics here.

```vi testscript.sh
hit “i” to enter insert mode
add the following lines:

#!/bin/bash

read value
echo "The first value is $value"
read value
echo "The second value is $value"
read value
echo "The third value is $value"
read value
echo "The fourth value is $value"

# hit “esc” key
type in :wq
# hit enter

chmod 755 testscript.sh

# Now type in this (don’t type in the > those will just be there in your shell):

[xgqa6cha@N01APL4244 ~]$ echo "yes

> no
> 10
> why" | ./testscript.sh
> yes
> no
> 10
> why
```

What happened here is we read the input from the comman dline and gave it, in order
to the script to read then output. This is something we do if we know an installer
wants certain values throughout it, but we don't want to sit there and type them in, or
we're doing it across 100 servers quickly, or all kinds of reasons.
A quick and dirty input "hack" that counts as a redirect.

**Working with permissions**

permissions look like this

```
ls -l
```

| Permission | # of links | UID Owner | Group Owner | Size (b) | Creation Month | Creation Day | Creation Time | File Name |
| -rw-r--r--,|     1      |  Root     |   root      | 58       | Jun            |     22       |      08:52    |  datefile |

```
RWX    RWX    RWX
421    421    421
Owner  Group  Everyone
```

```
ls -ld /root/
# drwx------. 5 root root 4096 Jun 22 09:11 /root/
```

The first character is if its a directory, file, or link

rwx: For UID (me)
  - What permissions do I have? read, write, and execute

`---`: For group
  - Who are they? 
    - a group is a collection of users that share the same file permissions
    - each user is part of their own group by default
    - wheel is a group that represents sudoers
  - What can my group do?
    - the group can do nothing

`---`: For everyone else
  - What can everyone else do?
    - nothing


Go find some other interesting files or directories and see what you see there.
Can you identify their characteristics and permissions?
