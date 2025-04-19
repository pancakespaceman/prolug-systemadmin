# Unit 2 Lab - Essential Tools

## Resources / Important Links

(Killercode Labs)[https://killercoda.com/learn]
(Top 50+ Linux CLI Commands)[https://www.digitalocean.com/community/tutorials/linux-commands]

## Pre-Lab Warm-Up

Exercises

```
cd ~
ls
mkdir evaluation
mkdir evaluation/test/round6
# This fails, can you find out why?

This fails because its trying to create a round6 directory, inside of the test directory,
inside of the evaluation directory. However, the test directory doesn't exist.

mkdir -p evaluation/test/round6
# This works, think about why?

The -p tag creates parent directories as needed, so this will also create the test directory.

cd evaluation
pwd
# What is the path you are in?

/root/evaluation

touch testfile1
ls
# What did this do?

this creates a single file called testfile1
ls prints out the contents of the current directory, which will show the new file.

touch testfile{2..10}
ls
# What did this do differently than earlier?

This command uses bash parameter expansion to create files named testfileNum
where Num is integers in the range from 2 to 10

# touch .hfile .hfile2 .hfile3

ls
# Can you see your newest files? Why or why not? (man ls)

no, as by default files and directories that are prefixed with a period are considered
hidden and ls will not show hidden files by default

# What was the command to let you see those hidden files?

ls -a (i prefer ls -la)

ls -l
# What do you know about this long listing? Think about 10 things this can show you.

Long listing will show:
  - file permissions
    - first character: type (- for regular file, d for directory, l for symlink)
    - next 9 characters: permissions
  - number of hard links (directories) pointing to the file
  - Owner: the user who owns the file
  - Group: the group associated with the file
  - File size: in bytes, but can use -h flag for human-readable
  - Modified date and time: when was the file last modified
  - Filename

# Did it show you all the files or are some missing?

It still wont show hidden files

```


## Lab

This lab is designed to help you get familiar with the basics of the systems you will be working on.
Some of you will find that you know the basic material but the techniques here allow you to put it 
together in a more complex fashion.


**Gathering system information:**

```
hostname
cat /etc/*release
# What do you recognize about this output? What version of RHEL (CENTOS) are we on?

hostname gave back rocky17
there were four files in /etc when you run ls /etc/*release
os-release, redhat-release, rocky-release, and system-release
the latter three all had the same text, Rocky Linux release 9.5 (Blue Onyx) which is the distro
os-release has variables assigned release information, such as OS, ID, logo,
home url, and more

uname
uname -a
uname -r

# man uname to see what those options mean if you don’t recognize the values

uname prints system information (i like fastfetch :P)

uname -a prints all information
  - in the following order: kernel name, network node hostname, kernel version
  machine hardware name, processor type (omit if unknown), hardware platform (also omit),
  operating system
uname -r prints kernel-release

```

**Check the amount of RAM:**
```
cat /proc/meminfo
free
free -m

# What do each of these commands show you? How are they useful?
/proc/meminfo describes the total amount of memory, the amount of free memory, 
the memory that is available, and more information about memory.

free displays the mount of free and used memory in the system
free -m shows it in mebigytes, default is kibibytes
```

**Check the number of processors and processor info:**
```
cat /proc/cpuinfo
# What type of processors do you have? How many are there? (counting starts at 0)
The rocky machine has 2 processors of an Intel Xeon CPU E5-2665 0 @ 2.40GHz
My machine has 12 processors of an AMD Ryzen 5 2600X
I assume these processors include threads, as my CPU is a 6 core, but 12 processors are listed

cat /proc/cpuinfo | grep proc | wc -l
# Does this command accurately count the processors?

yes. grep grabs only the lines that have the substring "proc" which only include
the processor number lines. wc -l shows 2 for the rocky system, which matches the
number of processors 0 & 1.
my machine is counted properly as well.
```

**Check Storage usage and mounted filesystems:**
```
df
# But df is barely readable, so find the option that makes it more readable `man df`

df reports file system disk space usage
df has the -h flag which sets it to human readable

df -h
df -h | grep -i var
# What does this show, or search for? Can you invert this search? (hint `man grep`
# look for invert or google “inverting grep’s output”)

On the rocky machine, df -h | grep -i var shows nothing. we can use the -v flag
to invert matching, so it grabs non-matching lines
so df -h | grep -iv var

df -h | grep -i sd
# This one is a little harder, what does this one show? Not just the line, what are
# we checking for? (hint if you need it, google “what is /dev/sda in linux”)

- /dev/
  - a directory in the Unix tree that contains system devices
  - hard drives, terminals, USB devices, similar
- sda
  - sd stands for SCSI disk but also applies to SATA and USB drives
  - a is a letter to signify the order the drives were found, a first, b second, and so on
  - if there is a number afterwards, it refers to a partition on that device
  e.g. sda1, sdb2

The rocky system does not have any sd devices so the above command outputs nothing to stdout


mount
# Mount by itself gives a huge amount of information. But, let’s say someone is asking
# you to verify that the mount is there for /home on a system. Can you check that
# quickly with one command?

the mount command by itself lists all mounted filesystems

mount | grep -i home
#This works, but there is a slight note to add here. Just because something isn’t
# individually mounted doesn’t mean it doesn’t exist. It just means it’s not part of
# it’s own mounted filesystem.

this command searchs for any mounted systems with /home. In the rocky system,
/home is mounted. In my arch system, /home is not mounted separately and this command doesn't
show anything. I still have /home though in my arch system.

mount | grep -i /home/xgqa6cha
# will produce no output

this is because there is no directory in /home with that name, which
also should mean there is no user with that username

df -h /home/xgqa6cha
# will show you that my home filesystem falls under /home.

if you use it with a directory that exists, you get information about that directory

cd ~; pwd; df -h .
# This command moves you to your home directory, prints out that directory,
# and then shows you what partition your home directory is on.

Multiple bash statements can be written in a single line separated by a semicolon

du -sh .
# will show you space usage of just your directory

du estimates file space usage
-s displays only a total for each argument
-h prints the sizes in human readable format (e.g., 1K 234M 2G)

try `du -h .` as well to see how that ouput differs
# read `man du` to learn more about your options.

du -h by itself recursively shows sizes for directories, not just the top level directory

-a writes counts for all files
-b writes the size in bytes
-c produces a grand total
-d max depth for the recursion
-B --block-size=SIZE scale sizes by SIZE before printing them
-k is like block-size=1K
-m is like block-size=1M
-t exclude entries smaller than SIZE if positive, greater than is SIZE is negative
```

**Check the system uptime:**
```
uptime

man uptime
# Read the man for uptime and figure out what those 3 numbers represent.
# Referencing this server, do you think it is under high load? Why or why not?

uptime gives a one line display of: current time, how long the system has been running,
how many users are currently logged on, and the system load averages for the past
1, 5, and 15 minutes

For rocky system:
23:20:42 up 8 days,  8:03,  1 user,  load average: 0.02, 0.01, 0.00


no the server is not under high load.
The server load averages is the average number of process that are either in a runnable or
uninterruptable state.
the current average shows there are basically no processings waiting or actively running.
```

**Check who has recently logged into the server and who is currently in:**
```
last
# Last is a command that outputs backwards. (Top of the output is most recent).
# So it is less than useful without using the more command.

last | more
# Were you the last person to log in? Who else has logged in today?

I was the last person to log in today.
you could also just do last | head to get the top results showing the last 10 connections.

w - show who is logged on and what they are doing
who - show who is logged on
whoami - print efftive userid
# how many other users are on this system? What does the pts/0 mean on google?

I am the only user logged into this system at the moment.
pts/0 means that the user logged in with a virtual temrinal, such as over ssh
pts means virtual terminal, the number is an identifier

another option is TTY representing a physicial terminal. if you are using a console
locally, you'll probably see something like tty1 or tty2
```

**Check who you are and what is going on in your environment:**
```
printenv
# This scrolls by way too fast, how would you search for your home?

this prints out all your environment variables, such as your path, your home directory, username, etc. 

printenv | grep -i home
  - grabs env variables and settings that use the string "home".
  - this would specifically grab the HOME env var which is where your home directory is
whoami
  - prints out your effective user id. such as root for the rocky machine
id
  - prints real and effective user and group IDs
echo $SHELL
  - prints out the value of the SHELL env var, which default is bash
```

**Check running processes and services:**
```
ps -aux | more
  - -a grabs processes of all users
  - -u select by effective user ID or name, so groups info by user who owns each process
  - -x includes processes that don't have a controlling terminal, such as background and
  daemon processes
  - Format:
    - USER: user owning the process
    - PID: process ID
    - %CPU: CPU usage percentage
    - %MEM: Memory usage percentage
    - VSZ: Virtual memory size
    - RSS: Resident Set Size (physical memory used)
    - TTY: Terminal associated with the process ( or ? if there is no terminal)
    - STAT: Process state (e.g. S for sleeping, R for running)
    - START: Start time of the process
    - TIME: Total CPU time used by the process
    - COMMAND: the command that started the process
ps -ef | more
  - -e shows every process
  - -f shows full format, including detailed info and a heirarchical view (tree-like) of processes
  indicating parent-child relationships
  - FORMAT:
    - UID: user id
    - PID: process id
    - PPID: parent process ID
    - C: CPU usage
    - STIME: start time of the process
    - TTY: termianl associated with the process
    - TIME: Total CPU time used by the process
    - CMD: command that started the process

ps -aux is good if you want a quick, user-friendly listing of processes
ps -ef is good if you need a full system-wid listing of processes

ps -ef | wc -l
  - this shows the number of processes
```

**Check memory usage and what is using the memory:**
```
# Run each of these individually for understanding before we look at part b.
free -m
  - shows total, used, free, shared, buff/cache, and available memory, in mebibytes

NOTE:
  - Megabyte is defined as 1,000,000 bytes (10^6 bytes) based on decimal system
    - a 500 MB file is 500,000,000 bytes
  - Mebibyte is 1,048,576 bytes (2^20 bytes) based on binary system
    - commonly used where exact binary sizes are important, such as RAM
    - a 500 MiB file is 524,288,000 bytes


free -m | egrep “Mem|Swap”
  - egrep (extened grep) supports extended regular expressions
  - equivalent to runing grep -E
  - this line only grabs the lines that contain Mem or Swap. for the free command, this
  means that the column headers are gone.

free -m| egrep “Mem|Swap” | awk ‘{print $1, $2, $3}’
  - this command passes to awk, which is a utility for text data manipulation
  - awk prints out columns 1, 2, and 3 with this command, which include the total and used fields
free -t | egrep "Mem|Swap" | awk '{print $1 " Used Space = " ($3 / $2) * 100"%"}'
  - awk here prints the first field, which is either Mem or swap
  - then it prints the string " Used Space = CALC%" where CALC is a calculation
  of the 3rd field divided by the 2nd field multiplied by 100
  - this gives us a percentage of space being used.

# Taking this apart a bit:
# You’re just using free and searching for the lines that are for memory and swap
# You then print out the values $1 = Mem or Swap
# You then take $3 used divided by $2 total and multiply by 100 to get the percentage
```

Have you ever written a basic check script or touched on conditional statements or loops? 
(Use ctrl + c to break out of these):

```
while true; do free -m; sleep 3; done

# Watch this output for a few and then break with ctrl + c
# Try to edit this to wait for 5 seconds
# Try to add a check for uptime and date each loop with a blank line between
# each and 10 second wait:

what i came up with before reading the next line lol:
while true; do free -m; echo; uptime; echo; date; sleep 10; done

while true; do date; uptime; free -m; echo “ “; sleep 10; done
# Since we can wrap anything inside of our while statements, let’s try adding
# something from earlier:
while true; do free -t | egrep "Mem|Swap" | \
awk '{print $1 " Used Space = " ($3 / $2) * 100"%"}'; sleep 3; done

the awk command fails on swap due to division by zero attempted,
but this now prints the used space continuously every 3 seconds.
```

```
seq 1 10
# What did this do?

prints a sequence of numbers from the first to last

# Can you man seq to modify that to count from 2 to 20 by 2’s?

seq 2 2 20

# Let’s make a counting for loop from that sequence

for i in `seq 1 20`; do echo "I am counting i and am on $i times through the loop"; done
```
Can you tell me what is the difference or significance of the $ in the command above?
What does that denote to the system?

$ is for referencing variables in bash.
The for loop created the i variable, and will iterate over the sequence 1 - 20.
i will hold the each value of the sequence one at a time until the sequence is finished.
$i then accesses the value of i.


