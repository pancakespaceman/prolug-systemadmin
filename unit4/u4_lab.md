# Unit 4 Lab - Operating Running Systems

## Resources / Important Links

- (Killercoda)[https://killercoda.com/learn]
- (Wiki - Cron)[https://en.wikipedia.org/wiki/Cron]
- (tldp.org's cron guide)[https://tldp.org/LDP/lame/LAME/linux-admin-made-easy/using-cron.html]

## Pre-Lab Warm-up

1. `cd ~`

```
[root@rocky7 ~]# cd ~
[root@rocky7 ~]#
```

2. `ls`

```
[root@rocky7 ~]# ls
anaconda-ks.cfg  anaconda-post.log  lab-file  original-ks.cfg
```

3. `mkdir unit4`

```
[root@rocky7 ~]# mkdir unit4
[root@rocky7 ~]# ls
anaconda-ks.cfg  anaconda-post.log  lab-file  original-ks.cfg  unit4
```

4. `mkdir unit4/test/round6`

```
[root@rocky7 ~]# mkdir unit4/test/round6
mkdir: cannot create directory ‘unit4/test/round6’: No such file or directory
```

5. `mkdir -p unit4/test/round6`

```
[root@rocky7 ~]# mkdir -p unit4/test/round6
[root@rocky7 ~]#
```

6. `cd unit4`

```
[root@rocky7 ~]# cd unit4
[root@rocky7 unit4]#
```

7. `ps`

```
[root@rocky7 unit4]# ps
    PID TTY          TIME CMD
    973 pts/0    00:00:00 bash
   1008 pts/0    00:00:00 ps
```

8. `ps -ef`

```
[root@rocky7 unit4]# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 16:49 ?        00:00:00 /sbin/init
root           2       0  0 16:49 ?        00:00:00 [kthreadd]
root           3       2  0 16:49 ?        00:00:00 [rcu_gp]
root           4       2  0 16:49 ?        00:00:00 [rcu_par_gp]
root           5       2  0 16:49 ?        00:00:00 [slub_flushwq]
....
```

9. `ps -ef | grep -i root`

```
[root@rocky7 unit4]# ps -ef | grep -i root
root           1       0  0 16:49 ?        00:00:00 /sbin/init
root           2       0  0 16:49 ?        00:00:00 [kthreadd]
root           3       2  0 16:49 ?        00:00:00 [rcu_gp]
root           4       2  0 16:49 ?        00:00:00 [rcu_par_gp]
root           5       2  0 16:49 ?        00:00:00 [slub_flushwq]
root           6       2  0 16:49 ?        00:00:00 [netns]
```

10. `ps -ef | grep -i root | wc -l`

```
[root@rocky7 unit4]# ps -ef | grep -i root | wc -l
96
```

11. `top`

```
[root@rocky7 unit4]# top
top - 16:56:38 up 7 min,  1 user,  load average: 0.06, 0.32, 0.24
Tasks:  97 total,   2 running,  95 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.2 hi,  0.0 si,  0.2 st
MiB Mem :   3892.0 total,   1501.8 free,   2449.0 used,   2060.3 buff/cache
MiB Swap:      0.0 total,      0.0 free,      0.0 used.   1443.0 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    924 root      20   0 1446760  72416  44032 S   0.7   1.8   0:01.91 promtail-linux-
      1 root      20   0  107764  16204  11068 S   0.0   0.4   0:00.91 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
```


### Pre-Lab - Disk Speed tests

1. Real quick check for a package that is useful.

```
rpm -qa | grep -i iostat #should find nothing
```

```
[root@rocky7 unit4]# rpm -qa | grep -i iostat
[root@rocky7 unit4]#
```

2. Let's find what provides iostat by looking in the YUM (we'll explore more in later lab)

```
dnf whatprovides iostat
```

This should tell you that `sysstat` provides `iostat`.

```
[root@rocky7 unit4]# dnf whatprovides iostat
Extra Packages for Enterprise Linux 9 - x86_64                                              9.5 MB/s |  23 MB     00:02
Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64                        2.8 kB/s | 2.5 kB     00:00
Rocky Linux 9 - BaseOS                                                                      2.1 MB/s | 2.3 MB     00:01
Rocky Linux 9 - AppStream                                                                   8.6 MB/s | 8.4 MB     00:00
Rocky Linux 9 - Extras                                                                       27 kB/s |  16 kB     00:00
Last metadata expiration check: 0:00:01 ago on Tue Apr 22 16:59:27 2025.
sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : @System
Matched from:
Filename    : /usr/bin/iostat

sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : appstream
Matched from:
Filename    : /usr/bin/iostat
```

3. Let's check to see if we have it

```
rpm -qa | grep -i sysstat
```

```
[root@rocky7 unit4]# rpm -qa | grep -i sysstat
sysstat-12.5.4-9.el9.x86_64
```

4. If you don't, lets install it

```
dnf install sysstat

```

```
Already installed
```

5. Re-check to verify we have it now

```
rpm -qa | grep -I sysstat
rpm -qi sysstat<version>
iostat # We'll look at this more in a bit
```

```
[root@rocky7 unit4]# iostat
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           9.06    0.00    3.18    0.10    7.75   79.91

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.18         3.16         0.00         0.00       2276          0          0
xvdb              0.18         3.08         0.00         0.00       2220          0          0
xvdc              0.18         3.08         0.00         0.00       2220          0          0
xvde              0.18         3.08         0.00         0.00       2220          0          0
```

While we're working with packages, make sure that Vim is on your system.
This is the same procedure as above.

```
rpm -qa | grep -i vim  # Check if vim is installed
# If it's there, good.
dnf install vim
# If it's not, install it so you can use vimtutor later (if you need help with vi commands)
```

```
Installed.
```

## Lab

1. Gathering system information release and kernel information

```
cat /etc/*release
uname
uname -a
uname -r
```

```
[root@rocky7 unit4]# uname
Linux
[root@rocky7 unit4]# uname -a
Linux rocky7 5.14.0-427.42.1.el9_4.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Oct 31 14:01:51 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
[root@rocky7 unit4]# uname -r
5.14.0-427.42.1.el9_4.x86_64
```

run `man uname` to see what those options mean if you don't recognize the values.

```
rpm -qa | grep -i kernel
```

What is your kernel number? Highlight it.

- 5.14

```
rpm -qi <kernel from earlier>
```

```
[root@rocky7 unit4]# rpm -qi kernel-core-5.14.0-503.23.1.el9_5
Name        : kernel-core
Version     : 5.14.0
Release     : 503.23.1.el9_5
Architecture: x86_64
Install Date: Mon Feb 10 18:37:23 2025
Group       : Unspecified
Size        : 68807354
License     : ((GPL-2.0-only WITH Linux-syscall-note) OR BSD-2-Clause) AND ((GPL-2.0-only WITH Linux-syscall-note) OR BSD-3-Clause) AND ((GPL-2.0-only WITH Linux-syscall-note) OR CDDL-1.0) AND ((GPL-2.0-only WITH Linux-syscall-note) OR Linux-OpenIB) AND ((GPL-2.0-only WITH Linux-syscall-note) OR MIT) AND ((GPL-2.0-or-later WITH Linux-syscall-note) OR BSD-3-Clause) AND ((GPL-2.0-or-later WITH Linux-syscall-note) OR MIT) AND Apache-2.0 AND BSD-2-Clause AND BSD-3-Clause AND BSD-3-Clause-Clear AND GFDL-1.1-no-invariants-or-later AND GPL-1.0-or-later AND (GPL-1.0-or-later OR BSD-3-Clause) AND (GPL-1.0-or-later WITH Linux-syscall-note) AND GPL-2.0-only AND (GPL-2.0-only OR Apache-2.0) AND (GPL-2.0-only OR BSD-2-Clause) AND (GPL-2.0-only OR BSD-3-Clause) AND (GPL-2.0-only OR CDDL-1.0) AND (GPL-2.0-only OR GFDL-1.1-no-invariants-or-later) AND (GPL-2.0-only OR GFDL-1.2-no-invariants-only) AND (GPL-2.0-only WITH Linux-syscall-note) AND GPL-2.0-or-later AND (GPL-2.0-or-later OR BSD-2-Clause) AND (GPL-2.0-or-later OR BSD-3-Clause) AND (GPL-2.0-or-later OR CC-BY-4.0) AND (GPL-2.0-or-later WITH GCC-exception-2.0) AND (GPL-2.0-or-later WITH Linux-syscall-note) AND ISC AND LGPL-2.0-or-later AND (LGPL-2.0-or-later OR BSD-2-Clause) AND (LGPL-2.0-or-later WITH Linux-syscall-note) AND LGPL-2.1-only AND (LGPL-2.1-only OR BSD-2-Clause) AND (LGPL-2.1-only WITH Linux-syscall-note) AND LGPL-2.1-or-later AND (LGPL-2.1-or-later WITH Linux-syscall-note) AND (Linux-OpenIB OR GPL-2.0-only) AND (Linux-OpenIB OR GPL-2.0-only OR BSD-2-Clause) AND Linux-man-pages-copyleft AND MIT AND (MIT OR GPL-2.0-only) AND (MIT OR GPL-2.0-or-later) AND (MIT OR LGPL-2.1-only) AND (MPL-1.1 OR GPL-2.0-only) AND (X11 OR GPL-2.0-only) AND (X11 OR GPL-2.0-or-later) AND Zlib
Signature   : RSA/SHA256, Thu Feb  6 11:30:18 2025, Key ID 702d426d350d275d
Source RPM  : kernel-5.14.0-503.23.1.el9_5.src.rpm
Build Date  : Thu Feb  6 04:36:54 2025
Build Host  : iad1-prod-build001.bld.equ.rockylinux.org
Packager    : Release Engineering <releng@rockylinux.org>
Vendor      : Rocky
URL         : https://www.kernel.org/
Summary     : The Linux kernel
Description :
The kernel package contains the Linux kernel (vmlinuz), the core of any
Linux operating system.  The kernel handles the basic functions
of the operating system: memory allocation, process allocation, device
input and output, etc.
```

What does this tell you about your kernel?
	- verion, release, architecture, install date, group, size, license, signiture,
	source rpm, build date, build host, packager, vendor, url, summary, description
When was the kernel last updated?
	- Feb 10
What license is your kernel released under?
	- GPL-2.0

2. Check the number of disks

```
fdisk -l
ls /dev/sd*
```

```
[root@rocky7 unit4]# fdisk -l
Disk /dev/xvdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvdb: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvda: 15 GiB, 16106127360 bytes, 31457280 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvde: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
[root@rocky7 unit4]# ls /dev/xvd*
/dev/xvda  /dev/xvdb  /dev/xvdc  /dev/xvde
```

https://github.com/torvalds/linux/blob/master/Documentation/admin-guide/devices.txt
All devices are listed in the /dev folder.

fdisk -l only shows blocks/partitions that are found in `/proc/partitions`

```
[root@rocky7 unit4]# cat /proc/partitions
major minor  #blocks  name

 202       32    3145728 xvdc
 202       16    3145728 xvdb
 202        0   15728640 xvda
 202       64    3145728 xvde
  11        0    1048575 sr0
```

	- When might this command be useful?
		- when you want to see the disks that are available, as well as the
		partition tables for those disks
	- What are we assuming about the disks for this to work?
		- fdisk will only display blocks found in `/proc/partitions`
		- `/proc/partitions` is populated by the linux kernel
		- if fdisk -l doesn't show a specific disk, then the device isn't found
		by the kernel.
		- if not found, we need to check if the device is connected, either
		physically or virtually, then try to find the disk.
	- How many disks are there on this system?
		- 4 virtual disks
	- How do you know if it's a partition or a disk
		- partitions will have a `#` after the disk name (e.g. /dev/sda1)

What does xvd mean as a block name?
	- /dev/sd* (SCSI Disk)
	- /dev/xvd* (XEN Virtual Device)
	
```
pvs # What system are we running if we have physical volumes?
    # What other things can we tell with vgs and lvs?

if pvs displays something, then the disks have had physical volumes assigned 
with LVM
```

- Use `pvdisply`, `vgdisplay`, and `lvdisplay` to look at your carved up volumes.
Thinking back to last week's lab, what might be interesting from each of those?

- Try a command like `lvdisplay | egrep "Path|Size"` and see what it shows.

	- Does that output look useful?
	- Try to `egrep` on some other values. Separate with `|` between search terms.

TODO:
Can't try with the system unless we setup LVM again	

- Check some quick disk statistics

```
iostat reports CPU statistics and IO stats for devices and partitions
-d displays the device utilization report


iostat -d

[root@rocky7 unit4]# iostat -d
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.05         0.92         0.00         0.00       3332          0          0
xvdb              0.04         0.90         0.00         0.00       3276          0          0
xvdc              0.04         0.90         0.00         0.00       3276          0          0
xvde              0.04         0.90         0.00         0.00       3276          0          0


iostat -d 2   # Wait for a while, then use crtl + c to break. What did this do? Try changing this to a different number.

this displays a continuous device report every 2 seconds


iostat -d 2 5 # Don't break this, just wait. What did this do differently? Why might this be useful?

displays 5 device reports at 2 second intervals.

allows you to get a snapshot of device stats over a period of time, but only a specified
number of reports.
```	

3. Check the amount of RAM

```
cat /proc/meminfo
free
free -m
```

What do each of these commands show you? How are they useful?

`cat /proc/meminfo`
	- Kernel file that reports information about memory usage.
	- Used by free and other programs
	- Useful because it give a commplete breakdown of all memory info
`free`
	- displays the amount of free and used memory in the system
	- usefule as its a quick way to see how much memory is being used on a system.
`free -m`
	- displays the amount of free and used memory in mebibytes
	- Kilo, Mega, Giga, etc. are base 10 prefixes
	- Kibi, Mebi, Gibi, etc. are base 2 prefixes
	- the base 2 prefixes are more precise when dealing with exact amount of space
	such as memory
	- Useful as its more human readable

4. Check the number of processors and processor info

```
cat /proc/cpuinfo
```

What type of processors do you have? Google to try to see when they were released. 
	- Intel Xeon CPU E5-2665 @ 2.4GHz
	- released Mar 6th, 2012
Look at the flags. Sometimes when compiling these are important to know. 
This is how you check what execution flags your processor works with.
https://en.wikipedia.org/wiki/CPUID#EAX.3D1:_Processor_Info_and_Feature_Bits
https://unix.stackexchange.com/questions/43539/what-do-the-flags-in-proc-cpuinfo-mean


```
cat /proc/cpuinfo | grep proc | wc -l
```

Does this command accurately count the processors?
	- Yes

```
iostat -c
iostat -c 2 # Wait for a while, then use Ctrl+C to break. What did this do? Try changing this to a different number.
iostat -c 2 5 # Don't break this, just wait. What did this do differently? Why might this be useful?

[root@rocky7 unit4]# iostat -c
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           1.63    0.03    0.89    0.08    1.45   95.91


This works the same as before, but now showing cpu stats
```

5. Check the system uptime

```
uptime
man uptime
```

Read man uptime and figure out what those 3 numbers represent.
	- Current time, how long the system has been running, how many users are currently
	logged on, system load averages for the past 1, 5, and 15 minutes.

Referencing this server, do you think it is under high load? Why or why not?
	- No. the averages listed are 0.00, 0.02, and 0.00 which means that 5 min ago,
	there are almost no processes running on the CPU or waiting in the run queue.

A load average of 1.00 on a single-core system means the system is fully utilized — 1 process is using or waiting for the CPU.

On a 4-core system, a load average of 4.00 is full utilization; 2.00 would be about 50% usage.

If your load is greater than your number of CPU cores, it usually means CPU contention is occurring (processes are waiting for CPU time).
	

6. Check who has recently logged into the server and who is currently in

```
last
```

Last is a command that outputs backwards. (Top is most recent). 
So it is less than useful without using the more command.

```
last | more
```

Were you the last person to log in? Who else has logged in today?

```
[root@rocky7 unit4]# last | more
root     pts/0        192.168.11.245   Tue Apr 22 16:54   still logged in
reboot   system boot  5.14.0-427.42.1. Tue Apr 22 16:51   still running

wtmp begins Tue Apr 22 16:51:09 2025
```

I am the only person who has logged in (since I rebooted the machine xD)

```
w
who
whoami
```

```
[root@rocky7 unit4]# w
 18:14:53 up  1:25,  1 user,  load average: 0.00, 0.01, 0.00
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0     16:54    5.00s  0.31s  0.00s w
[root@rocky7 unit4]# who
root     pts/0        2025-04-22 16:54 (192.168.11.245)
[root@rocky7 unit4]# whoami
root
```

How many other users are on this system? What does the pts/0 mean on Google?
No other users currently

https://en.wikipedia.org/wiki/Pseudoterminal
https://unix.stackexchange.com/questions/21147/what-are-pseudo-terminals-pty-tty

pts/0 means that I'm logged in to pseudoterminal 0
	- A pseudo terminal is a device that has the functions of a physical terminal 
	without actually being one. Created by terminal emulators such as xterm. More detail is in the manpage pty(7).

7. Check running processes and services

```
ps -aux | more
	- BSD style syntax
	- a shows processes for all users, u shows the processes user/owner, x includes
	processes not attaced to a terminal
	- USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND

ps -ef | more
	- System V style syntax
	- e shows all processes, f displays full-format listing (including parent-child
	heirarchy)
	- UID PID PPID C(CPU Usage) STIME(start time) TTY TIME(CPU time) CMD(command)


ps -ef | wc -l
	- machine returned 100
```

Try to use what you've learned to see all the processes owned by your user
Try to use what you've learned to count up all of those processes owned by your user

By default, ps selects all processes with the same EUID as the current user and 
associated with the same terminal as the invoker.

```
ps -u
ps -u | wc -l
```


8. Looking at system usage (historical)

	- Check processing for last day
	`sar | more`
```
[root@rocky7 unit4]# sar | more
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

16:51:12     LINUX RESTART      (2 CPU)

17:00:23        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:10:23        all      0.90      0.00      0.53      0.07      0.19     98.30
17:20:23        all      0.06      0.00      0.40      0.10      0.19     99.24
17:30:23        all      0.07      0.00      0.42      0.08      0.19     99.24
17:40:23        all      0.06      0.00      0.41      0.07      0.19     99.27
17:50:23        all      0.07      0.00      0.42      0.08      0.19     99.24
18:00:23        all      0.07      0.25      0.47      0.07      0.18     98.95
18:10:23        all      0.06      0.00      0.42      0.07      0.18     99.27
18:20:23        all      0.07      0.00      0.42      0.07      0.18     99.26
18:30:23        all      0.12      0.00      0.46      0.07      0.18     99.17
Average:        all      0.16      0.03      0.44      0.08      0.19     99.11
```

	- Check memory for the last day
	`sar -r | more`
```
[root@rocky7 unit4]# sar -r | more
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

16:51:12     LINUX RESTART      (2 CPU)

17:00:23    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
17:10:23      1473304   1413360    200648      5.03         0   2138748   2610860     65.51   2242992        44         0
17:20:23      1472172   1412236    201708      5.06         0   2138752   2612128     65.54   2243888        44         0
17:30:23      1472052   1412124    201804      5.06         0   2138756   2610868     65.51   2242940        44         0
17:40:23      1472020   1412100    201828      5.06         0   2138756   2610868     65.51   2245104        44         0
17:50:23      1472384   1412460    201436      5.05         0   2138760   2610872     65.51   2245108        44         0
18:00:23      1482296   1422372    191532      4.81         0   2138764   2610876     65.51   2245140        44         0
18:10:23      1483280   1423360    190492      4.78         0   2138768   2612144     65.54   2246024        44         0
18:20:23      1482056   1422136    191728      4.81         0   2138776   2610888     65.51   2245192        44         0
18:30:23      1481340   1421420    192452      4.83         0   2138776   2610888     65.51   2245212        44         0
Average:      1476767   1416841    197070      4.94         0   2138762   2611155     65.52   2244622        44         0
```

Sar is a tool that shows the 10 minute weighted average of the system for the last day.

Sar is tremendously useful for showing long periods of activity and system load.
It is exactly the opposite in it's usefulness of spikes or high traffic loads. In a 
20 minute period of 100% usage a system may just show to averages times of 50% and
50%, never actually giving accurate info.

Problems typically need to be proactively tracked by other means, or with scripts,
as we will see below. Sar can also be run interactively. Run the command
`yum whatprovides sar` and you will see that it is the `sysstat` package. You may
have guessed that sar runs almost exactly like `iostat`.


- Try the same commands from earlier, but with their interactive information.

```
sar 2  # Ctrl+C to break
sar 2 5
# or
sar -r 2
sar -r 2 5
```

- Check sar logs for previous daily usage

```
cd /var/log/sa/
ls
# Sar logfiles will look like: sa01 sa02 sa03 sa04 sa05 sar01 sar02 sar03 sar04
sar -f sa03 | head


[root@rocky7 sa]# sar -f sa22 | head
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

16:51:12     LINUX RESTART      (2 CPU)

17:00:23        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:10:23        all      0.90      0.00      0.53      0.07      0.19     98.30
17:20:23        all      0.06      0.00      0.40      0.10      0.19     99.24
17:30:23        all      0.07      0.00      0.42      0.08      0.19     99.24
17:40:23        all      0.06      0.00      0.41      0.07      0.19     99.27
17:50:23        all      0.07      0.00      0.42      0.08      0.19     99.24

sar -r -f sa03 | head #should output just the beginning of 3 July (whatever month you're in).
-f is file name to specify which log
```

Most Sar data is kept for just one month but is very configurable. Read `man sar` for more info.


Sar logs are not kept in a readable format, they are binary. So if you needed to dump
all the sar logs from a server, you'd have to output it to a file that is readable.

You could do something like this:

- Gather information and move to the right location

```
cd /var/log/sa
pwd
ls
```

- Build a loop against that list of files

```
for file in `ls /var/log/sa/sa??`; do echo "reading this file $file"; done

---

[root@rocky7 sa]# for file in `ls /var/log/sa/sa??`; do echo "reading this file $file"; done
reading this file /var/log/sa/sa22

```


- Execute that loop with the output command of sar instead of just saying the filename

```
for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file ; done

---

[root@rocky7 sa]# for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file; done
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

16:51:12     LINUX RESTART      (2 CPU)

17:00:23        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:10:23        all      0.90      0.00      0.53      0.07      0.19     98.30
17:20:23        all      0.06      0.00      0.40      0.10      0.19     99.24
17:30:23        all      0.07      0.00      0.42      0.08      0.19     99.24
17:40:23        all      0.06      0.00      0.41      0.07      0.19     99.27
17:50:23        all      0.07      0.00      0.42      0.08      0.19     99.24
18:00:23        all      0.07      0.25      0.47      0.07      0.18     98.95
18:10:23        all      0.06      0.00      0.42      0.07      0.18     99.27
18:20:23        all      0.07      0.00      0.42      0.07      0.18     99.26
18:30:23        all      0.12      0.00      0.46      0.07      0.18     99.17
18:40:23        all      0.33      0.00      0.46      0.07      0.18     98.95
Average:        all      0.18      0.02      0.44      0.08      0.19     99.09

```

- But that is too much scroll, so let's also send it to a file for later viewing

```
for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file | tee -a /tmp/sar_data_`hostname`; done

---

[root@rocky7 sa]# for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file | tee -a /tmp/sar_data_`hostname`; done
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/22/25        _x86_64_        (2 CPU)

16:51:12     LINUX RESTART      (2 CPU)

17:00:23        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:10:23        all      0.90      0.00      0.53      0.07      0.19     98.30
17:20:23        all      0.06      0.00      0.40      0.10      0.19     99.24
17:30:23        all      0.07      0.00      0.42      0.08      0.19     99.24
17:40:23        all      0.06      0.00      0.41      0.07      0.19     99.27
17:50:23        all      0.07      0.00      0.42      0.08      0.19     99.24
18:00:23        all      0.07      0.25      0.47      0.07      0.18     98.95
18:10:23        all      0.06      0.00      0.42      0.07      0.18     99.27
18:20:23        all      0.07      0.00      0.42      0.07      0.18     99.26
18:30:23        all      0.12      0.00      0.46      0.07      0.18     99.17
18:40:23        all      0.33      0.00      0.46      0.07      0.18     98.95
Average:        all      0.18      0.02      0.44      0.08      0.19     99.09

```

- Let's verify that file is as long as we expect it to be:

```
ls -l /tmp/sar_data*
cat /tmp/sar_data_<yourhostname> | wc -l

---

[root@rocky7 sa]# ls -l /tmp/sar_data_rocky7
-rw-r--r--. 1 root root 1069 Apr 22 18:43 /tmp/sar_data_rocky7
[root@rocky7 sa]# cat /tmp/sar_data_rocky7 | wc -l
16

```

- Is it what you expected? You can also visually check it a number of ways.

```
cat /tmp/<filename>
more /tmp/<filename>
```

### Exploring Cron

Your system is running the cron daemon. You can check with:

```
ps -ef | grep -i cron
systemctl status crond

[root@rocky7 sa]# systemctl status crond
○ crond.service - Command Scheduler
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: inactive (dead)
```

This is a tool that wakes up between the 1st and 5th second of every minute and 
checks to see if it has any tasks it needs to run. It checks in a few places in
the system for these tasks. It can either read froma crontab or it can execute
tasks from files in the following locations.

`/var/spool/cron` is one location you can ls to check if there are any crontabs 
on your system.

The other locations are directories found under:

```
ls -ld /etc/cron*
```

These should be self-explanatory in their use. If you want to see if the user you
are running has a crontab, use the command `crontab -l`. If you want to edit,
use `crontab -e`.

We'll make a quick crontab entry and I'll point you (here)[https://en.wikipedia.org/wiki/Cron]
if you're interested in learning more.

Crontab format looks like this picture.

```
# .------- Minute (0 - 59)
# | .------- Hour (0 - 23)
# | | .------- Day of month (1 - 31)
# | | | .------- Month (1 - 12)
# | | | | .------- Day of week (0 - 6) (Sunday to Saturday - Sunday is also 7 on some systems)
# | | | | |
# | | | | |
  * * * * *  command to be executed
```

Let's do these steps:
	1. `crontab -e`
	2. Add this line
	`* * * * * echo 'this is my cronjob running at' `date` | wall`
	3. Verify with `crontab -l`
	4. Wait to see if it runs and echos out to wall.
	5. `cat /var/spool/cron/root` to see that it is actually stored where i said it was.
	6. This will quickly become very annoying, so I recommend removing that line, or
	commenting it out (#) from that file.
	


We can change all kinds of things about this to execute at different times. The one
above, we executed every minute throuhg all hours, of every day, of every month.

We could also have done some other things:
- Every 2 minutes (divisible by any number you need)
`*/2 * * * *`
- The first and 31st minute of each hour
`1,31 * * * *`
- The first minut of every 4th hour
`1 */4 * * *`
- NOTE: if you're adding system0-wide cron jobs (`/etc/crontab`), you can also
specify the user to run the command as.
`* * * * * <user> <command>`

There's a lot there to explore, I recommend looking into the Cron wiki or tldp.org's cron guide
(linked at the top) for more information.



